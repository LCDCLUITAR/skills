---
name: verify
description: Scan the wiki for all ⚠️ unverified claims, grill the user on each one, and update pages with confirmed facts or remove disproven claims. Use when user says "/verify", "verify claims", "check unverified", "resolve unverified", or wants to work through outstanding uncertainty markers in the wiki.
---

# Verify

## Quick start

Grep all wiki pages for `⚠️ unverified`. Present each claim to the user one at a time. Update the page based on their answer. Remove markers when resolved.

---

## Workflow

### 1. Collect all unverified claims

Grep `wiki/` (not `log.md`) for `⚠️ unverified`. For each hit, record:
- Page path
- The full sentence or bullet containing the marker
- Any context from surrounding lines that helps frame the question

If nothing is found, report "No unverified claims in the wiki." and stop.

### 2. Grill the user on each claim

Present claims one at a time. For each:

```
**[N of M] [[page/name]]**
> "Claim text here ⚠️ unverified"

Is this accurate? (Confirm / Correct / Remove)
```

- **Confirm** — remove the `⚠️ unverified` marker; optionally note the source of confirmation
- **Correct** — replace the claim with the correct information; remove the marker
- **Remove** — delete the claim entirely (it was wrong or no longer relevant)
- **Skip** — leave as-is for now (still unverified; do not remove marker)

Ask follow-up questions if the user's answer is ambiguous or introduces new information worth capturing.

### 3. Apply updates

After each answer (don't batch — update immediately so nothing is lost):
- Edit the wiki page to reflect the confirmed, corrected, or removed claim
- Update the page's `updated` frontmatter date

### 4. Summarize and log

After all claims are processed:

```markdown
## Verify session — YYYY-MM-DD
- Claims reviewed: N
- Confirmed: N
- Corrected: N
- Removed: N
- Skipped: N
```

Append to `wiki/log.md`:

```markdown
## [YYYY-MM-DD] verify
- Claims reviewed: N
- Confirmed: N | Corrected: N | Removed: N | Skipped: N
- Notes: [any notable corrections]
```

---

## Notes

- Do NOT grep `log.md` — unverified notes in log entries are historical, not live claims.
- If confirming a claim reveals new facts worth capturing elsewhere, update related pages too.
- If a correction changes a key decision or fact, check whether other pages reference the same claim and update them.
