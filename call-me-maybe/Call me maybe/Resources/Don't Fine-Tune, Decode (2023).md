← [[00 Reading Guide]]

# Don't Fine-Tune, Decode: Syntax Error-Free Tool Use via Constrained Decoding
**Zhang et al. (2023)** · [arxiv.org/abs/2310.07075](https://arxiv.org/abs/2310.07075)

---

## Core argument

Syntax constraints are only learned **implicitly** during fine-tuning — models still make frequent syntax errors. Enforcing constraints **explicitly at decode time** via finite state machines is more reliable and requires no expensive fine-tuning.

## System: TOOLDEC

A constrained decoding algorithm using FSMs to enforce tool syntax compliance. Works on top of instruction-tuned LLMs without modifying weights.

## Key result

Mistral-Instruct: tool use accuracy **0% → 52%** with constrained decoding. Zero syntax errors across all tested models and benchmarks.

## Key insight

> "Syntax constraints are better enforced explicitly during decoding than implicitly during training."

This is the direct theoretical backing for this project's approach. We don't fine-tune Qwen3-0.6B on JSON examples and hope it learns the format. We enforce the format structurally at every token step.

## Connection

→ [[Willard & Louf (2023) — Outlines]]: Willard & Louf provide the FSM-based vocabulary indexing mechanism. This paper applies the same principle specifically to tool/function calling and makes the empirical case against fine-tuning.
