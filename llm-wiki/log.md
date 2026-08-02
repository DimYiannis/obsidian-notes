# Log

Append-only. Parse with: `grep "^## \[" log.md | tail -10`

---

## [2026-06-01] setup | Wiki initialized
- Created directory structure: raw/, wiki/sources/, wiki/entities/, wiki/concepts/, wiki/synthesis/, wiki/meta/
- Wrote CLAUDE.md (schema)
- Wrote index.md, log.md
- Pages created: 0 | Pages updated: 0

## [2026-06-01] meta | Moved source pages to .sources/
- wiki/sources/ → .sources/ — source pages hidden from Obsidian graph
- Graph now shows only concepts, entities, synthesis, meta
- CLAUDE.md updated: ingest writes to .sources/, graph philosophy documented
- Pages moved: 1

## [2026-06-01] meta | Moved schema pages to .schema/
- Moved to .schema/: llm-wiki-idea source, llm-wiki-pattern, rag, memex, vannevar-bush, obsidian, overview
- Reason: dot-folders excluded from Obsidian graph/search by default
- index.md updated to remove schema entries; note added pointing to .schema/
- Pages moved: 7

## [2026-06-08] ingest | Draft-Conditioned Constrained Decoding (Reddy et al., 2025)
- Source: .raw/2026-06-08-dccd.md (arxiv.org/abs/2603.03305; source_type: paper)
- Concepts created: draft-conditioned-decoding
- Concepts updated: constrained-decoding (source_count→4, added variant reference + link to DCCD)
- Pages created: 3 | Pages updated: 3

## [2026-06-08] ingest | Don't Fine-Tune, Decode (Zhang et al., 2023)
- Source: .raw/2026-06-08-dont-fine-tune-decode.md (arxiv.org/abs/2310.07075; source_type: paper)
- Concepts updated: constrained-decoding (source_count→3, added fine-tuning vs decode-time section + TOOLDEC), function-calling (source_count→4, added 0%→52% result)
- Pages created: 2 | Pages updated: 4

## [2026-06-01] ingest | Call me maybe README (full version — delta update)
- Added named state machine states to constrained-decoding
- Added full algorithm steps + key papers section to call-me-maybe source page
- Pages created: 0 | Pages updated: 2

## [2026-06-01] ingest | Call me maybe — Function Calling in LLMs (personal project)
- Source: .raw/2026-06-01-call-me-maybe.md (user's own project; source_type: project)
- Concepts updated: constrained-decoding (source_count→2, JSON state machine + greedy decoding sections), function-calling (source_count→3, quantified impact + two compliance levels), token-character-mismatch (source_count→3, prefix-matching section)
- Pages created: 2 (source page, raw) | Pages updated: 5

## [2026-06-01] ingest | LLM Tokenization, Embeddings, and Context Windows (personal notes)
- Source: .raw/2026-06-01-tokenization-embeddings-context.md (user's own notes; source_type: note)
- Concepts created: embeddings
- Concepts updated: tokenization (source_count→3, added vocab tradeoff table), context-window (source_count→2, added effective context + size reference)
- Pages created: 2 | Pages updated: 4

## [2026-06-01] ingest | Intro to Large Language Models — Andrej Karpathy
- Source: .raw/2026-06-01-karpathy-llm-intro.md (video transcript pasted by user)
- Concepts created: llm-training-pipeline, pre-training, base-model, tokenization, supervised-fine-tuning, reinforcement-learning-llm, rlhf, chain-of-thought, hallucination, context-window
- Entities created: andrej-karpathy, deepseek-r1, alphago
- Concepts updated: function-calling (source_count→2), token-character-mismatch (source_count→2)
- Pages created: 14 | Pages updated: 3

## [2026-06-01] ingest | Function Calling for LLMs Using Constrained Decoding
- Source: raw/2026-06-01-constrained-decoding-function-calling.md (pasted by user; source_type: article)
- Concepts created: constrained-decoding, logit-masking, function-calling, token-character-mismatch
- Entities created: xgrammar, llguidance, outlines-library, fantase, vllm
- Source page created: wiki/sources/2026-06-01-constrained-decoding-function-calling.md
- Pages created: 10 | Pages updated: 1 (index.md)

## [2026-06-01] ingest | LLM Wiki — Idea File
- Source: raw/2026-06-01-llm-wiki-idea.md (pasted by user; source_type: idea)
- Concepts created: llm-wiki-pattern, rag, memex
- Entities created: obsidian, vannevar-bush
- Source page created: wiki/sources/2026-06-01-llm-wiki-idea.md
- Meta page created: wiki/meta/overview.md
- Pages created: 7 | Pages updated: 0

## [2026-06-13] ingest | Attention Is All You Need (Vaswani et al., 2017)
- Source: arxiv.org/abs/1706.03762 (source_type: paper; fetched via web)
- Source page created: wiki/sources/2026-06-13-attention-is-all-you-need.md
- Concepts created: transformer-architecture, self-attention, multi-head-attention, query-key-value, feed-forward-network
- Entities created: google-brain
- Concepts updated: embeddings (source_count→2), context-window (source_count→3) — added transformer/attention cross-links
- Pages created: 7 | Pages updated: 2

## [2026-06-13] ingest | A Mathematical Framework for Transformer Circuits (Elhage et al., 2021)
- Source: transformer-circuits.pub/2021/framework (source_type: paper; fetched via web)
- Source page created: wiki/sources/2026-06-13-transformer-circuits-framework.md
- Concepts created: mechanistic-interpretability, residual-stream, induction-heads
- Entities created: anthropic
- Concepts updated: self-attention (source_count→2), multi-head-attention (source_count→2), feed-forward-network (source_count→2), chain-of-thought (source_count→2)
- Note: flagged ⚠️-style caveat on feed-forward-network — paper studies attention-only models, does NOT establish the "MLP processes" claim from user's transformer explainer
- Pages created: 4 | Pages updated: 4

## [2026-07-28] ingest | Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks (Lewis et al., 2020)
- Source: arxiv.org/abs/2005.11401 (source_type: paper; fetched via web) — the original RAG paper, primary source underlying the earlier study-guide ingest
- Source page created: wiki/sources/2026-07-28-lewis-et-al-2020-retrieval-augmented-generation.md
- Raw capture: .raw/2026-07-28-lewis-et-al-2020-rag-paper.md
- Entities created: facebook-ai-research
- Concepts updated: retrieval-augmented-generation (source_count→2, added origin architecture: DPR+BART, RAG-Sequence/Token, index hot-swap, quantified hallucination reduction, retriever-collapse failure mode), dense-retrieval (source_count→2, added DPR/MIPS as concrete example)
- Pages created: 3 | Pages updated: 3

## [2026-07-29] meta | Expanded inverted-index and dense-retrieval with worked examples
- No new source; deepened existing concept pages from chat discussion
- inverted-index.md: added worked cat/dog forward→inverted build example, contrast case showing zero-term-overlap miss
- dense-retrieval.md: added Index Structures section (HNSW, IVF, Product Quantization mechanics), speed/recall dial note, distance-metric-must-match-training note, worked embedding-similarity example
- Cross-linked both pages' new examples to each other
- Pages created: 0 | Pages updated: 2

## [2026-07-29] meta | Reformatted examples as code blocks; added hash-map-vs-ANN-index distinction
- inverted-index.md, dense-retrieval.md: moved worked examples out of prose bullets into fenced code blocks
- Added Key Properties bullet to both clarifying they use different underlying data structures — inverted index is a hash map (exact key lookup), dense retrieval needs ANN structures (HNSW/IVF) since vectors have no exact-match concept. Noted IVF ("Inverted File Index") borrows the name/bucket idea but clusters vectors, not terms
- Pages created: 0 | Pages updated: 2

## [2026-07-12] ingest | RAG Theory — Study Context
- Source: .raw/2026-07-12-rag-theory-study-guide.md (local note; study guide for Codam RAG project)
- Concepts created: retrieval-augmented-generation, information-retrieval, lexical-retrieval, tf-idf, bm25, inverted-index, retrieval-tokenization, dense-retrieval, chunking, retrieval-evaluation, grounding, lost-in-the-middle
- Entities created: rag-against-the-machine (project)
- Concepts updated: hallucination (→2, RAG mitigation link), context-window (→4, RAG + lost-in-the-middle links), embeddings (→3, dense-retrieval link), tokenization (→4, retrieval-tokenization contrast)
- Entities updated: vllm (→2, corpus role)
- New index section: Retrieval / RAG
- Pages created: 14 | Pages updated: 6

## [2026-08-02] meta | Added general RAG theory to chunking (why chunk at all, why not whole docs/sentences)
- No new source; deepened chunking.md from chat discussion, distinct from the project-specific (RAG Against the Machine / IoU) content already there
- chunking.md: added "core problem chunking solves" (finite context window vs large corpus — RAG's retrieve-then-generate premise), "why not whole documents" (precision/context waste, ranking granularity, lost-in-the-middle), "why not smaller than a chunk" (loses surrounding context)
- Relabeled existing IoU/2000-char bullet as project-specific; added general size-tradeoff bullet alongside it
- Added [[lost-in-the-middle]] to Connections
- index.md: updated chunking one-line description
- Pages created: 0 | Pages updated: 1
