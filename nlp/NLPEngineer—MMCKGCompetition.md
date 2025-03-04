# 瑞金医院MMC人工智能辅助知识图谱大赛项目

## Scenario and Problem

* :question:
* :question:
* :question:
* :question:

## Idea and Approach

:bulb:

:bulb:

:bulb:

## DataLoader

```python
"""
Data instance:
    Content:
    Tag: T2
"""
```

```bash
pip install 
```

```python
import
```





```python
import os
import re


class DataLoader:
    def __init__(self, folder_path):
        self.data = {}
        self.folder_path = folder_path
        self.files_dict = {
            f[:-4]: (
                os.path.join(folder_path, f),
                os.path.join(folder_path, f[:-4] + '.ann')
            )
            for f in os.listdir(folder_path)
            if f.endswith('.txt')
        }
        print("Start parsing the data files......")
        self.data = self._parse_file()
        print(
            f"Finish parsing the data files. "
            f"There is total {self.data['content'].__len__()} str"
        )

    def _parse_file(self):
        data = {
            "content": [],
            "tags": [],
        }
        for pair in self.files_dict.values():
            txt_file = pair[0]
            ann_file = pair[1]
            content = list("".join(self._load_file(txt_file)))
            tags = ['O' for _ in content]
            tags_info = self._load_file(ann_file)
            parsed_tags_info = self._parse_tags_info(tags_info)
            for parsed_tag_info in parsed_tags_info:
                entity_start_index = int(parsed_tag_info[1])
                entity_end_index = int(parsed_tag_info[-2])
                if len(parsed_tag_info[-1]) == 1:
                    tags[entity_start_index] = "S-" + parsed_tag_info[0]
                    continue
                tags[entity_start_index] = "B-" + parsed_tag_info[0]
                tags[entity_end_index - 1] = "E-" + parsed_tag_info[0]
                for i in range(entity_start_index + 1, entity_end_index - 1):
                    tags[i] = 'I-' + parsed_tag_info[0]
            content, tags = self._rm_stop_word(content, tags)
            data['content'] += content
            data['tags'] += tags
        return data

    def _rm_stop_word(self, content, tags):
        stop_words = [
            word.replace("\n", "")
            for word in self._load_file('./data/stopwords.txt')
        ]
        for i in range(len(content) - 1, -1, -1):  # 从最后一个元素开始，倒序遍历
            if content[i] in stop_words:
                content.pop(i)
                tags.pop(i)
        return content, tags

    @classmethod
    def _parse_tags_info(cls, tags_info):
        split_tags_info = []
        start_tags_info = [
            tag_info.split('\t')[1:3] for tag_info in tags_info
        ]
        for start_tag_info in start_tags_info:
            split_tag_info = re.split(r'[; ]', start_tag_info[0])
            split_tag_info.append(start_tag_info[1].replace('\n',''))
            split_tags_info.append(split_tag_info)
        return split_tags_info

    @staticmethod
    def _load_file(file):
        with open(file, 'r', encoding='utf-8') as f:
            file_content = f.readlines()
            return file_content

    def __getitem__(self, index):
        return self.data['content'][index], self.data['tags'][index]

    def __call__(self, *args, **kwargs):
        pass
```



## :star: Method: BiLSTM+CRF



```python
import torch
import torch.nn as nn
from TorchCRF import CRF

class BiLSTMCRF(nn.module):
    def __init__(self, )
```



## :star: Method2: Bert+BiLstm+CRF

```python
import torch
import torch.nn as nn
from TorchCRF import CRF
from transformers import AutoTokenizer, AutoModelForTokenClassification

tokenizer = AutoTokenizer.from_pretrained("dslim/bert-base-NER")

# model_path = 'dslim/bert-base-NER'
class BertBiLSTMCRF(nn.module):
    def __init__(self, model_path):
        self.bert = model = AutoModelForTokenClassification.from_pretrained("dslim/bert-base-NER")
        self.lstm = nn.LSTM(768, 768, bidirectional=True, batch_first=True)
        self.crf = CRF(58)
        
    def forward(self, intput_ids, attention_mask):
        pass
    
```



