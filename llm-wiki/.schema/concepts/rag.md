---
title: "RAG (Retrieval-Augmented Generation)"
type: concept
tags: [llm, information-retrieval, knowledge-management]
source_count: 1
---

## Definition

A technique where an LLM retrieves relevant chunks from a document corpus at query time and generates an answer grounded in those chunks. The dominant paradigm for LLM + documents systems. Examples: NotebookLM, ChatGPT file uploads, most enterprise knowledge base products.

## Key Properties

- **Stateless**: no persistent synthesis — knowledge re-derived on every query
- **Retrieval-dependent**: answer quality bounded by retrieval quality (embedding similarity, chunk size, etc.)
- **Infrastructure-heavy at scale**: requires vector databases, embedding models, chunking pipelines
- **No accumulation**: asking the same question twice runs the same retrieval; nothing is built up between sessions

## Examples

- NotebookLM — upload PDFs, ask questions
- ChatGPT file uploads — attach documents to a conversation
- Most enterprise search + answer products

## Connections

- [[llm-wiki-pattern]] — the alternative approach: compile knowledge once, maintain persistently. Avoids re-derivation cost; accumulates synthesis over time.

## Open Questions

- At what scale does [[llm-wiki-pattern]] break down relative to RAG? (RAG scales to millions of docs; wiki pattern tops out at hundreds of pages before needing [[qmd]] or similar.)
- Hybrid approaches: RAG over raw sources + wiki for synthesis layer?
