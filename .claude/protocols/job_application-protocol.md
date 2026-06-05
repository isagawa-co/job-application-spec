# Job Application Protocol

**Domain:** job_application
**Type:** Indexed
**Created:** 2026-06-01T00:00:00Z

---

## Overview

Autonomous job application agent. User provides a URL and a profile — agent discovers the form,
fills it, and presents for human review before submit.

The agent uses universal form discovery (no ATS templates). Every page is treated as unknown.
Fields are mapped to a structured `profile.json`. Human reviews everything before submit.

---

## References

### Core Skill Files
| File | Purpose |
|------|---------|
| `.claude/skills/job-application/SKILL.md` | Identity, philosophy, tooling rules |
| `.claude/skills/job-application/workflow.md` | 9-step workflow with state tracking |

### Reference Files (Domain Rules)
| Category | Reference |
|----------|-----------|
| Profile onboarding | `.claude/skills/job-application/references/profile-onboarding.md` |
| Profile schema + field mapping | `.claude/skills/job-application/references/profile-schema.md` |
| Form discovery (universal) | `.claude/skills/job-application/references/form-discovery.md` |
| Question handling | `.claude/skills/job-application/references/question-handling.md` |
| HITL review before submit | `.claude/skills/job-application/references/hitl-review.md` |

### Entry Points
| Command | Purpose |
|---------|---------|
| `/job-apply [url]` | Apply to a job posting (supports `--dry-run`) |
| `/job-apply [url] --dry-run` | Fill form but do not submit |

### Kernel Commands
| Command | Purpose |
|---------|---------|
| `/kernel/session-start` | Initialize session (ALWAYS first) |
| `/kernel/anchor` | Re-read protocol + review work |
| `/kernel/learn` | Record lesson after failure |
| `/kernel/complete` | Final gate before done |
| `/kernel/fix` | Impact assessment before any fix |

### Lessons Learned
→ `.claude/lessons/lessons.md`

---

## Workflow (9 Steps)

```
/job-apply [url]
    │
Step 1: ONBOARD PROFILE
    Read profile.json (or create it from resume)
    Gate: required fields present, resume_path valid
    Ref: references/profile-onboarding.md
    │
Step 2: LOAD PROFILE
    Validate profile against schema
    Gate: personal.first_name, last_name, email non-empty
    Ref: references/profile-schema.md
    │
Step 3: NAVIGATE
    Open application URL via Playwright MCP
    Gate: page loads successfully
    │
Step 4: DISCOVER FORM
    Snapshot accessibility tree, extract all form fields
    Gate: field map generated with labels, types, selectors
    Ref: references/form-discovery.md
    │
Step 5: MAP FIELDS
    Match form fields to profile keys using label matching
    Gate: mapped vs unmapped fields identified
    Ref: references/form-discovery.md
    │
Step 6: FILL FORM
    Fill all mapped fields via browser_fill_form
    Gate: all mapped fields populated
    │
Step 7: HANDLE QUESTIONS
    Template bank first, Claude Code generation as fallback
    Gate: all questions answered, generated answers tagged
    Ref: references/question-handling.md
    │
Step 8: REVIEW (HITL)
    Present full summary — human approves, edits, or rejects
    Gate: human decision received
    Ref: references/hitl-review.md
    │
Step 9: SUBMIT
    Click submit (or log dry-run result)
    Gate: confirmation page captured
    │
COMPLETE → /kernel/complete → lesson recorded
```

---

## Patterns

- **Profile-driven** — All data comes from `profile.json`. Never hardcode applicant data.
- **Universal discovery** — Use `browser_snapshot` to discover forms. Never assume selectors.
- **MCP only** — All browser interaction via Playwright MCP (no WebFetch, no Selenium, no HTTP calls).
- **HITL gate** — Every application pauses at Step 8 for human review. Never skip.
- **Field mapping table** — See `references/profile-schema.md` for label → profile key mappings.
- **Question bank first** — Check `profile.question_bank` before generating answers.
- **Multi-page detection** — Detect Next/Continue buttons and re-run discovery per page.

## Anti-Patterns

- **Never submit without HITL approval** — Step 8 is non-negotiable.
- **Never use WebFetch for application pages** — Playwright MCP only.
- **Never hardcode ATS selectors** — Universal discovery every time.
- **Never skip profile validation** — Gate at Step 2 prevents bad submissions.
- **Never silently skip a field** — Every field appears in HITL review: mapped, unmapped, or failed.
- **Never call `/kernel/complete` without submitting or dry-run logging** — Gate is final.

---

*Protocol is an INDEX. Agent reads referenced files during /kernel/anchor.*
