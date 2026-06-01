---
title: "XGrammar"
type: entity
entity_type: tool
tags: [constrained-decoding, inference, llm, structured-output]
source_count: 1
---

## Overview

High-performance [[constrained-decoding]] engine using a Context-Free Grammar / Pushdown Automaton approach. Implemented in C++ with pthreads. As of 2026, the default constrained decoding backend in [[vllm]], SGLang, and TensorRT-LLM.

## Key Facts

- **Grammar approach**: CFG/PDA — handles recursive JSON structures (nested objects, variable arrays) that FSMs cannot
- **Implementation**: C++ with pthreads; moves compile-heavy work off Python's GIL
- **Per-token latency**: <40 µs — near-zero overhead at inference
- **Compile strategy**: grammar compiled once, cached; inference = state lookup
- **Paper**: Dong et al., Preprint 2024
- Solves the batching problem: FSM compilation per request blocked entire batches; XGrammar's C++ compile + PDA avoids this

## Appearances

- Function Calling for LLMs Using Constrained Decoding (2026-06-01) — detailed comparison with other engines; cited as 2026 default

## Connections

- [[constrained-decoding]] — the technique XGrammar implements
- [[logit-masking]] — the per-token mechanism XGrammar applies
- [[token-character-mismatch]] — solved via pre-computed per-vocab-token state lookup
- [[vllm]] — primary consumer; uses XGrammar as default backend
- [[outlines-library]] — the FSM-based predecessor XGrammar improved on
- [[llguidance]] — alternative engine (Rust, Earley parser, ~50 µs/token)
