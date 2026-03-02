# Assignment 06 – Neural Language Modeling (AIG 230)

## Overview

This assignment explores both statistical and neural language models using the Brown corpus (news category).  
We implemented:

- A statistical **Trigram Language Model** with Lidstone smoothing (Section A)
- A **Neural Language Model using RNN** (Section B)

The goal was to compare traditional n-gram modeling with neural sequence modeling in terms of perplexity and text generation quality.

---

# Section A – Statistical Trigram Model

## 1. Dataset Preparation

- Dataset: Brown Corpus (news category)
- Preprocessing steps:
  - Lowercasing all tokens
  - Removing punctuation-only tokens
  - Adding `<bos>` and `<eos>` tokens
- Result stored in:
  - `proc_sents`

## 2. Train / Validation / Test Split

- Sentence-level split (80/10/10)
- Variables:
  - `train_sents`
  - `val_sents`
  - `test_sents`

We reported:
- Number of sentences in each split
- Number of tokens in each split

## 3. Vocabulary

- Built only from `train_sents`
- `min_freq = 2`
- `<unk>` token included
- Final vocabulary size ≈ 5393

All validation and test OOV tokens were mapped to `<unk>`.

## 4. Trigram Model

- Built trigram counts from training data
- Used Lidstone smoothing (alpha = 0.1)
- Computed validation perplexity
- Generated sample text and analyzed:
  - Grammaticality
  - Coherence
  - Limitations

---

# Section B – Neural Language Model (RNN)

## 1. Numericalization

- Created:
  - `stoi` (string → index)
  - `itos` (index → string)
- Converted all tokenized sentences into integer IDs
- Built token streams:
  - `train_stream`
  - `val_stream`
  - `test_stream`

## 2. Dataset and DataLoader

- Implemented `LanguageModelDataset`
- Each sample:
  - Input: sequence of length `seq_len`
  - Target: next-token shifted sequence
- Shape confirmed:
  - `(batch_size, seq_len)`

## 3. Model Architecture

Implemented an RNN-based language model:

Embedding → RNN → Linear

Hyperparameters:
- Embedding dimension: 100
- Hidden dimension: 128
- 1 RNN layer
- Adam optimizer
- CrossEntropyLoss

Total model parameters ≈ 1.3–1.5 million.

## 4. Training

- Implemented:
  - `train_one_epoch`
  - `evaluate_perplexity`
- Used gradient clipping
- Trained for multiple epochs
- Tracked:
  - Training loss
  - Validation perplexity
- Plotted training loss curve

## 5. Test Perplexity

- Computed final test perplexity after training

## 6. Text Generation

Implemented autoregressive text generation:

- Start from `<bos>`
- Predict next-token distribution
- Sample using temperature
- Continue until `<eos>` or max length
- Enforced minimum 30-token output

Generated 3 samples and analyzed:

- Grammaticality
- Coherence
- Repetition
- Long-range dependency behavior

---

# Observations

- The RNN model generates longer and more structurally complex sentences compared to the trigram model.
- It captures some syntactic patterns (e.g., passive voice, relative clauses).
- However, it still struggles with:
  - Global coherence
  - Maintaining topic consistency
  - Handling rare words (`<unk>` tokens)

---

# Conclusion

The neural language model outperforms the trigram model in structural fluency and sentence length.  
However, simple RNN architectures still have limitations in modeling long-range dependencies and semantic coherence.

Future improvements could include:
- Using LSTM or GRU instead of vanilla RNN
- Increasing embedding and hidden dimensions
- Reducing `<unk>` tokens with larger vocabulary
- Training longer or with more regularization