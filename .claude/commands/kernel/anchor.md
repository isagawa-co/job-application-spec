# /kernel/anchor

Re-center on protocol. Invoke at session start, every 10 actions, or when context drifts.

## Instructions

### Part A: Refresh Protocol

**MANDATORY: Use the Read tool on EVERY file listed below. EVERY TIME. No exceptions.**

1. **Read protocol (USE READ TOOL):**
   - Open `.claude/protocols/job_application-protocol.md`
   - Read entire file — use the Read tool, not memory
   - **Compute protocol hash** — run:
     ```bash
     python -c "import hashlib; print(hashlib.sha256(open('.claude/protocols/job_application-protocol.md','rb').read()).hexdigest())"
     ```
     Save the output for Step 14 (protocol_hash field in session_state.json).

2. **Summarize key points:**
   - Workflow steps (1-9)
   - Tooling rules (MCP only)
   - HITL gate (Step 8)
   - Anti-patterns to avoid

3. **Read Lessons Cheat Sheet (USE READ TOOL):**
   - Open `.claude/lessons/lessons.md`
   - Read entire file — use the Read tool, not memory

4. **Apply rules to next action (MANDATORY):**
   - Identify your specific next action
   - For each lesson rule, decide: relevant or skip
   - State the concrete verb — "I will [test/read/verify] X before [action]"

5. **Restore conversation context (USE READ TOOL):**
   - Read `.claude/state/session_state.json`
   - If `context` key exists, internalize prior decisions and task thread

### Part B: Review All Inter-Anchor Work

6. **Read the actions log:**
   - Read `.claude/state/actions.jsonl`
   - Every Edit, Write, Bash that modified state IS work to review

7. **Review each action against protocol:**

   | Check | Status |
   |-------|--------|
   | MCP-only browser interaction? | ✓/✗ |
   | Profile-driven (no hardcoded data)? | ✓/✗ |
   | HITL gate respected? | ✓/✗ |
   | Anti-patterns avoided? | ✓/✗ |

8. **If violation found:**
   - STOP
   - Set `needs_learn: true, needs_learn_reason: "anchor_violation"` in session_state.json
   - Fix the violation
   - Invoke `/kernel/learn` to record lesson

9. **Learn self-enforcement check:**
   - If test failures occurred since last anchor but no lesson was recorded:
   - Set `needs_learn: true, needs_learn_reason: "test_failure"` in session_state.json
   - Invoke `/kernel/learn` before proceeding

### Part C: Save State and Proceed

10. **Save conversation context (STRUCTURED):**
    Update `context` key in `.claude/state/session_state.json`:

    ```json
    {
      "context": {
        "current_task": "NNN-task-name.md or null",
        "task_folder": "tasks/ or null",
        "progress": "N/M tasks complete",
        "last_completed": "task filename or null",
        "next_step": "what to do next",
        "notes": "key decisions, direction changes, constraints"
      }
    }
    ```

11. **Archive and reset actions log:**
    - If log is non-empty, archive to `.claude/state/anchor-logs/YYYY-MM-DD/HH-MM-SSZ.json`
    - Truncate `actions.jsonl` to empty
    - Clear `actions_log` array in `session_state.json` to `[]`

12. **State current task:**
    - What are you about to do?
    - How does it fit the protocol?

13. **Confirm anchor token (MANDATORY if token exists):**
    - Read `pending_anchor_token` from `session_state.json`
    - If a token exists: include it in your anchor confirmation output
    - Set `anchor_token_confirmed: true` in session_state.json
    - Clear `pending_anchor_token` (set to null)

14. **Update state:**

    Update `.claude/state/job_application_workflow.json`:
    ```json
    {
      "anchored": true,
      "anchor_timestamp": "...",
      "actions_since_anchor": 0
    }
    ```

    Update `.claude/state/session_state.json` (merge):
    ```json
    {
      "anchor_token_confirmed": true,
      "pending_anchor_token": null,
      "protocol_hash": "<sha256 hex digest from Step 1>",
      "protocol_hash_timestamp": "<ISO timestamp>"
    }
    ```

15. **Confirm:**
    ```
    ANCHORED: job_application
    Token: [token or "none"]

    Key patterns:
    - Universal form discovery via browser_snapshot
    - Profile-driven field mapping from profile.json
    - HITL gate at Step 8 — never submit without approval
    - MCP only — no WebFetch, no Selenium

    Next action: [exact next thing I'll do]

    Actions reviewed: N
    Violations: 0 | N

    Proceeding with protocol.
    ```

## State File Location

`.claude/state/job_application_workflow.json`
