# Claude Code Project Template

A professional bootstrap template for projects using [Claude Code](https://claude.ai/code) as your AI-assisted development environment. This template provides a complete framework for organizing your project, coordinating AI agents, tracking development decisions, and maintaining code quality.

## What's Included

### 1. **Custom AI Agents** (`.claude/agents/`)
Three specialized agents configured for different development workflows:
- **TDD Developer** (`tdd-developer.md`) — Handles Test-Driven Development cycles, implementing features with tests-first methodology
- **Code Reviewer** (`code-reviewer.md`) — Manages code quality, linting, and refactoring after TDD work
- **Test Engineer** (`test-engineer.md`) — Owns Playwright E2E tests, integration tests, and critical user journey validation

Each agent has specific boundaries and responsibilities to avoid overlap and ensure focused work.

### 2. **Working Memory System** (`.claude/memory/`)
A git-tracked memory system for storing development discoveries, patterns, and session summaries:
- **MEMORY.md** — Fast-scan index of all patterns and sessions (agents read this first)
- **session-notes.md** — Historical records of completed work and learnings
- **patterns-discovered.md** — Reusable code patterns that work well in your project
- **scratch/working-notes.md** — Ephemeral notes during active development (not committed)

The memory system grows over time, becoming your project's decision history and solution library.

### 3. **Project Documentation** (`CLAUDE.md`)
A comprehensive guide that describes:
- Project context (purpose, tech stack, current phase)
- Development principles and methodology
- Testing strategy and scope
- Workflow patterns for common tasks
- Agent usage and boundaries
- Git conventions and branching strategy
- Custom commands and utilities

### 4. **Document Templates** (`docs/templates/`)
- **PRD Template** — Product Requirements Document for defining features, scope, and requirements
- **Epic & Stories Template** — Breaking PRDs into epics, stories, acceptance criteria, and technical requirements

---

## Quick Start

### 1. Download and Setup
```bash
# Clone or download this template
git clone <template-url> my-project
cd my-project

# Install dependencies (if applicable to your project)
npm install  # or: pip install, etc.
```

### 2. Customize for Your Project
Open **`CLAUDE.md`** and update these sections with your project's actual information:

| Section | What to Fill In |
|---------|---|
| **Project Context** | What you're building, tech stack, current phase, blockers |
| **Documentation References** | Links to your existing docs (architecture, API spec, deployment, etc.) |
| **Development Principles** | Your team's methodology (TDD, BDD, etc.) and quality standards |
| **Testing Scope** | What tests exist, which frameworks, your testing strategy |
| **Workflow Patterns** | Common development workflows (feature, bug fix, refactor, etc.) |
| **Agent Usage** | When to use which agent; adjust agent descriptions if needed |
| **Workflow Utilities** | Custom commands available in your project |
| **Git Workflow** | Your commit conventions, branching strategy, merge rules |
| **Notes for Claude** | Any special instructions or project quirks |

**Tip:** Don't fill in placeholder sections if you don't have that yet (e.g., "Documentation References"). Leave them empty; you can add them incrementally.

### 3. Update Agent Descriptions (Optional)
Each agent file in `.claude/agents/` has a "Stack Context" section that references CLAUDE.md:
```markdown
<!-- TODO: Replace this section with your actual project stack, or reference CLAUDE.md -->
```

You can either leave this reference as-is or customize each agent file with your specific stack details (frameworks, test runners, linters, etc.).

### 4. Verify Directory Structure
The template includes these directories. Adjust as needed for your project:

```
my-project/
├── .claude/                          # Claude Code configuration
│   ├── agents/                       # Custom agent definitions
│   │   ├── tdd-developer.md
│   │   ├── code-reviewer.md
│   │   └── test-engineer.md
│   └── memory/                       # Working memory system
│       ├── MEMORY.md                 # Index (agents read this first)
│       ├── README.md                 # Memory system documentation
│       ├── session-notes.md          # Historical session records
│       ├── patterns-discovered.md    # Reusable patterns
│       └── scratch/
│           ├── .gitignore
│           └── working-notes.md      # Active session notes (ephemeral)
│
├── docs/
│   └── templates/
│       ├── prd-template.md           # Product Requirements Document
│       └── epic-stories-template.md  # Epic & Stories breakdown
│
├── CLAUDE.md                         # Project documentation for Claude
└── README.md                         # This file
```

If you have other directories (src/, tests/, config/, etc.), keep them alongside these folders.

---

## How to Use This Template

### Starting a Development Session

1. **Clarify your task** — Know what you're building (feature, bug fix, refactor, etc.)
2. **Check CLAUDE.md** — Refresh on your project's conventions and principles
3. **Check MEMORY.md** — See if similar work has been done before
4. **Run Claude Code** — Open the IDE, call Claude, describe your task
5. **Claude picks the agent** — Based on your task, Claude selects the right agent (or you can specify one)

### Working with Agents

**TDD Developer** — Use for features and bug fixes
```
"Implement login form using TDD. Write tests first, then code."
```

**Code Reviewer** — Use after TDD work to clean up code quality
```
"Run the linter and fix any errors or code smells."
```

**Test Engineer** — Use for E2E tests and critical user journey validation
```
"Write Playwright tests for the user registration flow."
```

### Updating Your Memory System

**During a session** — Open `.claude/memory/scratch/working-notes.md` and take notes as you work:
- Current task and goal
- Approach you're taking
- Key findings
- Decisions made and why
- Blockers

**After a session** — Review your notes and:
1. Summarize key learnings into `.claude/memory/session-notes.md`
2. Extract any reusable patterns into `.claude/memory/patterns-discovered.md`
3. Add one-line entries to `.claude/memory/MEMORY.md` pointing to your notes

Example entry:
```markdown
- [Login Form Implementation](session-notes.md#L42) — TDD approach for form validation and error handling
```

This system compounds over time — after a few weeks, your MEMORY.md becomes a searchable guide of solutions and patterns.

---

## Customizing Agent Behavior

Each agent can be customized for your project. Open the agent files in `.claude/agents/`:

- **Model selection** — Change the `model:` field (e.g., `claude-opus-4-8` for complex tasks, `claude-haiku-4-5` for simple ones)
- **Tool access** — Adjust which tools each agent has (e.g., add `WebSearch` for research)
- **Instructions** — Update the workflow sections to match your project's specific needs

Example:
```yaml
name: tdd-developer
description: Use for Test-Driven Development...
model: claude-opus-4-8  # Upgrade to Opus for complex logic
tools:
  - Read
  - Edit
  - Write
  - Bash
  - TodoWrite
  - WebSearch  # Add if needed for research-heavy features
```

---

## Project-Specific Customization Checklist

- [ ] **CLAUDE.md** — Filled in with your project's actual context, tech stack, and workflows
- [ ] **.claude/agents/** — Updated model selections if needed
- [ ] **docs/templates/** — Ready to use for PRDs and epic breakdowns
- [ ] **.claude/memory/MEMORY.md** — Starting fresh (or populated with existing project learnings)
- [ ] **.gitignore** — Ensures `.claude/memory/scratch/` is not committed
- [ ] **Testing environment** — Confirmed linter, test runner, type checker are installed and configured
- [ ] **Git remote** — Set up if you plan to use GitHub integration with Claude Code

---

## Common Issues and Solutions

### "The agent doesn't know about my project"
→ Update **CLAUDE.md** with your project's context. Agents read this file first to understand conventions, testing strategy, and development principles.

### "The agent picked the wrong tool"
→ Either ask Claude which agent is right, or specify explicitly: "Use the tdd-developer agent to write a test for..."

### "Tests are failing but the agent isn't fixing them"
→ Different agents handle different test types:
- **Unit/integration tests** → tdd-developer agent
- **Playwright E2E tests** → test-engineer agent
- **Linting/style issues in tests** → code-reviewer agent

### "My memory system is getting too large"
→ Keep **MEMORY.md** under ~50 lines. Archive very old sessions if needed. Each entry should be one line with a link.

---

## Verification Checklist

After customizing the template, verify everything is in place:

- [ ] All file references in **CLAUDE.md** point to actual files
- [ ] Agent files have been reviewed and customized for your stack
- [ ] Memory system directories exist and are git-tracked (except `scratch/`)
- [ ] Document templates are accessible and ready to use
- [ ] Git remote is configured (if using GitHub features)
- [ ] `.gitignore` in `.claude/memory/scratch/` prevents ephemeral notes from being committed

---

## Documentation Links

All references in this template point to actual files:

- [CLAUDE.md](CLAUDE.md) — Complete project documentation
- [TDD Developer Agent](.claude/agents/tdd-developer.md) — Test-first development workflow
- [Code Reviewer Agent](.claude/agents/code-reviewer.md) — Code quality and linting
- [Test Engineer Agent](.claude/agents/test-engineer.md) — E2E and integration testing
- [Memory System Guide](.claude/memory/README.md) — How to use the working memory
- [PRD Template](docs/templates/prd-template.md) — Define features and requirements
- [Epic & Stories Template](docs/templates/epic-stories-template.md) — Break PRDs into actionable work

---

## Next Steps

1. **Read CLAUDE.md** — Understand the project documentation format
2. **Customize CLAUDE.md** — Fill in your project's actual context
3. **Review the agents** — Understand when to use each one
4. **Start your first session** — Open Claude Code and describe your first task
5. **Maintain your memory** — After each session, update the memory system

---

## Questions or Feedback?

This template is designed to evolve with your project. As you use it:
- Update CLAUDE.md as your project principles evolve
- Expand the memory system as you discover patterns
- Customize agents as your team's needs change
- Keep the directory structure consistent so Claude can find everything

For feedback on Claude Code itself, see [/help](https://claude.ai/code) or report issues at https://github.com/anthropics/claude-code/issues.

---

**Template Version:** 1.0  
**Last Updated:** June 2026  
**Designed for:** Claude Code with Claude 3.5+ models
