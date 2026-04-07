# Job Application Workflow

User provides URL → agent fills application → human reviews → submit.

---

## Steps

| Step | Action | Input | Output | Reference |
|------|--------|-------|--------|-----------|
| 1 | Onboard profile | Resume (file path or paste) | `profile.json` | → `references/profile-onboarding.md` |
| 2 | Load profile | `profile.json` path | Validated profile object | → `references/profile-schema.md` |
| 3 | Navigate | Application URL | Page loaded | — |
| 4 | Discover form | Page snapshot | Field map (labels, types, selectors) | → `references/form-discovery.md` |
| 5 | Map fields | Field map + profile | Mapped fields + unmapped flags | → `references/form-discovery.md` |
| 6 | Fill form | Mapped fields | Populated form | — |
| 7 | Handle questions | Custom/freeform fields | Generated answers | → `references/question-handling.md` |
| 8 | Review (HITL) | Filled form summary | Approve / edit / reject | → `references/hitl-review.md` |
| 9 | Submit | Approved form | Confirmation or dry-run log | — |

## Execution Flow

```
/job-apply [url]
  → Agent reads SKILL.md
  → Agent reads workflow.md
  → FOR step 1 to 9:
       READ reference file (if listed)
       EXECUTE step
       UPDATE state
  → /kernel/complete
```

## Multi-Page Handling

Some applications span multiple pages:

1. After filling visible form, detect "Next" / "Continue" button
2. `browser_click` to advance
3. `browser_snapshot` new page
4. Repeat steps 4-7 per page
5. Final page triggers step 8 (HITL review)

## State Tracking

| Field | Type | Purpose |
|-------|------|---------|
| `application_url` | string | Current job posting URL |
| `status` | enum | `loading` · `discovering` · `filling` · `review` · `submitted` · `rejected` |
| `fields_discovered` | number | Form fields found |
| `fields_mapped` | number | Successfully mapped to profile |
| `fields_unmapped` | number | Flagged for HITL |
| `questions_generated` | number | AI-generated answers |
| `pages_completed` | number | Multi-page progress |
| `dry_run` | boolean | Skip submit step |

## Error Handling

| Failure | Recovery |
|---------|----------|
| Page won't load | Report URL error, stop |
| Login wall | Flag HITL — user logs in, agent resumes |
| CAPTCHA | Flag HITL — user solves, agent resumes |
| File upload fails | Retry once, then flag HITL |
| Field fill fails | Try alternate selector, then flag HITL |
| Unknown form structure | Snapshot + flag entire page for HITL |
