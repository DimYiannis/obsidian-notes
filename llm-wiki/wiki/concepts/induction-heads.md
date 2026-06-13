---
title: "Induction Heads"
type: concept
tags: [transformer, interpretability, attention, mechanistic-interpretability, in-context-learning]
source_count: 1
---

## Definition

A specialized attention-head behavior discovered in *A Mathematical Framework for Transformer Circuits* (Elhage et al., 2021): heads that implement a copy/completion rule of the form `[A][B] ... [A] → [B]`. Seeing token `A` again, an induction head attends to whatever followed `A` earlier and predicts it next. They emerge only in transformers with **two or more layers** and are a major mechanism behind in-context learning.

## How it works

Induction requires composition across two layers:

1. An earlier-layer head writes information about the *previous token* into the [[residual-stream]].
2. A later-layer induction head uses that to find the earlier occurrence of the current token and copy what came after it.

This cross-layer cooperation is why a single layer cannot produce induction — it needs [[multi-head-attention]] heads composing through the residual stream.

## Key Properties

- **Emergent with depth**: absent in zero- and one-layer models; appear at two layers.
- **In-context learning substrate**: generalize beyond literal repetition to pattern completion, helping explain how models learn from examples in the prompt without weight updates.
- **Interpretability landmark**: one of the first concrete, reverse-engineered algorithms found inside transformers.

## Connections

- [[mechanistic-interpretability]] — induction heads are a flagship result of the program
- [[residual-stream]] — the channel through which the two composing heads communicate
- [[multi-head-attention]] — induction arises from heads composing across layers
- [[transformer-architecture]] — the depth ≥ 2 requirement ties the behavior to stacking blocks
- [[chain-of-thought]] — in-context learning underpins prompt-based reasoning behaviors

## Open Questions

- How much of large-model in-context learning is attributable to induction heads vs other mechanisms?
