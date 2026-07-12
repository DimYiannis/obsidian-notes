---
title: "Retrieval Evaluation (recall@k, precision@k, IoU)"
type: concept
tags: [information-retrieval, evaluation, metrics]
source_count: 1
---

## Definition

How retrieval quality is measured. **Recall@k**: for each query, is at least one relevant item in the top k results — a yes/no per query, averaged over the dataset. **Precision@k**: what fraction of the top k is relevant. When "documents" are text spans, relevance itself is decided by span overlap — **IoU** (intersection over union, Jaccard on character ranges): `overlap_len / union_len` between the retrieved span and the gold span.

## Key Properties

- **Recall@k is binary per query** — one hit in the top k scores the query; nine junk results alongside it cost nothing.
- **IoU threshold sets span strictness** — a low bar (e.g. > 0.05) means "right file, roughly right region" suffices; span precision barely matters. This directly drives [[chunking]] strategy (bias large).
- **Why RAG optimizes recall at retrieval, precision at generation** — the generator can ignore junk chunks in its prompt, but a missing relevant chunk is unrecoverable. Retrieval's job is to get the answer into the [[context-window]] at all.
- **Compute IoU by hand** — spans (100, 300) and (250, 400): intersection 50, union 300, IoU ≈ 0.167.

## Examples

- *RAG Against the Machine* targets: recall@5 ≥ 80% on docs questions, ≥ 50% on code questions, IoU bar 0.05, measured by an external grader reimplemented independently in the project's own `evaluate` command.

## Connections

- [[information-retrieval]] — standard IR evaluation methodology
- [[chunking]] — IoU threshold makes chunk size an evaluation lever
- [[bm25]] — the scoring function being evaluated
- [[retrieval-augmented-generation]] — explains the recall-first asymmetry
- [[grounding]] — the generation-side quality axis, complementary to retrieval metrics

## Open Questions

- What does "relevant" mean for multi-hop questions whose answer needs two chunks — does recall@k generalize?
