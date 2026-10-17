# Comparative Study of RNN Architectures for Autoregressive Text Generation

## Overview

This project investigates word-level next-word prediction and autoregressive text generation using four recurrent neural network architectures: SimpleRNN, Bidirectional RNN (BiRNN), Long Short-Term Memory (LSTM), and Gated Recurrent Unit (GRU).

The models are trained on Arthur Conan Doyle's *The Adventures of Sherlock Holmes*. Their performance is compared using test loss, perplexity, prediction accuracy, training time, and text-generation characteristics such as lexical diversity, repetition, grammatical coherence, and contextual consistency.

The objective is to evaluate the strengths and limitations of these architectures under comparable experimental conditions and investigate how sampling temperature influences generated text.

## Objectives

- Prepare a literary text corpus for word-level next-word prediction.
- Implement and train SimpleRNN, BiRNN, LSTM, and GRU architectures.
- Compare predictive performance using test loss, perplexity, accuracy, and training time.
- Generate text from the same seed prompts using multiple sampling temperatures.
- Analyse generated text in terms of diversity, repetition, coherence, and contextual consistency.
- Identify the strongest model based on the measured predictive results and qualitative generation analysis.

## Dataset

**Dataset:** *The Adventures of Sherlock Holmes* by Arthur Conan Doyle  
**Source:** Project Gutenberg  
**Source URL:** https://www.gutenberg.org/cache/epub/48320/pg48320.txt

The dataset consists of 12 Sherlock Holmes stories. The source text is downloaded and processed by the notebook before being used for model training.

The cleaned corpus contains approximately 104,549 whitespace-separated words before tokenization. This count refers to the cleaned text and is distinct from the number of tokens and input-target sequences produced during preprocessing.

### Data Preprocessing

The following preprocessing steps are performed:

1. Download the source text from Project Gutenberg.
2. Extract the 12 stories and remove irrelevant ebook metadata and supplementary text.
3. Split the text chronologically into training, validation, and test sets using an 80/10/10 ratio.
4. Fit the tokenizer using only the training text to avoid fitting the vocabulary on validation or test data.
5. Configure a vocabulary limit of 10,000 words, including the out-of-vocabulary token.
6. Convert the text into numerical token sequences.
7. Construct input-target pairs using a context window of 20 tokens, where each input sequence is used to predict the next word.
8. Prepare the sequences for model training using a batch size of 64.

The processed sequences and tokenizer are saved as supporting artifacts in the repository.

## Model Architectures

Four recurrent neural network architectures are implemented and evaluated.

| Model | Description |
|---|---|
| SimpleRNN | A standard recurrent neural network that processes the input sequence recurrently. |
| BiRNN | A bidirectional recurrent layer that processes the supplied context window in forward and backward directions. |
| LSTM | A recurrent architecture that uses input, forget, and output gates to regulate information flow and maintain longer-term dependencies. |
| GRU | A recurrent architecture that uses update and reset gates to control information retention and updating. |

All four models use a 128-dimensional embedding, 128 recurrent units, and a softmax output layer over the vocabulary for next-word prediction.

The BiRNN processes only the supplied input context window. Its backward direction does not receive future target words.

## Training Configuration

The models are trained under a common experimental configuration to support a fair comparison.

| Parameter | Configuration |
|---|---|
| Embedding dimension | 128 |
| Recurrent units | 128 |
| Context window | 20 tokens |
| Batch size | 64 |
| Optimizer | Adam |
| Initial learning rate | 0.001 |
| Loss function | Sparse categorical cross-entropy |
| Maximum epochs | 10 |
| Early stopping | Based on validation loss, with patience of 2 epochs |
| Learning-rate scheduling | Reduce learning rate when validation loss plateaus |

Early stopping restores the best model weights according to validation loss. The learning rate is reduced when validation loss stops improving, subject to the configured minimum learning rate.

## Evaluation

The models are evaluated on the held-out test set. The reported metrics include test loss, perplexity, next-word prediction accuracy, and measured training time.

### Test-Set Results

| Model | Test Loss ↓ | Perplexity ↓ | Accuracy ↑ | Training Time (min) |
|---|---:|---:|---:|---:|
| GRU | **5.7839** | **325.03** | **13.88%** | 0.83 |
| SimpleRNN | 5.8334 | 341.52 | 13.36% | **0.63** |
| BiRNN | 5.8521 | 347.96 | 12.05% | 0.68 |
| LSTM | 5.9055 | 367.04 | 13.45% | 1.18 |

Lower test loss and perplexity indicate better predictive performance, while higher accuracy indicates a greater proportion of correct next-word predictions.

GRU achieved the lowest test loss and perplexity and the highest next-word prediction accuracy among the four models in this experiment. SimpleRNN had the shortest measured training time.

These results identify GRU as the strongest model according to the reported predictive metrics under the selected experimental configuration. Training time measurements may vary with the computational environment and should not be interpreted as universal speed benchmarks.

### Temperature-Controlled Text Generation

Autoregressive text generation is performed using the same three seed prompts for each model:

- `Sherlock Holmes looked at the stranger`
- `I had been watching Holmes`
- `The room was silent and`

Each model generates continuations at three sampling temperatures: **0.5, 1.0, and 1.3**. The generated continuations are evaluated using lexical diversity and repetition metrics, alongside qualitative inspection.

#### Effect of Sampling Temperature

- **Temperature 0.5:** Produces more concentrated sampling from the predicted distribution. This can result in more predictable continuations, but may also encourage repetition.
- **Temperature 1.0:** Uses the model's predicted distribution without additional temperature scaling, providing a reference point for comparison.
- **Temperature 1.3:** Produces a flatter sampling distribution, allowing less probable words to be selected more often. This can increase variety but may also reduce coherence.

The effect of temperature varies across architectures, so no single temperature should be assumed to be optimal for every model.

#### Diversity Analysis

Distinct-1 measures the proportion of unique unigrams (individual words), while Distinct-2 measures the proportion of unique bigrams (consecutive two-word sequences) in the generated continuations. Higher scores indicate greater lexical diversity, but do not directly measure grammatical correctness or semantic coherence.

The average Distinct-1 results for the evaluated generated samples were:

| Temperature | SimpleRNN | BiRNN | LSTM | GRU | Highest Distinct-1 |
|---|---:|---:|---:|---:|---|
| 0.5 | 0.556 | 0.522 | 0.506 | **0.622** | GRU |
| 1.0 | **0.839** | 0.789 | 0.806 | 0.783 | SimpleRNN |
| 1.3 | 0.900 | **0.928** | 0.878 | 0.878 | BiRNN |

At temperature 1.3, all four models achieved an average Distinct-2 score of 1.000 for the evaluated continuations. This indicates that no bigrams were repeated within the measured continuations in these averaged results; it does not establish that the generated text was coherent or meaningful.

#### Qualitative Analysis

Manual inspection of generated samples showed that the models could produce text with limited grammatical coherence and contextual consistency. These limitations remained evident even when higher temperatures increased lexical diversity.

Temperature therefore presents a trade-off between predictable sampling and lexical variety. A more diverse continuation is not necessarily a better continuation, and diversity metrics should be interpreted alongside the generated samples themselves.

## Main Findings

- **Best predictive performance:** GRU achieved the lowest test loss (5.7839), lowest perplexity (325.03), and highest test accuracy (13.88%).
- **Shortest measured training time:** SimpleRNN completed training in approximately 0.63 minutes in the reported run.
- **Temperature affects diversity:** The architecture with the highest Distinct-1 score varied across temperatures. GRU ranked highest at 0.5, SimpleRNN at 1.0, and BiRNN at 1.3.
- **Diversity is not equivalent to quality:** Higher Distinct-1 and Distinct-2 scores do not guarantee grammatical or contextually consistent text.
- **Generation limitations:** Qualitative inspection revealed weaknesses in grammatical coherence and contextual consistency across the generated samples.

Overall, GRU was the strongest model according to the measured predictive metrics. However, the available results do not establish that it produced the most coherent text in every generation setting. Determining the best generation model requires considering both quantitative performance and qualitative output quality.

## Repository Contents

- `sherlock_holmes_next_word_prediction.ipynb` — complete preprocessing, model training, evaluation, text generation, and analysis workflow.
- `data/` — cleaned corpus, extracted stories, and processed sequence and tokenizer files.
- `data/processed/` — prepared next-word sequences and tokenizer artifacts.
- `results/` — model comparison tables, test and training results, generated samples, generation metrics, plots, and experiment summaries.
- `README.md` — project overview, execution instructions, evaluation results, and conclusions.

## How to Run

1. Open `sherlock_holmes_next_word_prediction.ipynb` in Google Colab.
2. Select a suitable runtime and execute the notebook cells in order.
3. Allow the notebook to download the source text from Project Gutenberg and extract the 12 stories.
4. Run the preprocessing cells to create the cleaned corpus, tokenizer, and input-target sequences.
5. Train and evaluate the four model architectures.
6. Run the text-generation and analysis cells to generate continuations at temperatures 0.5, 1.0, and 1.3.
7. Review the resulting evaluation tables, generated samples, diversity metrics, and plots.

The notebook downloads the source corpus during execution, so a separate dataset upload is not required for the standard workflow.

### Requirements

The notebook uses:

- Python
- TensorFlow/Keras
- NumPy
- pandas
- Matplotlib

The notebook contains the required imports and configuration. Running it in Google Colab or another compatible Python environment with the required dependencies installed is recommended.

## Limitations

- **Single dataset:** The experiment uses one literary corpus, so the results may not generalize to other genres or datasets.
- **Word-level tokenization:** Rare words and vocabulary limits can affect the model's ability to represent unfamiliar text.
- **Predictive performance:** Next-word prediction accuracy remains relatively low, reflecting the difficulty of predicting words in a literary text using the selected configurations.
- **Generation quality:** Generated text can contain grammatical errors, weak contextual consistency, and repetitive patterns.
- **Potential overfitting:** Training and validation loss curves should be considered when interpreting generalization performance.
- **Computational variability:** Training times depend on the runtime, hardware, and software environment.
- **Limited generation sample size:** Diversity results are calculated from the evaluated prompts and generated continuations; they should not be treated as universal measures of each architecture's generation quality.
- **Metric limitations:** Loss, perplexity, accuracy, and lexical diversity do not fully capture coherence, meaning, or narrative quality.

## Conclusion

This project compares SimpleRNN, BiRNN, LSTM, and GRU for word-level autoregressive text generation using *The Adventures of Sherlock Holmes*.

Under the selected experimental configuration, GRU achieved the strongest predictive results, while SimpleRNN had the shortest measured training time. Temperature-controlled generation demonstrated that sampling temperature influences lexical diversity and repetition, with higher temperatures generally permitting more varied word choices.

However, the generated samples also exhibited limitations in grammatical coherence and contextual consistency. The findings show that predictive performance and generation quality are related but distinct aspects of autoregressive text generation.

Future work could explore larger and more diverse training corpora, alternative tokenization strategies, systematic hyperparameter tuning, and transformer-based language models.

## Dataset Attribution

Arthur Conan Doyle, *The Adventures of Sherlock Holmes*. Project Gutenberg.

Source: https://www.gutenberg.org/cache/epub/48320/pg48320.txt

Consult the source ebook for its applicable terms and redistribution information.
