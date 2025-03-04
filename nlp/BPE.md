# BPE:zzz:

BPE(Byte Pair Encoding is a subword tokenization method commonly used in NLP, especially in models like GPT, Bert and other transformer-based models)

:pushpin: Process the corpus

1. **Tokenization**: Start with a corpus of text. For BPE, you'll typically work with character-level tokenization initially. Each word is split into individual characters. 
2. **Vocabulary Initialization**: The initial vocabulary consists of all individual characters in your text corpus, including spaces and punctuation.

:pushpin: Counting pairs

1. **Initial Pair Counting**: Start by scanning though the corpus to find all pairs of consecutive characters(bigrams). For example, in the word `low`, the pairs would be `('l', 'o')` and `('o', 'w')`.
2. **Count Frequency**: Count how frequency about each pairs appears across the entire corpus.

:pushpin: Merging the most frequent pair​

1. **Merging the most frequent pair**: Choose the most frequent pair from the counts and merge it into a single token. For example, pair `('l', 'o')` appears most frequent, merge them into a new token `lo` .
2. **Update Vocabulary**: After merging the pair, update the vocabulary: remove the individual characters `'l'` and `'o'` and then add the new token `lo` .
3. **Re-tokenize**: After merging the most frequent pair, go though the entire corpus again and replace occurrences of this pair with new token.

:pushpin: Repeat pair Merging​

1. **Repeat pair Merging**: Keep repeating the process: count the pairs, merge the most frequent pairs, update the corpus, until you reach the designed vocabulary size (numbers of merges). The number will determine how fine-grained you subword units are.

:white_check_mark: ​Assume there is a word list containing [`low`,`lower`,`newest`,`wildest`], which frequencies are 5, 2, 6, 3.

By splitting these words, we can get the vocabulary `[l,o,w,e,r,s,n,t,i,d]`. we can easily find the combination of `es` appears **9** time in the word list, so we can get a new vocabulary like   `[l,o,w,e,r,n,t,i,d,es]`, and `s` does not have other pair in the word list, so we can remove the `s` from the new vocabulary. And then, the `est` becomes the most frequent combination in the vocabulary.  so we can re-form a new vocabulary `[l,o,w,e,r,n,i,d,est]`, similarly remove the `t`. Go down one by one, finally, we can get the vocabulary we need. Generally, the process will stop when come the max subwords numbers.

# N-gram:zzz:

 An n-gram is a **contiguous sequence** of *n* items(characters, words, or subwords) from a give text. N-grams are widely used in NLP.

:pushpin: Definition​ of N-gram

1. An **n-gram** is a sequence of **n** consecutive elements from a given text.
2. The elements can be **characters, words, or subwords**.
3. N-grams help capture **local dependencies** in text.

:white_check_mark: **Character-level N-grams (for "hello")**

| **n**                | **N-gram Examples**         |
| -------------------- | --------------------------- |
| **1-gram (unigram)** | `["h", "e", "l", "l", "o"]` |
| **2-gram (bigram)**  | `["he", "el", "ll", "lo"]`  |
| **3-gram (trigram)** | `["hel", "ell", "llo"]`     |

:white_check_mark: ​**Word-level N-grams (for "I love NLP models")**

| **n**                | **N-gram Examples**                    |
| -------------------- | -------------------------------------- |
| **1-gram (unigram)** | `["I", "love", "NLP", "models"]`       |
| **2-gram (bigram)**  | `["I love", "love NLP", "NLP models"]` |
| **3-gram (trigram)** | `["I love NLP", "love NLP models"]`    |

:pushpin: Types of N-grams

:white_check_mark: **Unigrams (1-grams)**

- Single tokens (words or characters).
- Example: `"hello"` → `["h", "e", "l", "l", "o"]`
- Used in **bag-of-words (BoW)** models.

:white_check_mark: **Bigrams (2-grams)**

- Two consecutive elements.
- Example: `"hello"` → `["he", "el", "ll", "lo"]`
- Captures some contextual information.

:white_check_mark: **Trigrams (3-grams)**

- Three consecutive elements.
- Example: `"hello"` → `["hel", "ell", "llo"]`
- Useful for **language modeling** (e.g., predicting the next word).

:white_check_mark: **Higher-order N-grams (4-grams, 5-grams, etc.)**

- Longer sequences, capturing more context.
- Example: `"NLP is amazing"` → `["NLP is amazing", "is amazing indeed"]`
- Higher $n$ values give **better context**, but **increase sparsity**.

:pushpin: Applications of N-grams

:white_check_mark: ​**Language Modeling**

- Used in **statistical language models (SLM)** (e.g., predicting the next word).
- Example: If `("I am")` is common, then `"happy"` may be predicted next.

:white_check_mark: ​**Text Classification & NLP Tasks**

- Used as **features** in machine learning.
- Example: Sentiment analysis (`"not good"` → negative sentiment).

:white_check_mark: ​**Speech Recognition**

- Helps in **word prediction**.
- Example: `"I want to"` → `"eat"` (high probability).

:white_check_mark: ​**Spelling & Grammar Checking**

- Detects **incorrect word sequences**.

:white_check_mark: ​**Tokenization in NLP**

- Used in **subword-based tokenization** (e.g., BPE, SentencePiece).

:pushpin: Advantages & Disadvantages​

| **Feature**            | **Advantages**      | **Disadvantages**            |
| ---------------------- | ------------------- | ---------------------------- |
| **Unigrams (1-grams)** | Fast, simple        | No context                   |
| **Bigrams (2-grams)**  | Some context        | Limited dependencies         |
| **Trigrams (3-grams)** | More context        | Higher sparsity              |
| **Higher N-grams**     | Even better context | Data sparsity & large memory |
