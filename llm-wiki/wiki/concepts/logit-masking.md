---
title: "Logit Masking"
type: concept
tags: [llm, inference, constrained-decoding]
source_count: 1
---

## Definition

The core mechanism inside [[constrained-decoding]]. After the LLM's forward pass produces a raw logit vector (one score per vocabulary token), a logit processor sets invalid tokens' logits to −∞. Softmax then assigns them exactly zero probability — they can never be sampled. Valid tokens are determined by the current state of the grammar tracker (FSM or PDA).

## Key Properties

- Operates **between** forward pass and sampling — no model modification
- −∞ logit → e^(−∞) = 0 → zero weight in softmax → impossible to sample
- Grammar state advances with each emitted token → mask recomputed each step
- Works autoregressively for the full generation sequence

## Pipeline

```
LLM forward pass → raw logits [vocab_size]
        ↓
Grammar state → valid token set
        ↓
mask: invalid tokens → logit = −∞
        ↓
softmax over masked logits
        ↓
sample from valid-only distribution
```

## Connections

- [[constrained-decoding]] — logit masking is the mechanism; constrained decoding is the technique that uses it
- [[token-character-mismatch]] — engineering challenge: grammar state tracks chars, tokens are multi-char; bridged by pre-computing valid states per vocab token

## Open Questions

- Probability distortion: masking renormalizes the distribution over valid tokens — probabilities shift from what the model intended. Grammar-Aligned Decoding (ASAp method, Feng et al. 2024) attempts to correct this. Material for a separate concept page.
