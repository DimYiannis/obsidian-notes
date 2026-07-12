---
title: "Retrieval-Augmented Generation (RAG)"
type: concept
tags: [rag, information-retrieval, llm, architecture]
source_count: 1
---

## Definition

An architecture that combines an LLM with an external retrieval system: at inference time, documents relevant to the query are retrieved from a corpus and placed in the prompt, and the model answers from that retrieved context. Introduced by Lewis et al. (2020). RAG bolts **non-parametric knowledge** (an external, updatable store) onto a model whose **parametric knowledge** (weights) is frozen at training time.

## Key Properties

- **Parametric vs non-parametric knowledge** — weights are a vague, frozen "long-term memory"; retrieved text in the [[context-window]] is precise working memory. RAG shifts factual load from the former to the latter.
- **Motivations** — [[hallucination]] (grounding forces answers from real text), finite [[context-window]] (can't paste a whole corpus; retrieval selects), knowledge cutoff and domain gap (the model never saw the target corpus).
- **Pipeline** — corpus → [[chunking]] → indexing → retrieval ([[lexical-retrieval]] or [[dense-retrieval]]) → prompt construction → generation with [[grounding]].
- **Asymmetric optimization** — recall at retrieval, precision at generation: a missing relevant chunk is unrecoverable, junk chunks can be ignored by the model. See [[retrieval-evaluation]].

## Examples

- *RAG Against the Machine* (Codam/42 project): [[bm25]] retrieval over the [[vllm]] 0.10.1 codebase, Qwen3-0.6B generation, graded on recall@k.
- Production Q&A systems over private documentation, where fine-tuning is too slow or costly to keep current.

## Connections

- [[information-retrieval]] — the retrieval half is classic IR
- [[hallucination]] — RAG is a grounding mitigation
- [[context-window]] — the constraint that makes retrieval selection necessary
- [[chunking]] — sets the granularity of what can be retrieved
- [[lost-in-the-middle]] — generation-side failure mode affecting chunk ordering
- [[dense-retrieval]] / [[lexical-retrieval]] — the two retrieval families

## Open Questions

- At what context length does long-context in-context retrieval subsume RAG over an external store? (Carried over from [[context-window]].)
- How much does retrieval quality vs generator quality bound end-to-end answer quality?
