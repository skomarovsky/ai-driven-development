# AI-Assisted Development Methodology

**Version:** 1.1  
**Last updated:** 2025-10-21  
**Purpose:** A complete session management protocol for AI-assisted coding

---

## What Is This?

This is a **deterministic, document-driven methodology** for human engineers and AI assistants to collaborate on software development. It solves the critical problem of **context continuity** across AI sessions while maintaining quality through explicit gates.

**Key Innovation:** The canonical `handoff.md` mechanism that preserves context between sessions, enabling multi-day/week projects with AI assistance.

---

## Quick Start (5 Minutes)

1. **Read** `methodology.md` (the SSOT) — understand the workflow and gates
2. **Copy templates** to your project:
   - `scope_template.md` → `docs/scope.md`
   - `design_template.md` → `docs/design.md`
   - `tracker_template.md` → `docs/tracker.md`
   - `handoff_template.md` → `docs/handoff.md`
3. **Fill in templates** with your project specifics
4. **Start a session** by pasting `methodology_prompt_v1.2.md` to your AI
5. **Follow the workflow**: Read → Plan → Implement → Validate → Document → Handoff

---

## Document Map

### Core Process Documents (Must Read)

| Document | Purpose | When to Use |
|----------|---------|-------------|
| **methodology.md** | SSOT: defines workflow, gates, schemas, templates | Before starting, when resolving conflicts |
| **human_instructions.md** | Operator runbook: commands, checklists, git workflow | During sessions, for technical steps |
| **methodology_prompt_v1.2.md** | Universal AI session-start prompt | Start of every AI session |
| **ai_patterns.md** | Human-AI interaction patterns and best practices | When unsure how to prompt AI, handle mistakes |

### Project Documents (Create for Each Project)

| Document | Purpose | When to Create |
|----------|---------|----------------|
| **scope.md** | Vision, goals, SLOs, boundaries | Project kickoff |
| **design.md** | Architecture, tech stack, patterns, ADRs | After scope, before coding |
| **tracker.md** | Task list with acceptance criteria, status, evidence | Project kickoff, update continuously |
| **handoff.md** | Session continuity: context, decisions, next steps | After every session |
| **todo.md** (optional) | Near-term task priorities | When backlog is large |

### Templates (Copy These)

| Template | Use For | Output |
|----------|---------|--------|
| **scope_template.md** | Defining project boundaries and success | `docs/scope.md` |
| **design_template.md** | Documenting architecture and patterns | `docs/design.md` |
| **tracker_template.md** | Tracking tasks and progress | `docs/tracker.md` |
| **handoff_template.md** | Session continuity | `docs/handoff.md` |

### Examples (Reference These)

| Example | Shows | Use When |
|---------|-------|----------|
| **design.md** (full example) | Complete design doc with real content | Need inspiration for what goes in design.md |
| **scope.md** (in original zip) | Methodology's own scope | Need example of scope doc |
| **tracker.md** (in original zip) | Methodology's own tasks | Need example of tracker doc |

---

## Document Authority & Reading Order

**Authority hierarchy (conflicts resolved by precedence):**
1. `methodology.md` — Process SSOT, always wins
2. `scope.md` — Project boundaries and goals
3. `design.md` — Technical implementation details
4. Other project docs

**Reading order at session start:**
1. `handoff.md` — Where we left off
2. `scope.md` — What we're building and why
3. `design.md` — How we're building it
4. `tracker.md` — Active tasks and acceptance criteria
5. `todo.md` — Near-term priorities (if exists)

---

## Workflow Overview

```
Session Start
    ↓m
Read Docs (handoff → scope → design → tracker)
    ↓
Opening Brief (context + plan + risks)
    ↓
Plan ≤1-day Slice
    ↓
Implement (diffs + tests + docs)
    ↓
Validate (run commands, capture evidence)
    ↓
Update Docs (tracker.md status + handoff.md)
    ↓
Closing Report (what/validation/status/decisions/risks/next)
    ↓
Commit & Push
    ↓
Session End
```

**Every session MUST:**
- Start with Opening Brief
- End with Closing Report
- Update handoff.md using canonical schema (8 sections)
- Update tracker.md with status/evidence

---

## Key Concepts

### 1. Canonical Handoff Schema

**The 8 required sections** (exact headings, no custom sections):
1. Context Snapshot (3-7 bullets)
2. Active Task(s) (ID + acceptance criteria)
3. Decisions Made (brief + links)
4. Changes Since Last Session (files + LOC + rationale)
5. Validation & Evidence (tests/coverage/benchmarks)
6. Risks & Unknowns (owner + review date)
7. Next Steps (≤1 day each)
8. Status Summary (glyph + %)

**Why critical:** Preserves context across sessions, prevents AI from "forgetting"

### 2. Definition of Done (DoD)

A change is **Done** only when:
- ✅ Implements referenced design section
- ✅ Lints clean, tests pass
- ✅ Coverage ≥80% on changed lines (methodology.md §7)
- ✅ Security scans clean (methodology.md §8)
- ✅ tracker.md updated (status, %, evidence)
- ✅ handoff.md updated (canonical schema)
- ✅ PR checklist complete (methodology.md §10)

### 3. Small Slices (≤1 Day)

**Why:** Enables faster feedback, easier review, maintains momentum

**How to slice:**
- Focus on one acceptance criterion at a time
- Vertical slice (API → service → DB → tests)
- If >1 day, break into sub-tasks

### 4. Evidence-Based

**Never accept claims without proof:**
- ❌ "Tests pass" → ✅ "14/14 tests passing, 87% coverage"
- ❌ "Faster" → ✅ "P95: 145ms (baseline: 230ms)"
- ❌ "Secure" → ✅ "0 critical findings in npm audit"

### 5. AI as Collaborator, Not Oracle

**Human owns:**
- Running commands and providing outputs
- Committing/pushing code
- Final decisions on architecture and security
- Enforcing process gates

**AI provides:**
- Code generation and diffs
- Test suggestions
- Debugging assistance
- Documentation drafts
- Tradeoff analysis

---

## File Organization

Recommended project structure:

```
project-root/
├── docs/
│   ├── methodology/          # This methodology package
│   │   ├── methodology.md
│   │   ├── human_instructions.md
│   │   ├── methodology_prompt_v1.2.md
│   │   ├── ai_patterns.md
│   │   └── [templates]
│   ├── scope.md              # Your project's scope
│   ├── design.md             # Your project's design
│   ├── tracker.md            # Your project's tasks
│   └── handoff.md            # Updated after every session
├── src/                      # Your code
├── test/                     # Your tests
└── [other project files]
```

---

## Success Metrics (From Scope)

Track these to measure methodology effectiveness:
- **PR lead time**: ≤3 days (p50), ≤7 days (p90)
- **Test coverage**: ≥80% on changed lines
- **Secret findings**: 0 critical on main
- **Session continuity**: 100% sessions have valid handoff.md
- **Rework rate**: ≤10% (reverts/fixes within 48h)

---

## Common Pitfalls & Solutions

### Pitfall 1: Skipping Handoff Updates
**Problem:** Next session starts with stale context  
**Solution:** Make handoff.md update part of Closing Report (non-negotiable)

### Pitfall 2: Vague Acceptance Criteria
**Problem:** Unclear when task is "done"  
**Solution:** Use measurable criteria ("14 tests pass, 80% coverage" not "good test coverage")

### Pitfall 3: Long Sessions Without Check-ins
**Problem:** AI loses context, goes off track  
**Solution:** Check in every ~10 exchanges: "What are we working on?"

### Pitfall 4: Accepting AI Output Without Verification
**Problem:** Bugs, security issues, hallucinated APIs  
**Solution:** Always run commands, verify outputs, use evidence checklists (ai_patterns.md §5)

### Pitfall 5: Bypassing Quality Gates
**Problem:** Technical debt, production issues  
**Solution:** DoD is mandatory, no exceptions (methodology.md §6)

---

## Customization Guidelines

**You can customize:**
- ✅ Coverage threshold (default 80%, adjust in scope.md)
- ✅ Branch naming (default `feature/<slug>`)
- ✅ Tech stack and tools (define in design.md)
- ✅ Additional ADRs in design.md
- ✅ Task types/tags in tracker.md

**You cannot customize:**
- ❌ Handoff.md schema (8 sections, exact headings)
- ❌ Session workflow (methodology.md §3)
- ❌ DoD requirements (methodology.md §6)
- ❌ Authority hierarchy (methodology.md is SSOT)

**Why:** Consistency enables AI to reconstruct context reliably

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.1 | 2025-10-21 | Added ai_patterns.md, improved prompt v1.2, created templates |
| 1.0 | 2025-10-20 | Initial methodology documents |

---

## Getting Help

**If confused about:**
- **Process/gates** → Read `methodology.md` §[relevant section]
- **Technical steps** → Read `human_instructions.md`
- **AI interaction** → Read `ai_patterns.md`
- **Document structure** → Check templates with "_template" suffix
- **Examples** → Look at `design.md` (full example) or methodology's own docs

**If methodology doesn't cover your need:**
1. Check if it's in scope (this is session management, not full SDLC)
2. Adapt within the framework (don't break core principles)
3. Document your adaptation in project's design.md
4. Consider contributing back if it's generally useful

---

## Contributing & Feedback

This methodology is continuously improved based on usage.

**Feedback welcome on:**
- ✅ Clarity: What's confusing?
- ✅ Completeness: What's missing within scope?
- ✅ Practicality: What's hard to follow?
- ✅ Examples: What needs better examples?

**Out of scope for this methodology:**
- ❌ Requirements gathering
- ❌ Product management
- ❌ Team collaboration (beyond human-AI)
- ❌ Post-deployment operations

---

## FAQ

**Q: Do I need all these documents for a small project?**  
A: Minimum viable set: methodology.md, scope.md, tracker.md, handoff.md. Skip design.md for very simple projects.

**Q: Can I use this without AI?**  
A: Yes! The small slices, evidence-based validation, and handoff mechanism work for human-only teams too.

**Q: How long does a typical session take?**  
A: 1-3 hours. Longer sessions risk context loss. Break into multiple sessions if needed.

**Q: What if AI makes a mistake?**  
A: See ai_patterns.md §6 for recovery protocol. Always verify, never trust blindly.

**Q: Can I use this with [my favorite AI]?**  
A: Yes, methodology is AI-agnostic. Prompt v1.2 is tuned for Claude but adaptable.

**Q: How do I handle large codebases?**  
A: Focus AI on one module at a time. Use design.md to document the broader architecture.

**Q: What about code review by humans?**  
A: PRs should still be reviewed by humans. This methodology doesn't replace peer review.

**Q: Coverage is below 80%, what do I do?**  
A: Either write more tests (preferred) or document exception in handoff.md → Risks with justification.

---

## License & Attribution

Copyright (c) 2025 Stan Komarovsky

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to use,
copy, modify, and distribute the Software for any purpose, provided that
proper credit is given to the author:

    "Based on work by Stan Komarovsky (github.com/skomarovsky/ai-driven-development)"

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.

**Attribution:** Methodology developed for AI-assisted software development, October 2025.

---

## Next Steps

**For new projects:**
1. Read methodology.md (15 min)
2. Copy templates to docs/ directory
3. Fill in scope.md with your project goals
4. Start first session with methodology_prompt_v1.2.md
5. Follow the workflow

**For existing projects:**
1. Read methodology.md
2. Create scope.md to document existing project
3. Create design.md to document existing architecture
4. Create tracker.md for current and planned work
5. Start using handoff.md for session continuity

**For learning:**
1. Read this README
2. Read methodology.md
3. Study the examples (design.md, scope.md, tracker.md)
4. Read ai_patterns.md for interaction tips
5. Try a pilot task end-to-end

---

**End of README**

---

## Document Index (Complete Set)

### Must Read First
- ✅ **README.md** (this file) — Overview and quick start
- ✅ **methodology.md** — SSOT for process, gates, schemas

### Process Guides
- ✅ **human_instructions.md** — Operator runbook
- ✅ **methodology_prompt_v1.2.md** — AI session-start prompt
- ✅ **ai_patterns.md** — Human-AI interaction patterns

### Templates (Copy & Fill)
- ✅ **scope_template.md** → your `scope.md`
- ✅ **design_template.md** → your `design.md`
- ✅ **tracker_template.md** → your `tracker.md`
- ✅ **handoff_template.md** → your `handoff.md`

### Examples (Reference)
- ✅ **design.md** (full example) — Complete design doc with all sections filled
- ✅ **scope.md** (methodology's own)
- ✅ **tracker.md** (methodology's own)

**Total:** 13 documents covering session management protocol for AI-assisted coding
 
