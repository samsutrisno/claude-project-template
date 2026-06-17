# Project Documentation for Claude Code

This file guides Claude in understanding your project's structure, conventions, and workflows. Update each section to match your project's reality.

## Project Context

Describe your project's purpose, scope, and current state. This helps Claude understand what you're building and why.

**How to fill this:**
- What is the project about? (e.g., "full-stack TODO app", "data pipeline", "CLI tool")
- What's the tech stack? (optional, but helpful)
- What phase is it in? (e.g., "MVP development", "stabilization", "maintenance")
- Any current focus areas or blockers?

**Example:**
```
- Full-stack TODO application with React frontend and Express backend
- Tech stack: React 18, TypeScript, Node.js, PostgreSQL, Docker
- Focus on iterative, feedback-driven development
- Current phase: Backend stabilization and frontend feature completion
- Blockers: Authentication service needs refactoring before feature work can resume
```

---

## Documentation References

Link to your project's existing documentation. This helps Claude navigate your codebase efficiently.

**How to fill this:**
- List paths to key documentation files in your `docs/` folder
- Include brief descriptions of what each document covers
- Only list documents that actually exist (delete empty references)

**Example:**
```
- docs/architecture.md - System design, component relationships, data flow
- docs/testing-guidelines.md - Test patterns, coverage expectations, mocking strategies
- docs/api-spec.md - REST endpoints, request/response formats, error codes
- docs/development-setup.md - Environment setup, dependencies, running locally
- docs/deployment.md - CI/CD pipeline, deployment steps, rollback procedures
```

---

## Development Principles

State the core principles that guide your team's development work.

**How to fill this:**
- What methodology do you follow? (e.g., TDD, BDD, agile)
- What quality standards matter most? (e.g., test coverage, code review, performance)
- Any constraints or non-negotiables? (e.g., "must support IE11", "zero downtime deploys")

**Example:**
```
- **Test-Driven Development**: Red-Green-Refactor cycle for all features
- **Incremental Changes**: Small, testable modifications; avoid massive refactors
- **Systematic Debugging**: Use test failures as guides; don't guess
- **Code Review**: All PRs require review before merge
- **Performance First**: Database queries logged and analyzed; N+1 queries blocked
```

---

## Testing Scope

Define what tests exist, how they're organized, and when to use each type.

**How to fill this:**
- What testing frameworks/tools do you use? (e.g., Jest, Playwright, Cypress)
- Which types of tests exist? (unit, integration, E2E, etc.)
- What's your testing strategy? (e.g., TDD, coverage targets, manual validation)
- Why this approach? (What problem does it solve?)

**Example:**
```
This project uses unit tests, integration tests, and UI end-to-end tests:

**Test Types:**
- Backend: Jest + Supertest for API testing (unit + integration)
- Frontend: React Testing Library for component tests (unit + integration)
- UI testing: Playwright for critical user journey automation (E2E)
- Manual browser testing for exploratory validation and visual checks

**Reason:** Combine fast feedback (unit/integration) with end-to-end quality confidence (UI tests)

**Testing Approach by Context:**
- Backend API changes: Write Jest tests FIRST, then implement (RED-GREEN-REFACTOR)
- Frontend component features: Write React Testing Library tests FIRST for behavior, then implement
- UI/integration flows: Write Playwright tests for critical journeys after E2E works manually
- Visual/UX changes: Manual browser testing required before commit
- This is true TDD: Test first, then code to pass the test
```

---

## Workflow Patterns

Describe standard workflows developers should follow for common tasks.

**How to fill this:**
- What are your common development workflows? (e.g., "add a feature", "fix a bug", "refactor module")
- What steps should be followed? (e.g., write test → fail → implement → pass → refactor)
- In what order? (specificity matters for consistency)

**Example:**
```
**1. TDD Workflow (New Features)**
1. Write failing test(s) that describe the desired behavior
2. Run tests → confirm they fail (RED)
3. Implement code to pass the test (GREEN)
4. Refactor for clarity and efficiency (REFACTOR)
5. All tests pass → code is ready
6. Submit PR for review

**2. Bug Fix Workflow**
1. Reproduce the bug manually (document steps)
2. Write a test that captures the bug (should fail)
3. Fix the code to pass the test
4. Verify the bug is gone manually
5. Run full test suite → all pass
6. Submit PR with bug reproduction steps

**3. Code Quality Workflow**
1. Run linter and type checker
2. Categorize issues: logic errors vs. style violations
3. Fix systematically (logic errors first)
4. Re-validate: linter clean, types safe, tests pass
5. Commit changes

**4. Integration Testing Workflow**
1. Identify the critical user journey to test
2. Write Playwright test for that journey
3. Run locally → debug failures
4. Verify coverage: Does this test catch regressions?
5. Commit with other feature code
```

---

## Agent Usage

Explain when to use Claude Code's specialized agents (if applicable to your project).

**How to fill this:**
- What agents are available for your project?
- When should each be used? (specific task types)
- What should they NOT do? (boundaries)

**Example:**
```
When working with Claude, use these specialized agents for the right tasks:

- **tdd-developer**: For implementation and unit/integration TDD cycles
  - Use for: Writing/fixing unit tests, implementing features, refactoring
  - Do NOT: Create or run Playwright UI tests (that's test-engineer's role)

- **code-reviewer**: For addressing lint errors and code quality improvements
  - Use for: Fixing eslint warnings, simplifying code, removing dead code
  - Do NOT: Change logic or business behavior without explicit request

- **test-engineer**: Owns all Playwright UI test authoring and execution
  - Use for: Creating new UI tests, debugging test failures, isolation checks
  - Do NOT: Modify application code (that's for tdd-developer)

**When in doubt:** Ask Claude which agent is right for the task, or use the general agent.
```

---

## Workflow Utilities

Document any custom scripts, commands, or utilities that speed up development.

**How to fill this:**
- What custom commands exist in your project? (e.g., `/run`, `/test`, `/lint`)
- What do they do?
- How should they be used?
- How do you create new ones?

**Example:**
```
**Custom Development Commands:**

- `/run` - Start the development server (frontend + backend)
  - Usage: `/run`
  - What it does: Spins up React dev server and Express API server, opens browser to localhost:3000

- `/test` - Run all tests with watch mode
  - Usage: `/test` or `/test --coverage` for coverage report
  - What it does: Runs Jest tests, watches for changes, re-runs affected tests

- `/lint` - Check and fix code style
  - Usage: `/lint` to check, `/lint --fix` to auto-fix
  - What it does: Runs ESLint and Prettier

- `/ui-test` - Run Playwright tests
  - Usage: `/ui-test` for headless, `/ui-test --ui` for browser mode
  - What it does: Runs all Playwright tests in headless or UI mode

**To create a new command:** See docs/workflow-patterns.md for the skill definition pattern.
```

---

## Git Workflow

Define your commit conventions and branching strategy.

**How to fill this:**
- What commit format do you use? (e.g., conventional commits)
- What branch naming convention? (e.g., `feature/`, `bugfix/`, `hotfix/`)
- Any merge strategies? (e.g., squash, rebase, merge commits)
- Release process?

**Example:**
```
**Commit Convention:** Conventional Commits

Format: `<type>(<scope>): <description>`

Types:
- `feat`: New feature
- `fix`: Bug fix
- `chore`: Non-code changes (deps, config, tooling)
- `docs`: Documentation only
- `refactor`: Code refactor (no behavior change)
- `test`: Test changes only

Examples:
- `feat(auth): add password reset flow`
- `fix(api): handle null response from external service`
- `chore: upgrade React to 18.2`
- `docs: add setup instructions`

**Branch Naming:**
- Feature: `feature/<descriptive-name>` (e.g., `feature/user-authentication`)
- Bug: `fix/<bug-description>` (e.g., `fix/login-redirect-loop`)
- Hotfix: `hotfix/<issue>` (e.g., `hotfix/db-connection-leak`)
- Main branch: `main` (do not commit directly; always use PRs)

**Commit Process:**
1. Make changes on your feature branch
2. Run tests and linter: `/test` and `/lint --fix`
3. Stage all changes: `git add .`
4. Commit with conventional message: `git commit -m "feat(scope): description"`
5. Push to your branch: `git push origin feature/<name>`
6. Create a PR against `main` with description of changes

**PR Requirements:**
- Descriptive title and body
- All tests passing (CI check)
- Code review approval (at least one reviewer)
- No merge conflicts with `main`

**Issues:**
- Report bugs or feature requests as GitHub Issues
- Use labels to categorize (bug, enhancement, documentation, etc.)
- Link PRs to issues with `Closes #<issue-number>` in PR body
```

---

## Notes for Claude

Add any special instructions or context that will help Claude help you better. This section is flexible and project-specific.

**Example:**
```
- This is a learning project; feedback on code structure and best practices is welcome
- We prefer explicit over implicit; favor clarity over cleverness
- If you see a pattern repeated 3+ times, it might be worth extracting (but ask first)
- Avoid adding new dependencies without discussion
- Database migrations must be reversible
```
