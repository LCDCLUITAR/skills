---
name: eod
description: End-of-day reflection and wiki reconciliation for the work-brain. Asks 2 questions, updates the daily note, reconciles tasks.md, and updates wiki pages live. No raw dump files. Use when user says "/eod", "end of day", "wrap up today", or "close out the day".
---

# EOD (End of Day)

## Workflow

Run these steps in order.

### 1. Read context

- Get today's date (YYYY-MM-DD)
- Read `daily/YYYY-MM-DD.md` — extract the Top 3 from the morning section
- Read `tasks.md` — note all open tasks in My Tasks and Needs Review

### 2. Ask 2 questions (one at a time, wait for each answer)

**Q1:** "How'd it go with [priority 1], [priority 2], and [priority 3]?"
- For each item the user answers: done, partial, or didn't touch
- If partial: ask "What's left on [item]?" before moving on
- If no daily note exists for today: ask "What did you get done today?" instead

**Q2:** "Anything to dump? (freeform, optional — skip if nothing)"

### 3. Reconcile tasks.md (silent — no questions)

- Tasks from Q1 marked "done" → move to `## ✔️ Completed` with `[x]`
- Tasks from Q1 marked "partial" or "didn't touch" → ensure they're in My Tasks as carryovers; update description if Q1 clarified what's left
- Extract any action items from Q2 freeform → add to `## 🔍 Needs Review` (not My Tasks — they haven't been confirmed yet)
- Dedup check: if any extracted task already exists in My Tasks, skip it

### 4. Update wiki pages live

If Q2 contains:
- A decision made → update the relevant `decisions/` page or create a new one
- Context about a person → update their `people/` page
- A feature update → update the relevant `features/` page
- New concept or pattern → update or create a `concepts/` page

Do this now, not during next ingest. These are your words with full context.

### 5. Append EOD section to today's daily note

```markdown
## 🌙 EOD

### ✅ Done
- [items from Q1 marked done]

### ➡️ Carrying Over
- [items from Q1 marked partial or not touched, with what's left]

### 💭 Dump
[freeform from Q2 — omit this section entirely if Q2 was empty]
```

### 6. Append to log.md

```markdown
## [YYYY-MM-DD] eod
- Tasks completed: N
- Tasks carrying over: N
- Tasks extracted: N (to Needs Review)
- Wiki pages updated: [list or "none"]
```

## Edge cases

- **No daily note for today**: Create `daily/YYYY-MM-DD.md` with just the EOD section.
- **Empty Q2**: Skip the Dump section entirely — no "Nothing today" placeholder.
- **Task already completed in tasks.md**: Skip — don't double-move.
- **Q1 item not in tasks.md**: Still mark it done in the EOD section; don't add a new completed task entry for it.
