# Function Calling in LLMs — Xe Iaso
[xeiaso.net/talks/2024/llm-function-calling/](https://xeiaso.net/talks/2024/llm-function-calling/)

---

## The key demystification

**Function calling is a classification task, not actual function execution.** The model generates text (JSON) that the surrounding system interprets and executes. The model has no agency over what happens next.

## What the model actually does

Recognises statistical patterns from training data. Scaled up autocomplete. It doesn't "know" what a function does — it learned that certain prompt patterns correspond to certain JSON output patterns.

## Three core capabilities of LLMs

1. **Classification** — categorising input into intents or tool selections
2. **Summarisation** — condensing provided information
3. **Fabrication** — generating plausible responses from learned patterns (hallucinations are misaligned fabrications)

## Technical mechanics

ChatML encoding + special tokens. When the model outputs a tool-call token followed by JSON parameters, the runtime parses it, executes the actual tool, feeds results back for response generation.

## The right mental model

"A bunch of unintelligent components working together to create the illusion of intelligence." System design around the model matters more than the model's inherent capabilities. Understand limitations to leverage strengths.
