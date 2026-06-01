---
title: "Function Calling"
type: concept
tags: [llm, tool-use, api, structured-output]
source_count: 3
---

## Definition

LLM capability to generate structured arguments (typically JSON) for invoking external tools or APIs. The model receives a tool schema describing available functions, their parameters, and valid values, then produces a syntactically and semantically valid call. The structured output is parsed and executed by the application layer.

## Key Properties

- Requires **precise format**: malformed JSON or invalid parameter values cause runtime failures
- **Schema-defined**: tool schemas (JSON Schema) specify function names, argument names, types, enums
- **Failure modes without enforcement**: hallucinated function names, wrong argument types, out-of-enum values, malformed JSON — all possible with prompt-only approaches
- **Enforcement options**: prompt engineering (soft, unreliable) vs [[constrained-decoding]] (hard, guaranteed structure)

## Quantified impact of constrained decoding

Empirical result from a 0.6B parameter model (Qwen3-0.6B):

| Approach | JSON validity |
|---|---|
| Unconstrained (prompt only) | ~30% |
| Constrained decoding (JSON state machine) | 100% |

Structural reliability is determined by the decoding mechanism, not model size. A tiny model with [[constrained-decoding]] outperforms a large model on format compliance.

Function selection accuracy (~90%+) remains model-dependent — the constraint guarantees structure, not semantic correctness.

## Two levels of schema compliance

[[constrained-decoding]] for function calling enforces both simultaneously:
1. **Syntactic**: valid JSON structure throughout the generation
2. **Semantic**: function name must be from the known function set; argument keys must match the definition; argument types must match the schema (number, string, boolean)

## Examples

- OpenAI function calling API: model generates JSON tool call, application executes it
- [[fantase]] (EMNLP 2024): Token Search Trie makes hallucinating function names/enum values structurally impossible
- "Call me maybe" project: Qwen3-0.6B + JSON state machine constrained decoding → 100% schema-compliant function calls
- Any agentic pipeline where LLM decides which tool to call and with what arguments

## Connections

- [[constrained-decoding]] — the hard enforcement mechanism; guarantees schema-valid function calls
- [[logit-masking]] — underlying mechanism making invalid tokens impossible
- [[fantase]] — research paper specifically targeting faithful function call generation
- [[rag]] — sometimes combined: RAG retrieves context, function calling acts on it

## Open Questions

- How does constrained function calling interact with multi-step / multi-turn tool use?
- When does enforcing the schema reduce model autonomy in beneficial ways (e.g. preventing hallucination) vs harmful ways (e.g. preventing a creative but valid call)?
