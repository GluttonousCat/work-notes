# DistilledPairwiseModel:zzz:

## TS Train Strategy

Teacher Model: bge_reranker-m3-v2, hidden_size: 1024,  inherit XLM-Roberta

Student Model: DistillBert, hidden_size: 768, inherit XLM-Roberta

Loss Function: Ranknet loss + kl + L2

Dataset: duread2022

## RerankModel

```python
import torch.nn as nn

# RerankModel
class RerankModel(nn.Module)
    def __init__(self, teacher_model, student_model):
        super().__init__()
        self.teacher_model = teacher_model
        self.student_model = student_model
        self.teacher_proj = nn.Linear(1024, 768)  
        self.teacher_proj_intermediate = nn.Linear(1024, 768)  

        self.student_fc = nn.Linear(768, 1)
    def forward(self, tea_inputs, stu_inputs):
        teacher_output = self.teacher_model(**tea_inputs, output_hidden_states=True)
        teacher_hidden_state = teacher_output.hidden_states[-1]  
        teacher_features = self.teacher_proj(teacher_hidden_state[:, 0, :])
        
        student_output = self.student_model(**stu_inputs, output_hidden_states=True)
        student_hidden_state = student_output.hidden_states[-1]
        student_features = student_hidden_state[:, 0, :]
        student_result = self.student_fc(student_features)  

        teacher_intermediate = teacher_output.hidden_states[-2]      
        teacher_intermediate_features = self.teacher_proj_intermediate(
            teacher_intermediate[:, 0, :]
        )  
        student_intermediate = student_output.hidden_states[-2] 

        return teacher_features, student_result, teacher_intermediate_features, student_intermediate        
```

## Loss Function

### RanknetLoss

```python 
import torch

class RankNetLoss(nn.Module):
    def __init__(self):
        super().__init__()

    def forward(self, scores, labels):
        """
        :param scores: 学生模型的预测分数，形状为 [batch_size]
        :param labels: 排序标签，形状为 [batch_size]，值为 1 或 0，表示优劣关系
        """
        scores_diff = scores.unsqueeze(1) - scores.unsqueeze(0) 
        sigmoid_diff = torch.sigmoid(scores_diff)  
        labels_diff = labels.unsqueeze(1) - labels.unsqueeze(0) 
        target = (labels_diff > 0).float()  
        loss = -(
            target * torch.log(sigmoid_diff + 1e-10) + 
            (1 - target) * torch.log(1 - sigmoid_diff + 1e-10)
        )
        return loss.mean()
```

### KL

```python
import torch.nn.functional as F

class KLLoss(nn.Module):
    def __init__(self):
        super().__init__()

    def forward(self, teacher_probs, student_probs):
        """
        :param teacher_probs: 教师模型的概率分布 [batch_size, num_classes]
        :param student_probs: 学生模型的概率分布 [batch_size, num_classes]
        """
        loss = F.kl_div(
            input=student_probs.log(), 
            target=teacher_probs,
            reduction="batchmean"  
        )
        return loss
```

### L2

```python
class MSELoss(nn.Module)
    def __init__(self):
        super().__init__()

    def forward(self, teacher_intermediate, student_intermediate):
        """
        :param teacher_intermediate: 教师模型中间层特征 [batch_size, seq_len, hidden_dim]
        :param student_intermediate: 学生模型中间层特征 [batch_size, seq_len, hidden_dim]
        """
        return F.mse_loss(student_intermediate, teacher_intermediate)

```

## Dataset

```python
import json
from transformers import Dataset, Dataloader

class RerankDataset(Dataset):
    def __init__(self, data_path):
        super().__init__()
        
```

## Train

```python
import torch
from torch.utils.data import DataLoader
from torch.nn import MSELoss
from transformers import (
    DistilBertForSequenceClassification,
    DistilBertTokenizer,
    AutoModelForSequenceClassification,
    AutoTokenizer,
)

device = torch.device("cuda" if torch.cuda.is_available() else 'cpu')
tea_model = AutoModelForSequenceClassification.from_pretrained("path").to(device)
stu_model = DistilBertForSequenceClassification.from_pretrained("path").to(device)
stu_tokenizer = DistilBertTokenizer.from_pretrained("path")
tea_tokenizer = AutoTokenizer.from_pretrained("path")

model = RerankModel(tea_model, stu_model).to(device)
optimizer = torch.optim.Adam(model.parameters(), lr=1e-4)

mse_loss = MSELoss()
rank_loss = RankNetLoss()
kl_loss = KLLoss()

data = RankDataset('')
dataloader = DataLoader(data, batch_size=8, shuffle=True)

model.train()
for epoch in range(20):
    for batch in dataloader:
        label = batch['label'].to(device)
        query = batch['query']
        answer = batch['answer']
        tea_inputs = tea_tokenizer(query, answer, return_tensors="pt", padding=True, truncation=True).to(device)
        stu_inputs = stu_tokenizer(query, answer, return_tensors="pt", padding=True, truncation=True).to(device)
        tea_features, stu_result, tea_intermediate, stu_intermediate = model(
            tea_inputs, stu_inputs
        )
        total_loss = (
            mse_loss(tea_intermediate, stu_intermediate) +
            rank_loss(stu_result, label) +
            kl_loss(F.softmax(tea_features, dim=-1), F.softmax(stu_result, dim=-1))
        )
       
        optimizer.zero_grad()
        total_loss.backward()
        optimizer.step()
        
stu_model.save_pretrained("student_model_path")
stu_tokenizer.save_pretrained("student_model_path")
```

