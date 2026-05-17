---
name: system-audit
description: >
  [Med] 🚩 Check system health against context density premise — CLAUDE.md bloat,
  skill file sizes, permission gaps, orphan skills, cold start cost, and more.
  Flags: `deep` [High] — adds duplication and parent tax analysis (+3-5k tokens).
  Use when user says "/system-audit", "audit my system", "check system health",
  or wants to optimize their Claude setup.
---

# /system-audit

System health check against the context density premise.

## Workflow

Mode: default (unless $ARGUMENTS contains "deep")

### Step 1: Read registry

Read `~/.claude/system-registry.md` to get all context paths.

### Step 2: Collect metrics (shell commands per context)

For each registered scoped context, run:

```bash
# Metric 1: CLAUDE.md size
wc -l <path>/CLAUDE.md

# Metric 2: Skills count
ls <path>/.claude/skills/ 2>/dev/null | wc -l

# Metric 3: Cross-brain skill ratio
# grep skill files for paths outside their own context
grep -rl "second-brains/" <path>/.claude/skills/ 2>/dev/null | grep -v "<own-context-name>"

# Metric 4: Cold start token cost
# Sum lines of CLAUDE.md + parent CLAUDE.md + index.md (if brain)
wc -l <path>/CLAUDE.md <parent>/CLAUDE.md <path>/wiki/index.md 2>/dev/null

# Metric 5: Skill file size
wc -l <path>/.claude/skills/*/SKILL.md 2>/dev/null
```

### Step 3: Qualitative checks

- **Permission gaps:** contexts with no explicit permissions entry in registry
- **Skill overlap:** skills with identical names across different contexts (e.g., "ingest" in 3 brains — flag it, might be intentional)
- **Orphan skills:** `ls` each context's `.claude/skills/` and compare against what's listed in the registry

### Step 4: Apply traffic lights

| Metric | Green | Yellow | Red |
|--------|-------|--------|-----|
| CLAUDE.md size | <150 lines | 150-300 lines | >300 lines |
| Skills count | <10 | 10-15 | >15 |
| Cross-brain ratio | 0-1 skills | 2-3 skills | >3 skills |
| Cold start tokens | <5k | 5-10k | >10k |
| Skill file size (SKILL.md only, not REFERENCE.md) | <80 lines | 80-150 lines | >150 lines |

### Step 5: Output report

```
# System Audit — YYYY-MM-DD

## context-name (type) — overall: GREEN/YELLOW/RED

| Metric | Value | Status |
|--------|-------|--------|
| CLAUDE.md size | N lines | G/Y/R |
| Skills count | N | G/Y/R |
| Cross-brain ratio | N/total | G/Y/R |
| Cold start | ~Nk tokens | G/Y/R |
| Largest skill | name (N lines) | G/Y/R |

Recommendations:
- specific recommendation if any metric is yellow/red

## Qualitative Issues

- Permission gaps: [list or "none"]
- Skill overlap: [list or "none"]
- Orphan skills: [list or "none"]
```

### Deep mode (only when $ARGUMENTS contains "deep")

Additional steps after the default pass:

6. Read all CLAUDE.md files across contexts. Flag sections with >70% text similarity.
7. Read parent-level CLAUDE.md files. Flag sections that reference only one child context (should be moved down).

Also read `~/.claude/system-reference.md` for cross-brain relationship context.

### Step 6: Append to audit log

Append a summary entry to `~/.claude/audit-log.md` (create if it doesn't exist):

```markdown
## [YYYY-MM-DD] [mode: default|deep]

| Context | Overall | Notes |
|---------|---------|-------|
| orchestrator | 🟢/🟡/🔴 | e.g. "17 orphan skills" |
| work-brain | 🟢/🟡/🔴 | e.g. "morning-briefing 172 lines" |
| ...

Actions taken this session: [list or "none"]
```

Only include contexts that are not 🟢, unless all are green (then include all with a one-liner).

## Rules

- Traffic light thresholds are initial guidelines — don't be rigid about borderline cases
- Skill overlap is not always bad (e.g., "ingest" in each brain is intentional by design). Note it but don't alarm.
- Recommendations must include specifics: what file, how many lines, what to do. No vague advice.
- Overall context status = worst metric status (one red metric = red context)
- Run shell commands in batches where possible to minimize tool calls
