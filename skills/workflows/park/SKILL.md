---
name: park
description: "[Med] Saves the current session to ~/.claude/parking-lot/ as a structured handoff file. Use when user explicitly calls /park."
argument-hint: "Optional slug (e.g. auth-refactor)"
---

1. Get timestamp: `date +%Y-%m-%dT%H-%M`
2. Build filename: `<timestamp>[-<slug>].md` — save to `~/.claude/parking-lot/` (create dir if needed)
3. Review the conversation and identify which skills the next session should invoke (if any)
4. Write the file — **delta only**, reference existing memory/plans/ADRs/commits by path, do not restate them:

```md
## Context

## Decisions

## Next Steps

## Suggested Skills
<!-- List skills by name the next session should invoke. Leave blank if none. -->
```

5. Tell the user the full saved path.

See [REFERENCE.md](REFERENCE.md) for what belongs in each section.
