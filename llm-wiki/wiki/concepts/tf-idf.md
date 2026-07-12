---
title: "TF-IDF"
type: concept
tags: [information-retrieval, scoring, bag-of-words]
source_count: 1
---

## Definition

Term Frequency × Inverse Document Frequency — the classic term-weighting scheme for [[lexical-retrieval]]. Intuition: a term is a strong signal for a document if it appears **often in that document** (TF) but is **rare across the corpus** (IDF). A document's score for a query is the sum of TF-IDF weights of the shared terms.

## Key Properties

- **TF (term frequency)** — count of the term in the document; more occurrences → more evidence the document is "about" that term.
- **IDF (inverse document frequency)** — `log(N / df)`: terms that appear in few documents (small `df`) discriminate well; terms in every document (e.g. `self` in Python code) carry almost no information.
- **Known weaknesses** — raw TF grows without bound (a term appearing 50× isn't 50× more relevant than once), and long documents accumulate higher scores just by containing more terms. [[bm25]] fixes both with TF saturation and length normalization — learn TF-IDF first, BM25 is TF-IDF evolved.
- **Bag-of-words assumption** — word order and proximity ignored entirely.

## Examples

- In a codebase corpus, `lora` has high IDF (few files mention it) while `def` has near-zero IDF (every Python file has it) — a query "enable lora" is effectively decided by the `lora` postings.

## Connections

- [[bm25]] — its direct successor; adds k1 saturation and b length normalization
- [[lexical-retrieval]] — the paradigm TF-IDF scores
- [[inverted-index]] — stores exactly the (term, document, tf) data TF-IDF needs
- [[retrieval-tokenization]] — determines what a "term" is before any weighting

## Open Questions

- None open — superseded in practice by [[bm25]]; kept as the pedagogical stepping stone.
