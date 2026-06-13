---
title: "Attention Is All You Need"
type: source
date_ingested: 2026-06-13
source_url: https://arxiv.org/abs/1706.03762
source_type: paper
tags: [transformer, attention, architecture, nlp, machine-translation]
---

## Summary

Vaswani et al. (2017, Google Brain) introduce the [[transformer-architecture]], a sequence transduction model built entirely on attention, dispensing with recurrence and convolution. The core operation is scaled dot-product [[self-attention]] — `softmax(QKᵀ/√dₖ)V` — generalized into [[multi-head-attention]] so the model attends to different representation subspaces in parallel. Each layer pairs an attention sublayer with a position-wise [[feed-forward-network]], wrapped in residual connections and layer normalization. The architecture is more parallelizable than RNNs and trains far faster, setting new state-of-the-art BLEU on WMT 2014 translation. This is the foundational architecture underlying every modern LLM in this wiki.

## Key Points

- **Attention replaces recurrence entirely**: prior sequence models (RNNs, LSTMs) processed tokens sequentially, blocking parallelization. The transformer computes all token interactions at once, enabling massively parallel training.
- **Scaled dot-product attention**: `Attention(Q,K,V) = softmax(QKᵀ/√dₖ)V`. The `√dₖ` divisor counteracts large dot products that would push softmax into low-gradient regions.
- **Query / Key / Value**: each token is projected into a query ("what I'm looking for"), key ("what I contain"), and value ("what I provide"). Compatibility of queries with keys weights the values — see [[query-key-value]].
- **Multi-head attention**: 8 parallel heads, each of dimension 64 (model dimension 512). Heads can specialize on different relationships; outputs are concatenated and projected — see [[multi-head-attention]].
- **Encoder-decoder, 6 layers each**: the original is an encoder-decoder for translation. Modern GPT-style LLMs are the decoder-only descendant — the "96 layers" figure in personal notes refers to GPT-3, not this paper.
- **Positional encoding**: since attention is order-agnostic, sinusoidal positional encodings are added to input [[embeddings]] to inject sequence order.
- **Results**: 28.4 BLEU EN→DE and 41.8 BLEU EN→FR on WMT 2014 (new SOTA), trained in 3.5 days on 8 GPUs — a fraction of prior models' cost.

## Entities

- [[google-brain]] — research org where the work was done

## Concepts

- [[transformer-architecture]] — the architecture introduced here
- [[self-attention]] — the core mechanism; sequence attends to itself
- [[multi-head-attention]] — parallel attention heads over different subspaces
- [[query-key-value]] — the Q/K/V projection scheme
- [[feed-forward-network]] — the position-wise MLP sublayer in each block

## Connections

- [[embeddings]] — token embeddings (plus positional encodings) are the transformer's input
- [[tokenization]] — produces the token IDs that get embedded before entering the transformer
- [[context-window]] — full self-attention over the context is what this architecture enables (and what makes context length quadratically expensive)
- [[mechanistic-interpretability]] — later work reverse-engineers the components this paper defines

## Quotes

> "We propose a new simple network architecture, the Transformer, based solely on attention mechanisms, dispensing with recurrence and convolutions entirely."
