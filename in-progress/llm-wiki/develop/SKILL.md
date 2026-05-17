---
name: develop
description: Flesh out an existing idea through structured questioning, move status to developing
trigger: /develop [idea-name]
---

# /develop

Open an existing seed or idea and grill the user to flesh it out.

## Steps

1. Read `wiki/index.md` to find the idea
   - If no argument provided, list all seeds and ask which one to develop
   - If argument provided, find the matching idea page
2. Read the idea page
3. Ask questions one at a time to flesh out:
   - "What problem does this solve?"
   - "What's your rough approach?"
   - "What are the open questions or unknowns?"
   - "Who would you need buy-in from?"
   - "Any related ideas or prior art?"
4. After each answer, update the idea page with the new content
5. Update status from `seed` → `developing`, update the `updated` date
6. Update `wiki/index.md` — move from Seeds to Developing
7. Append to `wiki/log.md`
