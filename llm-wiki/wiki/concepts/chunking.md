---
title: "Chunking"
type: concept
tags: [rag, information-retrieval, preprocessing]
source_count: 1
---

## Definition

Splitting corpus documents into the retrievable units ("chunks") a RAG system indexes and returns. Chunking sets **retrieval granularity**: a whole file is too big to place in a prompt and scores poorly on span overlap; a single sentence is too small to carry context. The chunk — not the file — is the "document" of the [[inverted-index]].

## Key Properties

- **Strategies** — fixed-size windows, sliding (overlapping) windows, and **structure-aware**: split code at AST boundaries (functions, classes), markdown at headers. Structure-aware chunks align with the units questions actually target.
- **Size tradeoff** — big chunks: more context per hit, better span-overlap (IoU) with gold answers, but diluted term statistics in [[bm25]] (length normalization pushes back). Small chunks: precise but fragmenting — the answer straddles a boundary.
- **Evaluation-driven bias** — with a low IoU bar (0.05 in *RAG Against the Machine*), being in the right file and rough region is what counts → bias toward large chunks (near the 2000-char cap).
- **Offsets are ground truth** — each chunk must carry exact character offsets into its source file (`file[first:last] == chunk.text`); evaluation compares spans, so offset bugs silently destroy recall.

## Examples

- Python file → one chunk per top-level function/class via `ast` (fall back to line windows on `SyntaxError`); markdown → one chunk per header section, merged/split to the size cap.

## Connections

- [[retrieval-augmented-generation]] — chunking is its preprocessing stage
- [[inverted-index]] — chunks are its posting targets
- [[bm25]] — chunk length distribution interacts with the b parameter
- [[retrieval-evaluation]] — IoU-based relevance makes chunk size an evaluation lever
- [[context-window]] — chunk size × k must fit the generation prompt

## Open Questions

- Do overlapping windows measurably beat disjoint structure-aware chunks once IoU bars are low?
