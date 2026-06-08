---
title: "Draft-Conditioned Decoding (DCCD)"
type: concept
tags: [llm, inference, constrained-decoding, structured-output]
source_count: 1
---

## Definition

A two-pass, training-free inference procedure that improves on standard [[constrained-decoding]] by separating semantic planning from structural enforcement. First generate an unconstrained draft (the model plans freely). Then run constrained decoding conditioned on that draft (enforce form on a semantically coherent plan). Introduced by Reddy et al. (2025).

## The problem it solves

Standard constrained decoding applies [[logit-masking]] at every token step regardless of context. When the model's probability mass falls mostly on invalid tokens, the mask forces a valid token anyway — this is the **projection tax**: form is preserved but meaning gets distorted.

Example: model wants to write `fn_compute` (not in the valid set) → constraint forces it to pick `fn_add_numbers` → structurally valid but semantically wrong. Standard decoding can't distinguish "model was going to say the right thing in a roundabout way" from "model was about to hallucinate."

## Mechanism

```
Step 1 — Draft (unconstrained):
  Generate full output freely.
  Result: semantically meaningful, possibly syntactically invalid.
  e.g. {"name": "fn_add_numbers", "parameters": {"a": 40, "b": 2}}  ← correct intent

Step 2 — Constrained pass (conditioned on draft):
  Run constrained decoding with the draft as context.
  Model already "knows" where it's going → probability mass on valid tokens is high.
  Constraint enforces form without fighting the model's intent.
  Result: structurally guaranteed AND semantically correct.
```

## Why the projection tax is lower

Conditioning on the draft shifts the model's distribution toward valid continuations before masking applies. The mask now mostly agrees with what the model already wanted to output — it's confirming a plan rather than overriding one.

## Optional: best-of-K

Generate K unconstrained drafts → select best → apply constrained pass on it. Better output quality at K× inference cost.

## Contrast with standard constrained decoding

| | Standard constrained decoding | DCCD |
|---|---|---|
| Passes | Single | Two (draft + constrained) |
| When constraints apply | Every token from step 1 | Second pass only |
| Semantic guidance | Prompt only | Prompt + unconstrained draft |
| Projection tax | Higher (can distort semantics) | Lower (draft aligns distributions) |
| Inference cost | 1× | 2× (or K+1× with best-of-K) |
| Training required | No | No |

## Key result

GSM8K mathematical reasoning, 1B model: **15.2% → 39.0%** (+24 percentage points). Small model matches much larger constrained baselines.

## When to use which

- **Standard constrained decoding**: prompt already provides strong semantic context (e.g. function calling with clear user intent) — the model's distribution naturally favors valid tokens, so projection tax is low anyway. Lower inference cost.
- **DCCD**: complex structured generation where the model struggles to simultaneously plan semantics and satisfy constraints — especially smaller models on harder tasks.

## Connections

- [[constrained-decoding]] — the standard technique DCCD extends; they share the same [[logit-masking]] mechanism
- [[logit-masking]] — used in both; DCCD reduces how often it distorts the model's intended distribution
- [[function-calling]] — primary application domain for both techniques
