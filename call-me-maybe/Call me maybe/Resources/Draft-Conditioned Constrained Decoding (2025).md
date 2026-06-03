← [[00 Reading Guide]]

# Draft-Conditioned Constrained Decoding (DCCD)
**Reddy et al. (2025)** · [arxiv.org/pdf/2603.03305](https://arxiv.org/pdf/2603.03305) · [GitHub](https://github.com/avinashreddydev/dccd)

---

## Core idea

Conditions decoding constraints on **intermediate draft outputs**. Instead of applying a fixed grammar at every step, the constraint layer uses a draft of the output to inform what valid continuations look like — more flexible, context-aware structure enforcement.

## Why masking to `-inf` works

This paper provides production-level justification: every token emitted under constraint provably preserves global validity. Masking invalid tokens to negative infinity means they receive zero probability after softmax — they are structurally impossible, not just unlikely.

## Evaluated on

Mathematical reasoning tasks (GSM-Symbolic) and other structured generation benchmarks.

## Relevance

Good theoretical backing for the core mechanic in `decoder.py`. The masking step isn't a heuristic — it's a structural guarantee.
