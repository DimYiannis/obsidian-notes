---
title: "Retrieval Tokenization (Text Analysis)"
type: concept
tags: [information-retrieval, tokenization, code-search]
source_count: 1
---

## Definition

The process of turning text into the **terms** a lexical index matches on: lowercasing, splitting on non-alphanumerics, stemming, identifier splitting. A different problem from LLM [[tokenization]] (BPE): BPE compresses text into model-input token IDs; retrieval tokenization defines match units for search. In IR literature this is "analysis" (Lucene's term).

## Key Properties

- **Same analyzer on both sides, always** — index terms and query terms must come from the identical tokenizer, or vocabularies mismatch and overlap drops to zero. The single most common silent recall killer.
- **Identifier splitting for code** — split `snake_case` and `CamelCase` into subtokens **while also keeping the original identifier as a term**: `enable_lora` → `enable_lora`, `enable`, `lora`. Sub-terms catch natural-language queries ("how to enable LoRA"); the intact identifier keeps its high-IDF exact match. This is the main lever for code recall.
- **Lowercasing / normalization** — trades a little precision (case distinctions) for query robustness.
- **Determines IDF statistics** — the analyzer decides the vocabulary, hence every `df` count that [[tf-idf]] and [[bm25]] rely on.

## Examples

- Query "How to configure the OpenAI server?" → terms `how, to, configure, the, openai, server` — matching chunks containing `OpenAIServingChat` (which splits to `openai, serving, chat` + original).

## Connections

- [[tokenization]] — the LLM counterpart; same word, different problem (BPE model input vs lexical match units)
- [[lexical-retrieval]] — retrieval tokenization defines its term space
- [[inverted-index]] — keyed by the terms this process produces
- [[bm25]] — consumes the resulting term statistics

## Open Questions

- Does stemming help or hurt on mixed code+prose corpora? (Code identifiers shouldn't be stemmed; doc prose might benefit.)
