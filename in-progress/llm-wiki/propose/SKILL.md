---
name: propose
description: Generate a shareable artifact from a developed idea and mark it as proposed
trigger: /propose [idea-name]
---

# /propose

Generate a shareable artifact (write-up, message draft, doc) from a developed idea.

## Steps

1. Read `wiki/index.md` to find the idea
   - If no argument, list all ideas with status `developing` and ask which one
2. Read the idea page — it must be in `developing` status
   - If still a seed, suggest running `/develop` first
3. Ask: "Where are you proposing this? (Slack message, doc, meeting talking points, email)"
4. Generate the artifact in the appropriate format:
   - **Slack message**: concise, conversational, includes the problem and proposed approach
   - **Doc/write-up**: structured with problem statement, proposal, tradeoffs, next steps
   - **Meeting talking points**: bullet points for verbal delivery
5. Present the artifact for review — let the user edit or approve
6. Update the idea page:
   - Status: `developing` → `proposed`
   - Add: where/when proposed, the artifact generated
   - Update the `updated` date
7. Update `wiki/index.md` — move from Developing to Proposed
8. Append to `wiki/log.md`
