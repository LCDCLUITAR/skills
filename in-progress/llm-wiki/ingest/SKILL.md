---
name: ingest
description: Process all new files in raw/ that haven't been ingested yet. Compiles content into wiki pages, extracts tasks to Needs Review, adds confidence markers on uncertain claims. Use when user says "/ingest", "ingest", "process new files", or "what's new in raw".
---

# Ingest

## Workflow

### 1. Find new files

Read `wiki/log.md` to get the list of already-processed files.

Recursively scan all files in `raw/` that are NOT yet in the log. Skip:
- Everything in `raw/reference/` (personal process notes, not for ingestion)
- Files with `ingest: false` in their frontmatter
- `*-eod.md` files in `raw/notes/` (EOD content goes directly into daily notes now)
- `.DS_Store` and other system files
- Binary files (`.png`, `.jpg`, `.docx`) that aren't readable as text

Note: `raw/notes/` files may now be in monthly subfolders (`raw/notes/YYYY-MM/`). Scan recursively.

### 2. For each new file

**a. Identify entity types touched**
- People (names mentioned, speaker attributions)
- Features (product initiatives, tickets)
- Decisions (choices made, approaches agreed)
- Processes (team rituals, workflows)
- Concepts (patterns, standards, principles)

**b. Create or update wiki pages**

For each entity touched, create or update the relevant wiki page under `wiki/`.

**Confidence markers:** When extracting a claim that is:
- Tentative ("I think", "maybe", "we might")
- Spoken by one person with no confirmation from others
- Contradicts something already on a wiki page
- From a noisy/unclear transcript segment

→ Mark inline with `⚠️ unverified` immediately after the claim.

Example: `Brad owns the DataDog integration ⚠️ unverified`

**c. Extract action items → Needs Review**

Classify ownership per these rules:
- **My Tasks candidate**: Luis Chaparro (appears as "Chaparro, Luis") uses first-person ("I'll...", "I will...", "I need to...") OR another speaker assigns it to "Luis" or "Chaparro" by name
- **Others' task**: everything else

Add ALL extracted tasks (mine and others') to `## 🔍 Needs Review` in `tasks.md`, NOT directly to My Tasks or Others. Format:
```
- [ ] Task description — [[source-page]] — YYYY-MM-DD — (mine|theirs: PersonName)
```

**Dedup check before adding:** If a task already exists in My Tasks, Needs Review, or Others at the same or higher priority, skip it.

**d. Append to log.md**

```markdown
## [YYYY-MM-DD] ingest | path/to/file.md
- Pages created: [[page1]], [[page2]]
- Pages updated: [[page3]], [[page4]]
- Tasks extracted: N (to Needs Review)
- Notes: any relevant observations
```

### 3. Update wiki/index.md

Add any new pages created. Update summaries for significantly changed pages.

### 4. Surface Needs Review tasks

After processing all files, show the full list of tasks added to Needs Review:

```
## 🔍 Tasks Extracted — Review These

**Mine:**
- [ ] Task description — [[source]] — 2026-05-06

**Others:**
- [ ] Task description — [[source]] — 2026-05-06

Any of these wrong? (I can go through them one by one if you'd like)
```

Wait for response. If user says tasks are wrong or wants to review individually, go through each one: "Keep, edit, or reject?"

Confirmed mine → move to `## 🔥 My Tasks` under the appropriate feature group
Confirmed others → move to `## 👥 Others`
Rejected → delete from tasks.md

### 5. Report summary

```
## Ingest Summary
- Files processed: N
- Pages created: N
- Pages updated: N
- Tasks extracted: N (pending your review above)
- Confidence markers added: N
```
