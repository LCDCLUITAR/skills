---
name: session-audit
description: "[Med] 🚩 Token efficiency audit. Checks CLAUDE.md bloat, memory, .claudeignore, cache patterns. Use when user invokes /session-audit or asks to review session efficiency at end of session."
---

# Session Audit

Run at the end of a session to diagnose what burned tokens and how to improve next time.

## Quick Start

User invokes `/session-audit [optional: brief session description]`

Use the session description (if given) to contextualize findings — e.g. a "long refactor" makes repeated file reads more expected.

## Workflow

### Step 1 — Gather diagnostic data (run all in parallel)

- Read `~/.claude/CLAUDE.md` and any `CLAUDE.md` in the current project directory tree
- Read `~/.claude/projects/` memory directory: `MEMORY.md` index + all individual memory files
- Read `~/.claude/settings.json` and `.claude/settings.json` (if present) — count `mcpServers` entries
- Check for `.claudeignore` in the current working directory
- Estimate chars for each file (token estimate: chars ÷ 4)

### Step 2 — Score and rank findings

Score starts at 10. Deduct for each significant finding:
- CLAUDE.md over 8,000 chars (~2k tokens): **−1 per file**
- MEMORY.md over 4,000 chars or more than 15 entries: **−1**
- Any individual memory file over 2,000 chars: **−0.5 each**
- No `.claudeignore` present: **−1**
- 5+ MCP servers loaded: **−1**
- Cache-hostile pattern observed (very short turns, noted long pauses): **−0.5**

Minimum score: 1.

### Step 3 — Produce the 4-part report

See [REFERENCE.md](REFERENCE.md) for output format details.

### Step 4 — Apply approved changes

After showing Part 3, list all recommended changes and ask: "Which of these would you like me to apply now? (List numbers, or 'all', or 'none')"

Apply approved changes one at a time, showing a diff or summary before each edit.

## Key Rules

- **Never edit files without explicit user approval**
- Be specific with estimates: "~1,400 tokens/turn" not "some tokens"
- Tone: direct, like a performance profiler — no fluff
- If no issues found, say so plainly and give a 10/10
