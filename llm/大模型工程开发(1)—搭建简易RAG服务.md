# RAG

>:pushpin: ​Use Milvus and Mistral-8B-int4 to build a simple RAG system

The quantization tutorial refers to the LLM engineering dev documentation, and Milvus tutorial refers to the Milvus engineering dev documentation

## SDK Version

`python`:3.10+ 

`milvus`:2.4.0+

`transformer`:4.45

`pytorch`:2.4

`spacy`:3.8.4

`fastapi`:1.15

## Chunking Strategies

> :pushpin: ​Some chunking ideas for reference:
> https://www.analyticsvidhya.com/blog/2024/10/chunking-techniques-to-build-exceptional-rag-systems/

:point_right: Use Spacy to assistant dev (Spacy tutorial refers to the Spacy engineering dev documentation)

## Retrieval

:point_right: Use BM25 and vector retrieval to achieve hybrid search (BM25 config refers to the BM25 documentation)

we use the following bm25 config

```yaml
BM25:
  k: 1.2
  b: 0.75
  inverted_index_save_path: "/"
  
```

Use bge as embedding model. You can get the model from [hugging-face](https://huggingface.co/) or [modelscope](https://www.modelscope.cn/home) by python sdk or git tool

use python example:

```python
from transformers import AutoModel, AutoTokenizer

tokenizer = AutoTokenizer.from_pretrained("BAAI/bge-m3")
model = AutoModel.from_pretrained("BAAI/bge-m3")
```

use git example:

```bash
git lfs install
git clone https://huggingface.co/BAAI/bge-m3
```

use bge to embedding

```python
import torch
import numpy as np
from torch import nn
from typing import List
from transformers import AutoModel, AutoTokenizer


class Bge:
    def __init__(
            self, pretrained_model_name_or_path: str, 
            device: str, max_length: int = 0
    ):
        self.device = device
        self.tokenizer = AutoTokenizer.from_pretrained(
                pretrained_model_name_or_path
        )
        if max_length:
            self.max_length = max_length
        else:
            self.max_length = self.tokenizer.model_max_length
        self.model = AutoModel.from_pretrained(
            pretrained_model_name_or_path).to(self.device)

    @torch.no_grad()
    def encode(self, texts: str | List[str]) -> np.ndarray | None:
        self.model.eval()
        inputs = self.tokenizer(
            text=texts, return_tensors='pt', truncation=True,
            padding=True, max_length=self.max_length
        ).to(self.device)

        embeddings = self.model(**inputs).last_hidden_state[:, 0, :]

        embeddings = self.linear_layer(embeddings)
        norm_embeddings = (
                embeddings / torch.norm(embeddings, p=2, dim=1, keepdim=True)
        )
        return norm_embeddings

```





:point_right: 