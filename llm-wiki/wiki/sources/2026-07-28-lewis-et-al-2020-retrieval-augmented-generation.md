---
title: "Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks"
type: source
date_ingested: 2026-07-28
source_url: https://arxiv.org/abs/2005.11401
source_type: paper
tags: [rag, retrieval, dense-retrieval, dpr, nlp, open-domain-qa]
---

## Summary

The foundational RAG paper (Lewis et al., NeurIPS 2020, Facebook AI Research). Combines a parametric seq2seq generator (BART) with a non-parametric memory — a dense vector index over 21M Wikipedia passages, accessed via a pre-trained neural retriever (DPR) — and fine-tunes the whole system end-to-end by marginalizing over the top-k retrieved documents, with no direct supervision on which document is "correct." Introduces two formulations: RAG-Sequence (one retrieved document drives the whole output) and RAG-Token (can draw on different documents per token). Achieves state-of-the-art results on four open-domain QA benchmarks and produces more factual, diverse generations than a pure parametric baseline. The paper this project's whole [[retrieval-augmented-generation]] concept and the *rag-theory-study-guide* are built on, but not previously ingested as a primary source.

## Key Points

- **Architecture**: retriever (DPR — BERT bi-encoder, Maximum Inner Product Search over a Wikipedia index) + generator (BART, 400M params), combined probabilistically by marginalizing over top-k retrieved docs.
- **Training**: document encoder and index stay frozen; only the query encoder and BART generator are fine-tuned. Retrieval quality emerges purely from end-task supervision (negative marginal log-likelihood) — no labels on which document is right.
- **RAG-Sequence vs RAG-Token**: Sequence keeps one retrieved document consistent across the whole generated output; Token can mix content from different documents at each generation step.
- **State-of-the-art open-domain QA** at publication: Natural Questions 44.5% EM, WebQuestions 68.0%, TriviaQA 56.8%, CuratedTrec 45.2% — beating both a purely parametric baseline (T5-11B+SSM) and a purely extractive one (DPR alone).
- **Knowledge editing without retraining**: swapping the retrieval index (2016 → 2018 Wikipedia dump) correctly changes answers to time-sensitive questions. The paper's central argument for non-parametric over parametric memory.
- **Hallucination reduction, quantified**: RAG still answers 11.8% of Natural Questions correctly even when the retrieved passages don't contain the answer (parametric memory fills the gap) — vs 0% for purely extractive models, which have no fallback.
- **Generation diversity**: RAG-Sequence produces 83.5% distinct trigrams vs BART's 70.7% on MS-MARCO, without any explicit diversity-promoting decoding.
- **Failure mode — retriever collapse**: on tasks with no clear factual grounding need (e.g. open-ended story generation), the retriever learns to retrieve the same document regardless of the input query, making retrieval a no-op.

## Entities

- [[facebook-ai-research]] — the lab behind RAG, DPR, and BART

## Concepts

- [[retrieval-augmented-generation]] — this is its origin paper
- [[dense-retrieval]] — DPR is the retriever used
- [[hallucination]] — quantified reduction vs extractive baseline
- [[grounding]] — the mechanism by which RAG improves factuality
- [[context-window]] — retrieved passages are concatenated into the generator's input

## Connections

- Directly underlies the *rag-theory-study-guide* source page and the [[retrieval-augmented-generation]] concept page, both ingested 2026-07-12 — this is the primary source those pages summarized secondhand.
- The project's own retriever ([[bm25]], lexical) is a deliberate departure from this paper's dense (DPR) retriever — see [[dense-retrieval]] vs [[lexical-retrieval]] for the tradeoff.
- Knowledge-editing-by-index-swap result answers part of the open question on [[context-window]] (external store vs long-context in-context retrieval) with a concrete, empirical case for the external-store side.

## Quotes

> "RAG models where the parametric memory is a pre-trained seq2seq model and the non-parametric memory is a dense vector index of Wikipedia, accessed with a pre-trained neural retriever."

> The retriever, on tasks lacking explicit factual grounding, "would collapse and learn to retrieve the same documents regardless of the input."
