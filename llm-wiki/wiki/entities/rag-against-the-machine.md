---
title: "RAG Against the Machine"
type: entity
entity_type: project
tags: [rag, codam, information-retrieval, project]
source_count: 1
---

## Overview

Codam (42) curriculum project: build a [[retrieval-augmented-generation]] system over the [[vllm]] 0.10.1 codebase. [[bm25]] retrieval over a hand-rolled [[inverted-index]], Qwen3-0.6B generation, graded on recall@k by an external binary (the moulinette) and defended at a whiteboard in peer review.

## Key Facts

- Retrieval: hand-rolled [[bm25]] (no rank-bm25 dependency) over structure-aware chunks ([[chunking]]: Python AST + markdown headers, ≤ 2000 chars)
- Tokenizer: identifier-aware [[retrieval-tokenization]] — splits snake_case/CamelCase, keeps the original identifier
- Evaluation: recall@5 targets 80% (docs) / 50% (code), span relevance via IoU > 0.05 ([[retrieval-evaluation]])
- Generation: Qwen/Qwen3-0.6B via transformers; [[grounding]]-focused prompt
- Stack: Python 3.10+, uv, Python Fire CLI, pydantic models
- Bonus phases: caching → HTTP API → incremental indexing → [[dense-retrieval]] embeddings → RRF hybrid

## Appearances

- RAG Theory — Study Context (2026-07-12) — the theory study guide prepared for this project

## Connections

- [[vllm]] — the corpus being indexed
- [[bm25]], [[inverted-index]], [[chunking]], [[retrieval-tokenization]], [[retrieval-evaluation]] — the concepts the project implements
- [[retrieval-augmented-generation]] — the architecture

## Open Questions

- Final (k1, b) and chunk-size settings after tuning on the public datasets?
