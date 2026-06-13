---
title: "Chain-of-Thought"
type: concept
tags: [llm, reasoning, reinforcement-learning, inference]
source_count: 2
---

## Definition

Extended reasoning traces in LLM responses where the model distributes computation across many tokens before reaching an answer. In [[reinforcement-learning-llm]]-trained models, chain-of-thought emerges spontaneously — the model discovers that longer, exploratory token sequences reliably produce correct answers. Not explicitly taught; no human labeled these reasoning paths.

## Key Properties

- **Emergent from RL**: models trained on verifiable domains discover backtracking, self-checking, multi-perspective approaches on their own
- **Not human-authored**: RL models find reasoning strategies that work *for the model* — may differ from human solution paths
- **Finite compute per token constraint**: each token can only spend a fixed amount of computation (one Transformer forward pass). Chain-of-thought distributes hard reasoning across many tokens so no single token carries too much work
- **"Tokens to think"**: "don't ask model to answer in one token" — cram all computation → wrong answer; spread it out → correct answer
- **Visible thinking**: DeepSeek R1 shows full chain-of-thought. OpenAI o1/o3 hide it (distillation risk concern) and show summaries instead
- **Response length increases**: models trained with RL generate longer responses — not padding, but actual thinking

## Examples of emergent strategies

From DeepSeek R1:
- "Wait wait wait — that's not right. Let me reevaluate step by step."
- Trying the same problem from multiple angles
- Checking results against an alternative method
- Explicitly flagging uncertainty before committing to answer

## Connections

- [[reinforcement-learning-llm]] — the training process that produces chain-of-thought
- [[supervised-fine-tuning]] — SFT can include step-by-step solutions written by humans, but these are human-style, not model-optimal
- [[deepseek-r1]] — paper demonstrating and analyzing emergent chain-of-thought from RL
- [[hallucination]] — chain-of-thought reduces hallucination in reasoning tasks by distributing work; model can self-correct mid-sequence
- [[context-window]] — chain-of-thought occupies context window; longer thinking = more tokens consumed
- [[induction-heads]] — an in-context-learning mechanism; prompt-based reasoning behaviors build on the same substrate

## Open Questions

- Do chain-of-thought strategies discovered in math/code transfer to unverifiable domains?
- What is the optimal "thinking budget" per problem difficulty?
- Could models develop non-English or non-natural-language internal reasoning tokens?
