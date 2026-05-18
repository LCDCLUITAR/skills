---
name: unpark
description: [Med] Loads a parking-lot file into context so the current session can resume parked work. Use when user explicitly calls /unpark.
argument-hint: "Optional partial slug to fuzzy-match (e.g. auth)"
---

1. List files in `~/.claude/parking-lot/` sorted newest-first:
   ```bash
   ls -t ~/.claude/parking-lot/
   ```

2. Resolve which file to load:
   - **0 files** → tell the user the parking lot is empty, stop
   - **1 file** → load it automatically
   - **Argument passed** → case-insensitive substring match on filename; if exactly one match load it, otherwise fall back to list behavior
   - **2+ files (no match)** → display numbered list and ask the user to pick

3. Read the resolved file and summarize its contents to the user — cover Context, Decisions, Next Steps in brief

4. If **Suggested Skills** is non-empty, recommend those skills for this session
