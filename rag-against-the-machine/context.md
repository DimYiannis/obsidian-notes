# RAG Theory — Study Context

Theory foundation for the *RAG Against the Machine* project (Codam/42): BM25 retrieval
over the vLLM 0.10.1 codebase, Qwen3-0.6B generation, graded on recall@k, defended
at a whiteboard. Learn the layers in order — each builds on the previous.

---

## Layer 1 — Why RAG exists

1. **Parametric vs non-parametric knowledge** — model weights hold "knowledge"
   frozen at training time; RAG bolts external knowledge on at inference. The core
   motivation for the whole architecture.
2. **Hallucination** — models generate fluent but wrong facts. RAG is a grounding fix:
   force the answer to come from retrieved text.
3. **Context window** — why you can't just paste the whole vLLM codebase into the
   prompt. Hard limits force retrieval *selection*.
4. **Knowledge cutoff + domain gap** — Qwen never saw vLLM 0.10.1 internals.
   Retrieval bridges the gap.

## Layer 2 — Retrieval theory (heart of the project)

5. **Information Retrieval basics** — query, document, corpus, relevance. IR is a
   50-year-old field; RAG retrieval is classic IR.
6. **Lexical (sparse) retrieval** — match exact terms. Bag-of-words model: a document
   is a term multiset, word order ignored.
7. **TF-IDF** — term frequency × inverse document frequency. Intuition: a term matters
   if it's frequent in the document but rare in the corpus. Learn before BM25 —
   BM25 is TF-IDF evolved.
8. **BM25** — TF saturation (k1) + document length normalization (b), rooted in the
   probabilistic relevance framework. The project's retriever. Must be able to derive
   the formula and explain k1/b at the whiteboard:
   - `IDF(t) = ln((N - df + 0.5)/(df + 0.5) + 1)` — rare term = high weight
   - TF part: `tf·(k1+1) / (tf + k1·(1 - b + b·(len/avglen)))`
   - **k1** (~1.2–2.0): TF saturation. k1=0 → binary presence; large k1 → raw counts.
   - **b** (0–1): length normalization. b=0 → ignore length; b=1 → full penalty.
9. **Inverted index** — `term → posting list`. Why search is fast: only score chunks
   sharing a term with the query. The data structure behind the entire search industry.
10. **Tokenization/analysis for retrieval** — how text becomes terms: lowercasing,
    splitting on non-alphanumerics, identifier splitting (snake_case/CamelCase, while
    keeping the original identifier). NOT the same as LLM tokenization (BPE) — know
    the difference. Same tokenizer for indexing and queries, always.
11. **Dense retrieval (embeddings)** — semantic vectors, cosine similarity, nearest
    neighbor. Not used here (until bonus), but be able to explain why not: lexical wins
    on exact identifiers (`enable_lora`), needs no model, fast, explainable.
12. **Sparse vs dense tradeoff** — vocabulary mismatch (synonyms kill lexical) vs
    exact-match blur (embeddings smear identifiers). Hybrid/RRF = bonus phase.

## Layer 3 — Chunking theory

13. **Why chunk** — retrieval granularity. Whole file: too big for context and IoU
    scoring. Single sentence: too small, no context.
14. **Chunking strategies** — fixed-size, sliding window, structure-aware (AST for
    code, headers for markdown). This project: structure-aware. Know the tradeoffs.
15. **Chunk size tradeoff** — big chunks: more context, better IoU overlap, diluted
    BM25 scores. Small chunks: precise but fragmenting. Project bias: near 2000 chars,
    because the IoU bar (0.05) is low.

## Layer 4 — Evaluation theory

16. **Recall@k / precision@k** — standard IR metrics. Recall@k: is a relevant item in
    the top k — yes/no per query, averaged over the dataset.
17. **IoU (Jaccard on spans)** — the overlap measure that decides "relevant":
    intersection / union of character ranges. Bar here: > 0.05.
18. **Why recall, not precision** — the generator can ignore junk chunks, but a missing
    right chunk is unrecoverable. RAG systems optimize recall at retrieval, precision at
    generation.

## Layer 5 — Generation side

19. **RAG prompt construction** — question + retrieved chunks in the prompt, instruct
    the model to answer from context. An application of in-context learning.
20. **Grounding / faithfulness** — the answer should come from the retrieved text, not
    from model memory.
21. **Lost in the middle** — models attend worse to the middle of a long context.
    Chunk ordering in the prompt matters.

---

## Reading path

- **Start:** Lewis et al. 2020, *Retrieval-Augmented Generation for
  Knowledge-Intensive NLP Tasks* — the original RAG paper; abstract + intro suffice.
- **IR foundation:** *Introduction to Information Retrieval* (Manning, Raghavan,
  Schütze) — free online. Ch 1 (boolean retrieval, inverted index), Ch 2 (term
  vocabulary), Ch 6 (TF-IDF scoring), Ch 8 (evaluation), Ch 11 intro (probabilistic
  models → BM25). Best single source; skip the rest.
- **BM25 depth:** Elastic *Practical BM25* blog series — best k1/b intuition.
- **Modern RAG overview:** Gao et al. 2023 survey, *Retrieval-Augmented Generation
  for Large Language Models* — skim for the landscape.

## Priorities

Layers 2 + 4 carry ~70% of the defense value. Layer 1 is the motivation story for the
README and the defense intro. If time is short: BM25 formula → offset math/AST →
tokenizer rationale → IoU/recall → the rest.
