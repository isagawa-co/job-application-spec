# Job Application Spec

### AI Execution Management for Job Applications

> "AI can fill forms. But can you trust it to apply correctly?"

Most AI tools generate cover letters and leave you to copy-paste. This spec **manages the entire application process** — discovering form fields, mapping your profile, filling everything, and pausing for your review before submit.

This isn't a browser macro. It's **AI execution management for job applications**.

---

## Get Started (Step by Step)

Follow each step in order. Do not skip any step. Everything is done inside VS Code.

### Step 1: Install VS Code

VS Code is the code editor where you will do all your work.

1. Go to https://code.visualstudio.com/
2. Click the big **Download** button
3. Open the file you downloaded
4. Follow the installer — click **Next** on each screen, then click **Install**
5. When it finishes, open VS Code

### Step 2: Install Git

Git is a tool that downloads and tracks code. You need it to download this project.

1. Go to https://git-scm.com/downloads
2. Click the download for your operating system (Windows, Mac, or Linux)
3. Open the file you downloaded
4. Follow the installer — use the default options on every screen, click **Next**, then **Install**
5. When it finishes, restart VS Code if it is already open

**Check that Git is installed:**
1. In VS Code, open the terminal: press ``Ctrl + ` `` (the backtick key, above the Tab key on your keyboard)
2. Type this and press **Enter**:
   ```bash
   git --version
   ```
3. You should see something like: `git version 2.44.0`. If you see this, Git is installed.

### Step 3: Install Node.js

Node.js is needed for the Playwright MCP browser tool that fills out application forms.

1. Go to https://nodejs.org/
2. Click the **LTS** download button (the one that says "Recommended")
3. Open the file you downloaded
4. Follow the installer — use the default options, click **Next**, then **Install**
5. Restart VS Code after installing

**Check that Node.js is installed:**
1. In the VS Code terminal (``Ctrl + ` ``), type:
   ```bash
   node --version
   ```
2. You should see something like: `v20.11.0`. The number must be 18 or higher.

### Step 4: Install Claude Code Extension

Claude Code is the AI agent that fills out job applications for you inside VS Code.

1. In VS Code, click the **Extensions** icon on the left sidebar (it looks like 4 small squares)
2. In the search box, type: `Claude Code`
3. Find **"Claude Code"** by Anthropic — click **Install**
4. Wait for the install to finish
5. You will see a **sparkle icon (&#10033;)** appear in the top-right area of VS Code

> **You need an Anthropic account.** If you do not have one, go to https://claude.ai and create an account first.

### Step 5: Download This Project

Do this inside VS Code. Do not use a separate terminal.

1. In VS Code, open the terminal: press ``Ctrl + ` ``
2. Go to your Desktop (so the project saves there):
   ```bash
   cd Desktop
   ```
3. Download the project:
   ```bash
   git clone https://github.com/isagawa-co/job-application-spec.git
   ```
4. Wait for the download to finish

### Step 6: Open the Project in VS Code

This step is important. Claude Code needs to be inside the project folder to work correctly.

1. In VS Code, click **File** > **Open Folder**
2. Find and select the `job-application-spec` folder on your Desktop
3. Click **Select Folder** (Windows) or **Open** (Mac)
4. VS Code will reload with the project open
5. You should see the project files on the left sidebar (`.claude/`, `.mcp.json`)

### Step 7: Verify Playwright MCP

The AI agent uses Playwright MCP to open a browser, discover form fields, and fill applications.

1. Click the **sparkle icon (&#10033;)** in VS Code to open Claude Code
2. Type:
   ```
   /mcp
   ```
3. You should see **playwright** in the list of MCP servers
4. If you do NOT see it, close VS Code and open it again, then check `/mcp` again

### Step 8: Set Up the AI Agent

This runs the kernel domain setup, which builds the agent's protocol and onboards your profile.

1. In Claude Code, type:
   ```
   start
   ```
2. The agent will run `/kernel/domain-setup` automatically
3. During setup, the agent will ask you to provide your resume
4. Place your resume at `.claude/skills/job-application/references/resume.md` (or paste it when asked)
5. The agent extracts your resume into a structured `profile.json` and asks you to review it
6. Fill in any gaps the agent flags (salary range, work authorization, etc.)
7. When setup finishes, the agent will say: **"Restart Claude Code to activate hooks"**
8. Close and reopen Claude Code

### Step 9: Start Applying

1. In Claude Code, type:
   ```
   continue
   ```
2. The agent anchors and is ready

3. To apply to a job:
   ```
   /job-apply https://boards.greenhouse.io/company/jobs/12345
   ```

4. To do a dry run (fills the form but does NOT submit):
   ```
   /job-apply https://boards.greenhouse.io/company/jobs/12345 --dry-run
   ```

5. The agent will:
   - Open a browser and navigate to the application
   - Discover all form fields via accessibility tree snapshot
   - Map fields to your profile
   - Fill everything automatically
   - Generate answers for custom questions
   - Pause and show you a full review before submitting

6. You review, edit if needed, then approve or reject

---

## The Problem

Applying to jobs is repetitive, error-prone, and time-consuming:

- Every ATS (Greenhouse, Lever, Workday) has different form layouts
- You re-enter the same information hundreds of times
- Custom questions need tailored answers, not copy-paste
- One wrong field and the application is rejected
- Browser autofill breaks on complex forms

**The cycle:** Open application > fill 30 fields > miss one > start over > repeat 50 times.

## The Solution

The Job Application Spec combines **universal form discovery** with the **Isagawa Kernel** — a self-building, self-improving enforcement system that runs *inside* the AI agent.

The agent doesn't use ATS templates. It treats every application page as unknown — discovering the form structure via Playwright MCP, mapping fields to your profile, and filling everything. Every failure makes it permanently smarter.

---

## How It Works

### The 9-Step Workflow

When you invoke `/job-apply`, the agent follows a 9-step process:

```
/job-apply [url]
    │
Step 1: ONBOARD PROFILE
    Read profile.json (or create it from your resume)
    Gate: required fields present, resume_path valid
    │
Step 2: LOAD PROFILE
    Validate profile against schema
    Gate: personal.first_name, last_name, email non-empty
    │
Step 3: NAVIGATE
    Open application URL via Playwright MCP
    Gate: page loads successfully
    │
Step 4: DISCOVER FORM
    Snapshot accessibility tree, extract all form fields
    Gate: field map generated with labels, types, selectors
    │
Step 5: MAP FIELDS
    Match form fields to profile keys using label matching
    Gate: mapped vs unmapped fields identified
    │
Step 6: FILL FORM
    Fill all mapped fields via browser_fill_form
    Gate: all mapped fields populated
    │
Step 7: HANDLE QUESTIONS
    Template bank first, Claude Code generation as fallback
    Gate: all questions answered, generated answers tagged
    │
Step 8: REVIEW (HITL)
    Present full summary — human approves, edits, or rejects
    Gate: human decision received
    │
Step 9: SUBMIT
    Click submit (or log dry-run result)
    Gate: confirmation page captured
    │
COMPLETE → /kernel/complete → lesson recorded
```

### Universal Form Discovery

The agent doesn't know Greenhouse from Lever from Workday. It discovers every form from scratch:

1. **Snapshot** — `browser_snapshot` captures the page's accessibility tree
2. **Extract** — Identify all interactive elements (text inputs, dropdowns, checkboxes, file uploads, radio buttons, textareas)
3. **Classify** — Match each field label against the profile schema's field mapping table
4. **Map** — Tag fields as `mapped` (profile match) or `unmapped` (needs human input or AI generation)

```json
{
  "fields": [
    {
      "selector": "input#first-name",
      "label": "First Name",
      "type": "text",
      "required": true,
      "profile_key": "personal.first_name",
      "status": "mapped"
    },
    {
      "selector": "textarea#cover-letter",
      "label": "Why are you interested in this role?",
      "type": "textarea",
      "required": false,
      "profile_key": null,
      "status": "unmapped",
      "handling": "question-handling"
    }
  ]
}
```

Multi-page forms are handled automatically — the agent detects "Next" / "Continue" buttons, advances, and runs discovery again on each page.

### Question Handling (Hybrid)

Custom questions use a three-tier approach:

```
1. Exact match in profile.question_bank        → use stored answer
2. Fuzzy match in profile.question_bank.custom  → use closest match
3. No match                                     → Claude Code generates answer
```

Generated answers are:
- Tailored to the specific job posting (reads the title and description)
- Connected to your profile (uses your headline and experience)
- Tagged as `"source": "generated"` so you know what to review
- Never submitted without your approval

### Human-In-The-Loop Review

Every application pauses before submit. The agent presents:

```
APPLICATION REVIEW
==================
URL: https://boards.greenhouse.io/company/jobs/12345
Position: Senior Software Engineer
Company: Acme Corp

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

UNMAPPED FIELDS (need your input):
  ⚠ "Referral source": [empty — no profile match]

STATUS: Ready for review
==================
```

Your options:

| Action | Result |
|--------|--------|
| **Approve** | Agent clicks submit |
| **Edit** | You provide corrections, agent updates fields |
| **Reject** | Agent stops, no submit |
| **Dry-run complete** | If `--dry-run`, log result and stop |

---

## Profile Schema

Your profile is a single JSON file that all applications pull from. Created during setup, editable anytime.

```json
{
  "personal": {
    "first_name": "",
    "last_name": "",
    "email": "",
    "phone": "",
    "headline": ""
  },
  "location": {
    "city": "",
    "state": "",
    "zip": "",
    "country": ""
  },
  "experience": [
    {
      "company": "",
      "title": "",
      "start_date": "2022-01",
      "end_date": "",
      "current": true,
      "description": ""
    }
  ],
  "education": [
    {
      "school": "",
      "degree": "",
      "field": "",
      "graduation_year": ""
    }
  ],
  "links": {
    "linkedin": "",
    "github": "",
    "portfolio": ""
  },
  "resume_path": ".claude/skills/job-application/references/resume.md",
  "preferences": {
    "salary_min": null,
    "salary_max": null,
    "remote_preference": "",
    "start_date": ""
  },
  "authorization": {
    "work_authorized": true,
    "sponsorship_required": false,
    "visa_status": ""
  },
  "question_bank": {
    "why_interested": "",
    "strengths": "",
    "salary_expectations": "",
    "start_date": "",
    "custom": {}
  }
}
```

### Field Mapping

During form discovery, the agent maps form field labels to profile keys:

| Common Labels | Profile Key |
|---------------|-------------|
| First Name, Given Name | `personal.first_name` |
| Last Name, Surname, Family Name | `personal.last_name` |
| Email, Email Address | `personal.email` |
| Phone, Phone Number, Mobile | `personal.phone` |
| LinkedIn, LinkedIn URL | `links.linkedin` |
| Resume, CV, Upload Resume | `resume_path` |
| Cover Letter | `cover_letter_path` |
| Current Company, Employer | `experience[0].company` |
| Current Title, Job Title | `experience[0].title` |
| Salary, Compensation | `preferences.salary_min` / `preferences.salary_max` |
| Work Authorization | `authorization.work_authorized` |
| Sponsorship | `authorization.sponsorship_required` |

This table is a starting point. The agent extends it via `/kernel/learn` as new label patterns are encountered.

---

## Error Handling

| Failure | Recovery |
|---------|----------|
| Page won't load | Report URL error, stop |
| Login wall | Flag HITL — user logs in, agent resumes |
| CAPTCHA | Flag HITL — user solves, agent resumes |
| File upload fails | Retry once, then flag HITL |
| Field fill fails | Try alternate selector, then flag HITL |
| Unknown form structure | Snapshot + flag entire page for HITL |

The agent never silently skips a field. Every field appears in the HITL review — mapped, unmapped, or failed.

---

## The Kernel (Enforcement Engine)

The Isagawa Kernel is the enforcement engine that runs inside Claude Code. It's not a linter or a post-hoc checker — it **gates every action in real-time**.

### What It Does

1. **Self-builds** — On first run, the kernel reads the spec, extracts patterns, and builds its own protocol
2. **Self-enforces** — Every 10 actions, the hook forces the agent to re-read its protocol (`/kernel/anchor`)
3. **Self-improves** — After every failure, `/kernel/learn` updates the protocol permanently

### How It Works

```
session-start → anchor → WORK ──────────────────→ complete
                   ↑         ↓                       ↑
                   └─ every 10 actions ←─────────────┘
                             ↓
                   failure? → fix → learn (MANDATORY)
```

### Universal Gate Enforcer

The hook script (`universal-gate-enforcer.py`) intercepts every Write, Edit, and Bash command. It blocks if:

1. **Session not started** — Must invoke `/kernel/session-start` first
2. **Lesson not recorded** — A failure occurred but `/kernel/learn` wasn't called
3. **Protocol not anchored** — Must re-read protocol via `/kernel/anchor`
4. **Action limit reached** — 10 actions since last anchor, time to re-center

```python
# Simplified gate logic:
if not session_state.get('session_started'):
    BLOCK → "Invoke /kernel/session-start"

if session_state.get('needs_learn'):
    BLOCK → "Invoke /kernel/learn"

if not domain_state.get('anchored'):
    BLOCK → "Invoke /kernel/anchor"

if actions_since_anchor > actions_limit:
    BLOCK → "Invoke /kernel/anchor"
```

The agent cannot bypass these gates. Every failure becomes a permanent lesson. Every lesson makes the next application better.

---

## Supported ATS Platforms

The spec uses universal form discovery — it works on any application page. Tested on:

| ATS | Status | Notes |
|-----|--------|-------|
| **Greenhouse** | Tested | Complex forms (30+ fields), comboboxes, dropdowns |
| **Lever** | Tested | Simpler forms (15-20 fields), text + radio |
| **Workday** | Untested | Should work — same discovery approach |
| **iCIMS** | Untested | Should work — same discovery approach |
| **Any web form** | Untested | Universal discovery handles any accessible form |

---

## Project Structure

```
job-application-spec/
├── .claude/
│   ├── commands/
│   │   └── job-apply.md                    # /job-apply — apply to a job posting
│   └── skills/
│       └── job-application/
│           ├── SKILL.md                    # Identity, philosophy, tooling rules
│           ├── workflow.md                 # 9-step workflow with state tracking
│           └── references/
│               ├── profile-onboarding.md   # Resume → profile.json extraction
│               ├── profile-schema.md       # Profile JSON structure + field mapping
│               ├── form-discovery.md       # Universal form discovery via MCP snapshot
│               ├── question-handling.md    # Template bank + AI generation fallback
│               └── hitl-review.md          # Human review before submit
├── .mcp.json                               # Playwright MCP server config
└── README.md
```

### After Domain Setup (what gets created)

```
job-application-spec/
├── .claude/
│   ├── commands/
│   │   ├── kernel/                         # Kernel commands (session-start, anchor, learn, etc.)
│   │   └── job-apply.md
│   ├── hooks/
│   │   ├── universal-gate-enforcer.py      # Action gating + counter
│   │   └── test-failure-detector.py        # Sets needs_learn on failure
│   ├── lessons/
│   │   └── lessons.md                      # Accumulated failure lessons
│   ├── protocols/
│   │   └── job_application-protocol.md     # Self-built protocol (index of rules)
│   ├── skills/
│   │   └── job-application/                # (unchanged)
│   └── state/
│       ├── session_state.json              # Session tracking, context, actions log
│       └── job_application_workflow.json   # Workflow state, anchor counter
├── profile.json                            # Your structured profile (from resume)
├── .mcp.json
├── CLAUDE.md                               # Kernel instructions
└── README.md
```

---

## How It Works (End-to-End)

1. Provide a job application URL
2. The AI opens a browser, discovers every form field via accessibility tree
3. Fields are mapped to your profile and filled automatically
4. Custom questions get tailored answers from your profile + the job posting
5. You review everything before submit — approve, edit, or reject
6. Every failure makes the system permanently smarter

```
Input:
  /job-apply https://boards.greenhouse.io/spacex/jobs/8149154002

  │
  ├── Navigate: Playwright MCP opens browser, loads application page
  ├── Discover: Snapshot accessibility tree, find 38 form fields
  ├── Map: 12 fields from profile, 13 best-guess, 7 skipped (voluntary)
  ├── Fill: Text inputs, dropdowns, comboboxes, checkboxes
  ├── Generate: "Top two accomplishments" textarea from profile experience
  ├── Review: Full summary presented — you approve or edit
  └── Submit: Click submit (or log dry-run)
```

---

## Quick Start

### Prerequisites

- Node.js 18+ (for Playwright MCP)
- [Claude Code](https://claude.ai/claude-code)

### 1. Clone

```bash
git clone https://github.com/isagawa-co/job-application-spec.git
cd job-application-spec
```

### 2. Add your resume

Place your resume at:
```
.claude/skills/job-application/references/resume.md
```

### 3. Set up the AI agent

```bash
claude                    # Start Claude Code in the project directory
> start                   # Agent runs domain-setup, onboards your profile
                          # --> "Restart Claude Code to activate hooks"
claude                    # Restart
> continue                # Agent anchors and is ready
```

### 4. Apply to jobs

```bash
# Inside Claude Code:
/job-apply https://boards.greenhouse.io/company/jobs/12345

# Dry run (fill but don't submit):
/job-apply https://boards.greenhouse.io/company/jobs/12345 --dry-run
```

---

## Commands

| Command | Purpose | When to Use |
|---------|---------|-------------|
| `/job-apply` | Fill and submit a job application | Applying to a job |
| `/kernel/anchor` | Re-center on protocol | Automatic every 10 actions |
| `/kernel/learn` | Record lesson after failure | After fixing any issue |
| `/kernel/complete` | Final gate before done | After application submitted |

---

## The Bigger Picture

Job applications are one domain. The Isagawa Kernel supports **any** domain.

The kernel is domain-agnostic — it enforces how AI executes, not just what it generates. What you see here in job applications can be applied to:

- Test automation ([Selenium QA](https://github.com/isagawa-qa/platform-selenium))
- Healthcare compliance ([Healthcare QA](https://github.com/isagawa-co/healthcare-qa-spec))
- Financial auditing ([SOX Audit](https://github.com/isagawa-co/sox-audit-spec))
- Incident response ([Incident Response](https://github.com/isagawa-co/incident-response-spec))
- Any process where AI needs to execute correctly, not just generate output

The kernel is available separately. Domain specs — pre-loaded with patterns, anti-patterns, and quality gates for specific verticals — are built by the [Domain Spec Factory](https://github.com/isagawa-co/domain-spec-factory).

---

## AI Execution Management vs Browser Automation

| Browser Automation (Others) | AI Execution Management (Isagawa) |
|-----------------------------|-----------------------------------|
| Hardcoded selectors per ATS | Universal discovery on any form |
| Breaks when form changes | Adapts to any layout |
| Fill-and-submit macro | Profile-driven with human review |
| No learning from errors | Every failure = permanent lesson |
| Script per job board | One spec for all job boards |
| "Did it fill the form?" | "It can only fill the form correctly" |

This is AI you can actually delegate job applications to.

---

## Services

We deliver AI execution management solutions powered by the Isagawa Kernel. The kernel manages how AI executes — gating every action at runtime so the AI can only do it right.

### Demo

We'll run a live job application on **YOUR** target posting in 30 minutes. No setup. No waiting.

**[alain@isagawa.co](mailto:alain@isagawa.co)** | **[DM on LinkedIn](https://www.linkedin.com/in/alain-ignacio-54b9823)**

---

## License

[MIT](LICENSE) — Copyright (c) 2025 Isagawa

---

Built with the [Isagawa Kernel](https://github.com/isagawa-co/isagawa-kernel) — self-building, self-improving, safety-first.
