---
title: "A Mathematical Framework for Transformer Circuits"
type: source
date_ingested: 2026-06-13
source_url: https://transformer-circuits.pub/2021/framework/index.html
source_type: paper
tags: [interpretability, transformer, attention, mechanistic-interpretability, anthropic]
---

## Summary

Elhage et al. (2021, [[anthropic]]) provide a mathematical framework for reverse-engineering small transformers, founding much of [[mechanistic-interpretability]]. The central reframing treats the model as a [[residual-stream]] that every component reads from and writes to additively. Each attention head decomposes into two near-independent computations — a QK circuit (where to attend) and an OV circuit (what information to move). By building up from zero-, one-, and two-layer attention-only models, the authors discover [[induction-heads]]: an emergent in-context-learning mechanism that only appears with two or more layers. The paper deliberately studies **attention-only** transformers and admits limited success understanding [[feed-forward-network]] (MLP) layers.

## Key Points

- **Residual stream as a communication channel**: components don't pass activations in series — they read from and write to shared subspaces of a common residual stream, which persists across layers. See [[residual-stream]].
- **QK vs OV circuits**: an attention head factors into a QK circuit (computes the attention pattern — which tokens attend to which) and an OV circuit (computes how an attended token's information is written to output). This formalizes "attention = retrieval/routing."
- **Head composition**: heads in later layers can compose with earlier heads via queries, keys, or values — the substrate for multi-step algorithms.
- **Induction heads**: emerge in 2-layer models; implement a `[A][B]...[A] → [B]` copy/completion algorithm and are a major driver of in-context learning. See [[induction-heads]].
- **Progressive complexity**: zero-layer models = bigram statistics; one-layer = ensemble of bigrams + skip-trigrams; two-layer = compositional algorithms including induction.
- **MLP caveat (important)**: the framework covers attention-only models. The authors state they "had much less success in understanding MLP layers" — a "major weakness." So the paper supports the *attention-as-routing* half of the common "attention retrieves, MLP processes" claim, but does **not** establish the MLP half.

## Entities

- [[anthropic]] — research org behind the Transformer Circuits work

## Concepts

- [[mechanistic-interpretability]] — the research program this paper helped found
- [[residual-stream]] — the central organizing abstraction
- [[induction-heads]] — emergent in-context-learning mechanism discovered here

## Connections

- [[transformer-architecture]] — this paper reverse-engineers the architecture Vaswani et al. defined
- [[self-attention]] — reframed here as QK + OV circuits operating on the residual stream
- [[multi-head-attention]] — head composition across layers is central to the framework
- [[feed-forward-network]] — explicitly out of scope; flagged as the framework's main limitation
- [[chain-of-thought]] — in-context learning (which induction heads support) is a substrate for emergent reasoning behaviors

## Quotes

> "we've simply had much less success in understanding MLP layers so far" — the framework's acknowledged limitation regarding feed-forward sublayers.
