# Detecting Negation and Uncertainty in Text  
## Evaluating Rule-Based, Traditional Machine Learning, and Deep Learning Approaches

---

## Project Overview

This project addresses the task of **negation and uncertainty detection in clinical text** using three complementary approaches: a rule-based system, traditional machine learning models, and a deep learning architecture. The task is formulated as a **sequence labeling problem**, where each token is classified as a negation cue, an uncertainty cue, or as part of their corresponding scopes.

All approaches are evaluated on **medical texts written in Spanish and Catalan**, highlighting the linguistic and structural challenges of multilingual clinical narratives.

This project was developed for the course **Fundamentals of Natural Language Processing** at the **Universitat Autònoma de Barcelona**.

---

## Objectives

- Detect negation and uncertainty cues in clinical text.
- Identify the scope affected by each cue.
- Compare rule-based, traditional machine learning, and deep learning approaches.
- Analyze strengths and limitations of each method, particularly for uncertainty detection.

---

## Labeling Scheme

Each token in the text is assigned one of the following labels:

- `NEG`   – Negation cue  
- `UNC`   – Uncertainty cue  
- `NSCO`  – Scope of negation  
- `USCO`  – Scope of uncertainty  
- `other` – Token not involved in negation or uncertainty  

For machine learning scope detection, **BIO tagging** is applied:

- `B-NSCO`, `I-NSCO`
- `B-USCO`, `I-USCO`
- `O`

---

## Data Preprocessing

### Rule-Based Approach
- Conversion of JSON annotations to tabular format
- Minimal normalization:
  - Lowercasing
  - Whitespace normalization
- Preservation of character offsets to maintain alignment with annotated spans

### Machine Learning Approach
- Sentence segmentation using a Spanish syntactic parser
- Token-level feature extraction:
  - Lemmas
  - Part-of-speech tags
  - Dependency relations
  - Prefixes and suffixes
  - Lexical and punctuation features
- Separate representations for:
  - Cue detection (SVM)
  - Scope detection (CRF)

### Deep Learning Approach
- Construction of word-to-index and label-to-index mappings
- Sentence padding to uniform length
- Use of pretrained **FastText embeddings (300 dimensions)**:
  - Embeddings kept frozen during training
  - Random initialization for out-of-vocabulary tokens

---

## System Architectures

### 1. Rule-Based System
- Custom NER-style sequence labeling pipeline
- Handcrafted lexicons for negation and uncertainty cues
- Custom tokenizer with character-level alignment
- Scope detection based on:
  - Sentence boundaries
  - Termination terms
  - Tuned scope windows
- Designed to support both Spanish and Catalan without relying on standard NLP toolkits

### 2. Machine Learning System

#### Cue Detection (SVM)
- Linear Support Vector Machine (`LinearSVC`)
- Binary classification of negation and uncertainty cues
- Class balancing applied, especially for uncertainty detection

#### Scope Detection (CRF)
- Conditional Random Fields for sequence labeling
- BIO-tagged training data
- Feature-rich representation including:
  - Token context
  - Syntactic and morphological features
- Separate CRF models for negation and uncertainty scopes

### 3. Deep Learning System
- End-to-end **Bidirectional LSTM-based sequence labeling model**
- Architecture:
  - Embedding layer (FastText)
  - BiLSTM layer
  - BiGRU layer
  - Fully connected output layer with softmax activation
- Joint learning of cues and scopes
- **Weighted categorical cross-entropy loss** to address class imbalance

---

## References

1. Enger, M., Velldal, E., & Øvrelid, L. (2017). *An open-source tool for negation detection: A maximum-margin approach*.  
2. Solarte Pabón, O., et al. (2021). *Integrating speculation detection and deep learning to extract lung cancer diagnosis from clinical notes*. Applied Sciences.
