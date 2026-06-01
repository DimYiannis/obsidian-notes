# Index

Content catalog. Updated on every ingest, query filing, or page deletion. Read this first when answering queries — then drill into relevant pages.

Hidden from Obsidian graph: `.raw/` (immutable sources), `.sources/` (intake summaries), `.schema/` (wiki methodology).
Visible in graph: `wiki/concepts/`, `wiki/entities/`, `wiki/synthesis/`, `wiki/meta/`.

---

## Sources (in `wiki/sources/` — visible in graph, linked to concepts/entities)

- 2026-06-01-constrained-decoding-function-calling — constrained decoding for LLM function calling; logit masking, FSM vs CFG/PDA, XGrammar, FANTASE
- 2026-06-01-karpathy-llm-intro — Andrej Karpathy's full LLM training pipeline walkthrough; pre-training, SFT, RL, hallucinations, sharp edges
- 2026-06-01-tokenization-embeddings-context — personal notes on tokens, embeddings, vocabulary tradeoff, effective context window
- 2026-06-01-call-me-maybe — personal project: function calling via constrained decoding; JSON state machine, 30%→100% validity, greedy decoding insight

---

## Entities

- xgrammar — CFG/PDA constrained decoding engine, C++, default in vLLM/SGLang/TensorRT-LLM (source_count: 1)
- llguidance — Rust Earley parser, Microsoft, powers OpenAI Structured Outputs (source_count: 1)
- outlines-library — pioneering FSM-based constrained decoding library (Willard & Louf, 2023) (source_count: 1)
- fantase — Token Search Trie + RoBERTa reranker for function calling; Amazon, EMNLP 2024 (source_count: 1)
- vllm — LLM inference framework; uses XGrammar as default constrained decoding backend (source_count: 1)
- andrej-karpathy — AI researcher; OpenAI co-founder; author of LLM intro video and llm.c (source_count: 1)
- deepseek-r1 — open-weight reasoning model; first public RL training paper for LLMs (source_count: 1)
- alphago — DeepMind Go AI; historical RL precedent for surpassing human performance (source_count: 1)

---

## Concepts

### LLM Training Pipeline
- llm-training-pipeline — three-stage overview: pre-training → SFT → RL (source_count: 1)
- pre-training — stage 1: internet text → base model via next-token prediction (source_count: 1)
- base-model — output of pre-training; internet token autocomplete; not yet an assistant (source_count: 1)
- tokenization — text → token IDs via BPE; vocab size tradeoff; multi-char tokens cause spelling/counting failures (source_count: 3)
- embeddings — token IDs → dense vectors via embedding matrix (vocab_size × dim); semantic similarity in vector space (source_count: 1)
- supervised-fine-tuning — stage 2: human labeler conversations → assistant (source_count: 1)
- reinforcement-learning-llm — stage 3 (verifiable): trial-and-error → emergent reasoning; real magic (source_count: 1)
- rlhf — stage 3 (unverifiable): reward model simulates human preferences; gameable; limited (source_count: 1)
- chain-of-thought — emergent reasoning strategy from RL; backtracking, self-checking, multi-perspective (source_count: 1)
- hallucination — LLM fabrication; causes (SFT distribution) and mitigations (self-knowledge + tools) (source_count: 1)
- context-window — working memory; measured in tokens; effective context scales with tokenization quality (source_count: 2)

### LLM Inference / Structured Output
- constrained-decoding — inference-time structural guarantee via logit masking; JSON state machine; greedy decoding optimal (source_count: 2)
- logit-masking — core mechanism: invalid tokens set to −∞ before softmax → zero probability (source_count: 1)
- function-calling — LLM generates structured JSON arguments for external tools/APIs; 30%→100% validity with constrained decoding (source_count: 3)
- token-character-mismatch — chars vs multi-char tokens; prefix-matching for multi-token identifiers (source_count: 3)

---

## Synthesis

*(none yet)*
