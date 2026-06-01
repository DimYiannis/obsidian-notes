---
title: "Tokenization"
type: concept
tags: [llm, pre-training, inference, tokens]
source_count: 2
---

## Definition

The process of converting raw text into a one-dimensional sequence of integer token IDs that LLMs operate on. Necessary because neural networks require finite, discrete symbol sets. The reverse process (token IDs → text) is detokenization.

## Key Properties

- **Vocabulary size**: ~32k–128k tokens. GPT-4: 100,277 tokens.
- **Multi-character tokens**: most tokens span multiple characters ("hello" = one token, " world" = one token). Tradeoff: larger vocab → shorter sequences → more efficient computation.
- **Algorithm**: Byte Pair Encoding (BPE) — iteratively merge the most frequent byte pairs into new tokens until target vocab size is reached
- **Context sensitivity**: capitalization, spacing, and punctuation change token boundaries. "Hello world" ≠ "hello world" ≠ "Hello  world" (two spaces)
- **Token sequence = the LLM's native format**: everything — text, conversations, tool calls, images — must be tokenized before the model sees it

## BPE pipeline

```
raw text
  → UTF-8 bytes (256 possible symbols)
  → merge frequent pairs into new tokens (iterate N times)
  → vocabulary of ~100k tokens
  → text represented as shorter sequence of larger symbols
```

## Sharp edges from tokenization

- **Spelling failures**: model sees tokens not characters → "ubiquitous" = 3 tokens → can't index individual letters reliably → counting R's in "strawberry" fails
- **Arithmetic on tokens**: numbers tokenize inconsistently → "9.11" and "9.9" may be single tokens each, but comparison requires character-level reasoning the model can't do in one forward pass
- **[[token-character-mismatch]]**: grammar-based [[constrained-decoding]] operates at character level but model generates multi-character tokens → requires pre-computation of valid grammar states per vocab token

## Connections

- [[pre-training]] — tokenization is the prerequisite; model trains on token sequences
- [[context-window]] — length measured in tokens, not characters
- [[token-character-mismatch]] — engineering challenge arising from multi-char tokens in constrained decoding
- [[constrained-decoding]] — logit masking operates on tokens; must bridge to character-level grammars
- [[hallucination]] — tokenization doesn't cause hallucination directly, but spelling/counting failures are tokenization artifacts

## Open Questions

- Would character-level or byte-level models eliminate [[token-character-mismatch]] problems? (Yes, but sequences become impractically long.)
- How does BPE vocabulary choice affect multilingual performance?
