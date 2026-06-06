# /job-apply

Apply to a job posting. User provides URL, agent handles the rest.

## Kernel Loop (REQUIRED)

```
┌─────────────────────────────────────────────┐
│ Step 1: Anchor (REQUIRED)                   │
│   → Invoke /kernel/anchor                   │
│   → Wait for confirmation                   │
│   → If fails: resolve before proceeding     │
├─────────────────────────────────────────────┤
│ Step 2: Execute Workflow                    │
│   → Read protocol → Workflow section        │
│   → Follow steps 1-9 in workflow.md         │
│   → On failure: STOP → /kernel/fix →        │
│     /kernel/learn → retry                   │
├─────────────────────────────────────────────┤
│ Step 3: Complete                            │
│   → Invoke /kernel/complete                 │
│   → Resets action counter                   │
└─────────────────────────────────────────────┘
```

## Instructions

1. **Anchor (MANDATORY first step):**
   - Invoke `/kernel/anchor`
   - This reads the protocol and re-centers on rules

2. **Read skill files:**
   - Read `.claude/skills/job-application/SKILL.md`
   - Read `.claude/skills/job-application/workflow.md`

3. **Parse input:**
   - Extract application URL from user message
   - Check for `--dry-run` flag

4. **Validate profile:**
   - Read `profile.json` from project root
   - Validate per → `references/profile-schema.md`
   - If missing or invalid, stop and report

4b. **Duplicate check (MANDATORY):**
   - Read `applications/log.json`
   - Check if any entry matches this company + title (case-insensitive)
   - If match found: STOP and report "Already applied on [date] — skipping to prevent duplicate"
   - If no match: proceed

5. **Execute workflow:**
   - Follow steps 1-9 in `workflow.md`
   - Read each step's reference file before executing
   - Update state after each step
   - On any failure → `/kernel/fix` → `/kernel/learn`

5b. **Generate cover letter (if form has cover letter field):**
   - Check the application form for a cover letter section (text area or file upload)
   - If present: draft a tailored cover letter using the job description and profile
   - Cover letters must be written fresh for each application — do NOT reuse from prior applications
   - Use the job title, company, and key requirements to personalize the opening and closing
   - Draw from `profile.json` experience, question_bank, and open_source fields
   - No em-dashes anywhere in the cover letter text
   - Present draft to user at the HITL gate (Step 6) — do not fill the field until approved

6. **HITL gate (Step 8 — MANDATORY):**
   - After all fields are filled and questions answered, STOP
   - Present the application review summary per → `references/hitl-review.md`
   - Wait for user: Approve / Edit / Reject
   - Do NOT submit without explicit user approval
   - This gate is enforced by the workflow — it cannot be skipped

7. **Log application (MANDATORY after submission):**
   - Read `applications/log.json`
   - Append new entry: company, title, url, job_listing_url, ats, date_applied, score, location, compensation, status: "submitted", notes: ""
   - Write updated array back to `applications/log.json`

8. **Complete:**
   - Invoke `/kernel/complete`

## File Upload Requirement

Resume and cover letter uploads require the agent to be invoked from the `job-application-spec` repo context (not sr_dev_workspace). The Playwright MCP sandbox limits file access to the agent's working directory. If `resume_path` in profile.json is an absolute path outside the working directory, the upload will fail and be flagged for HITL.

## Usage

```
/job-apply https://boards.greenhouse.io/company/jobs/12345
/job-apply https://boards.greenhouse.io/company/jobs/12345 --dry-run
```
