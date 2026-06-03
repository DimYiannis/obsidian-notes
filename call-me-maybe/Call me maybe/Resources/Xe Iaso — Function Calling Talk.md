← [[00 Reading Guide]]

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

## ChatML encoding (what that is)

**ChatML** is a formatting scheme used to represent conversations in a structured way for models.

Instead of raw text, messages look like roles:

- system
- user
- assistant
- tool (or function result)

So the model doesn’t just see:

> “What’s the weather in Amsterdam?”

It sees something like:

```
<|system|>You are a helpful assistant.<|user|>What’s the weather in Amsterdam?<|assistant|>
```

This structure helps the model understand:

> “who is speaking and what kind of action is expected”

---

## 2. Special tokens (the control markers)

Special tokens are reserved symbols like:

- `<|assistant|>`
- `<|tool_call|>`
- `<|end|>`

They are **not normal words**.

They act like:

> “switch modes now”

So when the model outputs a special token, it’s not just continuing text — it’s changing behavior.

---

## 3. Tool-call token + JSON parameters

At some point, instead of replying normally, the model might output something like:

```
{  "tool_call": "get_weather",  "arguments": {    "city": "Amsterdam"  }}
```

Or in ChatML-style:

```
<|assistant|><tool_call>{"name": "get_weather", "arguments": {"city": "Amsterdam"}}</tool_call>
```

This means:

> “Don’t answer this yourself. Call a function instead.”

---

## 4. What the runtime does (this is the important part)

The model does NOT execute anything.

It just emits text that _looks like a function call_.

Then the **runtime system** does the real work:

### Step-by-step:

1. Model outputs tool call token + JSON
    
    > “Call get_weather(Amsterdam)”
    
2. Runtime intercepts it
    
    > “Ah yes, this is not a sentence. This is an instruction.”
    
3. Runtime executes real function:

```
get_weather("Amsterdam")
```

4. Function returns real data:

```
{"temp": 12, "condition": "cloudy"}
```

5. Runtime feeds result back into the model as a new message:

```
<|tool_result|>{"temp": 12, "condition": "cloudy"}
```

6. Model continues generation:

> “The weather in Amsterdam is 12°C and cloudy…”

---

## 5. Why this architecture exists

Because LLMs:

- are good at deciding _what to do_
- are bad at actually doing it reliably

So we split responsibilities:

|Component|Role|
|---|---|
|LLM|decides intent + parameters|
|Runtime|executes real actions|
|Tools|actually fetch data / compute|

---

## 6. The key idea in one sentence

> The model doesn’t execute tools — it generates structured “tool call messages,” and the runtime system interprets and executes them, then feeds results back into the conversation.

---

## 7. Mental model (the useful one)

Think of it like:

- LLM = “smart dispatcher”
- runtime = “obedient execution layer”
- tools = “real-world APIs”

So instead of:

> “I think the weather is…”

it becomes:

> “Run this function and tell me the result”
## Connection

→ [[Willard & Louf (2023) — Outlines]]: Xe Iaso explains *what* function calling is — the model outputs structured text, the runtime executes it. Willard & Louf explains *how* to enforce that the structured text is always valid — FSM-based logit masking. Together they cover the full picture.

## The right mental model

"A bunch of unintelligent components working together to create the illusion of intelligence." System design around the model matters more than the model's inherent capabilities. Understand limitations to leverage strengths.
