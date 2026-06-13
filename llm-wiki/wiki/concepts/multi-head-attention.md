---
title: "Multi-Head Attention"
type: concept
tags: [transformer, attention, mechanism, llm]
source_count: 2
---

## Definition

Running several [[self-attention]] computations in parallel — each a "head" with its own [[query-key-value]] projections — instead of a single one. Each head operates in a lower-dimensional subspace, so different heads can attend to different kinds of relationships at once. Their outputs are concatenated and linearly projected back to the model dimension before passing to the next sublayer.

## Original configuration

In *Attention Is All You Need* (Vaswani et al., 2017): **8 heads**, each of dimension 64, with model dimension 512 (8 × 64 = 512). Splitting the dimension across heads keeps total compute similar to single-head attention while adding representational diversity.

## Specialization

Different heads often specialize in different patterns:

```
Head 1 → grammar / syntax
Head 2 → subject–verb relationships
Head 3 → locations
Head 4 → long-range dependencies
...
```

This is a learned tendency, not a hard assignment — heads are not explicitly told what to track. Interpretability work (e.g. analyses of attention patterns) finds identifiable roles in some heads, while [[mechanistic-interpretability]] shows heads compose across layers into multi-step algorithms.

## Key Properties

- **Parallel subspaces**: heads attend to different learned representation subspaces simultaneously.
- **Concatenate + project**: head outputs are concatenated and passed through an output projection.
- **Composition across layers**: later-layer heads can build on earlier-layer heads (key/query/value composition) — the substrate for [[induction-heads]].
- **Redundancy**: research suggests many heads can be pruned with little loss, implying real specialization is concentrated in a subset.

## Connections

- [[self-attention]] — multi-head attention is several of these in parallel
- [[query-key-value]] — each head has its own Q/K/V projections
- [[transformer-architecture]] — the attention sublayer of each block is multi-head
- [[induction-heads]] — a specialized head behavior emerging from cross-layer composition
- [[mechanistic-interpretability]] — studies what individual heads compute

## Open Questions

- Why are so many heads prunable? Is redundancy a training artifact or a robustness feature?
