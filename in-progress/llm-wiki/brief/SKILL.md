---
name: brief
description: Generate a pre-meeting briefing for a given topic or meeting name. Pulls related feature, decision, and people pages. Use when user says "/brief [topic]", "brief me on [topic]", or when morning-briefing skill requests a meeting briefing.
---

# Brief

## Workflow

The meeting or topic is: $ARGUMENTS

1. Search `wiki/index.md` for the topic — match against features, people, decisions
2. Pull all related pages: feature page, relevant decision pages, people pages for likely attendees
3. If the topic references Jira tickets (by number or by name), fetch them using `acli jira --action getIssue --issue CCA-XXXX` for each ticket. Include summary, description, and acceptance criteria in your analysis.
4. Generate a structured briefing:

---
## 📋 Briefing: [topic]

### What this is about
[1-2 sentence context from the wiki]

### ✅ Decisions already made
[bullet list with links to decision pages]

### ❓ Open questions / blockers
[extracted from feature pages, synthesis pages, and Jira ticket ACs]

### 👥 People involved
[relevant people with brief context]

### 💬 Suggested talking points
[based on open tasks, recent decisions, and known blockers]

### 📐 Suggested story structure (refinement only)
[Only include this section if the meeting is a refinement session or the topic involves story sizing/splitting]

Analyze all tickets together. If any stories are too thin to deliver independently testable value, propose a consolidation. If any story is oversized (>5 tasks), propose a split. Follow the team's Jira work structure: 3–5 tasks per story, each story delivers end-to-end testable value.

For each proposed story:
- **Story N: [Title]**
  - Merges/replaces: [ticket numbers]
  - Why: [one sentence on why this grouping makes sense]
  - Tasks: [bullet list of 3–5 concrete implementation tasks]
  - Blocker / DoR gap: [any missing designs, decisions, or spikes that must complete first]

If the tickets are already well-scoped, say so and skip the restructure.

---

5. Append a query entry to `wiki/log.md`
