---
name: pull-accomplishments
description: Scan work-brain (read-only) for Luis-attributed work and draft accomplishment entries
trigger: /pull-accomplishments
---

# /pull-accomplishments

Scan work-brain for things Luis shipped, led, decided, or unblocked — then draft accomplishment entries for review.

## Cross-Brain Access

This skill reads from `../work-brain/` (read-only). It never modifies work-brain files.

## Steps

1. Read `wiki/log.md` to find the date of the last pull (to avoid re-surfacing old material)
2. Read work-brain sources for Luis-attributed activity:
   - `../work-brain/tasks.md` — Completed section (tasks Luis finished since last pull)
   - `../work-brain/wiki/log.md` — recent ingest/eod entries for context
   - `../work-brain/wiki/features/` — scan for status changes, shipped features
   - `../work-brain/wiki/decisions/` — scan for decisions where Luis was involved
3. For each potential accomplishment found:
   - Check if it already exists in `wiki/accomplishments/` (dedup)
   - If new, draft an accomplishment page with: what, when, impact, role, source
4. Present all drafted accomplishments to the user for review:
   - "I found N potential accomplishments since {last pull date}:"
   - List each with a one-line summary
   - Ask: "Confirm all, or review one by one?"
5. For confirmed accomplishments:
   - Create pages in `wiki/accomplishments/`
   - Update `wiki/index.md`
6. Append to `wiki/log.md`
