← [[00 Reading Guide]]

# Beyond Free-Form Text: How Constrained Decoding is Reshaping Structured Generation
[medium.com/@brijeshrn/...](https://medium.com/@brijeshrn/beyond-free-form-text-how-constrained-decoding-is-reshaping-structured-generation-in-llms-5f7a38bef259)

---

## Landscape overview

Four main approaches:

| Approach | How | Used for |
|---|---|---|
| Grammar-based filtering | FSMs reject invalid tokens at generation (XGrammar) | JSON, code, structured output |
| Iterative backtracking (IterGen) | Store parse states, fix errors without full regeneration | Interactive applications, code co-pilots |
| Black-box sketch-guided | Generate unconstrained draft, refine against schema | Proprietary APIs without logit access |
| Semantic monitoring | Static analysis: type-checking, scoping | Production code generation |

## Speed vs structure — the tradeoff resolved

Modern engines (XGrammar) achieve near-zero overhead while maintaining validity. The classical tradeoff has collapsed.

## The crucial caveat

Rigid formats hurt reasoning-heavy tasks but excel in classification tasks. Recommendation: **hybrid strategies** — free-form reasoning field paired with constrained structured output.

## Practical shift

Constrained decoding has moved from research curiosity to production infrastructure. JSONSchemaBench now lets engineers evaluate engines systematically rather than speculatively.

## Subword alignment problem

Mentioned explicitly — the challenge of matching character-level grammar constraints to multi-character token generation. Direct relevance to `vocabulary.py` prefix matching.
