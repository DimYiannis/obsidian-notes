---
title: "Constrained Decoding"
type: concept
tags: [llm, inference, structured-output, function-calling]
source_count: 1
---

## Definition

Inference-time technique that guarantees structurally valid LLM output by inserting a logit processor between the model's raw output distribution and the sampling step. The processor tracks position in a target grammar and applies [[logit-masking]] — invalid tokens set to −∞, zero probability after softmax, never sampled. No model weight changes required; works as an output-side wrapper around any autoregressive LLM.

## Key Properties

- **Hard structural guarantee**: schema-invalid tokens are impossible to emit, not just unlikely
- **Grammar-governed**: validity at each step determined by current state in an FSM or PDA
- **Autoregressively applied**: runs per token for the full generation
- **No weight changes**: model unchanged; constraint is entirely in the decoding pipeline
- **Negligible overhead**: ~40–50 µs/token with modern backends ([[xgrammar]], [[llguidance]])
- **Content is still model-dependent**: structure guaranteed; semantic correctness is not

## Grammar backends

| Backend | Approach | Per-token latency | Used by |
|---|---|---|---|
| [[outlines-library]] | FSM (JSON Schema → regex → FSM) | Slow compile on complex schemas | Early vLLM |
| [[xgrammar]] | CFG/PDA (C++, pthreads) | <40 µs | vLLM, SGLang, TensorRT-LLM |
| [[llguidance]] | Earley parser (Rust) | ~50 µs | OpenAI Structured Outputs |
| [[fantase]] | Token Search Trie | Function-call specialized | Research / Amazon |

**FSM** (Finite-State Machine): fast, simple, fails on recursive structures (nested JSON, variable arrays).
**CFG/PDA** (Context-Free Grammar / Pushdown Automaton): handles recursion natively; higher state overhead.

## Reasoning quality tradeoff

Rigid output formats hurt reasoning-heavy tasks (math, multi-hop QA) but help classification tasks (slot-filling, intent detection). Production fix:

```json
{
  "reasoning": "<free-form scratchpad, unconstrained>",
  "function_call": {
    "name": "<constrained>",
    "args": { "<constrained>" }
  }
}
```

Free-form field first → model thinks before committing to structure. Source: "Let Me Speak Freely?" (EMNLP Industry 2024).

## Examples

- [[function-calling]]: LLM generates JSON arguments for tools/APIs; constrained to schema
- Slot-filling: output constrained to valid slot values
- Classification: output constrained to label set
- Structured Outputs (OpenAI): product built on [[llguidance]]

## Connections

- [[logit-masking]] — the core mechanism inside constrained decoding
- [[function-calling]] — primary application
- [[token-character-mismatch]] — engineering challenge in implementation
- [[rag]] — orthogonal technique; RAG governs knowledge retrieval, constrained decoding governs output structure; can combine
- [[outlines-library]], [[xgrammar]], [[llguidance]], [[fantase]] — concrete implementations

## Open Questions

- Grammar-Aligned Decoding (Feng et al., 2024): logit masking distorts token probabilities — ASAp method attempts to fix this. Worth a dedicated page?
- IterGen (ICLR 2025): adds backtracking via forward/backward grammar-symbol generation. Useful for cases where greedy constrained generation gets stuck?
- At what schema complexity does XGrammar overhead become non-negligible?
