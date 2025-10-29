# todo.md
**Session Date:** YYYY-MM-DD  
**Time Budget:** [X hours]  
**Session Goal:** [Brief description of what this session aims to accomplish]

---

## Purpose

This document contains the **session-specific subset** of tasks from `tracker.md` that will be worked on in this session. It's the bounded, manageable work plan that fits within:
- AI's context window (can be fully loaded and reasoned about)
- Human's time budget (typically 1-3 hours)
- A single coherent goal (one feature, one fix, one investigation)

**Use this when:**
- Starting a new session (select tasks from tracker.md)
- AI needs to know "what are we working on RIGHT NOW"
- You want to adjust priorities before engaging AI

---

## Instructions for Use

1. **Before each session:** Copy relevant tasks from `tracker.md`
2. **Adjust priorities:** Reorder based on current needs, dependencies, blockers
3. **Set realistic scope:** Better to complete 1 task fully than start 3 and finish none
4. **Review with AI:** AI reads this to understand session focus
5. **After session:** Update `tracker.md` with progress, create fresh `todo.md` for next session

---

## Active Tasks for This Session

**Instructions:** List 1-3 tasks from tracker.md that you'll work on this session. Include task ID, title, and acceptance criteria for easy reference.

### T-XXX — [Task Title from Tracker]

**From tracker.md:**
- Acceptance criteria:
  - [Criterion 1]
  - [Criterion 2]
  - [Criterion 3]

**Session-specific notes:**
- [Any context specific to working on this NOW]
- [Dependencies to check before starting]
- [Known risks or blockers to watch for]

**Expected progress this session:**
- [What you realistically expect to complete]
- [What might carry over to next session]

---

### T-XXX — [Second Task, if time permits]

**From tracker.md:**
- Acceptance criteria:
  - [Criterion 1]
  - [Criterion 2]

**Session-specific notes:**
- [Context]
- [Only start if T-XXX completes early]

---

## Session Priorities

**Must complete (P0):**
- T-XXX: [Task title] — [Why this is critical]

**Should complete (P1):**
- T-XXX: [Task title] — [Nice to have, but not blocking]

**Could complete if time (P2):**
- T-XXX: [Task title] — [Bonus if we get here]

---

## Context for This Session

**What happened last session:**
- [Brief summary from handoff.md]
- [Any carry-over from last time]

**Current blockers/dependencies:**
- [Anything that could stop progress]
- [Dependencies on other tasks or external factors]

**Environment notes:**
- [Any special setup needed: test DB, API keys, etc.]
- [Known issues with local environment]

---

## Success Criteria for This Session

By end of session, we should have:
- [ ] [Specific deliverable 1]
- [ ] [Specific deliverable 2]
- [ ] [Tests written and passing]
- [ ] [Documentation updated]
- [ ] [Tracker.md updated with progress]
- [ ] [Handoff.md updated with session results]

If we don't complete everything:
- [What's the minimum viable progress?]
- [What can safely carry over?]

---

## Time Boxing

**Estimated breakdown:**
- Task 1 (T-XXX): [X minutes/hours]
- Task 2 (T-XXX): [Y minutes/hours]
- Testing/validation: [Z minutes]
- Documentation updates: [Z minutes]
- Buffer for unexpected issues: [Z minutes]

**Total:** [Should match time budget]

---

## Notes & Reminders

**Before starting:**
- [ ] Read handoff.md (where we left off)
- [ ] Verify local environment is working
- [ ] Check tracker.md for any updates to acceptance criteria
- [ ] Review design.md sections relevant to these tasks

**During session:**
- [ ] Run validation commands after each change
- [ ] Paste full outputs to AI (not summaries)
- [ ] Update tracker.md as tasks progress
- [ ] Flag any new risks or decisions in notes

**After session:**
- [ ] Generate Closing Report
- [ ] Update handoff.md using canonical schema
- [ ] Update tracker.md with status/evidence
- [ ] Commit and push changes
- [ ] Create todo.md for next session

---

## Example TODO (Reference)

```markdown
# todo.md

**Session Date:** 2025-10-23
**Time Budget:** 2 hours
**Session Goal:** Complete rate limiting implementation (T-015)

## Active Tasks for This Session

### T-015 — Add Rate Limiting to API

**From tracker.md:**
- Acceptance criteria:
  - 100 requests/min per IP enforced
  - Returns 429 status when limit exceeded
  - Unit tests ≥80% coverage
  - Integration test proves limit works

**Session-specific notes:**
- Last session completed auth (T-014), so Redis is already set up
- Design.md §5.3 specifies token bucket algorithm
- Need to handle Redis unavailable scenario (fail closed per security req)

**Expected progress this session:**
- Complete implementation (60 min)
- Write unit tests (40 min)
- Write integration test (20 min)

## Session Priorities

**Must complete (P0):**
- T-015: Rate limiting — Blocks T-016 (admin bypass)

## Context for This Session

**What happened last session:**
- Completed JWT auth (T-014)
- Redis is running and tested
- All auth tests passing

**Current blockers/dependencies:**
- None — Redis dependency already met

**Environment notes:**
- Redis running on localhost:6379
- Need to test with multiple IPs (can use curl with different headers)

## Success Criteria for This Session

By end of session, we should have:
- [x] Rate limiter middleware implemented
- [x] 12+ unit tests passing with ≥80% coverage
- [x] Integration test proves 100 req/min limit
- [x] Tracker.md updated to T-015: ✅ 100%
- [x] Handoff.md updated with session results

## Time Boxing

- Implementation: 60 min
- Unit tests: 40 min
- Integration test: 20 min
- Total: 2 hours (matches budget)
```

---

## Relationship to Other Documents

**todo.md connects to:**

- **tracker.md** — Source of tasks (todo is a subset)
  - Tasks copied FROM tracker.md
  - Progress flows BACK TO tracker.md

- **handoff.md** — Context of where we left off
  - Read handoff.md BEFORE creating todo.md
  - Update handoff.md AFTER completing todo.md work

- **design.md** — Technical context for tasks
  - Reference design.md sections while working on todo tasks
  - Ensure todo work follows design.md patterns

- **scope.md** — Validates tasks are in scope
  - Ensure todo tasks align with scope.md goals
  - Don't add todo items that are out of scope

---

## Template for Quick Sessions

For very short sessions (30-60 minutes):

```markdown
# todo.md — Quick Session

**Date:** YYYY-MM-DD | **Budget:** 1 hour

**Goal:** [One specific thing]

**Task:** T-XXX — [Title]

**Plan:**
1. [Step 1] — 20 min
2. [Step 2] — 20 min
3. [Validate] — 20 min

**Success:** [Done when X is true]
```

---

## Anti-Patterns (Don't Do This)

❌ **Overloading todo.md with too many tasks**
- More than 3 tasks usually means you won't finish any
- Better: 1 task done than 3 tasks half-done

❌ **Using todo.md as permanent task list**
- Todo.md is ephemeral (per-session)
- Tracker.md is permanent (project-wide)
- Don't lose tasks by keeping them only in todo.md

❌ **Skipping todo.md creation**
- Without it, AI doesn't know what to focus on
- Leads to wandering sessions without clear goals

❌ **Not updating after session**
- Stale todo.md confuses next session
- Always create fresh todo.md based on updated tracker.md

❌ **Ignoring time budget**
- Setting 4 hours of work in 2-hour session
- Leads to incomplete work and poor handoffs

---

## Changelog

| Date | Changes | Author |
|------|---------|--------|
| YYYY-MM-DD | Initial template created | [Name] |
| | | |

---

**End of todo.md template**

## Summary

**todo.md is:**
- ✅ Session-specific (new each session)
- ✅ Subset of tracker.md (bounded scope)
- ✅ Time-boxed (fits session budget)
- ✅ Focused (1-3 tasks maximum)
- ✅ Updated before each session starts
- ✅ Used to guide AI on "what now"

**todo.md is NOT:**
- ❌ Permanent task storage (that's tracker.md)
- ❌ Strategic planning (that's scope.md)
- ❌ Technical decisions (that's design.md)
- ❌ Session results (that's handoff.md)

Think of todo.md as your **session work order** — what's on the workbench right now.
