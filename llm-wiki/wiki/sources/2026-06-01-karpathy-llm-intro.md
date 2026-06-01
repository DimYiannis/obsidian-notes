---
title: "Intro to Large Language Models — Andrej Karpathy"
type: source
date_ingested: 2026-06-01
source_url: local
source_type: video
tags: [llm, training, pre-training, sft, reinforcement-learning, karpathy]
---

## Summary

Karpathy's comprehensive general-audience walkthrough of the full LLM training pipeline. Three stages: [[pre-training]] (internet text → [[base-model]]), [[supervised-fine-tuning]] (human labeler conversations → assistant), [[reinforcement-learning-llm]] (practice problems → thinking model). Covers LLM psychology ([[hallucination]], sharp edges, Swiss cheese capabilities), tool use, and the frontier of RL-trained reasoning models ([[deepseek-r1]], OpenAI o1/o3).

## Key Points

- **Three-stage pipeline**: [[pre-training]] → [[supervised-fine-tuning]] → [[reinforcement-learning-llm]]. Computationally: months on thousands of GPUs (pre-training) vs hours (SFT) vs moderate (RL).
- **[[base-model]]**: token autocomplete, internet statistical compressor. Not an assistant. 15T tokens for LLaMA 3.
- **SFT programs by example**: human labelers follow labeling instructions → write ideal responses → model imitates. "You're talking to a statistical simulation of a human data labeler."
- **[[context-window]] = working memory; parameters = vague long-term recollection**. Paste source text directly for better results than relying on model memory.
- **Finite compute per token**: fixed forward pass per token → can't cram reasoning into one token. Explains why step-by-step > direct answer, and why spelling/counting fail.
- **RL on verifiable domains = real magic**: models discover [[chain-of-thought]], backtracking, self-checking — emergent, not taught. [[deepseek-r1]] first public paper. [[alphago]] analogy.
- **[[rlhf]] ≠ RL**: reward model is gameable. Run only ~few hundred steps. "RLHF is a little fine-tune, not magic."
- **[[hallucination]] mitigation**: interrogate model's self-knowledge → add "I don't know" examples. Plus tool use (web search) for factual grounding.
- **Swiss cheese capabilities**: PhD-level math, but can't count R's in "strawberry". [[tokenization]] + finite-compute-per-token explain most failures.

## Entities

- [[andrej-karpathy]] — author and presenter
- [[deepseek-r1]] — paper that publicly revealed RL training details for LLMs
- [[alphago]] — DeepMind's Go AI; RL analogy for surpassing human performance

## Concepts

- [[llm-training-pipeline]] — the three-stage overview
- [[pre-training]] — stage 1: internet text → base model
- [[supervised-fine-tuning]] — stage 2: labeler conversations → assistant
- [[base-model]] — output of pre-training; token autocomplete
- [[tokenization]] — text → token sequence via BPE
- [[reinforcement-learning-llm]] — stage 3: verifiable domain RL → thinking models
- [[rlhf]] — RL from human feedback; gameable, limited
- [[chain-of-thought]] — emergent reasoning strategy discovered via RL
- [[hallucination]] — LLM fabrication; causes and mitigations
- [[context-window]] — working memory; directly accessible vs parameter recollection

## Quotes

> "The base model is basically an internet document simulator on the token level."

> "What are you talking to in ChatGPT? A statistical simulation of a human data labeler following OpenAI's labeling instructions."

> "Knowledge in the parameters is vague recollection. Knowledge in the context window is working memory."

> "RLHF is not RL. RLHF is a little fine-tune that slightly improves your model."

> "Models need tokens to think. Distribute your computation across many tokens."
