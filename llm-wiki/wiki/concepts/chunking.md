---
title: "Chunking"
type: concept
tags: [rag, information-retrieval, preprocessing]
source_count: 1
---

## Definition

Splitting corpus documents into the retrievable units ("chunks") a RAG system indexes and returns. Chunking sets **retrieval granularity**: a whole file is too big to place in a prompt and scores poorly on span overlap; a single sentence is too small to carry context. The chunk — not the file — is the "document" of the [[inverted-index]].

**The core problem chunking solves:** you can't feed the whole corpus to the LLM. A model's [[context-window]] is finite (thousands to low-millions of tokens); a real corpus (docs, codebase, wiki) is far bigger. [[retrieval-augmented-generation]]'s whole premise is: don't put everything in context, retrieve the small relevant slice first, then generate from just that. Chunks are the unit that gets retrieved.

## Key Properties

- **Why not whole documents as the retrieval unit:**
  - *Precision* — a document can be long and cover many topics; only one paragraph might answer the question. Retrieving the whole document wastes context budget on irrelevant text, and dilutes the signal the LLM has to work with.
  - *Ranking granularity* — retrieval scoring ([[bm25]], cosine similarity, whatever) works by comparing a query against a unit of text. A huge, topically-mixed document produces a blurry average signal; a small, coherent chunk (one section, one function) gives a sharp, specific match.
  - *[[lost-in-the-middle]]* — a real, named LLM phenomenon. LLMs attend better to the start/end of their context than the middle. Stuffing a giant retrieved document in means the actually-relevant sentence might be buried mid-context and get under-weighted by the model regardless of retrieval being correct.
- **Why not something even smaller than a chunk (single sentences):** loses surrounding context needed to make sense of the sentence — a sentence like "this defaults to 2000" means nothing without the paragraph naming what "this" is. Chunking tries to hit a middle ground: big enough to be self-contained and meaningful, small enough to be topically coherent and stay within a token budget.
- **Strategies** — fixed-size windows, sliding (overlapping) windows, and **structure-aware**: split code at AST boundaries (functions, classes), markdown at headers. Structure-aware chunks align with the units questions actually target.
- **Size tradeoff (general)** — bigger chunks = more context per retrieved unit, but blurrier/noisier ranking and worse lost-in-the-middle risk. Smaller chunks = sharper ranking, but risk splitting a coherent idea (a function, an explanation) across a chunk boundary, losing coherence. There's no universally correct chunk size — it's tuned per corpus/task.
- **Size tradeoff (this project's metric)** — the general tradeoff above, restated in terms of [[bm25]]/IoU: big chunks get better span-overlap (IoU) with gold answers, but diluted term statistics (length normalization pushes back). Small chunks: precise but fragmenting — the answer straddles a boundary.
- **Evaluation-driven bias (project-specific)** — with a low IoU bar (0.05 in *RAG Against the Machine*), being in the right file and rough region is what counts → bias toward large chunks (near the 2000-char cap). This is an artifact of that project's grading threshold, not a general chunking principle.
- **Offsets are ground truth** — each chunk must carry exact character offsets into its source file (`file[first:last] == chunk.text`); evaluation compares spans, so offset bugs silently destroy recall.

## Examples

- Python file → one chunk per top-level function/class via `ast` (fall back to line windows on `SyntaxError`); markdown → one chunk per header section, merged/split to the size cap.

## Connections

- [[retrieval-augmented-generation]] — chunking is its preprocessing stage
- [[inverted-index]] — chunks are its posting targets
- [[bm25]] — chunk length distribution interacts with the b parameter
- [[retrieval-evaluation]] — IoU-based relevance makes chunk size an evaluation lever
- [[context-window]] — chunk size × k must fit the generation prompt; the finite-window/large-corpus gap is why chunking exists at all
- [[lost-in-the-middle]] — whole-document retrieval reintroduces this failure mode inside a single retrieved item

## Open Questions

- Do overlapping windows measurably beat disjoint structure-aware chunks once IoU bars are low?
