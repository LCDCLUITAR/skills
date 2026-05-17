# Skills

A personal collection of Claude Code skills — organized by lifecycle and category.

## Quickstart

```bash
npx skills add lxc3592/skills
```

## Structure

```
stable/          Production-ready skills, organized by category
in-progress/     Work in progress — not yet reliable
deprecated/      Retired skills, kept for reference
external/        Third-party skills (managed by npx skills)
```

## Adding a Skill

1. Create a folder in `in-progress/<skill-name>/`
2. Add a `SKILL.md` with proper frontmatter (see CLAUDE.md for format)
3. Once stable, move to `stable/<category>/`
