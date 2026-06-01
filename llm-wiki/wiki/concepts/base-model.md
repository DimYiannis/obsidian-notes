---
title: "Base Model"
type: concept
tags: [llm, pre-training, inference]
source_count: 1
---

## Definition

The output of [[pre-training]]. A Transformer neural network whose parameters encode statistical patterns of internet text. Functions as a token autocomplete: given a sequence of tokens, predicts what comes next. Not an assistant — has no concept of questions and answers, just continues whatever token sequence it receives.

## Key Properties

- **Token autocomplete**: samples next token from predicted distribution, appended to context, repeats
- **Stochastic**: same prompt yields different completions each time (sampling from probability distribution)
- **Lossy internet compressor**: parameters ≈ a "zip file of the internet" — vague recollection, not exact recall
- **Can memorize**: high-quality, frequently seen documents (e.g. Wikipedia) may be recited nearly verbatim
- **Can hallucinate**: rare or post-cutoff facts → confident fabrication (see [[hallucination]])
- **Knowledge cutoff**: only knows what was in training data; no awareness of events after cutoff
- **Rarely released**: most companies only release fine-tuned assistant versions; exceptions: GPT-2 (2019), LLaMA 3 (Meta), DeepSeek

## Prompting a base model

Few-shot prompts that look like internet documents can elicit knowledge:
- "Here are my top 10 landmarks in Paris: 1." → model continues the list
- Translation pairs → model continues the pattern (in-context learning)
- Structured conversation format → model takes on assistant role

## Connections

- [[pre-training]] — produces the base model
- [[supervised-fine-tuning]] — transforms base model into assistant
- [[context-window]] — base model's "working memory"; what's in context is directly accessible
- [[hallucination]] — base models hallucinate freely; no mitigation applied yet
- [[tokenization]] — base model operates entirely on token sequences

## Open Questions

- How much of a base model's capability is recoverable via prompting alone vs. requiring SFT?
