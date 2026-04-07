# /job-apply

Apply to a job posting. User provides URL, agent handles the rest.

## Instructions

1. **Read skill files (MANDATORY — every invocation):**
   - Read `.claude/skills/job-application/SKILL.md`
   - Read `.claude/skills/job-application/workflow.md`

2. **Parse input:**
   - Extract application URL from user message
   - Check for `--dry-run` flag

3. **Validate profile:**
   - Read `profile.json` from project root
   - Validate per → `references/profile-schema.md`
   - If missing or invalid, stop and report

4. **Execute workflow:**
   - Follow steps 1-8 in `workflow.md`
   - Read each step's reference file before executing
   - Update state after each step

5. **Kernel integration:**
   - Requires `/kernel/anchor` before starting
   - On failure → `/kernel/fix` → `/kernel/learn`
   - On completion → `/kernel/complete`

## Usage

```
/job-apply https://boards.greenhouse.io/company/jobs/12345
/job-apply https://boards.greenhouse.io/company/jobs/12345 --dry-run
```
