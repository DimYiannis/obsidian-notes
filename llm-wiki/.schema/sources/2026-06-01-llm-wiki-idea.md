---
title: "LLM Wiki — Idea File"
type: source
date_ingested: 2026-06-01
source_url: local
source_type: idea
tags: [knowledge-management, llm, second-brain, methodology]
---

## Summary

Proposes a pattern where an LLM incrementally builds and maintains a persistent, interlinked wiki from raw sources — rather than re-deriving knowledge from scratch on every query (as in RAG). The wiki is a compounding artifact: cross-references pre-built, contradictions pre-flagged, synthesis already there. The human curates and directs; the LLM does all maintenance. Implemented with [[obsidian]] as the interface and CLAUDE.md as the operating schema.

## Key Points

- **Core distinction from RAG**: knowledge compiled once into structured pages, not re-retrieved per query. The wiki accumulates.
- **Three layers**: raw sources (immutable), wiki (LLM-written), schema (CLAUDE.md — governs LLM behavior).
- **Three operations**: ingest (process new source → update wiki), query (answer from wiki, optionally file as synthesis), lint (health-check for orphans, contradictions, gaps).
- **Navigation at scale**: `index.md` (content catalog) + `log.md` (chronological record) replace embedding-based search up to ~hundreds of pages.
- **Human role**: source curation, directing analysis, asking good questions. LLM role: everything else.
- **Historical antecedent**: [[vannevar-bush]]'s [[memex]] (1945) — same vision of private, associatively-linked personal knowledge store. LLM solves the maintenance problem Bush couldn't.
- **Optional tooling**: [[obsidian-web-clipper]] for clipping articles, [[qmd]] for search at scale, [[marp]] for slide output, [[dataview]] for frontmatter queries.

## Entities

- [[obsidian]] — recommended interface (IDE to the wiki's codebase)
- [[vannevar-bush]] — historical antecedent, inventor of the Memex concept
- [[qmd]] — optional local search engine (BM25/vector hybrid, has MCP server)
- [[obsidian-web-clipper]] — browser extension for converting articles to markdown
- [[marp]] — markdown slide deck format, Obsidian plugin available
- [[dataview]] — Obsidian plugin for querying frontmatter

## Concepts

- [[llm-wiki-pattern]] — the core concept described in this source
- [[rag]] — retrieval-augmented generation; the dominant alternative this pattern improves on
- [[memex]] — Bush's 1945 vision; direct spiritual ancestor

## Connections

- This source is the founding document of this wiki. All other pages descend from it.
- The schema in `CLAUDE.md` is a direct instantiation of the "schema layer" described here.

## Quotes

> "The wiki is a persistent, compounding artifact. The cross-references are already there. The contradictions have already been flagged."

> "Obsidian is the IDE; the LLM is the programmer; the wiki is the codebase."

> "Humans abandon wikis because the maintenance burden grows faster than the value. LLMs don't get bored."

> "The part he couldn't solve was who does the maintenance. The LLM handles that."
