---
title: "Function Calling"
type: concept
tags: [llm, tool-use, api, structured-output]
source_count: 2
---

## Definition

LLM capability to generate structured arguments (typically JSON) for invoking external tools or APIs. The model receives a tool schema describing available functions, their parameters, and valid values, then produces a syntactically and semantically valid call. The structured output is parsed and executed by the application layer.

## Key Properties

- Requires **precise format**: malformed JSON or invalid parameter values cause runtime failures
- **Schema-defined**: tool schemas (JSON Schema) specify function names, argument names, types, enums
- **Failure modes without enforcement**: hallucinated function names, wrong argument types, out-of-enum values, malformed JSON — all possible with prompt-only approaches
- **Enforcement options**: prompt engineering (soft, unreliable) vs [[constrained-decoding]] (hard, guaranteed structure)

## Examples

- OpenAI function calling API: model generates JSON tool call, application executes it
- [[fantase]] (EMNLP 2024): Token Search Trie makes hallucinating function names/enum values structurally impossible
- Any agentic pipeline where LLM decides which tool to call and with what arguments

## Connections

- [[constrained-decoding]] — the hard enforcement mechanism; guarantees schema-valid function calls
- [[logit-masking]] — underlying mechanism making invalid tokens impossible
- [[fantase]] — research paper specifically targeting faithful function call generation
- [[rag]] — sometimes combined: RAG retrieves context, function calling acts on it

## Open Questions

- How does constrained function calling interact with multi-step / multi-turn tool use?
- When does enforcing the schema reduce model autonomy in beneficial ways (e.g. preventing hallucination) vs harmful ways (e.g. preventing a creative but valid call)?
