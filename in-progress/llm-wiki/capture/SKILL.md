---
name: capture
description: Save raw content (transcript, note, brain dump, draft, spec) pasted directly into the conversation to the correct raw/ subfolder, then ingest or save based on content type. Use when user says "/capture", "capture this", "save this", "log this", or pastes a block of content they want added to the work-brain.
---

# Capture

## Quick start

User pastes content (or points to it). You:
1. Detect content type
2. Pick the right `raw/` subfolder and generate a filename
3. Ask to confirm or override if ambiguous
4. Write the file
5. Ingest immediately, save only, or ask — based on content type rules below

---

## Content type rules

| Type | Subfolder | Ingest? |
|---|---|---|
| Meeting transcript | `raw/transcripts/YYYY-MM/` | Always — ingest now |
| Post-meeting notes | `raw/notes/YYYY-MM/` | Always — ingest now |
| Brain dump / quick thought | `raw/notes/YYYY-MM/` | Always — ingest now |
| Draft spec / proposal | `raw/docs/` | Save only — still forming |
| Reference / external doc | `raw/docs/` | Save only — review first |
| WIP working document | `raw/docs/` | Save only — not final |
| Ambiguous | Ask | "Ingest now or save for later?" |

**Rule of thumb:** if it's a record of something that already happened → ingest now. If it's still forming or external → save only.

---

## Workflow

### 1. Detect content type

Read the content and/or the user's description. Look for signals:
- Speaker turns, timestamps, "meeting" language → transcript
- First-person narrative, loose thoughts → note/brain dump
- "Draft", "proposal", "spec", "WIP" → save only
- Copied from external source → save only

If ambiguous, ask: **"Is this a record of something that already happened, or still a work in progress?"**

For transcripts specifically: **never ask — always ingest now.**

### 2. Generate filename

Format: `YYYY-MM-DD-<slug>.md`

- Date = today's date
- Slug = 3–5 word kebab-case summary derived from content (e.g., `script-management-refinement`, `superuser-agency-field-notes`, `wolfe-gift-card-discovery`)
- For transcripts: prefix with meeting type if clear (e.g., `tech-solutioning-script-management`)

### 3. Strip noise before writing

Before writing, remove any Teams image blob lines matching the pattern:
```
![](https://nam.loki.delve.office.com/...)
```
Strip these lines entirely — they are JWT-signed CDN URLs that are large, ephemeral, and unreadable. All other content writes as-is.

### 4. Write the file

Write the cleaned content to the target path. Do not modify or summarize beyond the noise stripping above — raw/ files are immutable source records once written.

### 4. Ingest or save

**Ingest now:** Follow the full ingest workflow (compile into wiki pages, extract tasks to Needs Review, append to log.md, update index.md). Reference the ingest skill.

**Save only:** Confirm the file path to the user. Done.

---

## Example filenames

| Content | Path |
|---|---|
| Teams transcript from refinement meeting | `raw/transcripts/2026-05/2026-05-06-refinement-script-management.md` |
| Quick thought after a call | `raw/notes/2026-05/2026-05-06-post-call-wolfe-gift-cards.md` |
| Draft spec for new feature | `raw/docs/2026-05-06-future-credits-spec-draft.md` |
