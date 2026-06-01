---
title: "Supervised Fine-Tuning (SFT)"
type: concept
tags: [llm, training, post-training, alignment]
source_count: 1
---

## Definition

Stage 2 of the LLM training pipeline. Takes a [[base-model]] and continues training on a curated dataset of human-written conversations (prompt + ideal response pairs). Output: an assistant that imitates the response style and knowledge of the labelers. Algorithmically identical to [[pre-training]] — same loss, same update rule — only the dataset changes: internet text → conversations.

## Key Properties

- **Data**: millions of conversations across diverse topics; human-written prompts + ideal assistant responses
- **Labelers**: professional human contractors (via Upwork, Scale AI, etc.) trained on labeling instructions
- **Labeling instructions**: company-authored documents (hundreds of pages) specifying how to be helpful, truthful, harmless. The model's "personality" comes from these.
- **Cost**: hours to days (vs. months for pre-training); dataset is tiny compared to internet corpus
- **Modern stack**: heavily synthetic — LLMs now help generate conversations; humans curate/edit rather than write from scratch (e.g. UltraChat dataset)
- **Output**: model that statistically imitates human labeler behavior

## What you're talking to in ChatGPT

> "A statistical simulation of a human data labeler following OpenAI's labeling instructions."

The model doesn't have opinions or knowledge beyond what was in pre-training + what labelers wrote. Responses are statistically consistent with labeler style on the given prompt.

## Limitations

- Labelers can only write solutions they understand — model can't discover better strategies than its teachers
- Human cognition ≠ LLM cognition: what's easy/hard for a labeler may not match what's easy/hard for the model (see [[chain-of-thought]])
- Can't go beyond human performance — ceiling is the labelers' capability
- Requires [[rlhf]] or [[reinforcement-learning-llm]] to push further

## Connections

- [[pre-training]] — stage 1; SFT starts from the base model it produces
- [[reinforcement-learning-llm]] — stage 3; takes SFT model further via trial-and-error
- [[rlhf]] — variant of RL fine-tuning for unverifiable domains
- [[hallucination]] — SFT-trained models hallucinate because labelers wrote confident answers even for unknown facts
- [[chain-of-thought]] — SFT can inject reasoning steps, but only human-style ones; RL discovers better ones
- [[llm-training-pipeline]] — where SFT fits

## Open Questions

- At what point does the SFT labeling instruction quality matter more than quantity of conversations?
- How does synthetic data generation (LLM-assisted) change the ceiling compared to purely human-written SFT?
