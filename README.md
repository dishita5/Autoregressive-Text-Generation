# Comparative Study of RNN Architectures for Autoregressive Text Generation

## Overview

This project investigates word-level next-token prediction and autoregressive text generation using four recurrent neural network architectures: SimpleRNN, Bidirectional RNN (BiRNN), Long Short-Term Memory (LSTM), and Gated Recurrent Unit (GRU).

The models are trained on Arthur Conan Doyle's *The Adventures of Sherlock Holmes* and evaluated using predictive performance metrics, computational training time, text diversity, repetition, and qualitative inspection of generated samples.

## Objectives

- Prepare a literary text corpus for next-word prediction.
- Train and compare four recurrent neural network architectures.
- Evaluate models using test loss, perplexity, accuracy, and training time.
- Investigate the effect of sampling temperature on generated text.
- Compare lexical diversity and repetition across models.
- Identify the strengths and limitations of the evaluated architectures.

## Dataset

**Source:** Project Gutenberg, *The Adventures of Sherlock Holmes* by Arthur Conan Doyle.

**Source URL:** https://www.gutenberg.org/cache/epub/48320/pg48320.txt

The corpus contains 12 stories and approximately 104,549 whitespace-separated words before tokenization.

### Preprocessing

- Extracted the story text and removed ebook metadata.
- Split the text chronologically into training, validation, and test sets using an 80/10/10 ratio.
- Fitted the tokenizer on the training text.
- Limited the vocabulary to 10,000 words, including the out-of-vocabulary token.
- Constructed input sequences of 20 tokens for next-token prediction.
- Used a batch size of 64.

## Model Architectures

All four models use a 128-dimensional embedding, 128 recurrent units, and a softmax output layer for next-token prediction.

| Model | Description |
|---|---|
| SimpleRNN | Standard recurrent neural network |
| BiRNN | Bidirectional recurrent layer applied to the input context window |
| LSTM | Recurrent architecture with input, forget, and output gates |
| GRU | Recurrent architecture with update and reset gates |

### Training Configuration

- Optimizer: Adam
- Initial learning rate: 0.001
- Loss function: Sparse categorical cross-entropy
- Maximum epochs: 10
- Batch size: 64
- Early stopping based on validation loss
- Learning-rate reduction on validation-loss plateau

## Evaluation

### Test-Set Results

| Model | Test Loss ↓ | Perplexity ↓ | Accuracy ↑ | Training Time (min) |
|---|---:|---:|---:|---:|
| GRU | **5.7839** | **325.03** | **13.88%** | 0.83 |
| SimpleRNN | 5.8334 | 341.52 | 13.36% | **0.63** |
| BiRNN | 5.8521 | 347.96 | 12.05% | 0.68 |
| LSTM | 5.9055 | 367.04 | 13.45% | 1.18 |

GRU achieved the lowest test loss and perplexity and the highest test accuracy in this experiment. SimpleRNN had the shortest measured training time. These results suggest that GRU provided the best overall predictive performance among the four architectures under the experimental settings used.
### Temperature-Controlled Generation

Text was generated using three seed prompts at temperatures of 0.5, 1.0, and 1.3.

- **Temperature 0.5:** More concentrated sampling, generally associated with more predictable text and greater repetition.
- **Temperature 1.0:** A balance between predictability and variety.
- **Temperature 1.3:** More varied sampling, which can also produce less coherent text.

Diversity was measured using Distinct-1 and Distinct-2. These metrics estimate the proportion of unique words and unique consecutive word pairs, respectively. Higher diversity does not necessarily mean better grammatical or contextual quality.

### Main Findings

- GRU performed best on the reported test loss, perplexity, and accuracy.
- SimpleRNN trained fastest under the tested configuration.
- Higher sampling temperatures generally increased lexical diversity and reduced repetition.
- Qualitative inspection showed limitations in grammatical coherence and contextual consistency across generated samples.

## Repository Contents

- `sherlock_holmes_next_word_prediction.ipynb` — experiment notebook.
- `data/` — dataset and processed data, where included.
- `results/` — evaluation tables, generated samples, plots, and experiment summaries.

## How to Run

1. Open the notebook in Google Colab.
2. Download the source corpus from the Project Gutenberg URL above, or upload the prepared dataset.
3. Run the preprocessing cells.
4. Train the four model architectures.
5. Evaluate the models and generate text at the specified temperatures.

### Requirements

The notebook uses Python, TensorFlow/Keras, NumPy, pandas, and Matplotlib. See the notebook imports for the exact environment and dependencies.

## Limitations

- The experiment uses a single literary corpus.
- The word-level vocabulary and out-of-vocabulary handling limit the representation of rare words.
- The models show signs of overfitting based on their training and validation loss curves.
- Next-word accuracy is relatively low, and generated text frequently lacks coherence.
- Results depend on the chosen architecture, hyperparameters, random initialization, and computational environment.
- Diversity metrics do not directly measure grammatical correctness or semantic consistency.

## Conclusion

This experiment compares four recurrent architectures for autoregressive text generation using a Sherlock Holmes corpus. Under the selected configuration, GRU provided the strongest overall predictive performance, while SimpleRNN offered the shortest training time. Temperature experiments demonstrated that increasing diversity can come at the expense of coherent generation.

Future work could explore larger training corpora, improved tokenization, hyperparameter tuning, and transformer-based language models.

## Dataset Attribution

Arthur Conan Doyle, *The Adventures of Sherlock Holmes*. Project Gutenberg, ebook source:

https://www.gutenberg.org/cache/epub/48320/pg48320.txt

Consult the source ebook for its applicable terms and redistribution information.
