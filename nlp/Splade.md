🤗 huggingface: https://huggingface.co/naver/splade-cocondenser-ensembledistil

<img src="./logo/github-mark.png" style="zoom:9%;" /> github: https://github.com/naver/splade 

<img src="./logo/arvix.jpg" style="zoom:18%;" /> Arxiv: https://arxiv.org/abs/2107.05720

通过显式的稀疏正则化和对词项权重的对数饱和效应来提升模型的性能，产生高度稀疏的向量

# 算法原理


$$
w_{ij}=\text{transform}(h_i)^T E_j + b_j
$$




$$
w_j = g_j \times \sum_{i \in t} \text{ReLU}(w_{ij})
$$



$$
w_j = \sum_{i \in t} \log(1 + \text{ReLU}(w_{ij}))
$$



$$
L_{\text{rank-IBN}} = -\log \frac{e^{s(q_i, d_i^+)}}{e^{s(q_i, d_i^+)} + e^{s(q_i, d_i^-)} + \sum_j e^{s(q_i, d_{i,j}^-)}}
$$




# 模型调用

可以通过transformer调用

```bash
pip install transformer
```

```python
from transformer import AutoModelForMaskedLM
from transformer import AutoTokenizer

tokenizer = AutoTokenizer.from_pretrained("naver/splade-cocondenser-ensembledistil")
model = AutoModelForMaskedLM.from_pretrained("naver/splade-cocondenser-ensembledistil")

content = "hello, word"
inputs = tokenizer(inputs, return_tensors="pt", padding=True, truncation=True, max_length=768)
outputs = model(**inputs)
```

```python
inputs
```

```
{'input_ids': tensor([[ 101, 7592, 1010, 2773,  102]]), 'token_type_ids': tensor([[0, 0, 0, 0, 0]]), 'attention_mask': tensor([[1, 1, 1, 1, 1]])}
```

```python
outputs
```

```
MaskedLMOutput(loss=None, logits=tensor([[[ -7.4025,  -8.6849,  -7.9479,  ...,  -8.1572,  -8.0747,  -6.7814],
         [-34.1390, -24.4607, -22.9773,  ..., -23.1794, -22.8901, -30.2398],
         [ -8.4153,  -9.3625,  -8.3080,  ...,  -8.6271,  -8.7100,  -7.6319],
         [-32.6357, -24.3928, -22.8281,  ..., -21.9120, -23.9582, -22.8618],
         [-18.3341, -15.1907, -14.5220,  ..., -14.9129, -13.8533, -16.0387]]],
       grad_fn=<ViewBackward0>), hidden_states=(tensor([[[ 0.0554,  0.0158, -0.0760,  ...,  0.0348, -0.0289,  0.0468],
         [ 0.3517, -0.0602, -0.3525,  ...,  0.1051,  0.1990,  0.1966],
         [-0.3564, -0.1344,  0.1547,  ...,  0.2134,  0.3744,  0.0149],
         [ 0.1594,  1.0084,  0.0698,  ...,  0.3591, -0.5096, -0.2100],
         [-0.3653, -0.2404,  0.0397,  ...,  0.0155, -0.0049,  0.2641]]],
       grad_fn=<NativeLayerNormBackward0>), ...), attentions=None)
```

```python
model
```

```
BertForMaskedLM(
  (bert): BertModel(
    (embeddings): BertEmbeddings(
      (word_embeddings): Embedding(30522, 768, padding_idx=0)
      (position_embeddings): Embedding(512, 768)
      (token_type_embeddings): Embedding(2, 768)
      (LayerNorm): LayerNorm((768,), eps=1e-12, elementwise_affine=True)
      (dropout): Dropout(p=0.1, inplace=False)
    )
    (encoder): BertEncoder(
      (layer): ModuleList(
        (0-11): 12 x BertLayer(
          (attention): BertAttention(
            (self): BertSdpaSelfAttention(
              (query): Linear(in_features=768, out_features=768, bias=True)
              (key): Linear(in_features=768, out_features=768, bias=True)
              (value): Linear(in_features=768, out_features=768, bias=True)
              (dropout): Dropout(p=0.1, inplace=False)
            )
            (output): BertSelfOutput(
              (dense): Linear(in_features=768, out_features=768, bias=True)
              (LayerNorm): LayerNorm((768,), eps=1e-12, elementwise_affine=True)
              (dropout): Dropout(p=0.1, inplace=False)
            )
          )
          (intermediate): BertIntermediate(
            (dense): Linear(in_features=768, out_features=3072, bias=True)
            (intermediate_act_fn): GELUActivation()
          )
          (output): BertOutput(
            (dense): Linear(in_features=3072, out_features=768, bias=True)
            (LayerNorm): LayerNorm((768,), eps=1e-12, elementwise_affine=True)
            (dropout): Dropout(p=0.1, inplace=False)
          )
        )
      )
    )
  )
  (cls): BertOnlyMLMHead(
    (predictions): BertLMPredictionHead(
      (transform): BertPredictionHeadTransform(
        (dense): Linear(in_features=768, out_features=768, bias=True)
        (transform_act_fn): GELUActivation()
        (LayerNorm): LayerNorm((768,), eps=1e-12, elementwise_affine=True)
      )
      (decoder): Linear(in_features=768, out_features=30522, bias=True)
    )
  )
)
```

