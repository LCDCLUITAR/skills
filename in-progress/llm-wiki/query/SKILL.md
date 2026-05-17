---
name: query
description: Answer a question using the wiki as the knowledge source. Reads index, drills into relevant pages, synthesizes an answer with citations. Use when user says "/query [topic]" or asks a question about the wiki knowledge base.
---

# Query

## Workflow

The query topic is: $ARGUMENTS

1. Read `wiki/index.md` to find all pages relevant to the topic
2. Read each relevant page, following wikilinks to related pages as needed
3. Synthesize a clear answer with inline citations using wikilinks: `([[decisions/page-name]])`
4. Append a query entry to `wiki/log.md`
5. If the answer is substantive and reusable (not a one-off), ask: "This looks worth keeping — should I file it to wiki/synthesis/?"
   - If yes, create a synthesis page and update `wiki/index.md`
