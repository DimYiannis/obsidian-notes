---
title: "Feed-Forward Network (MLP)"
type: concept
tags: [transformer, architecture, mlp, llm]
source_count: 2
---

## Definition

The position-wise multilayer perceptron (MLP) sublayer inside each transformer block, applied independently to every token after the [[multi-head-attention]] sublayer. Where attention moves information *between* tokens, the feed-forward network processes each token's representation *on its own*. In the [[transformer-architecture]] it is typically two linear layers with a nonlinearity between them, with a hidden dimension several times the model dimension.

## Structure

```
token representation
   ↓
Linear (dₘ → d_ff)      ← expand (often 4× the model dim)
   ↓
nonlinearity (ReLU / GELU)
   ↓
Linear (d_ff → dₘ)      ← project back
   ↓
output (added to residual stream)
```

## Role: attention retrieves, MLP processes

A common framing: [[self-attention]] decides *which* information is relevant and routes it; the feed-forward network *transforms and combines* that information. Most of a transformer's parameters live in these MLP layers.

⚠️ **Caveat:** this "attention = retrieval, MLP = processing" split is intuitive but only partly evidenced. *A Mathematical Framework for Transformer Circuits* (Elhage et al., 2021) reverse-engineers the attention side but explicitly studies **attention-only** transformers, stating the authors "had much less success in understanding MLP layers." So the MLP half of the claim remains largely an open interpretability problem, not an established result.

## Key Properties

- **Position-wise**: same network applied to each token independently; no information exchange between positions.
- **Parameter-heavy**: the expand/project pair (often 4× hidden) holds the bulk of model parameters.
- **Hypothesized as memory**: later interpretability work treats MLP layers as key–value memories storing factual associations — still an active area.

## Connections

- [[transformer-architecture]] — the MLP is one of the two sublayers in each block
- [[multi-head-attention]] — the complementary sublayer that moves information between tokens
- [[residual-stream]] — the MLP reads from and writes to it
- [[mechanistic-interpretability]] — flags MLP layers as the harder, less-understood component

## Open Questions

- What do individual MLP neurons compute, and how localized is factual knowledge within them?
- Is the "attention routes, MLP processes" division accurate, or an oversimplification?
