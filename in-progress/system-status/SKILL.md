---
name: system-status
description: >
  [Low] Global view of all scoped contexts — summary dashboard with counts
  and a tree view of the system (max 3 levels). Use when user says
  "/system-status", "show my system", "what does my setup look like", or
  asks for an overview of their Claude environment.
---

# /system-status

Global system dashboard.

## Workflow

1. Read `~/.claude/system-registry.md`
2. Compute and output summary:
   - Total scoped contexts (broken down by type: brains, projects, orchestrator)
   - Total skills (sum across all contexts)
   - Active MCPs (list names, or "none" if all none)
   - Hooks configured (count + names)
3. Output tree view grouped by scan path, max 3 levels deep:
   - Level 1: parent directory
   - Level 2: context name + type + domain one-liner
   - Level 3: skills list (truncate with "..." if more than 6)
4. If any context has "none" for skills, MCPs, hooks — omit those lines in the tree (only show what exists)

## Output format

```
Scoped Contexts: N (X brains, Y projects, Z orchestrator)
Total Skills: N
MCPs: none active | list
Hooks: N (names)

~/.claude/                          (orchestrator)
└── skills/: system-where, system-status, system-audit, system-scan

~/Documents/second-brains/
├── work-brain/                     brain | team delivery, tasks
│   └── skills/: ingest, capture, morning-briefing, eod, ...
├── ideas-brain/                    brain | idea lifecycle
│   └── skills/: idea, develop, propose, review-ideas, ingest
└── career-brain/                   brain | accomplishments, goals
    └── skills/: pull-accomplishments, prep-review, ingest

~/dev/
└── Tools/agent-skills/             project | skill development
    └── skills/: spec-driven-development, planning-and-task-breakdown, ...
```

## Rules

- Keep output scannable — no prose, just structured data
- Truncate skill lists at 6 with "..." suffix
- Contexts with no skills don't get a skills line in the tree
- Use box-drawing characters for the tree (├── └── │)
