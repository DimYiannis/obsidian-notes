---
title: "Wiki Overview"
type: meta
tags: [meta]
---

## What this wiki is

Second brain — a personal knowledge base maintained by Claude Code following the [[llm-wiki-pattern]]. The human curates sources and directs analysis. Claude writes and maintains all pages. Obsidian is the interface.

## How to use it

**Add knowledge**: drop a file in `raw/` or paste content and say "ingest [source]". Claude processes it, discusses takeaways, writes wiki pages, and updates the index and log.

**Query knowledge**: ask any question. Claude reads `index.md`, finds relevant pages, synthesizes an answer with citations. Optionally file good answers as synthesis pages.

**Maintain health**: periodically say "lint". Claude checks for orphan pages, contradictions, gaps, and missing cross-references.

## Current state

- **Sources ingested**: 2
- **Wiki pages**: 17 (6 concepts, 7 entities, 2 sources, 1 meta, 1 overview)
- **Domains**: knowledge management methodology; LLM inference / structured output

## Entry points by topic

### Knowledge management methodology
- [[llm-wiki-pattern]] — the core concept governing this wiki
- [[rag]] — the alternative approach; context for why this wiki exists
- [[memex]] — historical antecedent (Bush, 1945)
- [[vannevar-bush]] — inventor of the Memex

### LLM inference / structured output
- [[constrained-decoding]] — inference-time structural guarantee via logit masking
- [[logit-masking]] — the core mechanism
- [[function-calling]] — primary application of constrained decoding
- [[token-character-mismatch]] — engineering challenge in implementation
- [[xgrammar]] — CFG/PDA engine, default in vLLM/SGLang/TensorRT-LLM
- [[llguidance]] — Rust Earley parser, powers OpenAI Structured Outputs
- [[outlines-library]] — pioneering FSM library (2023)
- [[fantase]] — Token Search Trie + reranker (Amazon, EMNLP 2024)
- [[vllm]] — inference framework using XGrammar

### Tools
- [[obsidian]] — the interface for this wiki

## Schema

Governed by `CLAUDE.md` in the vault root. Read it at session start.
