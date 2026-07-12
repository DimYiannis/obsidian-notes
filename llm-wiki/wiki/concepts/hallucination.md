---
title: "Hallucination"
type: concept
tags: [llm, failure-mode, alignment, factuality]
source_count: 2
---

## Definition

When an LLM generates confident, fluent, false information. The model doesn't "know" it's wrong — it's sampling the statistically likely continuation of its training distribution, which always involved confident answers to factual questions.

## Root cause

[[supervised-fine-tuning]] training set: human labelers wrote confident answers ("Tom Cruise is an American actor...") for every "Who is X?" question. Model learns the *style* of a confident factual answer. At inference, for an unknown entity, it produces a confident fabrication — because that's what the training data looked like.

Internal state: some neuron likely represents "I'm uncertain" but it was never wired to produce "I don't know" in text. SFT didn't include examples of uncertainty.

## Mitigation 1: Self-knowledge interrogation

1. Take a document from training data → generate factual questions + correct answers (LLM-assisted)
2. Interrogate the model on those questions (ask 3–5 times)
3. If model answers consistently correctly → it knows
4. If model answers inconsistently or wrongly → it doesn't know
5. Add "I don't know" response examples for unknown facts to training set
6. Model learns to associate internal uncertainty neuron → "I'm sorry, I don't recall"

## Mitigation 2: Tool use (web search)

When model is uncertain, emit special tokens to trigger web search → retrieved text enters [[context-window]] → model reads and summarizes it. Knowledge in context window = working memory (direct, reliable) vs. parameters = vague recollection.

## Failure modes that look like hallucination but aren't

- **Spelling/counting errors**: caused by [[tokenization]] — model doesn't see characters, sees multi-char tokens. "How many R's in strawberry?" fails not from hallucination but from character-level blindness.
- **9.11 vs 9.9 failure**: reward model / training distribution artifact (Bible verse 9:11 comes after 9:9 → interfering association).
- **Finite compute per token**: asking for answer in one token → arithmetic fails → wrong answer that *looks* like hallucination.

## Connections

- [[supervised-fine-tuning]] — root cause; labelers wrote confident answers throughout
- [[base-model]] — base models hallucinate freely; mitigations applied in post-training
- [[context-window]] — tool use puts retrieved facts directly in context → more reliable than parameter recall
- [[function-calling]] — tool use mechanism for grounding model in real data
- [[tokenization]] — some apparent hallucinations are actually tokenization artifacts
- [[chain-of-thought]] — helps catch errors mid-sequence via self-checking
- [[retrieval-augmented-generation]] — systematizes Mitigation 2: retrieval puts corpus text in context so answers are [[grounding|grounded]] instead of recalled from parameters

## Open Questions

- At what scale of SFT data does self-knowledge become reliable enough to eliminate most hallucination?
- How does [[reinforcement-learning-llm]] on verifiable domains affect hallucination in unverifiable ones?
