# Doc Types

Use this file to classify what artifact the current grilling session should produce. Read mid-Phase 1, when the conversation has enough shape that one type is starting to feel right.

The four types are mutually exclusive within a single session (one doc per session). If two seem to fit, use the disambiguation rules at the bottom.

## PRD

**Definition:** A description of *what* is being built and *why*, before the *how*.

**Trigger:** Raw idea → needs definition. The user can't yet crisply answer "who is this for, what does success look like, what's out of scope."

**Sections:** Outcome, Users, Why now, Success, Scope, Out of scope, Spawned decisions.

**Not for:** Single tradeoffs (use ADR). Step ordering (use Plan).

**Boundary test:** If you removed every implementation detail and the doc was still useful → it's a PRD. If it collapsed → it's a Plan.

## ADR

**Definition:** A *single decision* between alternatives with non-trivial reversal cost.

**Trigger:** An existing or in-flight thing; the user is asking "should I do X or Y?" with real, distinct alternatives.

**Sections:** Context, Decision, Why this over alternatives, Consequences (optional), Spawned decisions.

**Not for:** Defining a new feature (use PRD). Describing how to do it (use Plan).

**Boundary test:** Can you state the decision in one sentence? If yes — ADR. If you need a paragraph, you're conflating multiple decisions; split or escalate to PRD.

**Numbering:** Scan `docs/adr/` for the highest existing N (e.g. `0007-foo.md` → highest is 7). Increment by one. Zero-pad to 4 digits.

## Plan

**Definition:** An *ordered set of steps* to execute a known goal.

**Trigger:** The *what* is clear; the *how* is not. The user wants sequencing across files or modules.

**Sections:** Goal, Preconditions, Steps (ordered), Verification, Spawned decisions.

**Not for:** Choosing what to build (use PRD). Justifying tradeoffs (use ADR).

**Boundary test:** Can each step be assigned to a single PR or commit? If steps are vague clusters of work, you're at PRD altitude — go back and grill the *what* first.

## Note

**Definition:** A *captured thought* without commitment to a next action.

**Trigger:** Doesn't fit the other three. The user wants to write something down without scoping it.

**Sections:** Title, Body (free-form), optional links.

**Not for:** Anything actionable. If there's a clear next step, it's a PRD, Plan, or ADR — not a Note.

**Boundary test:** Could this disappear and nothing breaks downstream? Then it's a Note.

## Cross-cutting boundary rules

- **PRD ↔ ADR.** PRD is the whole problem. ADR is one decision. A PRD can spawn many ADRs. A single decision is never a PRD.
- **PRD ↔ Plan.** PRD = *what + why*. Plan = *how + when*. A Plan presupposes PRD-level clarity (even if informal).
- **ADR ↔ Plan.** ADRs justify; Plans execute. *"Should we?"* → ADR. *"In what order?"* → Plan.

## When two seem to fit

- **PRD vs ADR:** *Is the user defining a new thing, or deciding about an existing thing?* Defining → PRD. Deciding → ADR.
- **PRD vs Plan:** *Does the user know what they're building?* No → PRD. Yes → Plan.
- **ADR vs Plan:** *Is the conversation about choosing between alternatives, or sequencing chosen work?* Choosing → ADR. Sequencing → Plan.

If still ambiguous after these tests, ask the user with both options framed concretely. Do not guess.
