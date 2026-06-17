# Memory Index

This is the entry point to your project's accumulated knowledge. **Agents read this file FIRST** to quickly locate relevant patterns and session notes.

**Format:** `- [Name](file.md#LXX) — One-line description`

---

## Session Notes

Historical summaries of completed development work. Reference these to understand what's been done and lessons learned.

- [Authentication Middleware Refactor](session-notes.md#L15) — JWT validation middleware implementation and Express error handling learnings
- [Testing Strategy Definition](session-notes.md#L30) — Established unit, integration, and E2E testing approach

## Patterns Discovered

Code patterns and solutions that work well in this project. Reference these before writing similar code.

- [Async Error Handling in Express](patterns-discovered.md#L20) — Wrapper pattern for clean async/await error handling in middleware
- [Service Initialization Pattern](patterns-discovered.md#L45) — Empty array vs null initialization for service dependencies

---

## How Agents Use This Index

1. **Read this file first** to scan available memories
2. **Click the link** to jump to the specific entry in the source file
3. **Read the full context** in the linked file
4. **Apply the learning** to current work

## How to Update This Index

- **After completing a session:** Add new entry to session-notes.md, then add link here with line number
- **After documenting a pattern:** Add new entry to patterns-discovered.md, then add link here
- **During debugging/decisions:** Check this index for relevant prior work before proceeding

---

**Max entries to keep:** Keep MEMORY.md under ~50 lines so it's always fast to scan. Archive very old sessions if needed.
