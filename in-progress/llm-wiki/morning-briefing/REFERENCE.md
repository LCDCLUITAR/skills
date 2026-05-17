# Morning Briefing — Reference

## MR Query

Single GraphQL command. Replace `THIRTY_DAYS_AGO` with ISO date 30 days before today (e.g. `2026-04-06T00:00:00Z`).

```bash
glab api graphql -f query='
{
  project1: project(fullPath: "yumbrands/PHUS/call-center/ph-cc-supreme-ui") {
    mergeRequests(reviewerUsername: "LXC3592", state: opened, createdAfter: "THIRTY_DAYS_AGO") {
      nodes {
        iid title author { name } createdAt webUrl
        reviewers { nodes { username mergeRequestInteraction { reviewState } } }
      }
    }
  }
  project2: project(fullPath: "yumbrands/PHUS/call-center/ph-cc-addon-api") {
    mergeRequests(reviewerUsername: "LXC3592", state: opened, createdAfter: "THIRTY_DAYS_AGO") {
      nodes {
        iid title author { name } createdAt webUrl
        reviewers { nodes { username mergeRequestInteraction { reviewState } } }
      }
    }
  }
}' | jq '
[
  .data | to_entries[] | .key as $repo |
  .value.mergeRequests.nodes[] |
  select(.title | test("^Draft:") | not) |
  {
    repo: (if $repo == "project1" then "ui" else "api" end),
    iid: .iid,
    title: .title,
    author: .author.name,
    created: (.createdAt | split("T")[0]),
    url: .webUrl,
    myState: (.reviewers.nodes[] | select(.username=="LXC3592") | .mergeRequestInteraction.reviewState)
  }
] |
  (map(select(.myState == "REQUESTED_CHANGES")) | sort_by(.created)) as $requested |
  (map(select(.myState == "UNREVIEWED")) | sort_by(.created) | reverse) as $unreviewed |
  ($requested + $unreviewed) | .[0:5]'
```

Count total non-draft open MRs from the raw response to know if there are more than 5.

State → emoji: `REQUESTED_CHANGES` → 💬, `UNREVIEWED` → 🆕

## REQUESTED_CHANGES Enrichment

Run both calls per MR (in parallel). Project paths URL-encoded:
- ui → `yumbrands%2FPHUS%2Fcall-center%2Fph-cc-supreme-ui`
- api → `yumbrands%2FPHUS%2Fcall-center%2Fph-cc-addon-api`

**Unresolved threads + last review timestamp:**
```bash
glab api "/projects/PROJECT_ENCODED/merge_requests/IID/discussions" | jq '{
  unresolved: [.[] | select(.resolvable == true and .resolved == false)] | length,
  reviewedAt: ([.[] | .notes[] | select(.system == true and .author.username == "LXC3592" and (.body | ascii_downcase | contains("requested changes")))] | sort_by(.created_at) | last | .created_at)
}'
```

**Commits after review (skip if `reviewedAt` is null):**
```bash
glab api "/projects/PROJECT_ENCODED/merge_requests/IID/commits" | jq --arg since "REVIEWED_AT" '[.[] | select(.created_at > $since)] | length'
```

Display format — append `↳` line below each 💬 MR:
```
💬 [!NNN (repo)](url) — TICKET: Title (Author) — Xd old
   ↳ N new commits since your review · N unresolved threads
```
- `reviewedAt` null → omit `↳` line
- 0 new commits → "no new commits since your review"
- 0 unresolved threads → omit threads part
