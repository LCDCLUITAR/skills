---
name: bootstrap
description: "[Med] One-time system scaffold — creates global CLAUDE.md, system registry, on-demand skills context, and brains parent. Idempotent: safe to re-run, only creates what's missing. Use when setting up Claude orchestration on a new machine or when user says /bootstrap."
---

# Bootstrap

Scaffold the Claude orchestration system from scratch. This creates the layered architecture of scoped contexts (brains + projects) with a system registry for awareness.

## Pre-flight

Before creating anything, check what already exists. This skill is idempotent — never overwrite, only fill gaps.

Check for:
- `~/.claude/CLAUDE.md`
- `~/.claude/system-registry.md`
- `~/.claude/on-demand-skills/` (this directory — if you're running, it exists)
- A brains parent directory (ask if not known)

## Workflow

### 1. Global CLAUDE.md

If `~/.claude/CLAUDE.md` does not exist, create it with:

```markdown
# Global Conventions

Rules that apply across all scoped contexts — brains, projects, and the orchestrator.

---

## Scoped Contexts

This system organizes work into **scoped contexts** — directories with their own CLAUDE.md and optional `.claude/skills/`. There are two types:

- **Brains** — LLM-maintained knowledge bases (wiki pattern). The LLM owns the wiki layer; the human curates sources and asks questions.
- **Projects** — codebases with their own conventions, permissions, and domain context.

Each context is isolated by default. Cross-context access must be explicitly declared.

---

## System Map Maintenance

Whenever you create, modify, or delete any of the following in any scoped context:
- Skills, CLAUDE.md files, MCPs, hooks, permissions, or structural changes (contexts added/removed)

Update `~/.claude/system-registry.md` with the change.

---

## Cross-Context Isolation

Brains do not read each other unless explicitly wired. Projects are independent. Cross-access requires a dedicated skill or explicit declaration in the relevant CLAUDE.md files and system-registry.md.
```

### 2. System Registry

If `~/.claude/system-registry.md` does not exist, create it with:

```markdown
# System Registry

_Last updated: <today's date>_

## Scan Paths

## Global Config

## Scoped Contexts

### Brains

### Projects

### Global

#### orchestrator
- Path: ~/.claude
- Type: orchestrator
- Domain: system-wide awareness, orientation, auditing
- Skills: bootstrap, system-scan, system-status, system-where, system-audit
- MCPs: none
- Hooks: none
- Permissions: global
```

### 3. On-demand skills context

If `~/.claude/on-demand-skills/CLAUDE.md` does not exist, create it with a brief explanation that these are system maintenance skills.

### 4. Brains parent directory

Ask the user: "Where should brains live? (e.g., ~/Documents/second-brains)"

If the directory doesn't exist, create it. If it exists but has no `CLAUDE.md`, create the shared-conventions file (see below). If it already has a `CLAUDE.md`, skip.

**Shared conventions CLAUDE.md** for the brains parent:

```markdown
# Second Brains — Shared Conventions

This system follows the LLM Wiki pattern (Karpathy, 2026) — the LLM incrementally builds and maintains a persistent wiki, a compounding artifact that sits between the human and raw sources. The human curates sources and asks questions; the LLM does the maintenance.

---

## Architecture (every brain)

Three layers:

1. **Raw sources** — immutable source documents. The LLM reads but never modifies.
2. **The wiki** — LLM-generated markdown files: summaries, entity pages, concept pages, cross-references. The LLM owns this entirely.
3. **The schema** — CLAUDE.md that tells the LLM how the wiki is structured, what conventions to follow, and what workflows to use.

---

## Brain Tiers

### Full Brain
For domains that will accumulate significant structured content (>8K tokens). Uses the wiki pattern with selective page loading.

```
brain-name/
├── CLAUDE.md          # brain-specific schema and behavioral rules
├── .claude/skills/    # workflows: ingest, query, lint, and domain-specific
├── raw/               # immutable source material — never modify
├── wiki/
│   ├── index.md       # content catalog — read first on every query
│   ├── log.md         # append-only processing history
│   └── <pages>/       # entity directories
└── .obsidian/         # optional — if using Obsidian as viewer
```

### Lite Brain
For smaller or newer domains. Same isolation, simpler structure. Graduates to full when content grows.

```
brain-name/
├── CLAUDE.md          # brain-specific schema and behavioral rules
├── .claude/skills/    # workflows
├── raw/               # immutable source material — never modify
├── reference.md       # single structured file (replaces wiki/)
└── .obsidian/         # optional
```

**Graduation trigger:** when `reference.md` exceeds ~8K tokens of structured content, promote to full brain (decompose into wiki/ with index.md, log.md, and page directories).

---

## Shared Rules

1. **raw/ is immutable.** Source files are permanent records. Read them, never modify.
2. **log.md is append-only.** Never edit past entries. Always append new ones.
3. **index.md is your map.** Update it on every ingest. Read it first on every query.
4. **All workflows are skills.** Everything lives in `.claude/skills/`.
5. **Cross-brain access is restricted.** Brains are isolated by default. Cross-brain access happens only through explicit skills designed for that purpose and declared in system-registry.md.

---

## Cross-Brain Wiring Pattern

When two brains need to communicate, declare the relationship in `~/.claude/system-registry.md` under a `## Cross-Brain Relationships` section:

```
brain-a ──reads──▶ brain-b (via /skill-name)
brain-a ──graduates──▶ brain-b (adopted items become pages)
```

The reading brain must have a dedicated skill that accesses the other brain's wiki. Direct reads without a skill are prohibited.

---

## Frontmatter (all wiki pages)

```yaml
---
title: Page Title
type: <brain-specific types>
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: []
---
```

---

## Wikilinks

Use Obsidian-compatible wikilinks: `[[path/page-name]]`
Use descriptive aliases when helpful: `[[path/page-name|Display Name]]`

---

## log.md Format

```markdown
## [YYYY-MM-DD] action | detail
- Pages created: [[page1]], [[page2]]
- Pages updated: [[page3]]
- Notes: any relevant observations
```
```

### 5. Register brains parent

Add the brains parent as a scoped context in `system-registry.md` under Projects (type: parent context for brains).

### 6. Install new-brain skill

If `<brains-parent>/.claude/skills/new-brain/` doesn't already exist, note to the user that they should install the `new-brain` skill there. (This skill ships alongside bootstrap — it's a separate file.)

### 7. Report

Tell the user what was created, what was skipped, and what to do next:
- "Run `/new-brain` from your brains directory to create your first brain"
- "Run `/system-scan` from `~/.claude/on-demand-skills/` to discover existing contexts"
