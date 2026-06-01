---
title: "llguidance"
type: entity
entity_type: tool
tags: [constrained-decoding, inference, llm, structured-output, microsoft]
source_count: 1
---

## Overview

[[constrained-decoding]] engine from Microsoft using a Rust-based Earley parser. Powers OpenAI's Structured Outputs feature; OpenAI credited it as foundational.

## Key Facts

- **Grammar approach**: Earley parser (handles full CFG including ambiguous grammars)
- **Implementation**: Rust — safe, zero-cost abstractions, no GC pauses
- **Per-token latency**: ~50 µs, negligible startup cost
- **Creator**: Microsoft
- **Deployed by**: OpenAI (Structured Outputs product)

## Appearances

- Function Calling for LLMs Using Constrained Decoding (2026-06-01) — ecosystem comparison table; cited for OpenAI connection

## Connections

- [[constrained-decoding]] — the technique llguidance implements
- [[xgrammar]] — alternative engine (<40 µs/token, C++/PDA, used in vLLM)
- [[outlines-library]] — earlier FSM-based alternative
