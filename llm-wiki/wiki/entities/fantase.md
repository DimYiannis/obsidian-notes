---
title: "FANTASE"
type: entity
entity_type: project
tags: [constrained-decoding, function-calling, nlp, research, amazon]
source_count: 1
---

## Overview

Research paper and system for faithful, efficient API call generation via state-tracked constrained decoding and reranking. Amazon Science. EMNLP 2024 Findings.

**Full title**: *FANTAstic SEquences and Where to Find Them: Faithful and Efficient API Call Generation through State-tracked Constrained Decoding and Reranking*

## Key Facts

- **Venue**: EMNLP 2024 Findings (Amazon Science)
- **Code**: https://github.com/amazon-science/faithful_and_efficient_api_call_generation
- **Component 1 — State-tracked Constrained Decoding (SCD)**:
  - Preprocesses API docs (function names, parameter names, enum values) into a Token Search Trie
  - At each step, only tokens reachable from current trie state are unmasked
  - Structurally impossible to hallucinate a function name or out-of-enum value
  - No model weight changes; output-side wrapper
- **Component 2 — Reranker**:
  - Beam search generates N constrained candidate sequences
  - Lightweight RoBERTa-based discriminator reranks using supervised signal
  - Without reranking: correct call ranked 5th on average; with reranking: rank 1

## Appearances

- Function Calling for LLMs Using Constrained Decoding (2026-06-01) — detailed treatment of both SCD and reranking components

## Connections

- [[constrained-decoding]] — FANTASE is a specialized application for API/function calling
- [[function-calling]] — the problem domain
- [[logit-masking]] — the mechanism SCD uses
