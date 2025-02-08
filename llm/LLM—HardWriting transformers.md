# Transformer:hugs:

## Embedding

### Position Embedding

In transformers models, PE(Position Embedding) is used to inject information about the relative or absolute position of tokens in a sequence since self-attention mechanisms are permutation-invariant. The core idea behind positional encoding is to provide each position in the sequence with a unique representation that helps the model understand order.

:white_check_mark: Math Formula

The sinusoidal positional encoding is defined as: ​​
$$
PE_{(pos,2i)}=sin(\frac{pos}{10000^\frac{2i}{d_{model}}})
$$

$$
PE_{(pos,2i+1)}=cos(\frac{pos}{10000^\frac{2i}{d_{model}}})
$$

where:

1. pos is the position index in the sequence.
2. $i$ is the dimension index. 
3. $d_{model}$ is the embedding dimension.
4. $10000^{\frac{2i}{d_{model}}}$ is the scaling factor that ensures different frequency scales are used across different dimensions.

:pencil: Example Code 

```python
import math
import torch
import torch.nn as nn
import torch.nn.functional as F

class PositionEmbedding(nn.module):
    """
    PositionEmbedidng
    Attribute:
        
    """
    def __init__(self, dimension: int = 768, max_len: str = 768):
        super().__init__()
        self.ecoding = torch.zeros(max_len, dimension)
        position = torch.arange(0, max_len).float().unsqueeze(1)  # shape: (max_len, 1)
        div_term = torch.exp(torch.arange(0, dimension, 2).float())
    
    def forward(self):
        pass
```

### Word Embedding

Word embedding is the foundation of modern NLP, converting the words to dense vector representations. To fully understand word embeddings, we need to explore **token**, **subword**, **vocabulary**, **embedding models** and so on.

:pushpin: Tokenization: First Step

Before feeding the sentence to embedding models, we must convert the full words to tokens. Token is the basic unit of meaning for deep model, which can be:

1.  A word: `ChatGPT`-> `[ChatGPT]` 
2.  A subword: `ChatGPT` -> `[Chat,GPT]`
3.  A char: `ChatGPT` ->`[C,h,a,t,G,P,T]`

:pushpin: Vocabulary: Model Dictionary

A **vocabulary** is the set of all possible tokens the model can understand. It is **finite** which meaning that model cant understand the token out of the Vocabulary(**OOV**). Many models will process the OOV tokens. The methods inclue:

1. Using a special token (e.g., `unk` in bert).
2. Using subword tokenization (e.g., `ChatGPT` ->`Chat`+`GPT`)

:pushpin: SubWord Tokenization: BPE(Popularity)

:pushpin: ToVector: Tokens to Vector

## Multi Head Attention

### Self-Attention

Sefl-Attention compute the similar score between the query-key pairs and . u can read the attention to get more detailed explanation and thought about attention and seq2seq.

:white_check_mark: Attention Math Formula

The attention formula is as following, This attention is called self-attention when Q, K, V is the sample matrix.
$$
Attention(Q,K,V)=softmax(\frac{QK^T}{\sqrt{d_k}})V
$$
where:

1.  $Q=XW_q$ (queries)
2.  $K=XW_k$ (keys)
3.  $V=XW_v$ (values)

:pencil: Example Code​

```python
class MultiHeadAttention(nn.module):
    def __init__(self):
        super().__init__()
        pass
    
    def forward(self):
        pass
```

## Feed Forward Network

```python
class FeedForward(nn.module):
    def __init__(self):
        super().__init__()
        pass
    
    def forward(self):
        pass
    
```

## Encoder-Decoder Layer

```python
class EncoderLayer(nn.module):
    def __init__(self):
        super().__init__()
        pass
    
    def forward(self):
        pass
    
```

## Transformer

```python
class Transformers(nn.module):
    def __init__(self):
        super().__init__()
        pass
    
    def forward(self):
        pass
```

## FAQ:

:question: So, Why to say self-attention mechanisms are permutation-invariant?​

:bulb: ​That is means self-attention does not inherently consider the order of tokens in a sequence. That is, without positional encoding, if we shuffle the input tokens in a sequence, the output of the self-attention mechanism will remain the same.

:question: Why use Sin and Cos Functions?

:bulb: ​

:question: How to Understand the Exponential Scaling Factor?

:bulb: 

