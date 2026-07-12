---
title: "Inverted Index"
type: concept
tags: [information-retrieval, data-structures, search]
source_count: 1
---

## Definition

The core data structure of search: a mapping from each term to its **posting list** — the documents containing that term, typically with term frequencies:

```
term → [(doc_id, tf), (doc_id, tf), ...]
```

"Inverted" because it flips the natural document→terms direction into terms→documents. Alongside it, a scoring engine keeps document lengths and the corpus average length (for [[bm25]]'s length normalization).

## Key Properties

- **Why search is fast** — a query only touches the posting lists of its own terms; documents sharing no term with the query are never visited, let alone scored. Corpus size matters far less than posting-list length.
- **Built once, queried many times** — construction is a single pass over the corpus (tokenize each document, append to posting lists); queries are then near-instant.
- **Top-k selection** — after scoring candidates, `heapq.nlargest(k, …)` gives top-k in O(n log k) instead of a full O(n log n) sort.
- **The analyzer defines the index** — the same [[retrieval-tokenization]] must produce index terms and query terms, or lookups silently miss.

## Examples

- *RAG Against the Machine*: in-memory Python dict `term → [(chunk_id, tf)]` persisted to disk; meets a "200 queries in under 10 s" target because only chunks sharing a query term are BM25-scored.
- Lucene's segment files — the industrial-strength version of the same idea.

## Connections

- [[lexical-retrieval]] — the paradigm it serves
- [[bm25]], [[tf-idf]] — scoring functions computed over posting lists
- [[retrieval-tokenization]] — produces the terms that key the index
- [[chunking]] — decides what a "document" (posting target) is in a RAG system

## Open Questions

- At what corpus scale does the simple in-memory dict stop sufficing (posting-list compression, skip lists, tiered indexes)?
