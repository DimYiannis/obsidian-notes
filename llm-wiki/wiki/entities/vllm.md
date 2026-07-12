---
title: "vLLM"
type: entity
entity_type: tool
tags: [llm, inference, serving]
source_count: 2
---

## Overview

Open-source LLM inference and serving framework. High-throughput, memory-efficient. Uses [[xgrammar]] as its default [[constrained-decoding]] backend (as of 2026).

## Key Facts

- High-throughput LLM serving via PagedAttention
- Supports continuous batching
- Default constrained decoding backend: [[xgrammar]] (replaced [[outlines-library]])
- Its 0.10.1 source tree is the retrieval corpus of [[rag-against-the-machine]]

## Appearances

- Function Calling for LLMs Using Constrained Decoding (2026-06-01) — cited as primary consumer of XGrammar
- RAG Theory — Study Context (2026-07-12) — its codebase is the corpus being indexed and searched

## Connections

- [[xgrammar]] — default constrained decoding backend
- [[constrained-decoding]] — capability vLLM exposes via XGrammar
- [[outlines-library]] — earlier default, now replaced
- [[rag-against-the-machine]] — project that indexes vLLM's source as a RAG corpus
