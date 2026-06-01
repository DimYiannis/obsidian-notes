# Function Calling for LLMs Using Constrained Decoding

*Source: pasted by user 2026-06-01. Immutable.*

---

## What problem does it solve?

Modern LLM applications often involve invoking external tools or APIs, which requires the LLM to generate arguments in a precise format (usually JSON) that the tool can understand. Relying on regexes or custom parsing logic after free-form generation is brittle, error-prone, and adds latency. Constrained decoding solves this at the source.

---

## The core mechanism: logit masking

Constrained decoding inserts a **logit processor** between the model's output and the sampling step. This processor tracks position within the target grammar and masks invalid tokens by setting their logits to **−∞**. After masking, softmax is applied — so invalid tokens get zero probability and can never be sampled.

A language model produces a probability distribution over its entire vocabulary (32k–128k tokens). It has no built-in knowledge of structural requirements and might assign high probability to tokens that would break the desired format. Token masking is the solution and forms the foundation of all constrained decoding techniques.

### Pipeline (per token, autoregressively):

```
User prompt + tool schema
        ↓
LLM forward pass → raw logit vector (one score per vocab token)
        ↓
Logit processor (grammar mask)
  - Sets invalid token logits to −∞
  - Valid tokens determined by current grammar state (FSM / PDA)
        ↓
Softmax + sampling
  - Only valid tokens get non-zero probability
        ↓
Token emitted (always schema-valid)
        ↓ (loop until EOS)
Final: guaranteed valid function call JSON
```

---

## Grammar representations

Two main approaches track which tokens are valid at each decoding step:

### 1. Finite-State Machines (FSM)
- Used by **Outlines** (pioneering library)
- Pipeline: JSON Schema → regex → FSM
- Fast state tracking, simple implementation
- **Limitation**: struggles with recursive structures (nested JSON objects, variable-length arrays)

### 2. Context-Free Grammars (CFG) / Pushdown Automata (PDA)
- Used by **XGrammar** and **llguidance**
- JSON Schema → EBNF grammar → PDA
- Handles recursive structures natively
- Higher state management overhead vs FSM
- Both approaches ultimately perform **logit masking**: disallowed tokens get −∞ → probability 0 after softmax

---

## Engineering challenges & modern solutions

### The batching problem
Compiling an FSM per request and computing the mask synchronously blocks all requests in a batch, causing high time-to-first-token (TTFT). FSM compilation is expensive.

### XGrammar solution
- Moves grammar compilation out of Python into **C++ with pthreads**
- Uses **pushdown automaton** for batch constrained decoding
- Default backend for **vLLM, SGLang, TensorRT-LLM** (as of 2026)
- **<40 µs/token**, near-zero overhead on JSON generation

### llguidance (Microsoft)
- Rust-based **Earley parser**
- ~50 µs/token, negligible startup cost
- OpenAI credited it as foundational for their Structured Outputs feature

---

## Application to function calling: FANTASE (EMNLP 2024)

**Paper**: *FANTAstic SEquences and Where to Find Them: Faithful and Efficient API Call Generation through State-tracked Constrained Decoding and Reranking*  
**Venue**: EMNLP 2024 Findings (Amazon Science)  
**Code**: https://github.com/amazon-science/faithful_and_efficient_api_call_generation

### Key contributions

**1. State-Tracked Constrained Decoding (SCD)**
- API documentation (function names, parameter names, legal enum values) is preprocessed into a **Token Search Trie**
- At each decoding step, only tokens reachable from the current trie state are unmasked
- Guarantees the model cannot hallucinate a function name that doesn't exist, or a parameter value outside allowed enums
- No modification to model weights; works as an output-side wrapper around any autoregressive LLM

**2. Reranking component**
- Beam search generates N candidate function call sequences (constrained)
- A lightweight **RoBERTa-based discriminator** reranks candidates using supervised signal
- With regular beam search, the correct generation was only ranked 5th on average; reranking pushes it to rank 1

### Why this matters vs. prompt engineering
Prompt engineering *asks* the model to follow a format. SCD *enforces* it at the token probability level. A prompt-engineered model can still emit `"cuisine": "Thai"` when only `["American", "Chinese", "Indian", "Italian", "Mexican"]` are valid. SCD makes that impossible.

---

## The token-character mismatch problem

Grammar constraints operate at the **character level**, but LLMs generate **multi-character tokens**. A single token like `"name"` (including the quote) must be validated against a grammar that thinks in individual characters.

Solution: pre-compute, for every token in the vocabulary, which grammar states it can legally appear in. This is done **once at schema compilation time** and cached — the per-token masking cost at inference time is then just a lookup (~40 µs).

---

## Constrained decoding vs. reasoning quality

**"Let Me Speak Freely?" (EMNLP Industry 2024)** shows that rigid output formats can hurt reasoning-heavy tasks (math, multi-hop QA) but help classification tasks (slot-filling, intent detection).

**Recommended pattern (used in production)**:
```json
{
  "reasoning": "<free-form scratchpad, unconstrained>",
  "function_call": {
    "name": "<constrained to valid function names>",
    "args": { "<constrained to valid schema>" }
  }
}
```
Place the free-form reasoning field *before* the constrained fields so the model thinks before committing to a structured answer.

---

## Ecosystem comparison

| Engine | Grammar approach | Used by | Per-token latency |
|---|---|---|---|
| **Outlines** | FSM (regex/JSON→FSM) | Early vLLM | ~40s+ compile on complex schemas |
| **XGrammar** | CFG/PDA (C++ compiled) | vLLM, SGLang, TensorRT-LLM | <40 µs |
| **llguidance** | Earley parser (Rust) | OpenAI Structured Outputs | ~50 µs |
| **FANTASE** | Trie-based SCD + reranking | Research / Amazon | Function-call specialized |
| **Guidance** | Mixed | Microsoft ecosystem | Variable |

---

## Key papers & resources

| Paper | Venue | Notes |
|---|---|---|
| FANTASE (Wang et al.) | EMNLP 2024 | State-tracked constrained decoding for API calls |
| XGrammar (Dong et al.) | Preprint 2024 | High-performance CFG engine |
| Grammar-Aligned Decoding (Feng et al.) | Preprint 2024 | Fixes probability distortion from masking (ASAp method) |
| IterGen (Li et al.) | ICLR 2025 | Backtracking via forward/backward grammar-symbol generation |
| "Let Me Speak Freely?" | EMNLP Industry 2024 | Format restrictions hurt reasoning; recommends hybrid |
| JsonSchemaBench | Preprint 2025 | 10k-schema benchmark across Outlines, XGrammar, llama.cpp, OpenAI, Gemini |
| Outlines (Willard & Louf) | Preprint 2023 | FSM-based constrained generation, foundational library |

---

## Summary: genuine constrained decoding vs. prompt engineering

| | Prompt engineering | Constrained decoding |
|---|---|---|
| **Enforcement** | None — model can ignore | Hard — invalid tokens get p=0 |
| **Guarantee** | No | Yes (schema-valid output) |
| **Mechanism** | System prompt / few-shot | Logit masking at inference time |
| **Model weights** | Unchanged | Unchanged |
| **Overhead** | Zero | ~40–50 µs/token (negligible) |
| **Failure mode** | Hallucinated fields, wrong types | None for structure (content still model-dependent) |
