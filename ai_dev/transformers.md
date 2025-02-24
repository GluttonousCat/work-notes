# Transformers :hugs:

State-of-the-art Machine Learning for pytorch. This document mainly introduces how to use transformers in NLP engineering field include text classification, named entity recognition, question answering, language modeling, code generation, summarization, translation, multiple choice, and text generation.

If you want to know more details or computer vision and multimodal, you can lover the [official document](https://huggingface.co/docs/transformers/index).

## Install 

Casually, you can install transformer by simple pip command like `pip install transformers`, just note the version of transformers especially on domestic servers such as Ascend and Hygon.

## Task Guides

### Text Classification

Text classification is a common NLP task that assigns a label or class to text.  It is one of the most widely used application scenario. In fact, many nlp masks are essentially text classification like **sentiment analysis**, **ner** and so on. Next, use the sentiment analysis scenario to introduce the workflow of transformer text classification.

:pushpin: download the sentiment analysis dataset SST

```python
from datasets import load_dataset

# Load the Stanford Sentiment Treebank dataset
dataset = load_dataset("stanfordnlp/sst")
```

:white_check_mark: data example

```bash
```





```python
from transformers import AutoModelForSequenceClassification
model = AutoModelForSequenceClassification.from_pretrained()
```



```python
from transformers import AutoModel, AutoTokenizer

model = AutoModel.from_pretrained()
tokenizer = AutoTokenizer.from_pretrained()
```

