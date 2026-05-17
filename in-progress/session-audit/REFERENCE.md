# Session Audit — Output Format Reference

## Part 1: Efficiency Score

```
## Session Efficiency Score: X/10

[One sentence summary of the biggest issue, or "No significant issues found."]
```

## Part 2: What Burned Tokens This Session

Ranked from highest to lowest estimated impact. Be specific — include file names, sizes, entry counts.

```
## Token Burn Breakdown

1. `~/.claude/CLAUDE.md` — 14,200 chars (~3,550 tokens loaded every turn)
2. `MEMORY.md` index — 22 entries, 5,800 chars (~1,450 tokens/turn)
3. `memory/feedback_testing.md` — 3,100 chars (~775 tokens/turn)
4. MCP servers — 6 servers loaded (~300–600 tokens/turn for tool definitions)
5. No `.claudeignore` — any file reads include noise dirs (node_modules, .git, dist)
```

If a finding is unavailable (e.g. can't read a file), note it with "— could not read" rather than omitting it.

## Part 3: Recommended Changes

Ordered by ROI. For each item, provide:
- What to change (specific, not vague)
- Estimated token savings per turn
- Whether you'll ask before applying (always yes for file edits)

```
## Recommended Changes

### 1. Trim `~/.claude/CLAUDE.md` [saves ~1,200 tokens/turn]
The "Environment Setup" section (lines 45–120) duplicates what's in project CLAUDE.md.
Remove it or replace with a one-line pointer.
→ Apply this? (yes/no)

### 2. Prune stale memory entries [saves ~400 tokens/turn]
MEMORY.md has 8 entries older than 60 days with no recent references:
- `project_auth_migration.md` (project completed)
- `feedback_old_pr_style.md` (superseded)
[list them all]
→ Delete these entries? (yes/no)

### 3. Add `.claudeignore` [saves tokens on any file read/search]
No .claudeignore found. Recommend creating one excluding:
node_modules/, .git/, dist/, build/, *.lock, coverage/
→ Create this file? (yes/no)

### 4. Reduce MCP servers [saves ~200 tokens/turn]
6 MCP servers loaded. Consider disabling servers not used in typical sessions.
Currently loaded: [list from settings]
→ This requires manual edit to settings.json — no auto-apply.
```

## Part 4: Session Efficiency Memory Entry

Pre-formatted, ready to save. Use the standard auto-memory frontmatter format.

```
## Session Efficiency Memory Entry

Save this to your Claude memory (or copy to Obsidian):

---
name: [project-name or "global"] token efficiency profile
description: Token efficiency audit findings for [project/context] — what burns tokens and how to improve
type: project
---

**Audit date:** [date]
**Session:** [session description if provided, else "general"]
**Score:** X/10

**Key findings:**
- [Most significant issue]: [specific detail]
- [Second issue]: [specific detail]

**Applied fixes:** [what was changed this session, or "none"]
**Still to do:** [what was recommended but not yet applied]

**Why:** Recurring token efficiency issues in this project reduce cache hit rate and increase context exhaustion risk in long sessions.
**How to apply:** Reference before starting a long session to know what to watch for.
---
```

## Cache Concepts Reference (for generating advice)

- Cache writes cost ~25% more than normal input tokens (one-time cost)
- Cache reads cost ~10% of normal input tokens (90% discount)
- Cache expires after ~5 minutes of inactivity — long pauses bust the cache
- What Claude Code caches automatically: system prompt, CLAUDE.md, tool definitions, earlier conversation turns
- Optimal CLAUDE.md size: under ~2,000 tokens for single cache block; larger files may span blocks reducing hit rate
- Short turns (under ~100 tokens) don't contribute to cache continuity well
- Each MCP server adds its tool schema to every context window — unused servers waste tokens every single turn
