---
title: "Lexical (Sparse) Retrieval"
type: concept
tags: [information-retrieval, search, bag-of-words]
source_count: 1
---

## Definition

Retrieval by matching the exact terms of the query against the exact terms of documents. Built on the **bag-of-words** model: a document is treated as a multiset of terms, word order ignored. Called "sparse" because a document's vector representation has one dimension per vocabulary term, almost all zero.

## Key Properties

- **Exact matching** — a document scores only if it shares literal terms with the query; scoring functions ([[tf-idf]], [[bm25]]) weight those shared terms.
- **Vocabulary mismatch problem** — synonyms and paraphrase kill lexical retrieval: "GPU memory" won't match a chunk that only says "VRAM". This is the gap [[dense-retrieval]] fills.
- **Exact-identifier strength** — for code corpora, exact matching is a feature: a query containing `enable_lora` matches precisely the chunks containing that identifier, with full IDF weight. This is why *RAG Against the Machine* uses lexical retrieval as its core.
- **Term definition is a design choice** — what counts as a "term" is decided by [[retrieval-tokenization]]; the same analyzer must run at index and query time.
- **Fast and explainable** — served by an [[inverted-index]]; a score can be decomposed term by term, unlike a dense similarity.

## Examples

- BM25 over code chunks: query "How to configure the OpenAI server?" matches chunks containing `openai`, `server`, `configure` subtokens.
- Elasticsearch / Lucene default scoring (BM25 since Lucene 6).

## Connections

- [[tf-idf]] → [[bm25]] — its canonical scoring functions
- [[inverted-index]] — the data structure that serves it
- [[retrieval-tokenization]] — defines the terms being matched
- [[dense-retrieval]] — the complementary paradigm; hybrid fusion (RRF) combines both
- [[information-retrieval]] — parent field

## Open Questions

- How far can query-side expansion (adding synonyms/subtokens) push lexical retrieval before dense retrieval is actually needed?
