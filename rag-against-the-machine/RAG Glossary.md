# RAG Glossary — Every Term, Explained From Zero

Companion to `RAG General Knowledge.canvas`. Same content, linear/readable form for note-taking. Assumes no prior RAG knowledge.

---

## 1. Why RAG Exists

**RAG (Retrieval-Augmented Generation)** — an architecture that combines a search system with a text-generating LLM. Instead of an LLM answering purely from what it memorized during training, it first *retrieves* relevant text from an external source, then *generates* an answer using that text as evidence. Introduced by Lewis et al., 2020.

**Parametric knowledge** — what's baked into a model's weights during training. Frozen once training ends. "Fuzzy long-term memory."

**Non-parametric knowledge** — an external, searchable store of text (documents, a database, a codebase) that isn't baked into the model. Can be updated anytime without retraining.

**Hallucination** — when a model generates text that sounds fluent and confident but is factually wrong. Happens because the model is predicting plausible-sounding next words, not looking anything up.

**Grounding** (aka faithfulness) — making a model's answer traceable to real retrieved text, instead of invented from parametric memory. RAG's main defense against hallucination.

**Context window** — the maximum amount of text (measured in tokens) a model can read at once in a single prompt. Finite — you can't just paste an entire codebase or document collection into it.

**Knowledge cutoff** — the date after which a model has seen zero training data. It literally cannot know about anything published after that date.

**Domain gap** — separate from cutoff: even *within* the training period, a model's training data is general/public. Niche, private, or obscure material (an internal codebase, a specific software version) was probably never in the training data at all, cutoff or not.

---

## 2. The Standard RAG Pipeline

**Indexing pipeline** (offline, done once) — the prep phase: load source documents → split into chunks → convert chunks into a searchable form (embeddings or tokens) → store in an index. Re-run only when the source data changes.

**Query pipeline** (online, done per question) — the live phase: user asks a question → convert the question into the same searchable form used for indexing → search the index for the most relevant chunks → optionally re-rank them → build a prompt with those chunks → LLM generates the answer.

**Chunk** — a retrievable unit of text: a paragraph, a function, a section. Documents get split ("chunked") because a whole document is usually too big and unfocused to retrieve as one unit, but a single sentence is too small to carry context.

**Chunking strategies:**
- *Fixed-size* — split every N characters/tokens. Simple, ignores meaning/structure.
- *Sliding window* — fixed-size chunks that overlap, so content near a boundary isn't cut off from context on both sides.
- *Structure-aware* — split on natural boundaries (markdown headers, code function/class boundaries). Usually best quality.

**Embedding** — a numeric vector (a long list of numbers) that represents the *meaning* of a piece of text, produced by an embedding model. Texts with similar meaning get vectors that are numerically close together.

**Vector store / vector database** — a database specialized in storing embeddings and quickly finding the ones closest to a given query vector. Examples: Pinecone, Weaviate, Chroma, pgvector, FAISS.

**Reranking** — a second, more expensive scoring pass over the initial retrieved candidates, done by a more accurate (but slower) model, to reorder them by true relevance before they go into the prompt. Pattern: retrieve wide (top 50–100 cheap), rerank narrow (top 3–5 accurate).

**Prompt augmentation** — building the actual text sent to the LLM: instructions + retrieved chunks + the user's question, usually with an explicit instruction like "answer only using the context below."

**In-context learning** — the LLM adapting its behavior based purely on what's in the current prompt, with no retraining. This is the mechanism that makes RAG work at all — you're not teaching the model new facts permanently, just handing it facts to use *right now*.

---

## 3. Retrieval Methods

**Information Retrieval (IR)** — the decades-old field of finding relevant items in a large collection given a query. Core vocabulary:
- **Query** — what's being searched for
- **Document** — one retrievable item
- **Corpus** — the entire collection of documents
- **Relevance** — whether a document actually answers the query

**Lexical retrieval** (aka sparse retrieval) — matching based on exact words shared between query and document. Treats a document as a "bag of words" (a set/multiset of terms, order ignored).

**TF-IDF (Term Frequency – Inverse Document Frequency)** — a scoring formula: a term is important to a document if it appears often *in that document* (TF) but rarely *across the whole corpus* (IDF, i.e. it's a distinctive word, not "the"/"is"). Predecessor to BM25.

**BM25** — the standard, improved version of TF-IDF used by most real search engines (Elasticsearch, Lucene). Adds two tunable parameters:
- `k1` — controls *term frequency saturation*: how much repeating a word in a document keeps adding to its score. Low k1 → repetition barely matters after the first occurrence. High k1 → raw counts matter a lot.
- `b` — controls *document length normalization*: long documents naturally contain more words, so `b` discounts their score to keep long and short documents comparable. b=0 → length ignored. b=1 → fully penalize length.

**Inverted index** — the data structure that makes lexical search fast. Instead of scanning every document for every query, it stores a lookup table: `word → list of documents containing that word`. Only documents sharing at least one query term ever get scored.

**Tokenization for retrieval** — breaking text into searchable units: lowercase everything, split on punctuation, split compound identifiers (e.g. `getUserName` → `get`, `user`, `name`, but also keep the original `getUserName`). Different goal from an LLM's own tokenizer (which compresses text for the model to process) — retrieval tokenization exists purely to maximize what gets matched. Critical rule: use the *exact same* tokenization method for indexing documents and for handling queries, or matching silently breaks.

**Dense retrieval** — instead of matching exact words, encode both query and documents as embeddings (vectors) and find the documents whose vectors are numerically closest to the query's vector (cosine similarity / nearest-neighbor search). Captures *meaning*, not just literal word overlap — so it can match "car" to "automobile."

**ANN (Approximate Nearest Neighbor) search** — the fast search algorithm used to find close vectors in a huge vector store without comparing against every single one. Common implementation: **HNSW** (Hierarchical Navigable Small World graphs).

**Sparse vs dense tradeoff** — lexical (sparse) is fast, free, explainable, and wins on exact terms/identifiers, but is blind to synonyms (this is called *vocabulary mismatch*). Dense catches synonyms/paraphrasing but can blur precise matches and needs a model to run.

**Hybrid retrieval** — running both lexical and dense retrieval and merging their ranked results, commonly via **RRF (Reciprocal Rank Fusion)**: each document gets a combined score of `1 / (rank + constant)` summed across both rankings. Gets the strengths of both approaches. Common production default.

---

## 4. Key Infrastructure

**Embedding model** — the neural network that turns text into embeddings. Examples: OpenAI's `text-embedding-3`, BGE, Sentence-BERT. Choosing a good embedding model matters as much as choosing a good LLM.

**Vector database** — see above (section 2). Restated here because it's core infra, not just a pipeline step.

---

## 5. RAG Architectures (how real systems are built)

**Naive RAG** — the simplest possible version: index once, retrieve top-k chunks, stuff them in the prompt, generate. No query understanding, no verification. Brittle but a fine starting point.

**Advanced RAG** — naive RAG plus fixes before and after retrieval:
- *Pre-retrieval*: query rewriting/expansion (reformulate a vague question into a better search query), **HyDE** (Hypothetical Document Embeddings — have the LLM first guess what an ideal answer document would look like, then search using *that* as the query).
- *Post-retrieval*: reranking, filtering out irrelevant chunks before they reach the prompt.

**Modular RAG** — instead of one fixed pipeline, treat retrieval/generation as swappable, reorderable building blocks (multiple retrievers, routing logic, memory) chosen per query type.

**Agentic RAG** — an LLM agent decides on its own *whether* to retrieve, *how many times*, and can issue follow-up searches or call other tools, rather than retrieval being a fixed one-shot step.

**GraphRAG** — retrieves from a knowledge graph (entities + their relationships) instead of flat text chunks. Better for questions that require connecting multiple pieces of information ("multi-hop" questions).

---

## 6. Evaluation

**Recall@k** — did the top-k retrieved results include at least one relevant item? Yes/no per query, averaged across many queries into a percentage.

**Precision@k** — of the top-k retrieved results, what fraction were actually relevant?

**Why recall matters more than precision in RAG** — the generator can simply ignore an irrelevant chunk that got retrieved (precision problem, recoverable). But if the *correct* chunk was never retrieved at all, the model has no way to answer correctly (recall problem, unrecoverable). So RAG systems are tuned to prioritize recall at the retrieval stage.

**MRR (Mean Reciprocal Rank)** — measures how early the first relevant result appears in the ranking (position 1 scores higher than position 5).

**NDCG (Normalized Discounted Cumulative Gain)** — rewards having *multiple* relevant results, weighted by how high they rank.

**IoU (Intersection over Union)** — used when "relevant" is defined as an exact text span: overlap between the retrieved chunk's span and the true relevant span, divided by their combined span. Handles the case where chunk boundaries don't line up exactly with the "correct" answer location.

**Faithfulness / groundedness** — is the generated answer actually supported by the retrieved text, or did the model make something up anyway?

**Answer relevancy** — does the generated answer actually address the question asked (separate from whether it's grounded)?

**Context precision / context recall** — same precision/recall ideas, applied specifically to whether the *retrieved chunks* were relevant and complete.

**RAGAS** — a popular open-source framework that packages the above RAG-specific metrics (faithfulness, answer relevancy, context precision/recall) into a standard evaluation toolkit.

**Lost in the middle** — a documented LLM behavior: models pay more attention to the beginning and end of a long context than the middle (a U-shaped attention curve). Matters for RAG because it means *where* you place retrieved chunks in the prompt affects whether the model actually uses them.

**Common failure modes:**
- *Retrieval miss* — the right chunk simply never got retrieved. Unrecoverable.
- *Irrelevant context* — junk chunks retrieved alongside good ones, potentially distracting the model.
- *Contradictory chunks* — retrieved chunks disagree with each other; model has to arbitrate.
- *Stale index* — the underlying data changed but the index wasn't rebuilt, so retrieval returns outdated info.
- *Retriever collapse* — on tasks without a clear "right answer to look up" (e.g. open-ended creative writing), a trained retriever can degrade into always returning the same document regardless of the query, making retrieval useless.

---

## Quick Map: Where Each Term Fits

```
WHY RAG EXISTS → PIPELINE → RETRIEVAL METHOD → INFRASTRUCTURE → ARCHITECTURE → EVALUATION
(parametric      (index →    (lexical/dense/     (vector DB,     (naive →       (recall@k,
 limits,          query →     hybrid, BM25,        embedding      advanced →     faithfulness,
 hallucination,   generate)   inverted index)      models)        modular →      lost-in-middle)
 context window)                                                  agentic)
```
