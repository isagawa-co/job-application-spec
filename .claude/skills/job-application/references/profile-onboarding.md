# Profile Onboarding

Build `profile.json` from user's resume. Runs during domain setup.

---

## Trigger

- During `/kernel/domain-setup` — spec instructs agent to onboard profile
- On `/job-apply` if `profile.json` does not exist (fallback gate)

## Process

### 1. Collect Resume

Ask user for resume source:

| Method | How |
|--------|-----|
| File path | User provides path to PDF/DOCX/TXT on local machine |
| Paste | User pastes resume text directly into chat |

Read the file or pasted text. For PDF/DOCX, use the Read tool.

### 2. Extract Profile Data

Parse resume content and map to → `profile-schema.md` structure:

| Section | Extract |
|---------|---------|
| Personal | Name, email, phone, headline/summary |
| Location | Address, city, state, zip (if present) |
| Experience | Each role: company, title, dates, description |
| Education | Each entry: school, degree, field, year |
| Links | LinkedIn, GitHub, portfolio URLs |

### 3. Flag Gaps

Fields NOT typically on a resume — agent asks user directly:

| Field | Why Missing |
|-------|-------------|
| `preferences.salary_min/max` | Rarely on resumes |
| `preferences.remote_preference` | Personal preference |
| `authorization.work_authorized` | Legal status |
| `authorization.sponsorship_required` | Legal status |
| `question_bank.*` | Pre-written answers need user input |
| `resume_path` | Agent needs the actual file path for uploads |
| `cover_letter_path` | Optional — ask if user has one |

### 4. Present for Review (HITL)

Show the extracted profile as formatted JSON. User can:
- Confirm as-is
- Edit specific fields
- Fill in flagged gaps

### 5. Save

Write validated `profile.json` to project root.

## Updates

- User can edit `profile.json` directly anytime
- Or tell the agent to update specific fields
- Agent does NOT overwrite profile without user consent

## Anti-Patterns

- Do NOT guess legal/authorization fields — always ask
- Do NOT skip HITL review of extracted profile
- Do NOT store raw resume content in profile — only structured data
- Do NOT proceed with `/job-apply` if required fields are empty
