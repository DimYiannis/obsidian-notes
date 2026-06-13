---
title: "Residual Stream"
type: concept
tags: [transformer, interpretability, architecture, mechanistic-interpretability]
source_count: 1
---

## Definition

The central abstraction in *A Mathematical Framework for Transformer Circuits* (Elhage et al., 2021): the running vector that flows through a transformer from embeddings to final logits, which every component reads from and writes to. Because each sublayer adds its output back via a residual connection, the stream acts as a shared communication channel across layers rather than a strict series pipeline.

## How it works

```
embeddings → + attn₁ → + mlp₁ → + attn₂ → + mlp₂ → ... → final logits
              (each component reads the current stream, writes its contribution back)
```

- Every [[multi-head-attention]] and [[feed-forward-network]] sublayer **reads** from the stream and **writes** its result back by addition.
- Different components communicate by using different **subspaces** of the stream — one component writes a direction another later reads.
- This reframes the [[transformer-architecture]] as components passing messages through a shared workspace, not just a stack of transformations.

## Key Properties

- **Linear and additive**: contributions sum, making it possible to attribute parts of the output to specific components.
- **Subspace communication**: components coordinate by writing to and reading from designated directions in the stream.
- **Foundation for circuits**: enables tracing information flow — the basis for identifying [[induction-heads]] and other circuits.

## Connections

- [[mechanistic-interpretability]] — the research program built on this framing
- [[transformer-architecture]] — residual connections are what create the stream
- [[self-attention]] — reads from / writes to the stream via QK and OV circuits
- [[feed-forward-network]] — also reads from / writes to the stream
- [[induction-heads]] — discovered by analyzing information flow through the stream

## Open Questions

- How many roughly-independent subspaces does a real model's residual stream support before interference dominates?
