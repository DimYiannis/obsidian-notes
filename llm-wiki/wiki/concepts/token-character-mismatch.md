---
title: "Token-Character Mismatch"
type: concept
tags: [llm, tokenization, constrained-decoding, inference]
source_count: 3
---

## Definition

Engineering challenge in [[constrained-decoding]]: grammar representations (FSM, PDA) track state at the **character level**, but LLMs generate **multi-character tokens**. A single token like `"name"` (5 characters including quote) must be validated against a grammar that expects individual character transitions.

## Key Properties

- Vocabulary size: 32k–128k tokens, most spanning multiple characters
- Grammar state machine: transitions on single characters
- Mismatch: can't directly apply per-character grammar logic to per-token generation

## Solution

Pre-compute, at **schema compilation time**, which grammar states each vocabulary token can legally appear in. Store as a lookup table (token → set of valid grammar states).

At inference time, mask computation = lookup, not re-computation. Cost: ~40 µs/token ([[xgrammar]]). One-time compile cost amortized across all tokens in the sequence.

```
Compile time (once):
  For each token in vocab:
    simulate character-by-character grammar traversal
    record valid grammar states → cache

Inference time (per token):
  current grammar state + candidate tokens
  → lookup cached valid-state sets
  → mask invalid tokens (−∞)
  → sample
```

## Connections

- [[constrained-decoding]] — the technique this problem appears in
- [[logit-masking]] — masking is the step where the mismatch is bridged
- [[xgrammar]] — C++/pthreads implementation that makes this lookup <40 µs/token

## Prefix-matching for multi-token identifiers

In function calling, identifiers like `fn_add_numbers` or `fn_reverse_string` span **multiple subword tokens**. The constraint checker cannot simply look up whether a complete token is valid — it must check whether a token is a valid **prefix continuation** of a function name still being assembled.

Practical approach: maintain a vocabulary mapping (token ID → string representation) and at each step, allow any token whose string is a valid prefix of at least one known function name given the tokens already emitted. This extends the core mismatch problem from character-level grammar tracking to multi-token identifier tracking.

## Open Questions

- How does this scale with vocabulary size? 128k-token vocabularies vs 32k?
- Does tokenizer choice (BPE vs SentencePiece vs others) affect mismatch severity?
