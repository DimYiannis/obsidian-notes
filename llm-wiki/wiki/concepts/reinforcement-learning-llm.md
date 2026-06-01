---
title: "Reinforcement Learning for LLMs"
type: concept
tags: [llm, training, post-training, reasoning, rl]
source_count: 1
---

## Definition

Stage 3 of the LLM training pipeline. Applied to verifiable domains (math, code, logic) where candidate solutions can be automatically scored against a known correct answer. The model generates many candidate solution sequences (rollouts), correct ones are reinforced via parameter updates. No human labels needed for the solutions — only for the problems and answers. Output: "thinking models" that exhibit emergent [[chain-of-thought]] reasoning.

## Key Properties

- **Verifiable domains only** (for magic RL): correct answer exists and can be checked programmatically — math answer matching, code execution, LLM judge
- **Trial-and-error**: sample N solutions per prompt → score all → reinforce correct ones → update parameters → repeat
- **Emergent reasoning**: models discover backtracking, self-checking, "wait let me reevaluate" strategies without being explicitly taught — these just happen to improve accuracy
- **Not constrained to human performance**: unlike [[supervised-fine-tuning]], model can discover strategies humans wouldn't think to write down (cf. [[alphago]] Move 37)
- **Scalable**: can run for thousands of steps; scoring function is not gameable (answer is either right or wrong)
- **Response length grows**: models learn to use more tokens to think → longer chains → higher accuracy
- **[[deepseek-r1]]**: first public paper detailing RL training for LLMs; reignited field interest in 2024

## Contrast with [[rlhf]]

| | RL (verifiable) | [[rlhf]] |
|---|---|---|
| Scoring | Exact answer check | Reward model (simulated human) |
| Gameable? | No | Yes |
| Run length | Thousands of steps | ~Hundreds before gaming |
| Output | "Real" reasoning | Slight style improvement |
| Magic? | Yes | No |

## School analogy

Pre-training = reading textbook exposition. SFT = studying worked examples. **RL = doing practice problems** — discovering solutions yourself, checking against answer key, learning what works for you specifically.

## Connections

- [[supervised-fine-tuning]] — SFT initializes the model; RL refines it
- [[chain-of-thought]] — the emergent reasoning strategy RL produces
- [[rlhf]] — the weaker variant for unverifiable domains
- [[deepseek-r1]] — paper that made RL training details public
- [[alphago]] — historical precedent: RL surpassed top human Go players; same paradigm
- [[llm-training-pipeline]] — stage 3

## Open Questions

- Does RL reasoning in verifiable domains (math/code) transfer to unverifiable domains (creative writing, summarization)?
- What is the LLM equivalent of AlphaGo's Move 37 — strategies no human would think of?
- What happens if you run RL indefinitely on very large, diverse problem distributions?
