---
name: prep-review
description: Assemble accomplishments and goals progress into a self-review draft
trigger: /prep-review
---

# /prep-review

Generate a self-review artifact from accumulated accomplishments and goals.

## Steps

1. Ask: "Which review period? (e.g., Q1 2026, H1 2026, 2025 annual)"
2. Read `wiki/index.md` to find all accomplishments and goals
3. Read accomplishment pages — filter to those within the review period
4. Read goal pages — pull progress notes and evidence for the period
5. Read `wiki/feedback/` — find any feedback from the period
6. Draft a self-review artifact structured as:
   - **Summary**: 2-3 sentence overview of the period
   - **Key accomplishments**: grouped by theme or goal area, with impact statements
   - **Goals progress**: status update on each active goal with evidence
   - **Growth areas**: what was learned, skills developed
   - **Looking ahead**: priorities for next period
7. Present draft for review — let the user edit, adjust tone, add context
8. Save to `wiki/artifacts/{period}-self-review.md`
9. Update `wiki/index.md`
10. Append to `wiki/log.md`
