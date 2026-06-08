---
title: "Constrained Decoding"
type: concept
tags: [llm, inference, structured-output, function-calling]
source_count: 4
---

## Definition

Inference-time technique that guarantees structurally valid LLM output by inserting a logit processor between the model's raw output distribution and the sampling step. The processor tracks position in a target grammar and applies [[logit-masking]] — invalid tokens set to −∞, zero probability after softmax, never sampled. No model weight changes required; works as an output-side wrapper around any autoregressive LLM.

Two variants exist — this page covers **standard constrained decoding** (single-pass, constraints from token 1). See [[draft-conditioned-decoding]] for the two-pass variant that separates semantic planning from structural enforcement.

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

## JSON parse state machine

For JSON-structured output, the grammar tracker is a lightweight **JSON parse state machine** that knows what is valid at each position without fully parsing the incomplete output. States include:

- `expecting_key` — inside an object, waiting for a quoted key
- `inside_string_value` — inside a string; most tokens valid but closing quote ends the state
- `expecting_number` — only digit tokens, `-`, `.` are valid
- `expecting_function_name` — only tokens that are valid prefixes of known function names

Named states for function-calling JSON output:

```
START               → expect {
EXPECT_KEY          → expect "name" or "parameters"
IN_NAME_VALUE       → expect one of the known function names
EXPECT_COLON        → expect :
IN_PARAMETERS       → expect { then argument keys
IN_ARG_KEY          → expect one of the known arg names for chosen function
IN_ARG_VALUE        → expect value of correct type (string / number / boolean)
EXPECT_COMMA_OR_END → expect , or }
DONE
```

The mask is **position-aware**, not global. The valid token set changes at every generation step depending on current parse state. This simultaneously enforces:
1. **Syntactic compliance** — valid JSON structure throughout
2. **Semantic compliance** — function name from known set, argument keys matching definition, types matching schema (number, string, boolean)

Empirical result: unconstrained 0.6B model (Qwen3-0.6B) → ~30% valid JSON. Same model with JSON state machine constrained decoding → **100%** schema-compliant output. Reliability from structural guidance, not model size.

## Fine-tuning vs. decode-time enforcement

Zhang et al. (2023) — *Don't Fine-Tune, Decode* — make the empirical case:

- Fine-tuning teaches syntax **implicitly** → models still produce frequent errors
- Decode-time constraints enforce syntax **explicitly** → zero syntax errors

**TOOLDEC**: FSM-based constrained decoding for tool/function call syntax. Applied to Mistral-Instruct: tool use accuracy **0% → 52%**. Outperforms specialized fine-tuned models with no training required.

Implication: structural reliability is a **decoding problem**, not a training problem. Enforcement timing determines reliability — not model size or training budget.

## Greedy decoding under constraint

When invalid tokens are already masked to −∞, temperature/sampling adds noise without benefit — it can only cause the model to pick a suboptimal token from the remaining valid set. **Greedy decoding is the optimal sampling strategy for constrained structured output.**

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
- [[draft-conditioned-decoding]] — the two-pass variant; solves the projection tax problem of standard constrained decoding
- [[function-calling]] — primary application
- [[token-character-mismatch]] — engineering challenge in implementation
- [[rag]] — orthogonal technique; RAG governs knowledge retrieval, constrained decoding governs output structure; can combine
- [[outlines-library]], [[xgrammar]], [[llguidance]], [[fantase]] — concrete implementations

## Open Questions

- Grammar-Aligned Decoding (Feng et al., 2024): logit masking distorts token probabilities — ASAp method attempts to fix this. Worth a dedicated page?
- IterGen (ICLR 2025): adds backtracking via forward/backward grammar-symbol generation. Useful for cases where greedy constrained generation gets stuck?
- At what schema complexity does XGrammar overhead become non-negligible?
