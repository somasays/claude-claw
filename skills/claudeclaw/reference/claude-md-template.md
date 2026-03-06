# CLAUDE.md Orchestrator Template

This template generates the project's `CLAUDE.md` file. Sections are conditional — omit any section where the user skipped or provided no content. Never leave empty headers.

---

## Full Mode Template

```markdown
# Project: {{project_name}}

## Context Files

Load these context files to understand how to operate in this project.

### Always load (every session)
- `.claude/context/SOUL.md` — Identity and worldview
- `.claude/context/STYLE.md` — Communication patterns and tone

### Load on demand
- `.claude/context/USER.md` — Human profile (when personalizing responses)
- `.claude/context/AGENTS.md` — Operating rules (when making decisions about safety, uncertainty, or memory)
- `.claude/context/IDENTITY.md` — Self-description card (when asked "who are you")
- `.claude/context/TOOLS.md` — Environment notes (when working with infrastructure, APIs, or deployment)
- `.claude/context/HEARTBEAT.md` — Session-start checks (run these at the beginning of each session)

### Persistent memory
- Memory is stored in the Claude Code auto-memory directory and persists across sessions automatically.

---

## Role

{{role}}

---

## Project Overview

{{project_overview}}

### Tech Stack
{{tech_stack}}

### Architecture
{{architecture}}

### Source Packages
{{source_packages}}

### Key Files
{{key_files}}

### Operations
{{operations}}

---

## Critical Rules

{{critical_rules}}

## Code Conventions

{{code_conventions}}

## Development Workflow

{{dev_workflow}}

## Bug Fix Workflow

{{bug_fix_workflow}}

## Claude Delegation

{{delegation_section}}

## Skills

{{skills}}
```

---

## Minimal Mode Template

```markdown
# Project: {{project_name}}

## Context Files

Load these context files to understand how to operate in this project.

### Always load (every session)
- `.claude/context/SOUL.md` — Identity and worldview

### Load on demand
- `.claude/context/USER.md` — Human profile (when personalizing responses)

### Persistent memory
- Memory is stored in the Claude Code auto-memory directory and persists across sessions automatically.

---

## Role

{{role}}

## Project Overview

{{project_overview}}

## Critical Rules

{{critical_rules}}
```

---

## Section Guidelines

When populating sections, follow these patterns based on the trading-platform-claw reference:

### Role
Should define PURPOSE, not just personality. Examples:
- "Active trading platform operator — not just a coding assistant"
- "Full-stack developer and DevOps operator for this SaaS platform"
- "API architect and integration specialist"

### Architecture
Use ASCII diagrams showing data flow when appropriate:
```
Input Source → Processor → Output
    ├── Module A
    ├── Module B
    └── Module C
```

### Source Packages
Use a table format:
```
| Package | Responsibility |
|---------|---------------|
| `config` | Configuration management |
| `api` | REST endpoints |
```

### Key Files
Use a table format:
```
| File | Purpose |
|------|---------|
| `src/main.py` | Entry point |
| `config/settings.yaml` | Base configuration |
```

### Operations
Use code blocks with comments:
```bash
# Development
npm install                    # Install dependencies
npm run dev                    # Start dev server
npm test                       # Run tests

# Deployment
npm run build                  # Production build
npm run deploy                 # Deploy to staging
```

### Critical Rules
Use bold "NEVER" statements:
- **NEVER** commit secrets or API keys
- **End-to-end tests required** for all behavior
- **Ask when unsure** — wrong assumptions cost time

### Delegation
Only include if delegation was set up. Document presets, workflow, spec format, and auto-delegation policy.
Include: "You may auto-invoke `analyze` for read-only tasks. For all other
presets, describe the task and preset, then ask for confirmation before running."
Also note: the claw script auto-detects git repos — worktrees are used when available,
otherwise Claude runs in-place. Spec file paths (`-f`) are resolved to absolute paths,
so specs can live in any directory (including a separate hub repo).
