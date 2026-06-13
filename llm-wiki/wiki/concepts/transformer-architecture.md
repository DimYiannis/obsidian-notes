---
title: "Transformer Architecture"
type: concept
tags: [transformer, architecture, attention, neural-networks, llm]
source_count: 1
---

## Definition

The neural network architecture introduced in *Attention Is All You Need* (Vaswani et al., 2017) that underlies every modern LLM. It processes sequences using [[self-attention]] instead of recurrence or convolution, letting every token interact with every other token in a single parallel operation. The architecture is a stack of identical **transformer blocks**, each combining a [[multi-head-attention]] sublayer with a position-wise [[feed-forward-network]], wrapped in residual connections and layer normalization.

## Block structure

```
Input
  ↓
Multi-Head Self-Attention   ← tokens exchange information
  ↓
Add & Normalize             ← residual connection + layer norm
  ↓
Feed-Forward Network (MLP)  ← per-token processing
  ↓
Add & Normalize
  ↓
Output → next block
```

A model is this block repeated N times. GPT-3 uses 96 such blocks; larger models exceed 100.

## Key Properties

- **Parallelizable**: unlike RNNs/LSTMs, all positions are processed simultaneously — the key enabler of large-scale pretraining.
- **Self-attention core**: each block routes information between tokens via [[query-key-value]] projections and `softmax(QKᵀ/√dₖ)V`.
- **Residual connections**: every sublayer adds its output back to its input, forming the [[residual-stream]] that components communicate through.
- **Position-aware via encoding**: attention is order-agnostic, so positional information must be injected (sinusoidal encodings in the original; learned/rotary variants later) on top of [[embeddings]].
- **Two architectural families**: encoder-decoder (original, for translation) and decoder-only (GPT-style LLMs). The "stacked block" intuition applies to both.

## Pipeline position

```
Token IDs → Embeddings (+ positional) → [Transformer Block] × N → Logits → Softmax → Next Token
```

## Connections

- [[self-attention]] — the mechanism inside each block
- [[multi-head-attention]] — the parallel-heads form of attention used
- [[feed-forward-network]] — the MLP sublayer in each block
- [[residual-stream]] — what residual connections create across the stack
- [[embeddings]] — the transformer's input representation
- [[mechanistic-interpretability]] — research reverse-engineering this architecture's internals

## Open Questions

- How much capability comes from depth (more blocks) vs width (larger dimensions)?
- Where in the stack does factual recall vs reasoning predominantly happen?
