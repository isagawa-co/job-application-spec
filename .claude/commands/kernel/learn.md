# /kernel/learn

Update protocol AND lessons after fixing any failure. Make the same mistake impossible.

## Instructions

1. **Identify what failed:**
   - Error message
   - Root cause
   - How it was fixed

2. **Record lesson:**

   Open `.claude/lessons/lessons.md` and append:

   ```markdown
   ## [Date] [Issue Name]
   - **Issue:** What happened
   - **Root Cause:** Why it happened
   - **Fix:** How it was resolved
   - **Anti-Pattern Added:** What to avoid (if applicable)
   - **Quality Gate Added:** What to check (if applicable)
   ```

3. **Update reference files (if applicable):**

   If the lesson reveals a pattern worth codifying, check domain spec:
   - `.claude/skills/job-application/SKILL.md` — tooling rules, anti-patterns
   - `.claude/skills/job-application/references/form-discovery.md` — new label patterns
   - `.claude/skills/job-application/references/question-handling.md` — new question patterns
   - `.claude/skills/job-application/references/hitl-review.md` — review edge cases

   The domain spec defines pre-approved reference structure. Follow existing format.

4. **Update hooks if enforceable:**

   If this failure could be automatically prevented, consider adding to the gate enforcer.

5. **Update state:**

   Update `.claude/state/session_state.json`:
   ```json
   {
     "needs_learn": false,
     "needs_learn_reason": null
   }
   ```

   Update `.claude/state/job_application_workflow.json`:
   ```json
   {
     "lesson_recorded": true,
     "lessons_count": N,
     "last_lesson": "Issue name",
     "last_lesson_timestamp": "..."
   }
   ```

6. **Report:**
   ```
   LESSON RECORDED

   Issue: [what failed]
   Root Cause: [why]
   Fix: [how resolved]

   Lesson file updated: .claude/lessons/lessons.md
   Reference files updated:
   - [file]: [what was added]

   Hooks updated: [yes/no]

   Proceeding.
   ```

## When to Invoke

- After fixing any form discovery failure
- After fixing any field mapping error
- After fixing any submission failure
- After any anchor violation
- NEVER skip this after a fix
