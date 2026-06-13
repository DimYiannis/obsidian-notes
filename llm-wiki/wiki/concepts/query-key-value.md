---
title: "Query, Key, Value (Q/K/V)"
type: concept
tags: [transformer, attention, mechanism, llm]
source_count: 1
---

## Definition

The three vectors every token is projected into inside an attention layer. They drive [[self-attention]]: queries are compared against keys to decide relevance, and the resulting weights are applied to values to produce output. Each is computed by multiplying the token's representation by a learned weight matrix (`Wq`, `Wk`, `Wv`).

## The roles

- **Query (Q)** — "What information am I looking for?"
- **Key (K)** — "What information do I contain?"
- **Value (V)** — "What information should I provide?"

A token's query is matched against all tokens' keys (`QKᵀ`) to score relevance; the scores weight the values that get summed into the output:

```
Attention(Q, K, V) = softmax(QKᵀ / √dₖ) · V
```

## Intuition

Querying is like a soft, learned database lookup: the query asks a question, keys advertise what each token offers, and values carry the payload that gets retrieved in proportion to query–key match.

## Key Properties

- **Learned projections**: Q, K, V come from separate weight matrices, so a token can advertise (key) something different from what it seeks (query) or delivers (value).
- **Per-head**: in [[multi-head-attention]] each head has its own Q/K/V projections, operating in its own subspace.
- **Circuit decomposition**: [[mechanistic-interpretability]] groups these into a QK circuit (query–key matching → attention pattern) and an OV circuit (value → output), which can be analyzed largely independently.

## Connections

- [[self-attention]] — the mechanism that consumes Q/K/V
- [[multi-head-attention]] — replicates Q/K/V projections across heads
- [[transformer-architecture]] — Q/K/V live inside its attention sublayers
- [[residual-stream]] — Q, K, V are read from it; attention output is written back

## Open Questions

- How much do learned Q/K/V projections reflect interpretable, human-meaningful features vs entangled directions?
