# HITL Review

Human-in-the-loop review before submission. Every application pauses here.

---

## When Triggered

- Step 7 of workflow — after all fields are filled and questions answered
- Always triggered, regardless of confidence level

## Review Presentation

Present the completed application as a structured summary:

```
APPLICATION REVIEW
==================
URL: [application URL]
Position: [job title if detected]
Company: [company name if detected]

MAPPED FIELDS (from profile):
  First Name: John              ← personal.first_name
  Last Name: Doe                ← personal.last_name
  Email: john@example.com       ← personal.email
  Resume: uploaded              ← resume_path
  ...

GENERATED ANSWERS:
  Q: "Why are you interested in this role?"
  A: [generated answer]
  Source: Claude Code generated

  Q: "Describe a relevant project"
  A: [generated answer]
  Source: Claude Code generated

UNMAPPED FIELDS (need your input):
  ⚠ "Referral source": [empty — no profile match]
  ⚠ "Employee ID": [empty — no profile match]

FLAGGED ITEMS:
  ⚠ CAPTCHA detected — needs manual solve
  ⚠ Login required — needs manual auth

STATUS: Ready for review
==================
```

## User Actions

| Action | Result |
|--------|--------|
| **Approve** | Agent clicks submit |
| **Edit** | User provides corrections, agent updates fields |
| **Reject** | Agent stops, logs rejection, no submit |
| **Dry-run complete** | If `dry_run: true`, log result and stop |

## Edit Flow

When user provides corrections:
1. Agent updates the specific field(s) via `browser_fill_form`
2. Re-presents updated summary
3. Waits for final approve/reject

## Post-Submit

After successful submission:
1. `browser_snapshot` or `browser_take_screenshot` the confirmation page
2. Log result: URL, position, company, timestamp, status
3. Invoke `/kernel/complete`

## Anti-Patterns

- Do NOT auto-submit without review — even on dry-run
- Do NOT skip unmapped field warnings
- Do NOT hide generated answers — always show source
- Do NOT proceed if flagged items are unresolved
