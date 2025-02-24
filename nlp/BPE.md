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

