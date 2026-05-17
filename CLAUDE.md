# Skills Repo Conventions

## Structure

- `stable/<category>/<skill-name>/` — production-ready, actively used
- `in-progress/<skill-name>/` — under development, not yet reliable
- `deprecated/<skill-name>/` — retired, kept for reference
- `external/<source>/<skill-name>/` — third-party skills, managed by `npx skills`

## Categories (stable/)

- `engineering/` — dev integrations, methodology, build/ship skills
- `tooling/` — meta skills, scaffolding, system utilities
- `personal/` — personal productivity, non-engineering
- `workflows/` — session lifecycle, planning, task flow

## Groupings (in-progress/)

- `llm-wiki/` — brain-scoped skills for wiki/knowledge-base management

## README Rules

The README only catalogs skills in `stable/`. Never add skills from `in-progress/`, `deprecated/`, or `external/` to the README. When a skill is promoted to stable, add it to the appropriate catalog table. When demoted or deprecated, remove it.

## Skill Format

Every skill folder must contain a `SKILL.md` with frontmatter:

```yaml
---
name: kebab-case-name
description: "One-line description. Use when..."
---
```

Optional files: `REFERENCE.md`, `TEST.md`, additional `references/` folder.

## Lifecycle

- New skills start in `in-progress/`
- Move to `stable/<category>/` when reliable and tested
- Move to `deprecated/` when retired (never delete — keep for reference)
- External skills live in `external/<source>/` until you fork and customize them, then they move to `stable/`
