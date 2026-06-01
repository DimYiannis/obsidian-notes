# Thinking Before Constraining: A Unified Decoding Framework
**Nguyen et al. (2025)** · [arxiv.org/pdf/2601.07525](https://arxiv.org/pdf/2601.07525)

---

## Core idea

**In-Writing** — a hybrid decoding approach that decouples reasoning from formatting. The model generates unconstrained reasoning freely, then applies structured decoding only after a trigger token is generated.

## The problem it solves

Applying constraints too early interrupts the model's reasoning process ("premature triggering"). But fully unconstrained output lacks verifiable structure. In-Writing threads this needle: reason freely, then constrain the output format.

## Key finding

Up to **27% accuracy improvement** over purely natural generation on classification and reasoning tasks.

## Connection to this project

Validates the hybrid pattern already in the README: free-form `reasoning` field (unconstrained) before the constrained `function_call` field. The model thinks first, commits to structure second.

```json
{
  "reasoning": "<unconstrained>",
  "function_call": { "name": "<constrained>", "args": { "<constrained>" } }
}
```
