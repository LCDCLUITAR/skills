---
name: new-brain
description: "[Med] Scaffold a new brain (full or lite tier) — creates vault layout, schema, starter skills, permissions, cross-brain connections, and registers it. Use when user says /new-brain, 'create a brain', or 'new brain'."
---

# New Brain

Scaffold a new brain following the LLM Wiki pattern (see REFERENCE.md for the philosophy). A brain is a scoped context where the LLM incrementally builds and maintains a persistent, compounding wiki from raw sources.

## Gather Information

Ask the user these questions one at a time:

### 1. Name
"What should this brain be called? (kebab-case slug, e.g., `research-brain`, `health-brain`)"

### 2. Domain
"In one sentence, what does this brain track? (e.g., 'AI research papers and emerging techniques', 'personal health metrics and habits')"

### 3. Tier
"Full brain or lite brain?
- **Full** — wiki pattern with index.md, log.md, page directories. For domains that will accumulate significant content.
- **Lite** — single reference.md file. For smaller or newer domains. Graduates to full when content grows past ~8K tokens."

### 4. Entity types (full brain only)
"What kinds of pages will the wiki have? These become subdirectories under `wiki/`. (e.g., people, papers, concepts, tools — or features, decisions, processes)"

Skip this question for lite brains.

### 5. Reference sections (lite brain only)
"What sections should your reference.md have? (e.g., 'Current Program, Goals, Activity Log, Injuries')"

Skip this question for full brains.

### 6. Intended workflows
"How will you use this brain? Think about: what raw material goes in, how often, what you'll ask, and what outputs you want (summaries, comparisons, synthesis pages, etc.)"

Store the answer verbatim in the brain's CLAUDE.md.

### 7. Cross-brain connections
"Does this brain need to read from or write to any other brain? If yes, which brain and what data? (e.g., 'reads fitness-brain for macro goals', 'graduates ideas to projects-brain')"

If none, skip. If declared, register the relationship in system-registry.md after scaffolding.

### 8. Obsidian
"Initialize as an Obsidian vault? (creates .obsidian/ directory) — y/n"

## Scaffold

### Parent directory

This skill lives in the brains parent directory. Use the current working directory's parent as the brains root (the directory containing this `.claude/skills/` folder).

If the parent's `CLAUDE.md` (shared conventions) doesn't exist, something is wrong — bootstrap should have created it. Warn the user and offer to create it.

### Brain directory

#### Full brain — create `<parent>/<brain-name>/` with:

```
<brain-name>/
├── CLAUDE.md
├── raw/
│   └── .gitkeep
├── wiki/
│   ├── index.md
│   ├── log.md
│   └── <entity-type>/        # one per declared type
│       └── .gitkeep
├── .claude/
│   ├── skills/
│   │   ├── ingest/SKILL.md
│   │   ├── query/SKILL.md
│   │   └── lint/SKILL.md
│   └── settings.local.json
└── .obsidian/                 # only if user said yes
```

#### Lite brain — create `<parent>/<brain-name>/` with:

```
<brain-name>/
├── CLAUDE.md
├── raw/
│   └── .gitkeep
├── reference.md               # single structured file with user-declared sections
├── .claude/
│   ├── skills/
│   │   ├── ingest/SKILL.md
│   │   └── query/SKILL.md
│   └── settings.local.json
└── .obsidian/                 # only if user said yes
```

No lint skill for lite brains — the content is small enough to review directly.

### Brain CLAUDE.md

#### Full brain template:

```markdown
# <Brain Name> — Schema & Behavioral Rules

You are a disciplined wiki maintainer, not a generic chatbot. Your job is to compile raw input into a structured, cross-linked knowledge base and keep it consistent over time.

Shared conventions (frontmatter, wikilinks, raw/ immutability, log.md append-only, index.md maintenance) are defined in the parent `../CLAUDE.md`. This file covers <brain-name>-specific behavior only.

---

## Domain

<user's domain sentence>

---

## Vault Layout

```
<brain-name>/
├── CLAUDE.md
├── raw/               # immutable source files — never modify these
├── wiki/
│   ├── index.md       # content catalog — read this first on every query
│   ├── log.md         # append-only processing history
│   └── <entity-types listed>
└── .claude/skills/    # ingest, query, lint
```

---

## Entity Types

<list each entity type with a one-line description of what pages in that category contain>

---

## Intended Workflows

<user's verbatim workflow description>

---

## Cross-Brain Connections

<if declared: list relationships, e.g. "Reads fitness-brain/reference.md for macro goals (via /pull-goals skill)">
<if none: "None — this brain is fully isolated.">

---

## Evolve This Schema

This file is a starting point. As you use this brain, update it:
- Add page conventions specific to your entity types
- Add confidence markers or quality rules as patterns emerge
- Add task extraction rules if applicable
- Refine ingest behavior based on your source material
```

#### Lite brain template:

```markdown
# <Brain Name> — Schema & Behavioral Rules

You maintain a focused reference document for <domain>. Your job is to keep reference.md current, structured, and useful — ingesting raw material and updating the relevant sections.

Shared conventions (frontmatter, wikilinks, raw/ immutability) are defined in the parent `../CLAUDE.md`. This file covers <brain-name>-specific behavior only.

---

## Domain

<user's domain sentence>

---

## Vault Layout

```
<brain-name>/
├── CLAUDE.md
├── raw/               # immutable source files — never modify these
├── reference.md       # single structured reference — your primary output
└── .claude/skills/    # ingest, query
```

---

## Reference Sections

<list the sections declared by the user, with one-line descriptions>

---

## Intended Workflows

<user's verbatim workflow description>

---

## Cross-Brain Connections

<if declared: list relationships>
<if none: "None — this brain is fully isolated.">

---

## Graduation

When reference.md exceeds ~8K tokens of structured content, promote this brain to the full wiki pattern:
1. Decompose reference.md sections into page directories under wiki/
2. Generate index.md from existing content
3. Start log.md tracking
4. Add lint skill
5. Update this CLAUDE.md to the full brain template

---

## Evolve This Schema

This file is a starting point. As you use this brain, update it:
- Add conventions as patterns emerge
- Refine ingest behavior based on your source material
```

### wiki/index.md (full brain only)

```markdown
---
title: Index
type: index
created: <today>
updated: <today>
---

# <Brain Name> — Content Index

_No pages yet. This index will be updated on every ingest._

## <Entity Type 1>

## <Entity Type 2>

...
```

### wiki/log.md (full brain only)

```markdown
---
title: Log
type: log
created: <today>
---

# Processing Log

_Append-only record of all operations._
```

### reference.md (lite brain only)

```markdown
---
title: <Brain Name> Reference
type: reference
created: <today>
updated: <today>
---

# <Brain Name>

## <Section 1>

## <Section 2>

...
```

Populate the section headers from the user's declared reference sections. Leave content empty for the user to fill.

### .claude/settings.local.json

```json
{
  "permissions": {
    "allow": [
      "Edit(*<brains-parent>/<brain-name>/*)",
      "Write(*<brains-parent>/<brain-name>/*)",
      "Bash(mkdir *<brains-parent>/<brain-name>/*)",
      "Bash(mv *<brains-parent>/<brain-name>/*)",
      "Bash(cp *<brains-parent>/<brain-name>/*)",
      "Bash(find *<brains-parent>/<brain-name>/*)",
      "Bash(ls *<brains-parent>/<brain-name>/*)"
    ]
  }
}
```

### Starter Skills

#### .claude/skills/ingest/SKILL.md

```markdown
---
name: ingest
description: "Process new files in raw/ that haven't been ingested yet. Compiles content into wiki pages, updates index, appends to log. Use when user says /ingest, 'ingest', 'process new files', or 'what's new in raw'."
---

# Ingest

## Workflow

### 1. Find new files

Read `wiki/log.md` to get the list of already-processed files.
Scan all files in `raw/` recursively that are NOT yet in the log.

Skip: `.DS_Store`, system files, binary files that aren't readable as text.

### 2. For each new file

**a. Identify entities touched**

Based on the brain's entity types (defined in CLAUDE.md), identify what this source is about.

**b. Create or update wiki pages**

For each entity mentioned, create or update the relevant wiki page under `wiki/<entity-type>/`.

- If the page exists, integrate new information (don't duplicate, synthesize)
- If it doesn't exist, create it with proper frontmatter
- Add wikilinks to connect related pages
- Mark uncertain claims with `⚠️ unverified` inline

**c. Update index.md**

Add any new pages to `wiki/index.md` with a link and one-line summary.

**d. Append to log.md**

```markdown
## [YYYY-MM-DD] ingest | <source filename>
- Pages created: [[page1]], [[page2]]
- Pages updated: [[page3]]
- Notes: any relevant observations
```

### 3. Summary

Report what was processed: files ingested, pages created/updated, any issues or uncertainties noted.
```

#### .claude/skills/query/SKILL.md

```markdown
---
name: query
description: "Answer questions against the wiki. Reads index to find relevant pages, synthesizes an answer with citations. Optionally files good answers back as wiki pages. Use when user asks a question about the brain's domain."
---

# Query

## Workflow

### 1. Understand the question

Parse what the user is asking. Determine if it's:
- A factual lookup (find the page, cite it)
- A synthesis question (combine multiple pages)
- A comparison (contrast entities or concepts)
- An exploration (what do we know about X?)

### 2. Find relevant pages

Read `wiki/index.md` to identify which pages are relevant to the question. Then read those pages.

### 3. Synthesize answer

Compose an answer based on the wiki content. Always cite sources using wikilinks: [[page-name]].

If the wiki doesn't have enough information to answer, say so explicitly and suggest what sources could fill the gap.

### 4. Optionally file the answer

If the answer is a substantial synthesis, comparison, or analysis that would be valuable to keep, ask the user: "This seems worth keeping — should I file it as a wiki page?"

If yes, create it under the appropriate entity type (or `wiki/synthesis/` if it spans types), update index.md, and log it.
```

#### .claude/skills/lint/SKILL.md

```markdown
---
name: lint
description: "Health-check the wiki. Finds contradictions, orphan pages, missing cross-references, stale content, and gaps. Use when user says /lint, 'check wiki health', or 'lint'."
---

# Lint

## Workflow

### 1. Read the full index

Read `wiki/index.md` to understand the current state of the wiki.

### 2. Check each category

For each entity type directory, scan pages looking for:

- **Contradictions** — claims on one page that conflict with another
- **Stale content** — information that newer sources may have superseded (check log.md for recency)
- **Orphan pages** — pages with no inbound wikilinks from other pages
- **Missing pages** — entities referenced via wikilinks that don't have their own page yet
- **Missing cross-references** — pages that discuss related topics but don't link to each other
- **Thin pages** — pages with very little content that could be enriched

### 3. Report

Present findings organized by severity:

1. **Contradictions** (fix immediately — knowledge integrity)
2. **Missing pages** (create — broken links)
3. **Stale content** (update or mark as potentially outdated)
4. **Orphan pages** (add inbound links or consider merging)
5. **Missing cross-references** (add links)
6. **Thin pages** (note for enrichment on next ingest)

### 4. Optionally fix

Ask the user: "Want me to fix these now, or just log them?"

If fixing, make the changes and append a lint entry to log.md:

```markdown
## [YYYY-MM-DD] lint | health check
- Issues found: <count>
- Fixed: [[page1]], [[page2]]
- Flagged for review: [[page3]]
```
```

### Obsidian (if yes)

Create `.obsidian/` with a minimal `app.json`:

```json
{
  "alwaysUpdateLinks": true
}
```

## Register

Append the new brain to `~/.claude/system-registry.md` under `### Brains`:

#### Full brain:
```markdown
#### <brain-name>
- Path: <full path>
- Type: brain
- Domain: <domain sentence>
- Skills: ingest, query, lint
- MCPs: none
- Hooks: none
- Permissions: Edit/Write/mkdir/mv/cp/find/ls scoped to <brains-parent>/*
```

#### Lite brain:
```markdown
#### <brain-name>
- Path: <full path>
- Type: brain (lite)
- Domain: <domain sentence>
- Skills: ingest, query
- MCPs: none
- Hooks: none
- Permissions: Edit/Write/mkdir/mv/cp/find/ls scoped to <brains-parent>/*
```

#### Cross-brain connections (if declared):

If the user declared cross-brain relationships, also add them under a `## Cross-Brain Relationships` section in the registry (create the section if it doesn't exist):

```markdown
## Cross-Brain Relationships

<brain-a> ──reads──▶ <brain-b> (via /skill-name)
<brain-a> ──graduates──▶ <brain-b> (adopted items become pages)
```

## Done

Tell the user:
- Brain created at `<path>` (tier: full/lite)
- "Drop source material into `raw/` and run `/ingest` to get started"
- "The schema in CLAUDE.md is a starting point — it will evolve as you use the brain"
- (Lite only) "When reference.md grows past ~8K tokens, run graduation to promote to full wiki pattern"
- (Cross-brain) List any declared connections registered
