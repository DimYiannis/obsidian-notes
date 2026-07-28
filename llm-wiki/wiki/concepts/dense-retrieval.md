---
title: "Dense Retrieval"
type: concept
tags: [information-retrieval, embeddings, semantic-search]
source_count: 2
---

## Definition

Retrieval by semantic similarity in vector space: an embedding model maps queries and documents to dense vectors; relevance is cosine similarity (or dot product); retrieval is (approximate) nearest-neighbor search. The counterpart to [[lexical-retrieval]] — matching **meaning** instead of exact terms.

## Key Properties

- **Solves vocabulary mismatch** — "GPU memory" retrieves a chunk about "VRAM" because the vectors are close; this is exactly where lexical matching fails.
- **Blurs exact identifiers** — embeddings smear `enable_lora` toward everything LoRA-related; the exact-match precision that makes [[bm25]] strong on code is lost. The sparse-vs-dense tradeoff in one line: **synonyms kill lexical, identifier blur kills dense.**
- **Operational cost** — needs an embedding model at index and query time, vector storage, and ANN infrastructure; lexical needs a dict. Also less explainable: a similarity score doesn't decompose per term.
- **Hybrid fusion** — running both and merging rankings (e.g. Reciprocal Rank Fusion, RRF) captures both strengths; the bonus phase of *RAG Against the Machine*.

## Examples

- **DPR** (Dense Passage Retriever, Karpukhin et al., used in Lewis et al. 2020's original RAG paper): BERT bi-encoder producing query/document vectors, retrieval via Maximum Inner Product Search (MIPS) over a 21M-passage Wikipedia index. The retriever component paired with BART to make the original RAG model.
- Sentence-transformer embeddings + FAISS/HNSW nearest-neighbor search — the standard RAG-tutorial stack.
- *RAG Against the Machine* deliberately does **not** use dense retrieval for the mandatory part: lexical wins on exact identifiers, needs no model, is fast and explainable.

## Connections

- [[embeddings]] — the representation dense retrieval is built on (document/query-level rather than token-level)
- [[lexical-retrieval]] — the complementary paradigm; hybrid RRF combines both
- [[bm25]] — the lexical baseline dense methods are measured against
- [[retrieval-augmented-generation]] — DPR is the retriever the original RAG paper paired with a generator; the most common RAG retrieval choice in industry tutorials, not always the right one
- [[facebook-ai-research]] — origin of DPR

## Open Questions

- For code retrieval specifically, do code-tuned embedding models close the exact-identifier gap, or does hybrid remain necessary?
