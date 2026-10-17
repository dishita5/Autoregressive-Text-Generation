# Processed Dataset

This directory contains the preprocessed artifacts used for next-word prediction.

- `next_word_sequences.npz`: NumPy arrays containing the prepared input sequences and next-token targets.
- `tokenizer.pkl`: The fitted Keras tokenizer used to map words to token IDs.

The input sequence length is 20 tokens. The dataset was split chronologically into training, validation, and test sets with an 80:10:10 ratio.
