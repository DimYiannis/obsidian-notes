---
title: "Outlines"
type: entity
entity_type: tool
tags: [constrained-decoding, inference, llm, fsm]
source_count: 1
---

## Overview

Pioneering open-source [[constrained-decoding]] library by Willard & Louf (2023). First widely-used implementation of FSM-based constrained generation for LLMs. Foundational to the field; influenced all subsequent engines.

## Key Facts

- **Grammar approach**: FSM — JSON Schema → regex → Finite-State Machine
- **Limitation**: struggles with recursive structures (nested JSON objects, variable-length arrays) — FSMs cannot represent context-free languages
- **Compile cost**: slow on complex schemas (~40 seconds+ for large JSON Schemas)
- **Paper**: Willard & Louf, Preprint 2023
- Early default backend for [[vllm]] before [[xgrammar]] replaced it

## Appearances

- Function Calling for LLMs Using Constrained Decoding (2026-06-01) — ecosystem comparison; noted as pioneering but superseded for production use

## Connections

- [[constrained-decoding]] — the technique Outlines implements
- [[xgrammar]] — successor; solves Outlines' compile speed and recursion limitations
- [[llguidance]] — alternative successor using Earley parser
