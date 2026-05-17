---
name: idea
description: Fast-capture a new idea as a seed page with dedup check
trigger: /idea [description]
---

# /idea

Fast-capture an idea with zero friction.

## Steps

1. Read `wiki/index.md` to get the current list of ideas
2. Check for duplicates or similar ideas:
   - Compare the new idea against existing titles and descriptions
   - If a similar idea exists, ask: "I found something similar: [[ideas/existing-one]] — do you want to add to that one or create a new one?"
   - If no match, proceed silently
3. Create a new idea page at `wiki/ideas/{slugified-name}.md`:
   ```yaml
   ---
   title: {Idea Title}
   type: idea
   status: seed
   created: {today}
   updated: {today}
   tags: []
   ---
   ```
   Body: one-liner description from the user's input
4. Update `wiki/index.md` — add to Seeds section
5. Append to `wiki/log.md`
6. Report: "Captured: {title}" — nothing more
