---
title: "LLM Training Pipeline"
type: concept
tags: [llm, training, overview]
source_count: 1
---

## Definition

The three-stage process by which modern LLM assistants (ChatGPT, Claude, Gemini) are built. Each stage builds on the previous. Analogous to school: textbook exposition, worked examples, practice problems.

## The three stages

| Stage | Input | Output | Compute | School analogy |
|---|---|---|---|---|
| [[pre-training]] | Internet text (~15T tokens) | [[base-model]] | Months, thousands of GPUs, $millions | Reading textbook exposition |
| [[supervised-fine-tuning]] | Human-labeled conversations (~1M) | Assistant | Hours–days | Studying worked examples |
| [[reinforcement-learning-llm]] / [[rlhf]] | Practice problems + answer keys | Thinking model | Days–weeks | Doing practice problems |

## Stage 1: Pre-training

Internet text → [[tokenization]] → token sequences → train Transformer to predict next token → parameters encode statistical patterns of internet → [[base-model]].

## Stage 2: Supervised Fine-Tuning (SFT)

Human labelers follow labeling instructions → write ideal assistant responses → dataset of conversations → continue training base model → assistant that imitates labelers.

## Stage 3: Reinforcement Learning

- **Verifiable domains** (math, code): [[reinforcement-learning-llm]] — sample many solutions, score against answer, reinforce correct ones. Emergent [[chain-of-thought]]. Can run indefinitely. Real magic.
- **Unverifiable domains** (creative writing): [[rlhf]] — train reward model on human preference orderings, RL against reward model. Gameable. ~Few hundred steps. "A little fine-tune."

## Connections

- [[pre-training]] → [[supervised-fine-tuning]] → [[reinforcement-learning-llm]] — the sequence
- [[base-model]] — output of stage 1
- [[hallucination]] — introduced in stage 2; mitigated in stage 3
- [[chain-of-thought]] — emerges in stage 3 (verifiable RL)
- [[deepseek-r1]] — paper that detailed stage 3 publicly
- [[alphago]] — historical precedent for stage 3 magic: RL exceeds human performance

## Open Questions

- Will test-time training (updating parameters during inference) become a fourth stage?
- How does the pipeline change for multimodal models (audio, image tokens)?
