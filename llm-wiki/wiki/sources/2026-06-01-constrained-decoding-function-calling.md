---
title: "Function Calling for LLMs Using Constrained Decoding"
type: source
date_ingested: 2026-06-01
source_url: local
source_type: article
tags: [llm, constrained-decoding, function-calling, inference, structured-output]
---

## Summary

[[constrained-decoding]] solves the fragile-parsing problem in LLM tool use by enforcing structural validity at the token level: a logit processor masks invalid tokens to −∞ before softmax, so they can never be sampled. Two grammar backends dominate — FSMs ([[outlines-library]]) for simple schemas, CFG/PDA ([[xgrammar]], [[llguidance]]) for recursive structures. [[xgrammar]] is the 2026 default in [[vllm]], SGLang, and TensorRT-LLM at <40 µs/token. [[fantase]] (EMNLP 2024, Amazon) adds a Token Search Trie for API-specific enforcement plus a RoBERTa reranker. Key tradeoff: rigid formats hurt reasoning tasks; production fix is a free-form reasoning field before constrained fields.

## Key Points

- [[logit-masking]] is the universal mechanism: −∞ logit → zero post-softmax probability → structurally impossible to emit
- Two grammar families: FSM (simple, fast compile, breaks on recursion) vs CFG/PDA (handles nesting, higher overhead)
- [[xgrammar]]: C++/pthreads, PDA-based, <40 µs/token — now default in major inference frameworks
- [[llguidance]]: Rust Earley parser, ~50 µs/token — credited by OpenAI for Structured Outputs
- [[fantase]]: Token Search Trie makes hallucinating function names / enum values impossible; beam search + RoBERTa reranker lifts correct candidate from rank 5 → rank 1
- [[token-character-mismatch]]: grammars think in chars, tokens are multi-char — solved by pre-computing per-vocab-token valid states once at compile time
- Format constraints hurt reasoning (math, multi-hop QA), help classification (slot-filling, intent) — hybrid pattern recommended

## Entities

- [[xgrammar]] — CFG/PDA engine, C++, default in vLLM/SGLang/TensorRT-LLM
- [[llguidance]] — Rust Earley parser, Microsoft, powers OpenAI Structured Outputs
- [[outlines-library]] — pioneering FSM-based library (Willard & Louf, 2023)
- [[fantase]] — Token Search Trie + reranker; Amazon Science, EMNLP 2024
- [[vllm]] — inference framework; uses [[xgrammar]] as default constrained decoding backend

## Concepts

- [[constrained-decoding]] — the core technique; logit masking + grammar tracking
- [[logit-masking]] — mechanism: set invalid token logits to −∞ before softmax
- [[function-calling]] — the primary application domain
- [[token-character-mismatch]] — engineering challenge bridging char-level grammars and multi-char tokens

## Quotes

> "Prompt engineering asks the model to follow a format. SCD enforces it at the token probability level."

> "Invalid tokens get −∞ → probability 0 after softmax."

> "With regular beam search, the correct generation was only ranked 5th on average; reranking pushes it to rank 1."
