---
title: "Lost in the Middle"
type: concept
tags: [llm, context, attention, rag]
source_count: 1
---

## Definition

The empirical finding (Liu et al., 2023) that LLMs use information at the **beginning and end** of a long context much better than information in the **middle** — a U-shaped performance curve over the position of the relevant passage.

## Key Properties

- **U-shaped position curve** — accuracy is highest when the answer-bearing passage is first or last in the prompt, and dips substantially when it sits in the middle.
- **Direct RAG consequence** — chunk **ordering** in the generation prompt matters, not just chunk selection: place the highest-scored chunks at the edges (top-ranked first, or split best chunks between start and end).
- **Worsens with context length** — the longer the stuffed context, the deeper the middle dip; another reason not to blindly maximize k in top-k retrieval.
- **Model-dependent severity** — small models (e.g. Qwen3-0.6B) tend to suffer more; worth testing chunk order empirically rather than assuming.

## Examples

- A RAG prompt with 10 retrieved chunks where the correct one ranks 5th: reordering so it appears first (or last) measurably improves answer quality with identical retrieval.

## Connections

- [[context-window]] — the resource whose middle is under-attended
- [[retrieval-augmented-generation]] — makes chunk ordering a design decision
- [[grounding]] — a positional failure mode of context use
- [[self-attention]] — the mechanism whose learned position biases produce the effect

## Open Questions

- Do models trained with long-context curricula (post-2024) still show the U-curve, and at what lengths?
