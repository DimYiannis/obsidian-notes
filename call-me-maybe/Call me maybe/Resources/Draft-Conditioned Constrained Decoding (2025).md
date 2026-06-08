← [[00 Reading Guide]]

# Draft-Conditioned Constrained Decoding (DCCD)
**Reddy et al. (2025)** · [arxiv.org/abs/2603.03305](https://arxiv.org/abs/2603.03305) · [GitHub](https://github.com/avinashreddydev/dccd)

---

## The problem with standard constrained decoding

Standard constrained decoding enforces validity at every token step — but this can **distort generation** when the model assigns low probability mass to valid continuations. The model gets pushed toward locally valid but semantically wrong outputs. A single syntax error makes the output unusable, but fixing syntax by brute force at every step can break meaning.

## Core idea: separate planning from enforcement

DCCD is a **two-step, training-free inference procedure**:

1. **Draft step** — generate an unconstrained output. The model explores freely, producing something semantically meaningful but potentially syntactically invalid.
2. **Constrained step** — apply constrained decoding *conditioned on the draft*. The draft acts as a semantic guide; the constraint layer enforces structural validity on top of it.

Result: the model isn't fighting the constraint at every step. It already "knows" where it's going from the draft, so the constraint is enforcing form on a semantically sensible plan rather than blindly forcing validity at each token.

## Why this reduces the "projection tax"

Standard constrained decoding applies a KL-projection at each step — it forces the distribution over valid tokens, which distorts the model's intended probabilities. DCCD's draft conditioning **increases the feasible probability mass** assigned to valid continuations before the constraint is applied, so the projection tax is lower. The model's own distribution already leans toward valid outputs because it's guided by the draft.

## Optional: best-of-K draft selection

Run K unconstrained drafts, pick the best one, then apply constrained decoding on that. Improves output quality further at the cost of K× inference.

## Key result

GSM8K mathematical reasoning, 1B model: **15.2% → 39.0%** (+24 percentage points). Enables a small model to match much larger constrained baselines.

## Why masking to `-inf` works

Every token emitted under constraint provably preserves global validity. Masking invalid tokens to −∞ means they receive zero probability after softmax — structurally impossible, not just unlikely. DCCD provides the theoretical justification for this: it's not a heuristic, it's a guarantee backed by KL-projection analysis.

## Relevance to this project

This project uses **standard constrained decoding** — not DCCD. Constraints are applied from token 1, every step, no draft phase.

What guides the model semantically is the **prompt** — the system prompt already contains the function definitions and the user's request, so by the time generation starts, the model's probability distribution already leans toward the correct function name and arguments. The masking just enforces that whatever it picks is structurally valid.

Example:
```
Prompt: "Here are the functions: fn_add_numbers, fn_greet...
         User asked: 'What is the sum of 40 and 2?'
         Output JSON:"
```

At state `IN_NAME_VALUE`, the model assigns high probability to `fn_add_numbers` because the prompt made "addition" obvious. The constraint doesn't need to steer meaning — it just prevents the model from accidentally writing `fn_add_numberz` or something not in the list.

**This is different from DCCD**, where the model generates a full free draft first, *then* runs a second constrained pass on top of it. DCCD is a two-pass inference procedure. This project is single-pass with constraints active from the start.

DCCD's insight is still useful for understanding *why* masking works well here: because the prompt already loads the model with semantic intent before generation begins, the valid tokens happen to be the high-probability ones. The constraint isn't fighting the model — it's just formalising what the model already wanted to say.

## Connection

→ [[Willard & Louf (2023) — Outlines]]: Willard & Louf give the FSM vocabulary indexing mechanism. DCCD explains why applying constraints on top of a semantically informed generation process (like this project's prompt-conditioned generation) works better than blind per-token masking.
