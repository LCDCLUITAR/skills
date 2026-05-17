---
name: morning-briefing
description: Daily morning planning for Luis. Fetches MRs needing review, surfaces team blockers, asks for top 3 priorities, blockers, meeting briefings, and freeform thoughts. Writes daily/YYYY-MM-DD.md. Use when user says "/morning-briefing", "morning briefing", "start my day", or "good morning".
---

# Morning Briefing

## Workflow

Run these steps in order.

### 1. Auto-generate context (no questions yet)

**a. MRs Needing Review**

Run the GraphQL command from REFERENCE.md. Prioritize `REQUESTED_CHANGES` first (sorted oldest), then `UNREVIEWED` (sorted newest), cap at 5. For each `REQUESTED_CHANGES` MR, run the enrichment calls from REFERENCE.md to get unresolved thread count and new commits since last review.

**b. Team Waiting on You**

Read `tasks.md`. From the **Others** section, surface only tasks where Luis is explicitly named as the blocker (e.g. "waiting on Luis", "Luis to approve"). Maximum 3 items. Skip section if none.

**c. Weekly Calendar**

Check if today is Monday OR if `daily/` has no Monday note for this week with a `## 📅 This Week` section. If either is true, ask for the calendar screenshot before proceeding to questions (insert as Q0). Otherwise read `## 📅 This Week` from this week's Monday note and extract today's meetings.

### 2. Ask questions one at a time

**Q0 (Monday or missing calendar only):** "Paste your calendar for the week and I'll extract today's meetings and store the week view."

**Q1:** "Top 3 priorities today:"

**Q2:** "Anything blocking you? (skip if nothing)"

**Q3 (only if today has meetings):** "Want me to brief any of these? [list today's meetings]"
- If yes, run the `brief` skill for each requested meeting inline.

**Q4:** "Anything else on your mind?"

### 3. Write the daily note

Write to `daily/YYYY-MM-DD.md`. Omit sections with no content (except Top 3, Blockers, and MRs which always appear).

```markdown
---
date: YYYY-MM-DD
type: daily
---

# 📅 YYYY-MM-DD

## 🔀 MRs Needing Review
> 🆕 = not reviewed · 💬 = changes requested, waiting on updates · max 5 · last 30 days

- 💬 [!NNN (repo)](url) — TICKET: Title (Author) — Xd old
   ↳ N new commits since your review · N unresolved threads
- 🆕 [!NNN (repo)](url) — TICKET: Title (Author) — today
- _(+N more — check GitLab)_

## 🚦 Team Waiting on You
- Task description — [[people/person-name]]

## 🎯 Top 3
1. [priority 1]
2. [priority 2]
3. [priority 3]

## 🚧 Blockers
[answer or "None"]

## 💭 On My Mind
[freeform answer]

## 📅 Today's Meetings
- HH:MM — Meeting Name

## 📋 Meeting Briefings
[only if briefings were requested]
```

Also update `tasks.md` — replace the **🎯 Today's Priorities** section with today's top 3.

### 4. Archive last month's daily notes (first briefing of a new month only)

If today is the first day of a new month and `daily/` has notes from the previous month at the top level: create `daily/YYYY-MM/` and move those files in. When reading yesterday's note on the first of a month, check `daily/YYYY-MM/YYYY-MM-DD.md`.

### 5. Append to log.md

Follow the morning-briefing log format in `../CLAUDE.md`.
