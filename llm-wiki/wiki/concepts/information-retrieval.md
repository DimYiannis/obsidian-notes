---
title: "Information Retrieval"
type: concept
tags: [information-retrieval, search, foundations]
source_count: 1
---

## Definition

The field concerned with finding relevant documents in a large collection in response to a query. Core vocabulary: **query** (the information need), **document** (the retrievable unit), **corpus** (the collection), **relevance** (does this document satisfy the need). IR is a 50-year-old discipline — the retrieval half of [[retrieval-augmented-generation]] is classic IR, not something new.

## Key Properties

- **Ranked retrieval** — modern IR returns a scored ranking, not a boolean set; the scoring function (e.g. [[tf-idf]], [[bm25]]) defines the system.
- **Two families** — [[lexical-retrieval]] (exact term matching over a bag of words) and [[dense-retrieval]] (semantic vectors); each fails where the other succeeds.
- **Efficiency via index structure** — the [[inverted-index]] makes scoring large corpora tractable by only visiting documents that share terms with the query.
- **Evaluation is empirical** — precision/recall at k over labeled query–document pairs; see [[retrieval-evaluation]].

## Examples

- Web search engines (the founding application).
- Code search over a repository — the *RAG Against the Machine* setting, where documents are code/doc chunks from the [[vllm]] source tree.

## Connections

- [[retrieval-augmented-generation]] — RAG's retrieval stage is applied IR
- [[lexical-retrieval]], [[dense-retrieval]] — the two retrieval paradigms
- [[tf-idf]], [[bm25]] — canonical scoring functions
- [[inverted-index]] — canonical data structure
- [[retrieval-evaluation]] — how IR systems are measured

## Open Questions

- How much of classic IR evaluation methodology transfers cleanly to RAG, where the consumer is a model rather than a human reading a results page?
