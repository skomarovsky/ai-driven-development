# Methodology Document Relationships

**Visual guide to understanding how all documents connect**

---

## Document Hierarchy

```
┌─────────────────────────────────────────────────────────┐
│                    README.md                                      │
│              (Start Here - Overview)                              │
└────────────────────────┬────────────────────────────────┘
                             │
           ┌──────────────┴──────────────┐
           │                                   │
┌─────────▼──────────┐     ┌────────────▼─────────────┐
│  methodology.md        │     │  ai_patterns.md          │
│  (PROCESS SSOT)        │     │  (Interaction Guide)     │
│  - Workflow              │     │  - Trust/Verify          │
│  - Gates & DoD          │     │  - Prompt Patterns       │
│  - Schemas         │     │  - Error Handling        │
└─────────┬──────────┘     └──────────────────────────┘
          │
    ┌─────┴─────┐
    │           │
┌───▼───┐  ┌───▼──────────────┐
│ Human │  │ AI Prompt v1.2   │
│ Instr │  │ (Session Start)  │
└───────┘  └──────────────────┘
```

---

## Session Flow

```
NEW SESSION
    │
    ├─→ 1. Read methodology_prompt_v1.2.md
    │      (Paste to AI to start session)
    │
    ├─→ 2. AI reads project docs in order:
    │      │
    │      ├─→ handoff.md (where we left off)
    │      ├─→ scope.md (what & why)
    │      ├─→ design.md (how)
    │      └─→ tracker.md (active tasks)
    │
    ├─→ 3. AI produces Opening Brief
    │
    ├─→ 4. Human runs commands per human_instructions.md
    │      (Reference ai_patterns.md for how to interact)
    │
    ├─→ 5. AI produces Closing Report + updated handoff.md
    │
    └─→ 6. Human commits & pushes
           (Next session starts at step 1)
```

---

## Template → Project Document Flow

```
START NEW PROJECT
    │
    ├─→ Copy Templates to Project:
    │   │
    │   ├─→ scope_template.md     ─→  docs/scope.md
    │   ├─→ design_template.md    ─→  docs/design.md
    │   ├─→ tracker_template.md   ─→  docs/tracker.md
    │   └─→ handoff_template.md   ─→  docs/handoff.md
    │
    ├─→ Fill in [placeholders] with your project specifics
    │
    ├─→ Reference full examples when stuck:
    │   ├─→ design.md (full example)
    │   ├─→ scope.md (methodology's own)
    │   └─→ tracker.md (methodology's own)
    │
    └─→ Ready to start first session!
```

---

## Document Dependencies

### Core Process Layer (Methodology)
```
methodology.md (SSOT)
    ↓
    ├─→ References: No dependencies (this is the source of truth)
    ├─→ Referenced by: All other documents
    └─→ Update when: Process changes (rarely)
```

### Human Operator Layer
```
human_instructions.md
    ↓
    ├─→ References: methodology.md §§6-10 (gates, CI, branching)
    ├─→ Referenced by: Human during sessions
    └─→ Update when: Technical steps change
```

### AI Interaction Layer
```
methodology_prompt_v1.2.md
    ↓
    ├─→ References: methodology.md (by section number)
    ├─→ Referenced by: Pasted to AI at session start
    └─→ Update when: AI collaboration approach evolves

ai_patterns.md
    ↓
    ├─→ References: methodology.md §§7-8 (testing, security)
    ├─→ Referenced by: Human when unsure how to interact with AI
    └─→ Update when: New patterns discovered
```

### Project Definition Layer
```
scope.md (YOUR PROJECT)
    ↓
    ├─→ References: methodology.md (implicitly follows its principles)
    ├─→ Referenced by: design.md, tracker.md
    └─→ Update when: Goals or boundaries change

design.md (YOUR PROJECT)
    ↓
    ├─→ References: scope.md (must align with goals)
    ├─→ Referenced by: tracker.md (tasks reference design sections)
    └─→ Update when: Architecture decisions change

tracker.md (YOUR PROJECT)
    ↓
    ├─→ References: scope.md (goals), design.md (sections)
    ├─→ Referenced by: handoff.md (active tasks appear in context)
    └─→ Update when: Tasks change status (continuously)

handoff.md (YOUR PROJECT)
    ↓
    ├─→ References: tracker.md (active tasks), decisions from design.md
    ├─→ Referenced by: Next session's AI prompt
    └─→ Update when: After EVERY session (mandatory)
```

---

## Reading Order by Role

### New Team Member
```
Day 1:
1. README.md (15 min) ─→ Overview
2. methodology.md (30 min) ─→ Process understanding
3. Project's scope.md (10 min) ─→ What we're building
4. Project's design.md (30 min) ─→ How we're building it

Day 2:
5. human_instructions.md (20 min) ─→ Technical operations
6. ai_patterns.md (30 min) ─→ AI interaction
7. Project's tracker.md (10 min) ─→ Current work
8. Try a small task end-to-end
```

### Starting a Session
```
1. handoff.md ─→ Where we left off (2 min)
2. tracker.md ─→ Active task details (3 min)
3. (If needed) design.md sections relevant to task (5 min)
4. Paste methodology_prompt_v1.2.md to AI
5. Confirm with AI: "What task are we working on?"
```

### Making a Design Decision
```
1. scope.md ─→ Does this align with goals?
2. design.md §8 (ADRs) ─→ Similar past decisions?
3. ai_patterns.md §4 ─→ Use "Multi-Option Analysis" pattern
4. Get AI to show tradeoffs
5. Document as new ADR in design.md
6. Update tracker.md task with decision link
```

---

## Document Update Frequency

| Document | Update Frequency | Owner |
|----------|------------------|-------|
| **methodology.md** | Rarely (process changes) | Dev Lead |
| **human_instructions.md** | Occasionally (tool changes) | DevEx |
| **methodology_prompt_v1.2.md** | Occasionally (prompt improvements) | AI + Dev Lead |
| **ai_patterns.md** | Monthly (new patterns learned) | Team |
| **scope.md** | Quarterly (goals evolve) | Product/Tech Lead |
| **design.md** | Monthly (architecture decisions) | Dev Lead |
| **tracker.md** | Continuously (after every change) | Everyone |
| **handoff.md** | After EVERY session (mandatory) | Session operator |

---

## File Size Reference

| Document | Size | Complexity | Read Time |
|----------|------|------------|-----------|
| README.md | 13 KB | Easy | 15 min |
| methodology.md | 4.8 KB | Medium | 20 min |
| human_instructions.md | 8.4 KB | Medium | 25 min |
| methodology_prompt_v1.2.md | 24 KB | Medium | 30 min |
| ai_patterns.md | 22 KB | Medium | 40 min |
| design_template.md | 23 KB | Easy (fill-in) | 10 min |
| design.md (example) | 26 KB | Complex | 60 min |
| scope_template.md | 7.8 KB | Easy (fill-in) | 10 min |
| tracker_template.md | 7.6 KB | Easy (fill-in) | 10 min |
| handoff_template.md | 865 B | Easy (fill-in) | 5 min |

**Total reading time (all docs):** ~4 hours  
**Minimum to start:** ~1 hour (README + methodology.md + fill templates)

---

## Authority Chain (Conflict Resolution)

```
WHO WINS IN A CONFLICT?

methodology.md (SSOT)
    ↓ Always wins over everything
    │
    ├─→ vs. human_instructions.md?
    │   └─→ methodology.md wins (process > technical steps)
    │
    ├─→ vs. scope.md?
    │   └─→ methodology.md wins (process > project goals)
    │
    ├─→ vs. design.md?
    │   └─→ methodology.md wins (process > technical details)
    │
    └─→ vs. ai_patterns.md?
        └─→ methodology.md wins (process > interaction guide)

scope.md (Project SSOT)
    ↓ Wins within project definition
    │
    ├─→ vs. design.md?
    │   └─→ scope.md wins (goals > implementation)
    │
    └─→ vs. tracker.md?
        └─→ scope.md wins (goals > tasks)

design.md (Technical SSOT)
    ↓ Wins for implementation decisions
    │
    └─→ vs. tracker.md?
        └─→ design.md wins (architecture > tasks)

Exception: If methodology.md is WRONG, fix methodology.md first,
           then cascade changes to other docs.
```

---

## Common Navigation Paths

### "How do I start a session?"
```
methodology_prompt_v1.2.md
    (copy entire prompt, paste to AI)
```

### "What's the exact handoff format?"
```
methodology.md §4 → Canonical Schema
    OR
handoff_template.md → Example with placeholders
```

### "How do I know when I'm done?"
```
methodology.md §6 → Definition of Done
    ↓
Check all DoD items:
    ├─→ Lints clean?
    ├─→ Tests pass?
    ├─→ Coverage ≥80%?
    ├─→ Security clean?
    ├─→ Docs updated?
    └─→ PR checklist done?
```

### "AI made a mistake, what do I do?"
```
ai_patterns.md §6 → Handling AI Mistakes
    ↓
5-step recovery protocol:
    1. Identify mistake
    2. Request explanation
    3. Get correction
    4. Verify fix
    5. Add prevention test
```

### "What commands should I run?"
```
human_instructions.md §3 → Status sweep commands
    OR
human_instructions.md §13 → Templates & snippets
```

### "How do I document an architecture decision?"
```
design.md §8 → ADR template
    ↓
Copy template, fill in:
    - Context
    - Decision
    - Consequences
    - Alternatives
```

---

## Document Maturity Model

### Level 0: Just Starting
```
✅ Have: README.md, methodology.md
❌ Don't have: Project docs yet
Action: Copy templates, fill in scope.md first
```

### Level 1: Minimally Viable
```
✅ Have: methodology.md, scope.md, tracker.md, handoff.md
❌ Don't have: design.md (ok for very simple projects)
Action: Can start sessions, but document decisions as you go
```

### Level 2: Functional
```
✅ Have: All core docs + project docs
✅ Using: handoff.md updated after each session
❌ Missing: Some ADRs in design.md
Action: Operational, backfill ADRs when making new decisions
```

### Level 3: Mature
```
✅ Have: All docs complete and current
✅ Using: Continuous updates, regular reviews
✅ Evidence: tracker.md has evidence links, handoff.md always current
Action: Maintain and refine based on learnings
```

---

## Quick Reference: "I Need To..."

| Task | Document to Use |
|------|----------------|
| Start a new project | scope_template.md → fill in goals |
| Start an AI session | methodology_prompt_v1.2.md → paste to AI |
| Run build/test commands | human_instructions.md §3, §13 |
| Handle AI going off-track | ai_patterns.md §3 (context management) |
| Define acceptance criteria | tracker_template.md + methodology.md §5 |
| Document a tech decision | design.md §8 (ADR template) |
| Know what to log | design_template.md §2.3 (Logging Strategy) |
| Set up CI pipeline | methodology.md §9 + human_instructions.md §10 |
| Handle a blocked task | tracker_template.md (Blocked section) + methodology.md §11 |
| End a session | methodology_prompt_v1.2.md (Closing Report template) |
| Review quality gates | methodology.md §6 (DoD checklist) |
| Understand security requirements | methodology.md §8 + design_template.md §4 |

---

**End of Document Relationships Guide**

This visual guide helps you navigate the 11 methodology documents.
Keep this handy as a reference map!
