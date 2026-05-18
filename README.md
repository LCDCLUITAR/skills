# Skills

![Stable](https://img.shields.io/badge/stable-2_skills-blue)
![In Progress](https://img.shields.io/badge/in_progress-23_skills-yellow)
![External](https://img.shields.io/badge/external-managed_by_npx-gray)

A personal collection of Claude Code skills — curated, tested, and organized by lifecycle.

## Quickstart

```bash
npx skills add LCDCLUITAR/skills
```

## Skill Catalog

Only stable skills are listed here. In-progress, deprecated, and external skills live in their respective folders.

### Workflows

Session lifecycle and task flow.

| Skill | Description |
|-------|-------------|
| `park` | Save session context to a structured handoff file |
| `unpark` | Resume a previously parked session |

### Engineering

*No stable engineering skills yet.*

### Tooling

*No stable tooling skills yet.*

### Personal

*No stable personal skills yet.*

## Structure

```
skills/          Production-ready, organized by category (scanned by npx skills)
in-progress/     Under development — not yet reliable
deprecated/      Retired, kept for reference
external/        Third-party skills (managed by npx skills)
```

## Adding a Skill

1. Create a folder in `in-progress/<skill-name>/`
2. Add a `SKILL.md` with proper frontmatter (see `CLAUDE.md`)
3. Once stable, move to `skills/<category>/`
