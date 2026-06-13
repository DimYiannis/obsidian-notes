---
title: "Mechanistic Interpretability"
type: concept
tags: [interpretability, transformer, research-program, mechanistic-interpretability]
source_count: 1
---

## Definition

The research program of reverse-engineering the internal computations of neural networks into human-understandable algorithms — tracing *how* a model produces an output by identifying the circuits that implement it, rather than only measuring *what* it outputs. *A Mathematical Framework for Transformer Circuits* (Elhage et al., 2021, [[anthropic]]) is a foundational text, establishing the [[residual-stream]] and circuit decomposition as core analytical tools.

## Core ideas

- **Residual stream as substrate**: model the [[transformer-architecture]] as components reading from and writing to a shared [[residual-stream]] — see [[residual-stream]].
- **Circuit decomposition**: factor each attention head into a QK circuit (where to attend) and an OV circuit (what to move), analyzable largely independently — connects to [[query-key-value]].
- **Build up from small models**: study zero-, one-, and two-layer attention-only transformers to isolate mechanisms like [[induction-heads]] before tackling full models.

## Key Properties

- **Bottom-up**: starts from weights and activations, aiming for mechanistic (not just correlational) explanations.
- **Known limitation**: early work covers attention-only models; [[feed-forward-network]] (MLP) layers remain much harder to interpret.
- **Safety motivation**: understanding internal computation is pursued partly to anticipate and control model behavior.

## Connections

- [[residual-stream]] — the central abstraction it introduced
- [[induction-heads]] — a flagship reverse-engineered mechanism
- [[self-attention]] — reframed as QK + OV circuits
- [[feed-forward-network]] — the acknowledged hard frontier
- [[transformer-architecture]] — the object of study

## Open Questions

- Do circuit-level findings on small models scale to frontier-size models?
- Can MLP layers be decomposed as cleanly as attention heads?
