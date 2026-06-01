---
title: "RLHF (Reinforcement Learning from Human Feedback)"
type: concept
tags: [llm, training, post-training, alignment]
source_count: 1
---

## Definition

A technique for applying reinforcement learning to unverifiable domains (creative writing, summarization, joke quality) where no programmatic scoring function exists. Instead of scoring against a correct answer, a separate neural network — the **reward model** — is trained to simulate human preferences. RL is then run against this reward model. Introduced in InstructGPT paper (OpenAI, 2022).

## Key Properties

- **Reward model**: a Transformer trained on human preference orderings (rank 5 rollouts best→worst → gradient update). Outputs a score 0–1 for any (prompt, response) pair.
- **Indirection**: humans only need to rank outputs, not write ideal responses — easier task, less expert skill required (discriminator-generator gap)
- **Gameable**: reward model is a giant neural network → adversarial inputs exist → RL finds nonsensical inputs that score=1. "The the the" → reward model says excellent joke.
- **Run length limited**: ~few hundred steps before gaming begins; must stop and ship
- **Empirically improves models**: slight improvement from more human-aligned preference signal; reason is debated (probably discriminator-generator gap)

## Why RLHF ≠ real RL

Real [[reinforcement-learning-llm]] on verifiable domains:
- Scoring function = exact answer check → not gameable
- Can run thousands/millions of steps
- Discovers emergent strategies beyond human performance
- "Magic"

RLHF:
- Scoring function = gameable neural network
- Must stop early
- Slight style improvement only
- "A little fine-tune"

## Connections

- [[reinforcement-learning-llm]] — the real, scalable RL; RLHF is the weaker unverifiable-domain variant
- [[supervised-fine-tuning]] — RLHF usually follows SFT; same model, different training signal
- [[hallucination]] — RLHF can slightly reduce hallucinations but doesn't fix the root cause
- [[chain-of-thought]] — SFT/RLHF models don't exhibit true chain-of-thought; that's an RL phenomenon

## Open Questions

- Can better reward models (more robust to adversarial inputs) extend RLHF run length significantly?
- Constitutional AI (Anthropic) attempts to replace human raters with an LLM "constitution" — does this avoid the gaming problem?
