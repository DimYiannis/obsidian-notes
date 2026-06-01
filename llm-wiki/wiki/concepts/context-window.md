---
title: "Context Window"
type: concept
tags: [llm, inference, memory, architecture]
source_count: 2
---

## Definition

The finite token sequence a model can process in a single forward pass. Everything inside the context window is directly accessible to the model — "working memory." Everything outside must be recalled from parameters — "vague long-term recollection." Size measured in tokens.

## Key Properties

- **Direct access**: tokens in context feed directly into the neural network → model can reference them precisely
- **Parameters = long-term memory**: knowledge learned at pre-training → vague recollection, subject to [[hallucination]]
- **Context > parameters for accuracy**: pasting source text directly into context produces more accurate answers than relying on model memorization — same reason you'd reread a chapter before summarizing it
- **Precious, finite resource**: [[tokenization]] trades character sequence length for token sequence length; context window length limits how much reasoning ([[chain-of-thought]]) can fit
- **Scales**: GPT-2 (2019) — 1,024 tokens; modern models — 128k–1M+ tokens
- **System message**: hidden tokens prepended by the provider (identity, persona, instructions) — invisible to user but in context
- **Approximate text sizes**: 8k tokens ≈ 5–10 pages; 32k ≈ 25–50 pages; 128k ≈ 100–300 pages; 1M ≈ thousands of pages
- **Effective context**: better [[tokenization]] (fewer tokens per word) → more actual text in same token budget. Context window size stays fixed; readable content increases.

## Practical implications

| Scenario | Better approach |
|---|---|
| Summarize a book chapter | Paste chapter into context, don't rely on parameter recall |
| Factual Q about rare person | Use web search tool to pull text into context |
| Complex arithmetic | Ask model to use code interpreter (result enters context) |
| Long reasoning problem | Allow [[chain-of-thought]] — tokens think, context fills up |

## Connections

- [[pre-training]] — establishes max context length; trained on windows up to that size
- [[tokenization]] — context length measured in tokens; vocab size affects how much text fits per token
- [[embeddings]] — all tokens in the context window are embedded before entering the Transformer
- [[chain-of-thought]] — thinking traces occupy context; longer reasoning = more context consumed
- [[hallucination]] — mitigation: put facts in context rather than relying on parameter recollection
- [[function-calling]] — tool results (web search, code output) enter context window
- [[rlhf]] — [[reinforcement-learning-llm]] both operate within context window during rollout generation

## Open Questions

- At what context length does in-context retrieval become equivalent to RAG over external store?
- How does test-time training (updating parameters from context observations) change this memory model?
