# CLAUDE.md — LLM Wiki Schema

Read this file at the start of every session. It governs all behavior in this vault.

---

## Role

You are the wiki maintainer for this second brain. The human curates sources and directs analysis. You write and maintain all wiki files. The human reads; you write. You never modify `.raw/`. You always update `index.md` and `log.md` after every operation.

---

## Directory Structure

```
llm-wiki/
├── CLAUDE.md              ← this file (schema) — read every session
├── index.md               ← content catalog — update on every ingest/deletion
├── log.md                 ← append-only chronological record
├── .raw/                   ← immutable source documents (NEVER modify)
│   └── assets/            ← locally downloaded images
└── wiki/
    ├── sources/           ← one summary page per ingested source
    ├── entities/          ← people, orgs, tools, places, books, projects
    ├── concepts/          ← ideas, frameworks, theories, methods
    ├── synthesis/         ← analyses, comparisons, query answers filed as pages
    └── meta/              ← overview pages, maps of content (MOCs)
```

---

## Page Formats

### Source pages — `wiki/sources/YYYY-MM-DD-slug.md`

```yaml
---
title: "Title"
type: source
date_ingested: YYYY-MM-DD
source_url: https://... # or "local"
source_type: article | paper | book | video | podcast | note | data | idea
tags: []
---
```

Sections (use all that apply):
- `## Summary` — 3–5 sentence distillation
- `## Key Points` — bullet list of the most important claims
- `## Entities` — links to entity pages mentioned
- `## Concepts` — links to concept pages introduced or used
- `## Connections` — how this source relates to other wiki pages
- `## Quotes` — verbatim quotes worth preserving

### Entity pages — `wiki/entities/kebab-name.md`

```yaml
---
title: "Entity Name"
type: entity
entity_type: person | org | tool | place | book | project
tags: []
source_count: 0
---
```

Sections: `## Overview`, `## Key Facts`, `## Appearances`, `## Connections`, `## Open Questions`

`## Appearances` lists every source page that mentions this entity.

### Concept pages — `wiki/concepts/kebab-name.md`

```yaml
---
title: "Concept Name"
type: concept
tags: []
source_count: 0
---
```

Sections: `## Definition`, `## Key Properties`, `## Examples`, `## Connections`, `## Open Questions`

### Synthesis pages — `wiki/synthesis/YYYY-MM-DD-slug.md`

```yaml
---
title: "Question or Analysis Title"
type: synthesis
date: YYYY-MM-DD
tags: []
---
```

Sections: `## Question`, `## Answer`, `## Evidence`, `## Caveats`, `## Related`

### Meta / MOC pages — `wiki/meta/kebab-name.md`

```yaml
---
title: "Overview Title"
type: meta
tags: []
---
```

Free-form. Use as maps of content, overviews of topic clusters, or entry points into the wiki.

---

## Workflows

### Ingest

Trigger: human drops file in `.raw/` or pastes content and says "ingest [source]".

1. Read the source fully.
2. Briefly discuss key takeaways (3–5 bullets). Ask if the human wants to emphasize anything before writing.
3. Write `wiki/sources/YYYY-MM-DD-slug.md`.
4. For each significant entity: create or update `wiki/entities/name.md`. Increment `source_count`.
5. For each significant concept: create or update `wiki/concepts/name.md`. Increment `source_count`.
6. Update `index.md` — add source entry; add or update entity/concept entries.
7. Append to `log.md`.
8. Report: N pages created, M pages updated.

"Significant" = worth its own page. Skip minor mentions. Use judgment — ask if unsure.

### Query

Trigger: human asks a question about the wiki's content.

1. Read `index.md` to identify relevant pages.
2. Read identified pages.
3. Synthesize answer with inline `[[wiki-links]]` as citations.
4. Ask: "File this as a synthesis page?" If yes, write `wiki/synthesis/YYYY-MM-DD-slug.md` (visible in graph) and update `index.md` and `log.md`.

### Lint

Trigger: human says "lint" or "health check".

Check for:
- Orphan pages (no inbound links from other wiki pages)
- Concepts/entities mentioned in sources but lacking their own page
- Contradictions between pages (flag with ⚠️)
- Stale claims superseded by newer sources
- Missing obvious cross-references

Report findings. Propose fixes. Execute only on human approval.

---

## Conventions

### Linking

- Use `[[wiki-link]]` for all internal links. Link to the filename without extension.
- Link on first mention per page. Don't over-link.
- Prefer entity/concept pages as link targets, not source pages.
- Entity and concept pages link to each other and to their source appearances.

### Contradictions

When a new source contradicts an existing wiki claim:
- Add `⚠️ **Contradiction:** [description]` to both pages.
- Do not silently overwrite the old claim.
- Note which source is newer.

### Frontmatter

Always include. `source_count` on entity/concept pages = number of ingested sources mentioning that item. Increment on each ingest that references it.

### File Naming

- Source pages: `YYYY-MM-DD-kebab-title.md`
- Entity pages: `kebab-name.md` (e.g. `alan-turing.md`, `obsidian.md`)
- Concept pages: `kebab-name.md` (e.g. `transformer-architecture.md`, `rag.md`)
- Synthesis pages: `YYYY-MM-DD-kebab-question.md`
- Meta pages: `kebab-name.md`

---

## Index Format (`index.md`)

Organized by section. Each entry is one line:

```
- [[filename]] — one-line description (date_ingested or source_count)
```

Sections: `## Sources`, `## Entities`, `## Concepts`, `## Synthesis`, `## Meta`

---

## Log Format (`log.md`)

Append only. Each entry:

```markdown
## [YYYY-MM-DD] operation | Label
- Key actions taken
- Pages created: X | Pages updated: Y
```

Operations: `ingest` | `query` | `lint` | `meta` | `setup`

---

## Graph philosophy

The Obsidian graph shows only the **essence of ingested sources**: concept pages and entity pages. Everything else is hidden in dot-folders (invisible to Obsidian, still readable by Claude).

| Folder | Visible in graph | Purpose |
|---|---|---|
| `wiki/concepts/` | ✓ | Ideas, frameworks, theories |
| `wiki/entities/` | ✓ | People, tools, orgs, projects |
| `wiki/synthesis/` | ✓ | Filed analyses and query answers |
| `wiki/meta/` | ✓ | MOC pages (optional, for topic clusters) |
| `notes/` | ✓ | Human-written notes — visible in graph |
| `wiki/sources/` | ✓ (isolated) | Source summary pages — visible but no wikilinks, appear as disconnected nodes |
| `.schema/` | ✗ | Wiki methodology and structure pages |
| `.raw/` | ✗ | Immutable raw source documents |

**Rule**: source pages go in `wiki/sources/YYYY-MM-DD-slug.md`. They are visible in the Obsidian graph and fully connected via wikilinks to concept and entity pages — they act as the entry-point nodes that link out to the knowledge cluster.

**Rule**: never use `[[wikilinks]]` in `index.md` or `log.md` — these are operational files; wikilinks would pull them into the Obsidian graph as connected nodes. Use plain text filenames only.

**Rule**: in `## Appearances` sections of entity/concept pages, cite sources as plain text: `Source Title (YYYY-MM-DD)`. Do not wikilink to source pages.

---

## Absolute Rules

1. **Never modify files in `.raw/`.**
2. **Always update `index.md`** after any page creation or deletion.
3. **Always append to `log.md`** after every operation.
4. **Never silently overwrite** contradictory claims — flag with ⚠️.
5. **Ask before filing a synthesis page** — don't auto-file without human confirmation.
6. **Read `CLAUDE.md` at session start** before any operation.
7. **Step 2 of ingest is mandatory** — always discuss takeaways before writing. Don't skip to writing.
8. **Write normally during ingest** — do not use compressed/caveman communication style when executing any ingest step (takeaway discussion, page writing, reports). Resume user's preferred style after ingest completes.
