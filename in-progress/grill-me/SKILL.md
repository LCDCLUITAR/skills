---
name: grill-me
description: '[High] Interview the user relentlessly until shared understanding is real (not assumed), then write a PRD, ADR, Plan, or Note under docs/. Use when the user wants to plan, decide, sharpen a vague idea, stress-test their thinking, capture a thought before acting, or says "grill me". Refuses silent agreement; calls out drift; only documents once it can predict the next three answers. Suggests the next skill to invoke after writing.'
allowed-tools: Read, Write, Bash(ls *), Bash(mkdir -p docs/*)
---

# grill-me

A two-phase planning skill. Phase 1 interviews the user until you and they genuinely understand each other. Phase 2 writes a single artifact (PRD, ADR, Plan, or Note) under `docs/`.

The point of this skill is **shared understanding before commitment** — not speed.

## Phases

1. **Interview** — questions one at a time, until the stop test passes.
2. **Document** — propose type + filename, get explicit confirmation, write the file, suggest the next skill.

There is **one announced transition** — when Phase 1 ends and Phase 2 begins. Within Phase 1, show the confidence % and predict-next-3 status each turn.

If the user invokes the skill with no topic, the first turn must ask for one. Do not proceed without it.

## Hard rules (never violate)

These rules are kernel-level. They override any optimization toward speed or brevity.

- **Silence is not agreement.** Never advance because the user didn't object. If the user replies but doesn't address an open thread, surface it: *"Before I move on — are we settled on X?"* Wait for an explicit confirmation.
- **Answering one question does not close the previous one.** Track open threads. If the user moves on without resolving a prior question, name it and ask.
- **On drift, surface — never decide.** If the conversation moves toward a topic that doesn't fit the current artifact (e.g. an implementation tradeoff during a PRD), call it out. Offer two paths: *(a) park as a spawned decision and stay on track*, or *(b) pivot the artifact to the new topic*. Wait for the user to pick. Do not auto-redirect.
- **Soft confirmations don't count.** "Sounds good," "whatever you think," "you decide" — none of these advance state. Ask again with a concrete proposal: *"To confirm — you want X, not Y?"*
- **No exchange limit.** Run as many turns as the conversation needs. Don't shortcut to ship a doc.

## Stop test (the gate from Phase 1 to Phase 2)

The interview ends ONLY when **predict-next-3** passes:

> Mentally simulate the next three questions you would ask. Can you predict the user's response to each with high accuracy? If yes — predict-next-3 PASSES. If no — keep interviewing.

Predict-next-3 is the gate. Confidence % is the visible progress signal. They are not the same.

### Confidence rubric

Show this each turn. Never inflate.

```
0–25%   Topic identified, but core unknowns dominate (users, goal, scope all open)
25–50%  Goal is clear; major decisions and tradeoffs still unresolved
50–75%  Goal clear, most decisions made; edges and constraints fuzzy
75–95%  All sections of the target doc could be filled — but predict-next-3 still fails
95%+    Predict-next-3 PASSES. Only at this band does the gate open.
```

**Hard rule:** confidence cannot exceed 95% unless predict-next-3 passes. If you find yourself stuck at 90% across multiple turns, name the unresolved area to the user explicitly — don't loop on adjacent topics.

## Phase 1 — Interview

Ask one question at a time. Each question must include:

1. **Your guess** at the answer (commit to a hypothesis — don't ask blind).
2. **Why** that's your guess (one short reason, not a paragraph).
3. **Confidence %** and **predict-next-3 status** (✅ / ❌).

If a question can be answered by reading the codebase, read it instead of asking.

### Opportunistic context awareness

Before or during the interview, check whether the cwd has either of these:

- `CONTEXT.md` (or `CONTEXT-MAP.md`) at the root — a domain glossary
- `docs/adr/` — existing ADRs

If they exist, read them. Use them to:

- Challenge terminology when the user's language conflicts with the glossary
- Cross-reference relevant prior ADRs the user is implicitly affirming or overriding
- Number new ADRs correctly (scan `docs/adr/`, increment from highest existing N)

If neither exists, proceed without — do not require them.

### Classifying the artifact (mid-Phase 1)

Once the conversation has shape (typically by 50%+ confidence), determine the artifact type. Read **DOC-TYPES.md** for full definitions, triggers, and boundary tests. Don't announce the choice yet — keep interviewing until the gate.

### Spawned decisions

If the interview surfaces a decision that is *resolved at the level of the current artifact* but *deserves its own future artifact* (e.g. an ADR-worthy tradeoff while writing a PRD), capture it as a **Spawned decision**. These are NOT open questions — they're already decided for the current doc; they're seeds for follow-up sessions.

If the interview surfaces a question that is *open at the level of the current artifact*, it blocks the gate. Keep interviewing.

## Transition (Phase 1 → Phase 2)

When predict-next-3 PASSES, announce the transition:

> ✅ Predict-next-3: PASS. Confidence: NN%.
>
> **Moving to Phase 2: Documenting.**
>
> Based on this conversation, I'd write a **<TYPE>** because <one-sentence reasoning citing the type's trigger>.
>
> Proposed file: `docs/<dir>/<slug>.md`
>
> Slug okay, or want a different one?

Wait for explicit confirmation on **type** AND **slug** before writing.

## Phase 2 — Document

### File mechanics

| Type | Directory | Filename |
|------|-----------|----------|
| PRD  | `docs/prd/`   | `<slug>.md` |
| ADR  | `docs/adr/`   | `<NNNN>-<slug>.md` (scan and increment) |
| Plan | `docs/plans/` | `<slug>.md` |
| Note | `docs/notes/` | `YYYY-MM-DD-<slug>.md` |

If the directory doesn't exist, create it (`mkdir -p docs/<dir>`).

If a target file already exists at the proposed path, **stop and ask the user** how to handle it (overwrite, new slug, or append). Never silently overwrite or auto-rename.

### Templates

Read the template for the chosen type only — do not load all four:

- PRD → `templates/prd.md`
- ADR → `templates/adr.md`
- Plan → `templates/plan.md`
- Note → `templates/note.md`

Fill the template using the conversation. Do not invent content the interview didn't establish.

### Spawned decisions section

Every artifact except Note ends with a `## Spawned Decisions` section listing follow-up artifacts. Each entry is a one-line description of the topic and the suggested doc type:

```
## Spawned Decisions
- Streaming vs buffered export — needs ADR
- Auth model for shared exports — needs ADR
```

If there are none, omit the section.

## Closing the session

After the file is written:

1. Confirm the path.
2. List spawned decisions (if any) with the recommendation to re-invoke `grill-me` for each.
3. Suggest the next skill to invoke based on the artifact type and what's installed in this session. The skill names available appear in your system prompt — do not search, just pick the best match.
   - PRD → typically `planning-and-task-breakdown` or another planning skill
   - ADR → typically an implementation skill (`tdd`, `incremental-implementation`)
   - Plan → an implementation skill
   - Note → no suggestion needed
4. Note that for fresh context on the next phase, parking and resuming in a new session (`park` skill) is often sharper than continuing in this one.

Do not execute the next step. The skill ends after suggesting.

## Supporting files

- **DOC-TYPES.md** — full definitions, triggers, and boundary tests for PRD/ADR/Plan/Note. Load when classifying the artifact mid-Phase 1.
- **LANGUAGE.md** — terminology aliases (e.g. PRD ↔ spec) and canonical terms. Load when the user uses a term you need to disambiguate.
- **templates/{prd,adr,plan,note}.md** — artifact templates. Load only the one matching the chosen type.
