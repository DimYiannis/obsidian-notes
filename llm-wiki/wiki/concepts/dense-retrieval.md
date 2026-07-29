---
title: "Dense Retrieval"
type: concept
tags: [information-retrieval, embeddings, semantic-search]
source_count: 2
---

## Definition

Retrieval by semantic similarity in vector space: an embedding model maps queries and documents to dense vectors; relevance is cosine similarity (or dot product); retrieval is (approximate) nearest-neighbor search. The counterpart to [[lexical-retrieval]] — matching **meaning** instead of exact terms.

## Key Properties

- **Solves vocabulary mismatch** — "GPU memory" retrieves a chunk about "VRAM" because the vectors are close; this is exactly where lexical matching fails.
- **Blurs exact identifiers** — embeddings smear `enable_lora` toward everything LoRA-related; the exact-match precision that makes [[bm25]] strong on code is lost. The sparse-vs-dense tradeoff in one line: **synonyms kill lexical, identifier blur kills dense.**
- **Operational cost** — needs an embedding model at index and query time, vector storage, and ANN infrastructure; lexical needs a dict. Also less explainable: a similarity score doesn't decompose per term.
- **Hybrid fusion** — running both and merging rankings (e.g. Reciprocal Rank Fusion, RRF) captures both strengths; the bonus phase of *RAG Against the Machine*.
- **Brute-force doesn't scale** — comparing a query vector against every stored vector is O(N) per query; fine at thousands of vectors, too slow past roughly a million. This is the entire reason ANN (Approximate Nearest Neighbor) index structures exist — trade a small amount of recall for a large amount of speed.
- **Every ANN index has a speed/recall dial** — `ef_search` in HNSW, `nprobe` in IVF. Raise it: more candidates explored, higher recall, slower. Lower it: faster, some true nearest neighbors missed. Tuning this dial is the main operational lever of a vector index, the way `k1`/`b` are for [[bm25]].
- **Distance metric must match training** — cosine similarity, dot product, or L2 distance. Whichever the embedding model was trained/normalized for; using the wrong one silently degrades results without erroring.
- **Not the same structure as [[inverted-index]]** — that's a hash map, exact discrete key lookup. Vectors are continuous floats with no exact-match concept, so dense retrieval needs a structure that answers "what's close," not "what's equal" — hence HNSW graphs / IVF clusters instead of a dict. IVF ("Inverted File Index") reuses the inverted-index *name* and bucket→members idea, but its buckets are k-means centroids over vectors, not exact terms.

## Index Structures (how the ANN search actually works)

- **HNSW (Hierarchical Navigable Small World)** — a multi-layer graph of vectors. Top layer is sparse with long-range edges (few nodes, big jumps across the space); bottom layer is dense with short-range edges (every vector, fine-grained neighbors). Search starts at the top layer, greedily walks toward the query vector, drops a layer, repeats, refines at the bottom. Conceptually a skip-list, but for vector space instead of a sorted list. Default index in FAISS, Qdrant, pgvector, Weaviate.
- **IVF (Inverted File Index)** — cluster all vectors into *k* buckets via k-means at index-build time. At query time, find the nearest few centroids, then only scan vectors inside those buckets — cuts the search space from N to roughly N/k. Named "inverted" for the same reason as [[inverted-index]]: it maps cluster → member vectors instead of vector → cluster.
- **Product Quantization (PQ)** — a compression technique, usually paired with IVF (`IVF-PQ`, FAISS's default at billion-vector scale). Split each vector into subvectors, quantize each subvector to the nearest of a small codebook of centroids, store the compact codes instead of the full floats. Shrinks memory 10–30x at some accuracy cost.

## Examples

- **DPR** (Dense Passage Retriever, Karpukhin et al., used in Lewis et al. 2020's original RAG paper): BERT bi-encoder producing query/document vectors, retrieval via Maximum Inner Product Search (MIPS) over a 21M-passage Wikipedia index. The retriever component paired with BART to make the original RAG model.
- Sentence-transformer embeddings + FAISS/HNSW nearest-neighbor search — the standard RAG-tutorial stack.
- *RAG Against the Machine* deliberately does **not** use dense retrieval for the mandatory part: lexical wins on exact identifiers, needs no model, is fast and explainable.

Worked contrast — same three documents as [[inverted-index]], plus one with zero word overlap:

```
doc1: "the cat sat on the mat"     → [0.12, -0.44, 0.81, ...]
doc2: "the dog sat on the log"     → [0.15, -0.39, 0.77, ...]
doc3: "feline rested on rug"       → [0.13, -0.42, 0.79, ...]  ← no word overlap with doc1!
```

Query "cat sleeping on carpet" → nearest vectors = doc1, doc3. Doc3 is caught despite sharing zero words — this is the exact case [[inverted-index]] cannot handle, since no query term appears in its postings at all.

## Connections

- [[embeddings]] — the representation dense retrieval is built on (document/query-level rather than token-level)
- [[lexical-retrieval]] — the complementary paradigm; hybrid RRF combines both
- [[bm25]] — the lexical baseline dense methods are measured against
- [[retrieval-augmented-generation]] — DPR is the retriever the original RAG paper paired with a generator; the most common RAG retrieval choice in industry tutorials, not always the right one
- [[facebook-ai-research]] — origin of DPR

## Open Questions

- For code retrieval specifically, do code-tuned embedding models close the exact-identifier gap, or does hybrid remain necessary?
