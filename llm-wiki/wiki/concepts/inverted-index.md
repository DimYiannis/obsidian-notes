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
- **Not the same structure as [[dense-retrieval]]'s index** — this is a hash map (`term → posting list`), exact discrete key lookup, O(1) per term. Dense retrieval needs an ANN index (HNSW graph, IVF clusters) because vectors are continuous floats with no exact-match concept — you can only ask "what's close," never "what's equal." IVF ("Inverted File Index") borrows this page's name and bucket→members idea, but its buckets are k-means centroids over vectors, not exact terms — same concept, different underlying data.

## Examples

Worked build — three documents:

```
doc1: "the cat sat on the mat"
doc2: "the dog sat on the log"
doc3: "cats and dogs are friends"
```

Forward index (doc → terms):

```
doc1 → [the, cat, sat, on, the, mat]
doc2 → [the, dog, sat, on, the, log]
doc3 → [cats, and, dogs, are, friends]
```

Inverted (term → docs):

```
cat  → [doc1]
sat  → [doc1, doc2]
the  → [doc1, doc2]
dogs → [doc3]
```

Query "sat" is a single dict lookup returning `[doc1, doc2]` — doc3 is never even visited, since it shares no term with the query.

Real postings also carry position (phrase queries) and term frequency (scoring):

```
sat → [(doc1, pos=2, tf=1), (doc2, pos=2, tf=1)]
```

- *RAG Against the Machine*: in-memory Python dict `term → [(chunk_id, tf)]` persisted to disk; meets a "200 queries in under 10 s" target because only chunks sharing a query term are BM25-scored.
- Lucene's segment files — the industrial-strength version of the same idea.
- **Contrast case**: `doc3: "cats and dogs are friends"` shares zero terms with a query like "feline companions," so it is never retrieved by this index at all — the exact gap [[dense-retrieval]] is built to close.

## Connections

- [[lexical-retrieval]] — the paradigm it serves
- [[bm25]], [[tf-idf]] — scoring functions computed over posting lists
- [[retrieval-tokenization]] — produces the terms that key the index
- [[chunking]] — decides what a "document" (posting target) is in a RAG system

## Open Questions

- At what corpus scale does the simple in-memory dict stop sufficing (posting-list compression, skip lists, tiered indexes)?
