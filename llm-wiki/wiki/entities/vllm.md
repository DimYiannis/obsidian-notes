---
title: "vLLM"
type: entity
entity_type: tool
tags: [llm, inference, serving]
source_count: 1
---

## Overview

Open-source LLM inference and serving framework. High-throughput, memory-efficient. Uses [[xgrammar]] as its default [[constrained-decoding]] backend (as of 2026).

## Key Facts

- High-throughput LLM serving via PagedAttention
- Supports continuous batching
- Default constrained decoding backend: [[xgrammar]] (replaced [[outlines-library]])

## Appearances

- Function Calling for LLMs Using Constrained Decoding (2026-06-01) — cited as primary consumer of XGrammar

## Connections

- [[xgrammar]] — default constrained decoding backend
- [[constrained-decoding]] — capability vLLM exposes via XGrammar
- [[outlines-library]] — earlier default, now replaced
