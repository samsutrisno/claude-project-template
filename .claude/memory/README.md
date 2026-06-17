# Memory System for Development

This directory maintains a working memory system to track development discoveries, patterns, and lessons learned throughout your project's lifecycle.

## Purpose

During development, Claude and you will discover patterns, make decisions, and encounter challenges. This memory system captures those learnings so they can be referenced in future work. Rather than re-discovering the same pattern or making the same mistake twice, this system creates a growing knowledge base that becomes more valuable over time.

---

## Memory Architecture

### Two Types of Memory

**1. Persistent Memory (CLAUDE.md)**
- Location: Root of project
- Content: Foundational principles, workflows, conventions
- Lifespan: Stable, rarely changes
- Examples: Testing strategy, commit conventions, development principles
- Use Case: "How do I normally approach X in this project?"
- Committed to git: ✅ Yes (part of codebase documentation)

**2. Working Memory (.claude/memory/)**
- Location: This directory
- Content: Discoveries, patterns, session summaries
- Lifespan: Evolves continuously
- Examples: "We found that X pattern works well for Y", "Session on Z completed with outcomes"
- Use Case: "Have we solved this before? What did we learn?"
- Committed to git: ✅ Yes (except scratch/ directory)

---

## Directory Structure

```
.claude/memory/
├── MEMORY.md                    # Index/dictionary (agents read this FIRST)
├── README.md                    # This file (system documentation)
├── session-notes.md             # Historical session summaries (committed)
├── patterns-discovered.md       # Code patterns and learnings (committed)
└── scratch/                     # Active work (not committed)
    ├── .gitignore              # Ignores all files in scratch/
    └── working-notes.md        # Current session notes (ephemeral)
```

---

## Files and Their Purpose

### 1. MEMORY.md (Index - Committed to Git)

**Purpose:** Quick lookup index for all memories. Agents read this FIRST to find relevant information.

**What goes here:**
- One-line hook for each session summary (with file + line number)
- One-line hook for each pattern discovered (with file + line number)
- Format: `- [Name](file.md#L123) — Brief description`

**Lifespan:** Permanent. Keep it under ~50 lines for fast scanning.

**Size constraint:** If it grows beyond 50 lines, consider archiving old sessions or consolidating patterns.

**Why this matters:** Agents need to scan this file quickly to determine if a pattern or session is relevant before loading the full file. A fast index is more useful than a comprehensive list.

---

### 2. session-notes.md (Committed to Git)

**Purpose:** Document completed work sessions for future reference.

**When to use:**
- At the end of a development session
- When a task, bug fix, or feature is complete
- When you've learned something worth remembering

**What goes here:**
- Session name and date
- What was accomplished
- Key findings and decisions made
- Outcomes and results
- Lessons for future work

**Lifespan:** Permanent. These become your project's historical record.

**After writing:** Add a one-line entry to MEMORY.md pointing to this session.

**Example:** A session on "Refactoring authentication middleware" documents what was changed, why, what worked, what didn't, and what to watch for next time.

---

### 3. patterns-discovered.md (Committed to Git)

**Purpose:** Document recurring code patterns and best practices discovered during development.

**When to use:**
- When you find a pattern that solves a recurring problem
- When you notice the same approach working well across multiple places
- When you discover a gotcha or anti-pattern to avoid

**What goes here:**
- Pattern name (descriptive)
- Context (when/where this pattern applies)
- Problem it solves
- Solution description
- Code example
- Related files where it's used
- Trade-offs or considerations

**Lifespan:** Permanent. These become your project's pattern library.

**After writing:** Add a one-line entry to MEMORY.md pointing to this pattern.

**Example:** A pattern for "Async error handling in Express middleware" documents the problem (unhandled promise rejections), the solution (wrapper function), code examples, and which files use it.

---

### 4. scratch/working-notes.md (Not Committed)

**Purpose:** Take notes during an active development session.

**When to use:**
- During TDD cycles (capture test insights, refactoring decisions)
- During debugging (document what you tried, what worked, blockers)
- During code quality work (linting changes, refactoring notes)
- Any time you discover something mid-task

**What goes here:**
- Current task and goal
- Approach taken
- Key findings (what's working, what's not)
- Decisions made and why
- Blockers or challenges
- Next steps
- Misc notes

**Lifespan:** Ephemeral. At end of session, review this file and:
- Summarize key findings into `session-notes.md` (for posterity)
- Extract any recurring patterns into `patterns-discovered.md` (for future use)
- Add new entries to `MEMORY.md` if needed
- Delete or clear this file (it's ignored by git anyway)

**Workflow:**
1. Start session → Create/open `scratch/working-notes.md`
2. Work → Update notes as you go
3. Complete task → Review notes
4. Distill → Extract findings into persistent memory files
5. Update → Add entries to MEMORY.md
6. Clean up → Clear this file for next session

---

## How AI Agents Use This Memory System

When Claude works on your project, it will:

1. **Read MEMORY.md** → Scan index for relevant memories (fast, ~30 seconds)
2. **Identify relevant entries** → Click links to pattern or session that applies
3. **Read session-notes.md or patterns-discovered.md** → Understand the full context (the specific section)
4. **Read scratch/working-notes.md** (if session in progress) → Continue from where you left off
5. **Apply learnings** → Make context-aware suggestions based on accumulated knowledge

---

## Workflow Integration

### During TDD Cycles
1. Write failing test
2. Note in `scratch/working-notes.md`: "Testing approach - what the test covers, why"
3. Implement code
4. Note: "Implementation - decisions made, trade-offs considered"
5. Refactor
6. Note: "Refactoring - what improved, why"
7. Session complete → Summarize in `session-notes.md` → Add entry to `MEMORY.md`

### During Debugging
1. Reproduce issue
2. Note in `scratch/working-notes.md`: "Bug description, reproduction steps"
3. Debug
4. Note: "Investigation findings - what tried, what worked"
5. Fix implemented
6. Note: "Solution - root cause, fix approach"
7. Session complete → Summarize in `session-notes.md` → Add entry to `MEMORY.md`

### During Code Quality Work
1. Run linter
2. Note in `scratch/working-notes.md`: "Issues found, categorization"
3. Fix issues
4. Note: "Fixes applied, patterns noticed"
5. Session complete → Summarize findings → Extract patterns → Update `MEMORY.md`

---

## Best Practices

**DO:**
- ✅ Keep MEMORY.md fast-scannable (under 50 lines)
- ✅ Write session notes in natural language (not overly formal)
- ✅ Include code examples for patterns
- ✅ Note file paths and line numbers for context
- ✅ Link related patterns and sessions
- ✅ Be specific about trade-offs and why choices were made
- ✅ Update patterns-discovered.md as you learn what works

**DON'T:**
- ❌ Leave scratch/working-notes.md indefinitely (summarize it)
- ❌ Duplicate information between files (link instead)
- ❌ Write vague notes ("fixed stuff") - be specific
- ❌ Ignore patterns that appear 2-3 times (document them)
- ❌ Let MEMORY.md grow beyond 50 lines (archive old entries instead)

---

## Example Workflow

**Session: Implementing user authentication (June 17, 2026)**

1. Start day → Open `scratch/working-notes.md`
   - Current Task: Add JWT token validation middleware
   - Approach: Write test first (TDD), implement middleware, integrate with routes

2. Throughout day → Update notes
   - Key Findings: "Express middleware chaining requires error handling wrapper"
   - Decisions Made: "Using async/await wrapper pattern for cleaner error handling"
   - Blockers: "Need to understand how Express handles promise rejections"

3. End of day → Review and distill
   - Summarize session into `session-notes.md` (line 15+)
   - Extract pattern into `patterns-discovered.md` (line 20+)
   - Add entries to `MEMORY.md`:
     - `- [Authentication Middleware Refactor](session-notes.md#L15) — JWT validation middleware implementation`
     - `- [Async Error Handling in Express](patterns-discovered.md#L20) — Wrapper pattern for middleware`
   - Clear `scratch/working-notes.md` for next session

4. Next developer → Can reference:
   - MEMORY.md to see what's available
   - Session notes to see what was done and why
   - Patterns file to understand proven solutions
   - CLAUDE.md to understand project principles

---

## Getting Started

1. **First time?** Read this README to understand the system
2. **Starting a session?** Open `scratch/working-notes.md` and begin taking notes
3. **Completing a session?** Review your notes and summarize into persistent memory
4. **Looking for patterns?** Check `MEMORY.md` first, then dive into `patterns-discovered.md`
5. **Understanding history?** Check `MEMORY.md`, then read relevant `session-notes.md` entries

This system is collaborative. You and Claude both contribute to and learn from these files.
