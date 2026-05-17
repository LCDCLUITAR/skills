---
name: system-where
description: >
  [Low] Locate skills, MCPs, hooks, or scoped contexts by keyword with fuzzy
  matching across all registry fields. Use when user says "/system-where",
  "where is", "find skill", "which brain has", or asks where something lives
  in their Claude system.
---

# /system-where

Fuzzy search across the system registry.

## Workflow

Search query: $ARGUMENTS

1. Read `~/.claude/system-registry.md`
2. Search all fields for matches against the query:
   - Context names (exact and partial)
   - Skill names (exact and partial)
   - Domain descriptions (keyword match)
   - MCP names
   - Hook names
3. Rank results: exact match > partial name match > domain keyword match
4. Output matches in this format:

```
Found N match(es) for "query":

  context-name (type)
  ├── Matched: field-name — "matched text"
  └── Path: ~/path/to/context
```

If no matches: suggest checking spelling or running `/system-scan` if the context might not be registered.

## Rules

- Case-insensitive matching
- Match substrings (e.g., "accomplish" matches "pull-accomplishments")
- If query matches a context name directly, show all its fields as a summary
- Keep output concise — no extra commentary
