# Language

Terminology aliases and canonical terms used by `grill-me`. Load on demand when the user uses a term that needs disambiguation, or when proposing the doc type and the canonical name might surprise them.

## Canonical doc-type names

| Canonical | Aliases the user might say |
|-----------|----------------------------|
| **PRD**   | spec, product spec, feature spec, requirements doc, brief, design doc (sometimes) |
| **ADR**   | decision record, architecture decision, design decision, RFC (sometimes) |
| **Plan**  | implementation plan, task breakdown, todo, roadmap (small-scale), checklist |
| **Note**  | memo, jot, scratchpad, idea, capture |

If the user asks for a "spec," default to **PRD** but say so out loud: *"I'll write this as a PRD — same thing as a spec, this is just the canonical name in the skill."*

If the user asks for an "RFC," it depends — typically that's an ADR. Confirm with them by stating the boundary test: *"Is this a single decision with alternatives (ADR) or describing the whole feature (PRD)?"*

## Sharpening fuzzy language

When the user uses overloaded terms, propose precise alternatives. Examples:

- **"Account"** — Customer, User, Tenant, Workspace? Force a choice.
- **"Service"** — Module, Microservice, Server, Endpoint?
- **"User"** — End user, Admin, Internal user, Customer? In a B2B product these are very different.
- **"Owner"** — Code owner, Product owner, Data owner, On-call?
- **"Item"** — In an e-commerce context: Product, SKU, LineItem, Order? Each has different identity rules.

Sharpening is a two-step ask:
1. Surface the ambiguity: *"You're saying 'X' — that could mean A or B. Which?"*
2. Once chosen, lock it: *"OK, locking 'X' = A throughout this session."* If a `CONTEXT.md` exists, optionally update it.

## Project-specific glossary

If a `CONTEXT.md` (or `CONTEXT-MAP.md`) exists at the cwd root or under nested context directories, treat its definitions as authoritative. When the user uses a term that conflicts with the glossary, surface it:

> *"Your `CONTEXT.md` defines 'Cancellation' as X, but you seem to mean Y here. Which is it?"*

If no `CONTEXT.md` exists, do not require one. Sharpening still happens via the rules above.
