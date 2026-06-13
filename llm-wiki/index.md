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
- 2026-06-08-dont-fine-tune-decode — Zhang et al. (2023); TOOLDEC; fine-tuning vs decode-time enforcement; Mistral-Instruct 0%→52%
- 2026-06-08-dccd — Reddy et al. (2025); draft-conditioned decoding; projection tax problem; GSM8K 15.2%→39.0%
- 2026-06-13-attention-is-all-you-need — Vaswani et al. (2017); the Transformer; self-attention, multi-head, Q/K/V, softmax(QKᵀ/√dₖ)V; WMT 2014 SOTA
- 2026-06-13-transformer-circuits-framework — Elhage et al. (2021, Anthropic); mechanistic interpretability; residual stream, QK/OV circuits, induction heads; attention-only (MLP caveat)

---

## Entities

- xgrammar — CFG/PDA constrained decoding engine, C++, default in vLLM/SGLang/TensorRT-LLM (source_count: 1)
- llguidance — Rust Earley parser, Microsoft, powers OpenAI Structured Outputs (source_count: 1)
- outlines-library — pioneering FSM-based constrained decoding library (Willard & Louf, 2023) (source_count: 1)
- fantase — Token Search Trie + RoBERTa reranker for function calling; Amazon, EMNLP 2024 (source_count: 1)
- vllm — LLM inference framework; uses XGrammar as default constrained decoding backend (source_count: 1)
- andrej-karpathy — AI researcher; OpenAI co-founder; author of LLM intro video and llm.c (source_count: 1)
- anthropic — AI safety lab; Transformer Circuits interpretability thread; Claude models (source_count: 1)
- google-brain — Google deep-learning research group; origin of the Transformer paper (source_count: 1)
- deepseek-r1 — open-weight reasoning model; first public RL training paper for LLMs (source_count: 1)
- alphago — DeepMind Go AI; historical RL precedent for surpassing human performance (source_count: 1)

---

## Concepts

### LLM Training Pipeline
- llm-training-pipeline — three-stage overview: pre-training → SFT → RL (source_count: 1)
- pre-training — stage 1: internet text → base model via next-token prediction (source_count: 1)
- base-model — output of pre-training; internet token autocomplete; not yet an assistant (source_count: 1)
- tokenization — text → token IDs via BPE; vocab size tradeoff; multi-char tokens cause spelling/counting failures (source_count: 3)
- embeddings — token IDs → dense vectors via embedding matrix (vocab_size × dim); semantic similarity in vector space (source_count: 2)
- supervised-fine-tuning — stage 2: human labeler conversations → assistant (source_count: 1)
- reinforcement-learning-llm — stage 3 (verifiable): trial-and-error → emergent reasoning; real magic (source_count: 1)
- rlhf — stage 3 (unverifiable): reward model simulates human preferences; gameable; limited (source_count: 1)
- chain-of-thought — emergent reasoning strategy from RL; backtracking, self-checking, multi-perspective (source_count: 2)
- hallucination — LLM fabrication; causes (SFT distribution) and mitigations (self-knowledge + tools) (source_count: 1)
- context-window — working memory; measured in tokens; effective context scales with tokenization quality (source_count: 3)

### LLM Inference / Structured Output
- constrained-decoding — standard single-pass technique; JSON state machine; fine-tuning vs decode-time (source_count: 4)
- draft-conditioned-decoding — two-pass variant; draft freely then constrain; solves projection tax (source_count: 1)
- logit-masking — core mechanism: invalid tokens set to −∞ before softmax → zero probability (source_count: 1)
- function-calling — LLM generates structured JSON arguments for external tools/APIs; 30%→100% and 0%→52% with constrained decoding (source_count: 4)
- token-character-mismatch — chars vs multi-char tokens; prefix-matching for multi-token identifiers (source_count: 3)

### Transformer Architecture
- transformer-architecture — Vaswani 2017; stack of blocks = multi-head attention + feed-forward, residual + layernorm; underlies all LLMs (source_count: 1)
- self-attention — sequence attends to itself; softmax(QKᵀ/√dₖ)V; quadratic cost; order-agnostic (source_count: 2)
- multi-head-attention — parallel heads over different subspaces; 8 heads × 64 dim in original; learned specialization (source_count: 2)
- query-key-value — three learned projections per token; query↔key match weights values; QK vs OV circuits (source_count: 1)
- feed-forward-network — position-wise MLP sublayer; "processes" info attention routes; holds most params; MLP-role caveat (source_count: 2)

### Mechanistic Interpretability
- mechanistic-interpretability — reverse-engineering model internals into circuits; Anthropic Transformer Circuits (source_count: 1)
- residual-stream — shared additive channel all components read/write; basis for circuit analysis (source_count: 1)
- induction-heads — emergent [A][B]…[A]→[B] copy mechanism; needs ≥2 layers; drives in-context learning (source_count: 1)

---

## Synthesis

*(none yet)*
