# /kernel/domain-setup

Create complete enforcement for a new domain. Invoke when starting work in a new domain.

## Note for job-application-spec

Domain setup has already been completed. Domain: `job_application`

Protocol: `.claude/protocols/job_application-protocol.md`

If you need to extend the domain (add capabilities), use `/kernel/learn` to update the protocol.

## Instructions (for fresh setup)

1. Verify prerequisites (MCP, Node.js)
2. Discover repo structure
3. Read reference code
4. Extract patterns
5. Understand enforcement
6. Read workflow
7. Initialize tasks/
8. Build protocol
9. Wrap commands
10. Update state
11. Report & restart

## Domain Name Normalization

Domain name MUST be normalized:
- Lowercase
- Replace hyphens with underscores
- Remove special characters

Example: `job-application` → `job_application`
