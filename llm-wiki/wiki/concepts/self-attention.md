---
title: "Self-Attention"
type: concept
tags: [transformer, attention, mechanism, llm]
source_count: 2
---

## Definition

The mechanism by which a sequence attends to itself: every token computes how much it should draw from every other token in the same sequence, then builds a context-aware representation by mixing their information. It is the core operation of the [[transformer-architecture]]. "Self" distinguishes it from cross-attention (attending to a different sequence) — here queries, keys, and values all come from the same input.

## The computation

Each token is projected into three vectors via [[query-key-value]], and attention is computed as:

```
Attention(Q, K, V) = softmax(QKᵀ / √dₖ) · V
```

- `QKᵀ` scores how well each query matches each key (relevance).
- `√dₖ` scaling keeps scores from saturating the softmax.
- `softmax` turns scores into weights that sum to 1.
- multiplying by `V` produces a weighted blend of the values.

## Intuition

For the sentence `The cat sat on the mat because it was tired`, when processing `it` the mechanism can assign high weight to `cat` and near-zero to `mat` — resolving the reference. The model learns these relationships from data; they are not hand-coded.

```
Current token → scores all tokens → softmax weights → weighted sum of values → contextual representation
```

## Key Properties

- **Order-agnostic**: attention alone has no notion of position; positional encodings are added to [[embeddings]] to supply order.
- **Quadratic cost**: every token attends to every other, so compute/memory scale with sequence length squared — a key constraint on the [[context-window]].
- **Reframed as circuits**: [[mechanistic-interpretability]] splits each head into a QK circuit (where to attend) and an OV circuit (what to move) operating on the [[residual-stream]].

## Connections

- [[query-key-value]] — the projections self-attention operates on
- [[multi-head-attention]] — runs many self-attention computations in parallel
- [[transformer-architecture]] — self-attention is its defining sublayer
- [[context-window]] — quadratic attention cost bounds practical context length
- [[residual-stream]] — attention reads from and writes to it

## Open Questions

- How do alternatives (linear/sparse attention, state-space models) trade off the quadratic cost against quality?
