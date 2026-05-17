---
name: lint-wiki
description: Health check on the wiki. Finds orphan pages, broken links, index gaps, contradictions, missing pages, stale tasks, and unverified claims. Use when user says "/lint-wiki", "lint", "check the wiki", or "wiki health check".
---

# Lint Wiki

## Workflow

### 1. Run all checks

**a. Orphan pages** — wiki pages with no inbound wikilinks from any other page

**b. Broken wikilinks** — links that point to pages that don't exist as files

**c. Index gaps** — pages that exist in `wiki/` but aren't listed in `wiki/index.md`

**d. Index ghosts** — pages listed in `wiki/index.md` that don't exist as files

**e. Contradictions** — same decision or fact described differently across pages (use judgment; flag only clear conflicts, not nuance differences)

**f. Missing pages** — concepts, people, or features mentioned frequently across pages but lacking their own page

**g. Stale pages** — pages with `updated` date older than 90 days (not archived)

**h. Orphaned tasks** — tasks in `tasks.md` whose source wiki pages no longer exist

**i. Unverified claims** — grep all wiki pages for `⚠️ unverified`. List each one with its page and the claim text.

**j. Thin single-source pages** — pages whose content derives entirely from one transcript and has never been confirmed. Flag these as candidates for verification.

**k. Others task staleness** — tasks in `## 👥 Others` older than 7 days. These should be deleted (not moved to stale).

**l. Needs Review age** — tasks in `## 🔍 Needs Review` older than 30 days. Surface these for user decision.

### 2. Report

```markdown
## 🔍 Lint Report — YYYY-MM-DD

### 🔴 Critical (fix now)
- Broken wikilink: [[people/jane-doe]] referenced in [[features/script-management]] — page does not exist
- Index ghost: [[features/old-feature]] in index.md — file not found

### 🟡 Moderate
- Orphan page: [[concepts/some-concept]] — no inbound links
- Index gap: wiki/people/new-person.md exists but not in index.md

### ⚠️ Unverified Claims
- [[people/brad-momberger]] — "Brad owns the DataDog integration" — single source: April 29th DSU
- [[features/script-management]] — "script types stored in DB table" — single source: April 30th Refinement

### 🟠 Stale Tasks (Others > 7 days — will delete)
- "Sam to document feature flag process" — 2026-04-27 (8d old)

### 🔵 Needs Review Aging (> 30 days)
- "Clean up Lighthouse perf tests" — 2026-04-05 (30d old) — confirm, move to My Tasks, or reject?

### ℹ️ Minor
- Stale page: [[decisions/old-decision]] — last updated 2025-11-01 (185 days ago)
- Missing page: "Jhon Mora" mentioned in 4 pages — no wiki/people/jhon-mora.md

### 📊 Summary
- X issues found
- Y recommended to fix now
- Z unverified claims need your confirmation
```

### 3. Ask what to fix

"Should I fix any of these now?"

For unverified claims: go through each one — "Confirm or remove?"
For Others stale tasks: delete them automatically (no confirmation needed — 7-day rule).
For Needs Review aging: present each one for user decision.
For everything else: fix on user instruction.

### 4. Append to log.md

```markdown
## [YYYY-MM-DD] lint
- Issues found: N
- Issues resolved: N
- Unverified claims surfaced: N
- Notes:
```
