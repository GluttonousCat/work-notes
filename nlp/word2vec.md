# word2vec:zzz:

Word2vec uses shallow neural networks to learn word representations by predicting surrounding words (content) or the target word itself. The embeddings it produces allow similar words to have close vector representations, capturing semantic and syntactic relationships. Word2vec consists of two main architectures.

## :pushpin: ​Skip-Gram

N-gram is thea **contiguous sequence** of *n* items(characters, words, or subwords) from a give text. (refer to the document [**BPE**]()). The  Skip-Gram model learns word embeddings by predict **context words** given a **target word**. This means that for each word in the training data, the model tries to guess which words are likely to appear nearby.
$$
P(w_{t+j} | w_t) = \frac{\exp(\mathbf{v}_{w_{t+j}}^\top \mathbf{v}_{w_t})}{\sum_{w \in V} \exp(\mathbf{v}_{w}^\top \mathbf{v}_{w_t})}
$$







## :pushpin: ​CWOB

