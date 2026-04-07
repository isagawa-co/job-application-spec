# Job Application Agent

**Type:** Prescriptive (Template 1)
**Style:** Minimal — SKILL.md + workflow.md + references/
**Domain:** job-application

---

## What

Autonomous job application agent. User provides a URL and a profile — agent discovers the form, fills it, and presents for human review before submit.

## Philosophy

- **Universal discovery** — No ATS templates. Every page is unknown. Agent discovers form structure via Playwright MCP, maps fields to profile, fills.
- **Human-in-the-loop** — Auto-fill everything, pause before submit. Human reviews, edits, approves or rejects.
- **Profile-driven** — User maintains one JSON profile. All applications pull from it.
- **Learn from failure** — Form changed? Field missed? Kernel learn loop captures it.

## Tooling

All browser interaction: **Playwright MCP**

| Allowed | Not Allowed |
|---------|-------------|
| `browser_navigate` | WebFetch for application pages |
| `browser_snapshot` | Playwright CLI scripts |
| `browser_fill_form` | Selenium |
| `browser_click` | Direct HTTP/API calls |
| `browser_file_upload` | |
| `browser_take_screenshot` | |

## File Index

| File | Purpose |
|------|---------|
| `SKILL.md` | Identity, philosophy, file index |
| `workflow.md` | Step index + data flow + state tracking |
| `references/profile-onboarding.md` | Resume → profile extraction during setup |
| `references/profile-schema.md` | User profile JSON structure |
| `references/form-discovery.md` | How to discover any form via MCP snapshot |
| `references/question-handling.md` | Template bank + Claude Code fallback |
| `references/hitl-review.md` | Review/approval flow before submit |

## Integration

| Kernel Hook | Purpose |
|-------------|---------|
| Anchor | Re-read → `workflow.md` + `references/profile-schema.md` |
| Learn | Capture form discovery failures, unmapped fields, new question patterns |
| Complete | Application submitted or dry-run review passed |

## Setup Prerequisites

**During `/kernel/domain-setup`, BEFORE completing setup:**

1. **Profile onboarding is REQUIRED** — execute `references/profile-onboarding.md`
   - Ask user for resume (file path or paste)
   - Extract into `profile.json` at project root
   - Flag gaps, present for HITL review, save
2. **Do NOT mark setup complete until `profile.json` exists and passes validation**
   - Required fields: `personal.first_name`, `personal.last_name`, `personal.email`
   - `resume_path` must point to an existing file

This is not optional. The agent cannot apply to jobs without a profile.

## When Active

- User invokes `/job-apply`
- `session_state.json` has `active_workflow: "job-application"`
