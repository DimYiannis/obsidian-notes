---
title: "Call me maybe — Function Calling in LLMs"
type: source
date_ingested: 2026-06-01
source_url: local
source_type: project
tags: [function-calling, constrained-decoding, llm, project, qwen]
---

## Summary

Personal project implementing [[function-calling]] via [[constrained-decoding]] using a 0.6B parameter model (Qwen3-0.6B). Translates natural language prompts into structured, schema-validated JSON function calls. Core result: unconstrained model produces valid JSON ~30% of the time; constrained decoding → 100% schema compliance. Reliability comes from structural guidance, not model size.

## Key Points

- **Quantified baseline vs constrained**: 0.6B unconstrained → ~30% valid JSON; same model with constrained decoding → 100%. Model size irrelevant for structural reliability.
- **JSON parse state machine**: at each generation step, track *where* in the JSON structure you are ("expecting a key", "inside a string value", "expecting a number") and allow only tokens valid for that state — not a global mask but a position-aware one.
- **Two levels of compliance enforced simultaneously**: syntactic (valid JSON) AND semantic (function name from known set, argument keys matching definition, types matching schema).
- **Greedy decoding is optimal under constraint**: once invalid tokens are masked to −∞, temperature/sampling adds noise without benefit. Greedy is the correct choice for constrained structured output.
- **Subword prefix-matching challenge**: function names like `fn_add_numbers` span multiple subword tokens — constraint checker must prefix-match against vocabulary to allow partial valid continuations of a function name still being assembled.
- **Vocabulary mapping**: token IDs mapped to their string representations to enable constraint checking at inference time.
- **Function selection accuracy**: ~90%+ — the structural guarantee covers format; semantic accuracy depends on model understanding.

## Concepts

- [[constrained-decoding]] — core mechanism; JSON state machine and greedy decoding insight added
- [[function-calling]] — the application; quantitative result (30%→100%) documented
- [[token-character-mismatch]] — subword prefix-matching is the practical form of this challenge

## Full algorithm steps

1. Track JSON parse state (state machine)
2. Determine valid next strings given current state + schema
3. Map strings to token IDs via vocabulary prefix matching
4. Mask invalid logits to `-inf`
5. Argmax over remaining valid logits (greedy)
6. Advance state machine
7. Repeat until done

## Key papers (theoretical foundation)

- Willard & Louf (2023) — *Efficient Guided Generation for Large Language Models* — FSM-based constrained generation; direct foundation of this project
- Don't Fine-Tune, Decode (2023) — syntax error-free tool use via constrained decoding
- JSONSchemaBench (2024) — benchmark across Outlines, XGrammar, llama.cpp, OpenAI, Gemini

## Connections

- [[logit-masking]] — the underlying mechanism; this project applies it with a JSON parse state machine as the grammar tracker
- [[tokenization]] — subword tokenization is why prefix-matching is needed for multi-token identifiers
- [[outlines-library]] — theoretical foundation (Willard & Louf 2023) is the same paper Outlines is built on
