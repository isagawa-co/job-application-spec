# /kernel/complete

Final gate before marking work done.

## Instructions

1. **Check state:**

   | Gate | Required |
   |------|----------|
   | Protocol created | `protocol_created: true` |
   | Anchored | `anchored: true` |

2. **Verify deliverables (MANDATORY):**

   Before marking complete, verify what the task produced:

   | Deliverable type | How to verify |
   |-----------------|---------------|
   | Application submitted | Confirmation page captured |
   | Dry-run complete | Dry-run log recorded |
   | Profile created | `profile.json` exists, required fields present |
   | Form filled | All mapped fields populated |
   | HITL review passed | Human approved |

3. **Determine completion mode:**

   Read `session_state.json` and `job_application_workflow.json`.
   Check `one_shot` in `session_state.json` FIRST:

   ### Mode A: One-Shot (`one_shot: true`)

   1. Add `current_task` to `completed_tasks` in `job_application_workflow.json`
   2. Check if tasks remain in `tasks/`
   3. Reset state for next invocation

   ### Mode B: Cycling (`cycling: true`, NOT one-shot)

   1. Add `current_task` to `completed_tasks`
   2. Find next incomplete task in `tasks/`
   3. Announce next task, read it, continue

   ### Mode C: Single completion

   Default — save context and report done.

4. **Save final conversation context:**
   ```json
   {
     "context": {
       "current_task": null,
       "progress": "N/M tasks complete",
       "last_completed": "task filename",
       "next_step": "next action or 'cycling complete'",
       "notes": "key decisions, open items"
     }
   }
   ```

5. **Report:**
   ```
   COMPLETE

   Domain: job_application
   Task: [what was done]
   Application: [URL if applicable]
   Status: [submitted | dry-run | profile-created]

   Verified:
   - [what I checked] → [result]

   Done.
   ```

## When to Invoke

- ALWAYS before saying "done"
- NEVER skip this gate
