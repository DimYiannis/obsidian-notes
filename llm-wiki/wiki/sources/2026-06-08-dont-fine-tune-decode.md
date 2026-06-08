---
title: "Don't Fine-Tune, Decode: Syntax Error-Free Tool Use via Constrained Decoding"
type: source
date_ingested: 2026-06-08
source_url: https://arxiv.org/abs/2310.07075
source_type: paper
tags: [constrained-decoding, function-calling, tool-use, fsm, llm]
---

## Summary

Zhang et al. (2023) argue that fine-tuning teaches tool syntax only implicitly — models still produce frequent syntax errors. TOOLDEC, their FSM-based [[constrained-decoding]] algorithm, enforces tool syntax explicitly at decode time with no weight changes. Result: zero syntax errors across all tested models and benchmarks. Mistral-Instruct [[function-calling]] accuracy: **0% → 52%**. Outperforms specialized fine-tuned models while requiring no training.

## Key Points

- **Fine-tuning vs. decode-time enforcement**: fine-tuning learns syntax implicitly → errors persist. Explicit decode-time constraints via FSM → zero syntax errors. Enforcement *timing* determines reliability, not model size or training budget.
- **TOOLDEC**: FSM-based constrained decoding for tool/function call syntax. Works as output-side wrapper around any instruction-tuned LLM. No fine-tuning required.
- **Empirical result**: Mistral-Instruct tool use accuracy 0% → 52% with constrained decoding. Zero syntax errors across all tested models and benchmarks.
- **Broader implication**: structural reliability is a decoding problem, not a training problem. A small model with explicit constraints outperforms a larger fine-tuned model on format compliance.

## Concepts

- [[constrained-decoding]] — TOOLDEC is a direct application; adds the fine-tuning vs decode-time argument
- [[function-calling]] — primary application domain; 0%→52% result documented
- [[logit-masking]] — the underlying mechanism TOOLDEC uses

## Connections

- [[outlines-library]] — Willard & Louf (2023) provide the FSM-based vocabulary indexing foundation this paper builds on
- Prior work in this wiki: constrained-decoding page already covers JSON state machine and greedy decoding; this paper adds the fine-tuning comparison and the TOOLDEC system

## Quotes

> "Syntax constraints are only learned implicitly during fine-tuning — models still make frequent syntax errors."

> "Syntax constraints are better enforced explicitly during decoding than implicitly during training."
