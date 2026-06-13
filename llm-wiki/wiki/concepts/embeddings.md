---
title: "Embeddings"
type: concept
tags: [llm, representation, neural-networks, tokenization]
source_count: 2
---

## Definition

Dense numerical vectors that represent tokens in a continuous high-dimensional space. Every token in the vocabulary has a corresponding embedding vector. These vectors are learned during [[pre-training]] and stored in the **embedding matrix** — the first transformation applied to token IDs before they enter the Transformer layers.

## Embedding Matrix

A lookup table of shape `vocab_size × embedding_dim`:

```
Embedding Matrix = 100,000 tokens × 4,096 dimensions
```

At inference: token ID → row lookup → embedding vector. Computationally trivial (a table lookup), but the values in the table encode everything the model learned about that token's meaning.

## Key Properties

- **Semantic geometry**: semantically similar tokens have geometrically similar vectors. `cat` ≈ `dog`; `king - man + woman ≈ queen`. Meaning is encoded as direction and distance in vector space.
- **Learned, not hand-crafted**: embedding vectors are trained end-to-end via gradient descent — the model discovers useful representations from predicting next tokens on internet text.
- **Dimensionality**: typically 768–4,096 dimensions depending on model size. Higher dim = more expressive but more memory.
- **Matrix size scales with vocabulary**: 100k tokens × 4,096 dims = 400M parameters just in the embedding matrix. Larger vocab → larger matrix → more memory.
- **Input to Transformer**: embeddings are what the Transformer layers actually operate on. Raw token IDs never enter the attention mechanism directly.

## Pipeline position

```
Token IDs
    ↓
Embedding Matrix lookup (vocab_size × embedding_dim)
    ↓
Embedding Vectors  ← Transformer operates here
    ↓
Transformer Layers
    ↓
Next Token Prediction
```

## Connections

- [[tokenization]] — produces token IDs; embeddings are the next step
- [[pre-training]] — embedding matrix is learned during pre-training
- [[context-window]] — all tokens in the context window are embedded before entering the Transformer
- [[transformer-architecture]] — embeddings (plus positional encodings) are its input representation
- [[self-attention]] — operates on embeddings to build contextual representations
- [[constrained-decoding]] — operates on logits (output of Transformer), not embeddings directly, but both live in the same pipeline

## Open Questions

- How much of a model's "knowledge" lives in the embedding matrix vs. in the Transformer weights?
- Do embedding spaces for different languages align naturally, or require explicit alignment?
