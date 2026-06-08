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
