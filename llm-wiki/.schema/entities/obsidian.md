---
title: "Obsidian"
type: entity
entity_type: tool
tags: [note-taking, markdown, knowledge-management]
source_count: 1
---

## Overview

Local-first markdown note-taking app with a graph view, plugin ecosystem, and rich linking. Used as the interface (IDE) for the [[llm-wiki-pattern]] — the human browses the wiki in Obsidian while the LLM writes and maintains it via Claude Code.

## Key Facts

- Files stored as plain markdown on disk — no lock-in, git-compatible
- Graph view shows the link structure of the vault — best tool for seeing wiki shape (hubs, orphans)
- Plugin ecosystem: [[marp]] (slides), [[dataview]] (frontmatter queries), Web Clipper (article capture)
- Attachment folder path configurable (`raw/assets/` recommended for this wiki)
- Hotkey "Download attachments for current file" downloads inline images to local disk

## Appearances

- [[2026-06-01-llm-wiki-idea]] — recommended as the interface layer for the LLM Wiki pattern

## Connections

- [[llm-wiki-pattern]] — Obsidian is the "IDE"; the wiki is the "codebase"; Claude Code is the "programmer"
- [[obsidian-web-clipper]] — companion browser extension
- [[dataview]] — plugin for querying frontmatter
- [[marp]] — plugin for slide output

## Open Questions

- Obsidian Sync vs git for multi-device sync in this setup?
