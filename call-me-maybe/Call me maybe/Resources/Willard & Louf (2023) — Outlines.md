← [[00 Reading Guide]]

# Efficient Guided Generation for Large Language Models
**Willard & Louf (Willard & Louf (2023) — Outlines2023)** · [arxiv.org/abs/2307.09702](https://arxiv.org/abs/2307.09702)

**Theoretical foundation of this project.**

---

## Core idea

Reformulates neural text generation as **transitions between states of a finite-state machine**. This lets you enforce constraints (regex, JSON Schema, context-free grammars) at generation time with near-zero overhead.

## Key contributions

- **Vocabulary indexing** — pre-builds an index mapping FSM states → valid next tokens. O(1) average-case lookup at inference time (amortized from preprocessing).
- **Boolean logit masking** — at each step, apply a mask that zeros out unsuitable tokens. Reframes guidance as an iterative matching/parsing problem against incomplete strings.
- **Extension to CFGs** — for recursive structures (nested JSON, code syntax), extends FSM to pushdown automata. A trie indexes parser configurations for efficient queries.
- **Model-agnostic** — works as an output-side wrapper around any autoregressive LLM.

## Why it matters for this project

The Outlines library is the direct implementation of this theory. The vocabulary prefix-matching approach in `vocabulary.py` and the state machine in `grammar.py` are both grounded in this paper's framework.

## Complexity insight

Prior approaches (e.g. Guidance) matched from sequence start and scanned the full vocabulary at every step — O(N) per token. Outlines amortizes all computation to preprocessing → O(1) at inference.

## Connection

→ [[Xe Iaso — Function Calling Talk]]: Xe Iaso explains *what* function calling is and why the model never executes anything. Willard & Louf explains *how* to guarantee the output is always valid JSON. The two are the "what" and the "how" of the same system.
