---
name: system-scan
description: >
  [Med] Bootstrap or rescan — discover scoped contexts (directories with
  CLAUDE.md or .claude/) and register them in the system registry. First run
  asks for scan paths. Subsequent runs reuse saved paths. Use when user says
  "/system-scan", "rescan", "bootstrap registry", "find my contexts", or
  wants to refresh the system registry.
---

# /system-scan

Discover and register scoped contexts.

## Workflow

### Step 1: Determine scan paths

Read `~/.claude/system-registry.md`. Check if `## Scan Paths` section exists.

- **If exists:** use those paths
- **If missing (first run):** ask user "What directories should I scan for scoped contexts?" Save their answer as a `## Scan Paths` section at the top of the registry.

### Step 2: Discover candidates

For each scan path, run:

```bash
find <path> -maxdepth 4 \( -name "CLAUDE.md" -o -name ".claude" -type d \) \
  | grep -v node_modules \
  | grep -v "/.git/" \
  | grep -v vendor \
  | grep -v dist \
  | grep -v build \
  | grep -v ".venv"
```

### Step 3: Deduplicate and classify

- Resolve each hit to its parent directory (the context root)
- Deduplicate (a directory with both CLAUDE.md and .claude/ counts once)
- Classify type:
  - **brain** — has `wiki/` AND `raw/` directories
  - **project** — everything else
- Compare against existing registry entries to identify: new (unregistered), existing (already registered), stale (registered but not found on disk)

### Step 4: Present candidates

Show the user what was found, grouped:

```
New contexts found:
  ~/Projects/new-api/         project (has CLAUDE.md + .claude/skills/)
  ~/dev/experiments/foo/      project (has CLAUDE.md)

Already registered:
  work-brain, ideas-brain, career-brain, agent-skills, ui-libs

Stale (registered but not found on disk):
  [none or list]
```

Ask: "Which new contexts should I register? (all / list names / none)"

### Step 5: Collect metadata for confirmed contexts

For each new context to register:

```bash
# Domain: first non-heading, non-empty line from CLAUDE.md (or ask user)
head -20 <path>/CLAUDE.md

# Skills
ls <path>/.claude/skills/ 2>/dev/null

# MCPs, hooks, permissions: check for settings files
cat <path>/.claude/settings.json 2>/dev/null
cat <path>/.claude/settings.local.json 2>/dev/null
```

If domain isn't clear from CLAUDE.md, ask the user: "What's the one-line domain for [context-name]?"

### Step 6: Write to registry

- Add new entries under the appropriate group (Brains / Projects)
- If stale entries were found, ask whether to remove them
- Update `_Last updated:` date

### Step 7: Report

```
Registry updated:
  + context-name (type) — domain
  - removed-if-any

Total contexts: N (X brains, Y projects, Z orchestrator)
```

## Exclude patterns

Always filter out these directories during scan:
- `node_modules/`
- `.git/`
- `vendor/`
- `dist/`
- `build/`
- `.venv/`
- `__pycache__/`

## Rules

- Never auto-register without user confirmation
- Preserve existing registry entries — only add or remove, don't overwrite
- If a context is already registered, skip it silently (idempotent)
- For stale entries, always ask before removing (user might have moved the directory)
