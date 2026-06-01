# Call me maybe — Function Calling in LLMs

*Source: personal project by user (DimYiannis), GitHub. 2026-06-01. Immutable.*

Function calling system for LLMs using constrained decoding — translates natural language prompts into structured, schema-validated JSON function calls using a 0.6B parameter model (Qwen3-0.6B).

Core result: unconstrained model produces valid JSON ~30% of the time. With constrained decoding → 100% schema-compliant output.

Algorithm: at each generation step, track current JSON parse state → find valid token continuations → mask invalid tokens to -inf → sample from remaining valid tokens.

Enforces syntactic JSON validity AND semantic schema compliance (function name must be known, arg keys must match definition, types must match).

Design decisions: Pydantic for validation, vocabulary JSON maps token IDs to strings, greedy decoding for constrained steps (sampling adds noise without benefit when invalid tokens already masked).

Challenges: subword tokenization (function names span multiple subword pieces → prefix-matching needed), parse state tracking (lightweight JSON state machine), type coercion.
