---
title: "LLM Wiki Pattern"
type: concept
tags: [knowledge-management, methodology, second-brain]
source_count: 1
---

## Definition

A method for building personal knowledge bases where an LLM incrementally writes and maintains a persistent, interlinked wiki from raw sources. Contrasts with [[rag]]: instead of retrieving and re-deriving knowledge per query, the LLM compiles knowledge once into structured pages and keeps them current as new sources arrive.

## Key Properties

- **Persistent**: wiki survives between sessions; knowledge accumulates
- **Compounding**: each new source enriches existing pages; cross-references already built
- **LLM-maintained**: human never writes wiki pages; LLM does all bookkeeping
- **Schema-governed**: behavior rules live in `CLAUDE.md` (or `AGENTS.md`), not in prompts — makes the LLM a disciplined maintainer, not a generic chatbot
- **Three-layer architecture**:
  1. `raw/` — immutable source documents (source of truth)
  2. `wiki/` — LLM-generated structured pages
  3. `CLAUDE.md` — schema governing LLM behavior
- **Three operations**: ingest, query, lint

## Examples

- Personal second brain: journal entries + articles → structured self-knowledge
- Research wiki: papers + reports → evolving thesis with entity and concept pages
- Book companion: per-chapter filing → character/theme/plot wiki like Tolkien Gateway
- Team knowledge base: meeting transcripts + Slack threads → maintained internal wiki

## Connections

- [[rag]] — the dominant alternative; re-derives knowledge per query rather than compiling it
- [[memex]] — historical antecedent (Bush, 1945); same vision, LLM solves the maintenance problem
- [[obsidian]] — the recommended interface for this pattern

## Open Questions

- At what scale (sources, pages) does `index.md`-based navigation break down and require [[qmd]] or similar?
- How to handle conflicting synthesis from different domains in a multi-topic second brain?
- Best practices for schema evolution as wiki grows and conventions change?
