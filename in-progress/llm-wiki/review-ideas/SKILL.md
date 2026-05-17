---
name: review-ideas
description: Surface stale seeds and force a keep/develop/kill decision on each
trigger: /review-ideas
---

# /review-ideas

Review the idea backlog to prevent rot. Surface stale seeds and force decisions.

## Steps

1. Read `wiki/index.md`
2. Identify review candidates:
   - All seeds older than 14 days
   - All ideas in `developing` status older than 30 days without updates
3. If no candidates: report "Backlog is healthy — nothing stale." and stop
4. Present each candidate one at a time with:
   - Title and one-liner
   - Age (days since created)
   - Last updated date
5. For each, ask: "Keep (leave as-is), Develop (run /develop now), or Kill (abandon)?"
6. Based on answer:
   - **Keep**: no change, move to next
   - **Develop**: transition into the `/develop` flow for that idea
   - **Kill**: update status to `abandoned`, add reason if given, update `updated` date
7. After all candidates reviewed:
   - Update `wiki/index.md` (move any killed ideas to Abandoned)
   - Append summary to `wiki/log.md`: seeds reviewed, kept, killed, developed
