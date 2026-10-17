# Dataset

This directory contains the dataset used for the Sherlock Holmes next-word prediction experiment.

## Source

Arthur Conan Doyle, *The Adventures of Sherlock Holmes*, obtained from Project Gutenberg.

Source URL: https://www.gutenberg.org/cache/epub/48320/pg48320.txt

## Contents

- Cleaned Sherlock Holmes corpus
- Individual story text
- Processed next-word sequences
- Fitted tokenizer

## Preprocessing

The corpus was cleaned and divided chronologically into training, validation, and test sets using an 80:10:10 split. The tokenizer was fitted on training text only to reduce data leakage. Input sequences contain 20 tokens, and the target is the next token.
