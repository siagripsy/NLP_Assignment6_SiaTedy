# Assignment 06 – Language Modeling Report  
AIG 230 – Natural Language Processing  

## Overview

In this assignment, we implemented and compared two types of language models using the Brown corpus (news category):

1. A statistical Trigram Language Model (Section A)
2. A Neural Language Model using RNN (Section B)

The objective was to analyze model performance using perplexity and evaluate the quality of generated text.

---

# Section A – Statistical Trigram Model

## Data Preparation

- Dataset: Brown Corpus (news category)
- Preprocessing:
  - Lowercasing
  - Removed punctuation-only tokens
  - Added `<bos>` and `<eos>`
- Sentence-level 80/10/10 split:
  - Train
  - Validation
  - Test

Vocabulary:
- Built from training set only
- `min_freq = 2`
- `<unk>` token included
- Vocabulary size ≈ 5435

All OOV words in validation and test were mapped to `<unk>`.

---

## Model Details

- Trigram counts built from training data
- Lidstone smoothing with α = 0.1
- Validation perplexity ≈ 2535.6302358362896

---

## Text Generation Analysis (Trigram)

**Grammaticality:**  
Mostly fragmented and ungrammatical.

**Coherence:**  
Very weak. Sentences often consisted of disconnected phrases.

**Limitations Observed:**  
- Very local context (only 2-word history)
- No long-range dependency modeling
- Frequent unnatural transitions

---

# Section B – Neural Language Model (RNN)

## Model Architecture

Embedding → RNN → Linear

Hyperparameters:


Embedding dimension: 128
Hidden dimension: 256
Number of RNN layers: 1
Learning rate: 0.001
sequence length: 30

Total parameters ≈ 2,191,291.

---

## Training

- Implemented custom Dataset and DataLoader
- Trained for multiple epochs
- Monitored:
  - Training loss
  - Validation perplexity
- Plotted training loss per epoch

Validation Perplexity & Train Loss:
Epoch 01 | train loss: 1.9020 | val ppl: 885.51
Epoch 02 | train loss: 0.8734 | val ppl: 2604.91
Epoch 03 | train loss: 0.5756 | val ppl: 5392.54
Epoch 04 | train loss: 0.4638 | val ppl: 8965.05
Epoch 05 | train loss: 0.4089 | val ppl: 13116.96


Final Test Perplexity:  
Test perplexity: 13527.504986756088

---

## Text Generation Analysis (RNN)

Three samples were generated with minimum 30 tokens.

### Grammaticality
The RNN model produced longer and more structured sentences compared to the trigram model.  
Some grammatical constructions such as passive voice and relative clauses were correctly formed.  
However, errors and awkward phrases still appeared.

### Coherence
Coherence was moderate.  
Although local sentence structure improved, the model struggled to maintain a consistent topic throughout longer sentences.

### Repetition
Repetition was less severe than in the trigram model, but structural patterns such as repeated connectors appeared.

### Long-Range Dependency Behavior
The RNN demonstrated the ability to model short- to medium-range dependencies.  
However, it still had difficulty maintaining long-range semantic consistency.

---

# Comparison: Trigram vs RNN

| Aspect | Trigram | RNN |
|--------|---------|------|
| Context window | 2 words | Variable-length sequence |
| Grammaticality | Poor | Moderate |
| Coherence | Very low | Improved but limited |
| Sentence length | Short, fragmented | Longer and structured |
| Long-range dependency | None | Partial |

The RNN clearly outperformed the trigram model in structural fluency and sentence length, but both models struggled with global semantic coherence.

---

# Conclusion

This assignment demonstrated the difference between statistical and neural language models.

The trigram model relies strictly on local context and produces fragmented output.  
The RNN model captures sequential patterns better and generates more natural sentence structures.  
However, simple RNN architectures still have limitations in modeling long-range dependencies and maintaining global coherence.

Future improvements could include:
- Using LSTM or GRU
- Increasing model capacity
- Reducing `<unk>` tokens
- Training for more epochs