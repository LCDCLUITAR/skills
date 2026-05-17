# LLM Wiki Pattern (Karpathy, 2026)

Condensed reference for the brain scaffolding philosophy.

---

## Core Idea

Instead of retrieving from raw documents at query time (RAG), the LLM **incrementally builds and maintains a persistent wiki** — a structured, interlinked collection of markdown files between the human and the raw sources. Knowledge is compiled once and kept current, not re-derived on every query. The wiki is a persistent, compounding artifact.

The human curates sources, directs analysis, and asks questions. The LLM does the summarizing, cross-referencing, filing, and bookkeeping.

---

## Three Layers

1. **Raw sources** — curated source documents. Immutable — the LLM reads but never modifies. This is the source of truth.
2. **The wiki** — LLM-generated markdown files: summaries, entity pages, concept pages, comparisons, synthesis. The LLM owns this layer entirely — creates, updates, maintains cross-references, keeps everything consistent.
3. **The schema** — a CLAUDE.md that tells the LLM how the wiki is structured, what conventions to follow, and what workflows to use. Co-evolved between human and LLM over time.

---

## Three Operations

### Ingest
Drop a new source into raw, tell the LLM to process it. The LLM reads the source, writes/updates wiki pages, updates the index, appends to the log. A single source may touch 10-15 wiki pages. Can be supervised (one at a time, human reviews) or batch.

### Query
Ask questions against the wiki. The LLM reads the index to find relevant pages, drills into them, synthesizes an answer. Good answers can be filed back into the wiki as new pages — explorations compound.

### Lint
Periodic health check. Look for: contradictions between pages, stale claims superseded by newer sources, orphan pages with no inbound links, important concepts lacking their own page, missing cross-references.

---

## Indexing and Logging

**index.md** — content-oriented catalog of everything in the wiki. Each page listed with a link and one-line summary, organized by category. The LLM reads this first on every query to find relevant pages.

**log.md** — chronological, append-only record of what happened and when. Each entry starts with a consistent prefix (`## [YYYY-MM-DD] action | detail`) making it parseable. Gives timeline of wiki evolution.

---

## Why This Works

The tedious part of maintaining a knowledge base is the bookkeeping — updating cross-references, keeping summaries current, noting contradictions, maintaining consistency. Humans abandon wikis because maintenance burden grows faster than value. LLMs don't get bored and can touch many files in one pass. The wiki stays maintained because the cost of maintenance is near zero.
