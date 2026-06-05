# Alain Ignacio

**AI Agent Architect | Founder, Isagawa**

Las Vegas, NV · alain@isagawa.co · 808.354.4526
LinkedIn: linkedin.com/in/alain-ignacio
github.com/isagawa-co (kernel, specs) · github.com/isagawa-qa (QA platforms)
Portfolio: isagawa.co

---

## Summary

Built quality enforcement systems for over two decades before abstracting those patterns into AI agent governance. Designed and shipped the Isagawa Kernel: an open-source system where AI agents self-build their own enforcement infrastructure from authored specifications. Background spans healthcare, government ERP, telecom, and startup environments. The throughline is the same: identifying where processes break and building mechanical prevention so they cannot break that way again. Applied that discipline first to software testing, now to governing autonomous AI agents.

---

## Isagawa: Founder & Systems Architect
*2025 to Present · isagawa.co*

**80+ attested pipeline runs · 800+ autonomous tasks executed · 9 production repos · All signed to Rekor**

### Isagawa Kernel (Open Source · MIT) · github.com/isagawa-co/isagawa-kernel

The core invention. A self-building governance system for autonomous AI agents that enforces compliance at the tool-call boundary, not in the prompt.

The agent scans any repo, writes its own protocol and enforcement hooks, and improves them mechanically every time it fails. When a test fails or a protocol violation is detected, the agent is blocked from writing until it records a lesson. That lesson updates both the agent's knowledge and its enforcement rules simultaneously. The same mistake becomes impossible, not because the agent remembers, but because the system enforces.

Hooks run at the tool-call boundary so agents cannot bypass enforcement via prompt engineering or state file edits. A re-centering mechanism fires every N actions, forcing the agent to re-read its protocol and review accumulated lessons, making drift architecturally impossible over long sessions. State survives context compaction and restarts. The agent resumes exactly where it left off. The kernel is domain-agnostic. It governs how the agent works. Pair it with an agent harness from the Agent Factory to define what it builds.

### Agent Factory (Proprietary) · github.com/isagawa-co/domain-spec-factory

Autonomous agent harness construction. Given an industry vertical as input, the kernel-governed agent researches the domain, builds a complete governed harness from original research rather than templates, and validates it before publishing. The kernel governs the agent while it builds harnesses that are themselves kernel-governed, a self-reinforcing loop. 14 agent harnesses manufactured without human intervention across healthcare, DevOps, compliance, real estate, and financial verticals.

The emerging SDD ecosystem (Amazon Kiro, GitHub Spec Kit, BMAD) produces planning artifacts that guide agents. None enforce compliance during execution or learn from failures after. Agent Factory closes both gaps. Each harness is an operational artifact that self-assembles a governed workspace with enforcement and quality gates built in.

### Autonomous Delivery Pipeline

`/kernel/backlog` (intent + hash) → `/kernel/task-builder` (atomic decomposition + gate contracts) → `run-task.sh` (headless batch, one `claude -p` per task) → `/kernel/complete` → signed to Rekor. One sentence to verified, attested artifact with no human intervention between start and finish. 80+ signed runs. The pipeline built the portfolio site that describes it (isagawa.co).

---

## isagawa-qa: Production QA Platforms (Proprietary)
*github.com/isagawa-qa · 5 platforms built on the Isagawa Kernel*

All five platforms share the same pattern: a kernel-governed AI agent reads reference implementations before writing any code, enforces a 5-layer architecture during generation, and gets permanently smarter after every failure.

**LLM Evaluation Platform (DeepEval).** Structured LLM evaluation with 29+ metrics across RAG, Chat, Agent, and Conversational pipelines. The agent selects metrics appropriate to the pipeline type, sets thresholds from tested defaults, generates pytest-native eval suites, and runs them with failure triage. Golden datasets drive every test. No vibes, no spreadsheets.

**QA Platform: Playwright.** 5-layer TypeScript/Playwright test automation with three test paths (UI, API, and hybrid) sharing the same architecture. Playwright MCP provides real-time element and endpoint discovery during test generation. Locators live in one place, endpoints live in one place, business logic lives in Tasks and Roles, tests only assert.

**QA Platform: Selenium.** Same 5-layer governed architecture in Python/Selenium/pytest. The agent discovers page elements, generates Page Objects, Tasks, Roles, and Tests following strict architectural separation. Kernel enforcement prevents layer mixing and pattern drift across sessions.

**SSH Compliance Platform** · isagawa.co/ssh-compliance. Automated SSH compliance validation across 8 security frameworks: STIG, CIS, NIST, FIPS, PCI DSS, HIPAA, SOC 2, and ISO 27001. Connects to infrastructure, runs every check, produces auditor-ready reports with captured command output as evidence per check.

**Docker Compliance Platform.** Automated container image validation against CIS, DISA-STIG, and FIPS standards. Pulls the image, detects OS and package manager, runs every check, and produces structured compliance reports with pass/fail status per rule, per framework, per image.

---

## Also Shipped (isagawa-co, Proprietary)

**Vibe Coder** · github.com/isagawa-co/vibe-coder-spec. AI-powered app building for non-technical founders. The kernel-governed agent handles all technical decisions (stack, architecture, database, deployment) and explains every choice in plain English. The same governance model applied to a consumer product direction.

**AutoApply** · isagawa.co/job-application. End-to-end job application pipeline with Playwright MCP for universal form discovery, profile-driven form fill across any ATS, and a hook-enforced human review gate before any submission. This application was submitted using it.

**Attestation Pipeline** · isagawa.co/attestation. Every pipeline run signed with Sigstore and logged to Rekor. The full intent chain from hashed user input through shipped artifact is verifiable on a public tamper-evident log.

---

## Prior Experience: QA Architecture

**QA Lead · HMSA** · March 2026 to Present (Remote)
Returned to Hawaii's largest health insurer to lead QA for claims processing and establish a new QA automation function. Leading testing across adjudication workflows and EDI integrations while standing up the automation function from ground up. The evaluation scope covers UI, API, and database testing layers across HMSA's enterprise environment and compliance constraints.

**Senior QA Engineer · Helios Digital** · May 2025 to March 2026 (Remote)
Founding QA hire for a seed-stage startup building an AI-powered financial analysis platform. Built a QA Center of Excellence that enabled a 4 to 5x improvement in test velocity per engineer and reduced new engineer onboarding from three weeks to one week through AI-enforced patterns. Established the automation function from zero while integrating Claude Code into daily development workflows.

**Senior QA Automation Engineer · Nakupuna Consulting** · Nov 2022 to May 2025 (Remote, Navy contract)
Established the test automation function from scratch for Navy PeopleSoft ERP in a highly restricted environment. Scaled the automation team from zero to four engineers within six months, getting Python/Selenium approved in an environment where only legacy tools had been authorized. Designed a Page Object Model and data-driven framework that the development team adopted over PeopleSoft's native tooling for their own unit testing.

**Test Manager · HMSA** · May 2013 to Nov 2022 (Honolulu, HI)
Nine-year tenure at Hawaii's largest health insurer progressing from QA Analyst to Test Manager. Defined QA strategy and testing standards across Claims Processing, EDI, Membership, Billing, Enrollment, HL7-FHIR, and RESTful services. Built HL7-FHIR API automation with Postman and end-to-end test scripts in Cypress with Page Object Framework.

**Senior Test Analyst · IBM** · July 2008 to May 2013 (Honolulu, HI)
Led QA for Virgin Mobile USA's mission-critical billing platform across website, CRM, WAP, and charging/rating systems. Managed an 18-member onshore/offshore team and drove test execution across 15+ enterprise-level releases.

**QA Lead · Virgin Mobile USA** · 2004 to 2008 (Walnut Creek, CA)
Promoted from tester to lead for the billing platform, overseeing testing for MVNO pay-as-you-go services.

**QA Lead · Sony Electronics** · 1999 to 2004 (San Jose, CA)
Promoted from tester to lead for handheld devices and VAIO product lines.

---

## Technical Skills

**AI Agent Engineering:** multi-loop agent architecture, hook-based governance, mechanical enforcement, autonomous task cycling, MCP server design, Playwright MCP, Claude Code, context window management, session persistence

**LLM Evaluation:** DeepEval, RAG evaluation, agent evaluation, golden dataset design, pytest-native eval suites, 29+ metric frameworks

**Architecture:** Spec-Driven Development, tiered index architecture, defense-in-depth enforcement, state machines, event-driven workflows, domain-driven design

**Test Automation:** Playwright, Selenium, pytest, Cypress, 5-layer framework architecture, Page Object Model, BDD

**Compliance and Security:** STIG, CIS, NIST, FIPS, PCI DSS, HIPAA, SOC 2, ISO 27001, Sigstore, Rekor, cryptographic attestation

**Languages and Tools:** Python, TypeScript, JavaScript, SQL, Git, FastAPI, Postman
