---
title: "LLM Tokenization, Embeddings, and Context Windows"
type: source
date_ingested: 2026-06-01
source_url: local
source_type: note
tags: [tokenization, embeddings, context-window, llm, fundamentals]
---

## Summary

Personal notes covering three foundational concepts bridging raw text and LLM computation: [[tokenization]] (text → token IDs), [[embeddings]] (token IDs → dense vectors via embedding matrix), and [[context-window]] (how many tokens the model can see at once). Introduces the vocabulary size tradeoff and the concept of "effective context."

## Key Points

- Tokens are the atomic unit LLMs process — words, sub-words, or punctuation. Never raw characters.
- Multi-character tokens reduce sequence length → faster computation, lower memory, more text per [[context-window]].
- Vocabulary size tradeoff: larger vocab → fewer tokens, longer effective context, but larger [[embeddings]] matrix and more memory.
- Each token ID is looked up in an embedding matrix (vocab_size × embedding_dim, e.g. 100,000 × 4,096) to produce a dense vector. Semantically similar tokens produce geometrically similar vectors.
- [[context-window]] is fixed in tokens, not characters. Better [[tokenization]] = more readable text per token budget = "longer effective context."
- Full pipeline: Text → [[tokenization]] → Token IDs → Embedding Matrix → Vectors → Transformer Layers → Next Token Prediction.

## Concepts

- [[tokenization]] — core concept; vocabulary tradeoff detailed here
- [[embeddings]] — introduced in full here (token → vector, embedding matrix)
- [[context-window]] — measurement in tokens, effective context concept
