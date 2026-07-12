---
title: "RAG Theory — Study Context"
type: source
date_ingested: 2026-07-12
source_url: local
source_type: note
tags: [rag, information-retrieval, bm25, study-guide]
---

## Summary

A layered theory study guide written as preparation for the *RAG Against the Machine* project (Codam/42): a BM25-based RAG system over the vLLM 0.10.1 codebase with Qwen3-0.6B generation, graded on recall@k and defended at a whiteboard. It organizes 21 concepts into five layers — motivation (why RAG exists), retrieval theory, chunking, evaluation, and generation — ordered so each layer builds on the previous. It closes with a reading path (Lewis et al. 2020, Manning's IR textbook, Elastic's Practical BM25 series, Gao et al. 2023 survey) and defense priorities.

## Key Points

- RAG exists because model knowledge is parametric (frozen at training), context windows are finite, and models hallucinate — retrieval bolts non-parametric knowledge on at inference.
- The retrieval core is classic IR: bag-of-words lexical matching, TF-IDF intuition, and BM25 as its evolution — TF saturation (k1) plus document length normalization (b) on top of IDF.
- An inverted index (`term → posting list`) is what makes lexical search fast: only chunks sharing a query term are scored.
- Retrieval tokenization (lowercasing, identifier splitting, keeping the original identifier) is a different problem from LLM tokenization (BPE) — and the same tokenizer must be used for indexing and queries.
- Lexical beats dense retrieval on exact code identifiers; dense wins on synonyms/paraphrase — the vocabulary-mismatch vs exact-match tradeoff; hybrid (RRF) combines both.
- Chunking sets retrieval granularity; with a low IoU bar (0.05), bias toward large chunks (~2000 chars).
- RAG optimizes recall at the retrieval stage (a missing chunk is unrecoverable) and precision at generation (the model can ignore junk).
- Generation-side concerns: grounding/faithfulness and the lost-in-the-middle effect on chunk ordering.

## Entities

- [[vllm]] — the corpus being indexed (vLLM 0.10.1 source tree)
- [[rag-against-the-machine]] — the project this guide prepares for

## Concepts

- [[retrieval-augmented-generation]]
- [[information-retrieval]]
- [[lexical-retrieval]]
- [[tf-idf]]
- [[bm25]]
- [[inverted-index]]
- [[retrieval-tokenization]]
- [[dense-retrieval]]
- [[chunking]]
- [[retrieval-evaluation]]
- [[grounding]]
- [[lost-in-the-middle]]
- [[hallucination]] — RAG as grounding mitigation
- [[context-window]] — finite context forces retrieval selection
- [[embeddings]] — basis of dense retrieval
- [[tokenization]] — contrast with retrieval tokenization

## Connections

- Extends the hallucination-mitigation story from the Karpathy LLM intro (tool use / retrieved text in context) into a full retrieval architecture.
- Answers part of the open question on context-window: RAG over an external store vs long-context in-context retrieval.
- The project corpus is vLLM — previously in the wiki only as a constrained-decoding consumer.
