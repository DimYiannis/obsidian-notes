---
title: "Pre-Training"
type: concept
tags: [llm, training, neural-networks]
source_count: 1
---

## Definition

Stage 1 of the LLM training pipeline. A neural network (Transformer) is trained on massive internet text corpora to predict the next token in a sequence. Output: a [[base-model]] — a statistical compressor of internet text. Computationally the most expensive stage: months on thousands of GPUs, millions of dollars.

## Key Properties

- **Objective**: predict next token given context (all positions in parallel via teacher forcing)
- **Data**: filtered internet text — Common Crawl → URL filtering → text extraction → language filtering → deduplication → PII removal → ~44TB for FineWeb, ~15T tokens for LLaMA 3
- **[[tokenization]]**: raw text → token IDs via BPE (GPT-4: 100,277 vocab); token sequence is the input
- **Architecture**: Transformer neural network; billions of parameters randomly initialized, updated iteratively
- **Loss**: decreasing cross-entropy → model's predicted distribution matches training token statistics
- **Output**: [[base-model]] with parameters encoding statistical patterns of internet text
- **Scale**: LLaMA 3 — 405B params, 15T tokens; GPT-2 (2019) — 1.6B params, 100B tokens, ~$40k then, ~$100 now

## What it produces

A model that can continue any token sequence in a statistically plausible way. Not an assistant. Knows a lot (compressed internet knowledge in parameters) but answers questions only if cleverly prompted via few-shot. "An internet document simulator."

## Connections

- [[base-model]] — the direct output
- [[tokenization]] — prerequisite; text must be tokenized before training
- [[supervised-fine-tuning]] — stage 2; transforms base model into assistant
- [[context-window]] — maximum token sequence length the model can process (~1,024 for GPT-2; ~1M for modern models)
- [[llm-training-pipeline]] — where pre-training fits

## Open Questions

- How much does data quality vs quantity matter at pre-training scale?
- At what point does pre-training knowledge (parameter recollection) become unreliable vs. putting info directly in [[context-window]]?
