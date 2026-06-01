# LLM Tokenization, Embeddings, and Context Windows

*Source: personal notes by user 2026-06-01. Immutable.*

## 1. What is a Token?

Large Language Models (LLMs) do not process text character by character. Instead, they process **tokens**.

A token can be:
- A whole word
- Part of a word
- Punctuation
- A common sequence of characters

The tokenizer converts text into tokens before the model sees it.

## 2. Why Use Multi-Character Tokens?

Benefits of multi-character tokens:
- Faster computation
- Lower memory usage
- More text fits into the context window
- More efficient training and inference

## 3. Vocabulary

The tokenizer has a predefined vocabulary. A larger vocabulary allows more text chunks to be represented as single tokens.

Tradeoff:
- Benefits: fewer tokens, faster inference, longer effective context
- Costs: larger model size, more memory usage

## 4. What is an Embedding?

Each token is converted into a vector of numbers. Words with similar meanings often have similar vectors (e.g. cat ≈ dog semantically → similar vectors).

## 5. What is the Embedding Matrix?

The embedding matrix stores the vector for every token in the vocabulary.

Mathematically: Vocabulary Size × Embedding Dimension
Example: 100,000 × 4,096

A larger vocabulary means a larger embedding matrix.

## 6. What is a Context Window?

A context window is the amount of text an LLM can consider at one time. The model only sees the tokens currently inside its context window.

Context windows are measured in tokens:
- 8k tokens ≈ 5–10 pages
- 32k tokens ≈ 25–50 pages
- 128k tokens ≈ 100–300 pages
- 1M tokens ≈ hundreds to thousands of pages

## 7. Longer Effective Context

Better tokenization (fewer tokens per word) means more actual text fits in the same context window. The context size stays the same in tokens, but readable text increases.

## 8. Full LLM Pipeline

```
Text → Tokenizer → Token IDs → Embedding Matrix Lookup → Embedding Vectors → Transformer Layers → Next Token Prediction
```
