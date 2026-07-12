# Theory Concepts — Wiki Hub

Full concept pages live in `llm-wiki/wiki/concepts/`. Obsidian resolves these links
vault-wide — click through from here. Layer order = study order (see `context.md`).

## Layer 1 — Why RAG exists

- [[retrieval-augmented-generation]] — retrieve then generate; parametric vs non-parametric knowledge
- [[hallucination]] — fluent wrong facts; RAG is a grounding fix
- [[context-window]] — finite working memory; forces retrieval *selection*

## Layer 2 — Retrieval

- [[information-retrieval]] — query, document, corpus, relevance; the parent field
- [[lexical-retrieval]] — exact term matching over bag-of-words
- [[tf-idf]] — frequent in doc × rare in corpus; learn before BM25
- [[bm25]] — TF saturation (k1) + length normalization (b); **the** whiteboard topic
- [[inverted-index]] — term → posting list; why search is fast
- [[retrieval-tokenization]] — identifier splitting, keep-original rule, same analyzer both sides
- [[dense-retrieval]] — embeddings + nearest neighbor; why we *don't* use it (until bonus)
- [[embeddings]] — the representation dense retrieval builds on

## Layer 3 — Chunking

- [[chunking]] — granularity; structure-aware (AST/headers); size vs IoU tradeoff

## Layer 4 — Evaluation

- [[retrieval-evaluation]] — recall@k, precision@k, IoU on spans; recall-first asymmetry

## Layer 5 — Generation

- [[grounding]] — answer from context, not memory; faithfulness ≠ correctness
- [[lost-in-the-middle]] — U-shaped context use; chunk ordering matters

## Project

- [[rag-against-the-machine]] — project entity page (stack, targets, bonus phases)
- [[vllm]] — the corpus
