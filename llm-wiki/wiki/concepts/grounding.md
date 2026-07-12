---
title: "Grounding / Faithfulness"
type: concept
tags: [rag, llm, factuality, generation]
source_count: 1
---

## Definition

The property that a generated answer is supported by the retrieved context rather than by the model's parametric memory. In a [[retrieval-augmented-generation]] system, the generation prompt places the question plus retrieved chunks in the [[context-window]] and instructs the model to answer **from that context** — an application of in-context learning. A grounded answer can be traced back to specific retrieved spans.

## Key Properties

- **Grounding is the point of RAG** — retrieval only helps if the generator actually uses the retrieved text; a model that answers from weights anyway re-inherits [[hallucination]] and staleness.
- **Prompt construction matters** — explicit instructions ("answer only from the context below", "say you don't know if the context doesn't contain the answer") plus clear chunk delimitation.
- **Faithfulness ≠ correctness** — an answer can be faithful to retrieved-but-wrong context, or correct-but-unfaithful (right answer from parametric memory). Evaluation should distinguish them.
- **Failure modes** — ignoring context (parametric override), partial grounding (mixing context with memory), and position effects ([[lost-in-the-middle]]).

## Examples

- Qwen3-0.6B in *RAG Against the Machine*: a small model with weak parametric knowledge of vLLM internals — grounding is doing essentially all the factual work, which makes retrieval recall the binding constraint.

## Connections

- [[hallucination]] — grounding is the RAG-side mitigation
- [[retrieval-augmented-generation]] — the architecture whose generation stage this governs
- [[context-window]] — where the grounding evidence lives
- [[lost-in-the-middle]] — positional failure mode of context use
- [[retrieval-evaluation]] — retrieval recall bounds how grounded an answer can be

## Open Questions

- How to measure faithfulness cheaply without human judges — is LLM-as-judge reliable for small-model outputs?
