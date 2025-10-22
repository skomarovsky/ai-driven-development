# scope.md

**Version:** 1.0  
**Last updated:** YYYY-MM-DD  
**Status:** Active — defines project boundaries and success criteria

---

## Purpose

[1-2 sentences: What is this document? Why does it exist?]

**Example:**
> This document defines what we're building, why we're building it, what success looks like, and what's explicitly out of scope for {{project_name}}.

---

## Vision

[2-4 sentences describing the aspirational goal or problem being solved]

**Instructions:** Paint the picture of the desired end state. What does success look like from a high level?

**Template:**
```
[Describe the methodology, system, or solution you're building]

[What problem does it solve?]

[What is the high-level approach?]
```

**Example:**
> Establish a deterministic, document-driven methodology that lets human engineers and AI assistants co-design, co-code, and co-deliver software with preserved context, measurable quality, and fast iteration.

---

## Goals (what success looks like)

**Instructions:** List 3-7 concrete goals that define success. Make them specific and observable.

**Template:**
- [Goal 1: Observable outcome]
- [Goal 2: Observable outcome]
- [Goal 3: Observable outcome]
- [Goal 4: Observable outcome]
- [Goal 5: Observable outcome]

**Example:**
- Human+AI sessions follow one repeatable loop (read → plan → implement → validate → document → handoff)
- Work is sliced into ≤1-day increments with objective acceptance criteria
- Context survives between sessions via canonical `handoff.md`
- Quality gates (tests/coverage/security/CI) are explicit and enforced
- Branching/PR workflow yields small, reviewable changes

---

## Success Metrics (SLOs)

**Instructions:** Define 3-7 measurable Service Level Objectives. Include target values and percentiles where applicable.

**Template:**
- [Metric 1]: [Target value] ([p50/p90/p99 if applicable])
- [Metric 2]: [Target value]
- [Metric 3]: [Target value]

**Example:**
- PR lead time (first commit → merge) ≤ 3 days (p50), ≤ 7 days (p90)
- Changed-lines test coverage ≥ 80% on merged PRs
- Secret/SCA scans: 0 critical findings on `main`
- Session continuity: 100% sessions end with valid `handoff.md`
- Rework rate (revert/fix-forward within 48h): ≤ 10%

---

## In Scope

**Instructions:** Explicitly list what IS included in this project. Be specific about deliverables.

**Template:**
- [Deliverable 1]
- [Deliverable 2]
- [Deliverable 3]
- [Process/workflow element 1]
- [Process/workflow element 2]

**Example:**
- Process docs (SSOT methodology, prompt bootstrapper, operator runbook)
- Canonical schemas for `handoff.md` and `tracker.md`
- Minimal CI baseline (lint → build → unit (cov) → secret/SCA → integration)
- Branching/PR policy and checklists

---

## Out of Scope (for now)

**Instructions:** Explicitly list what is NOT included. This prevents scope creep and sets boundaries.

**Template:**
- [Thing 1 that's excluded]
- [Thing 2 that's excluded]
- [Thing 3 that's excluded]
- [Reason why it's excluded, if helpful]

**Example:**
- Team-specific tooling rollouts (e.g., full platform onboarding)
- Performance SLOs beyond what tasks require
- Vendor lock-ins or paid services
- Multi-team coordination workflows

---

## Constraints & Assumptions

**Instructions:** List technical, organizational, or process constraints that affect the project. Also list key assumptions.

**Constraints template:**
- [Constraint 1: e.g., must use existing tech stack]
- [Constraint 2: e.g., no budget for new tools]
- [Constraint 3: e.g., compliance requirement]

**Assumptions template:**
- [Assumption 1: e.g., users have Git experience]
- [Assumption 2: e.g., AI cannot execute code]
- [Assumption 3: e.g., English as working language]

**Example:**
- Source repositories use Git and PR reviews
- The AI cannot run code; the human runs commands and returns raw outputs
- Secrets never leave the operator's local environment
- Team has basic familiarity with CI/CD concepts

---

## Stakeholders

**Instructions:** List who is involved and their role in the project.

**Template:**
| Stakeholder | Role | Responsibility |
|-------------|------|----------------|
| [Name/Title] | [Sponsor/Owner/Advisor/Actor] | [What they do] |
| [Name/Title] | [Role] | [What they do] |

**Example:**
| Stakeholder | Role | Responsibility |
|-------------|------|----------------|
| Product/Tech Lead | Sponsor | Approves scope, resolves conflicts |
| Dev Lead | Owner | Maintains methodology, updates docs |
| QA/DevEx | Advisors | Review quality gates and tooling |
| AI Assistant | Actor | Implements features per methodology |
| Engineers | Actors | Use methodology, provide feedback |

---

## Risks (initial)

**Instructions:** List known risks and proposed mitigations. Update as project progresses.

**Template:**
- **[Risk description]** → Mitigation: [How you'll address it]
- **[Risk description]** → Mitigation: [How you'll address it]

**Example:**
- **Inconsistent handoffs** → Mitigation: Canonical schema enforced, regular reviews
- **Drift across docs** → Mitigation: SSOT rule (only `methodology.md` defines process)
- **Low adoption** → Mitigation: Pilot with one team, iterate based on feedback

---

## Milestones (target dates, adjust as needed)

**Instructions:** Define major milestones with target completion dates. These are checkpoints, not daily tasks.

**Template:**
- **M1**: [Milestone description] — Target: [YYYY-MM-DD]
- **M2**: [Milestone description] — Target: [YYYY-MM-DD]
- **M3**: [Milestone description] — Target: [YYYY-MM-DD]

**Example:**
- **M1**: SSOT v1.1 docs drafted and approved — Target: 2025-10-24
- **M2**: CI baseline template added to reference repo and passing — Target: 2025-10-31
- **M3**: First pilot cycle completed end-to-end with handoff continuity — Target: 2025-11-15
- **M4**: Methodology validated with 3+ teams — Target: 2025-12-01

---

## Dependencies

**Instructions:** List external dependencies (other projects, teams, tools, decisions) that could block progress.

**Template:**
- [Dependency 1]: [Status, owner, risk if not available]
- [Dependency 2]: [Status, owner, risk if not available]

**Example:**
- CI infrastructure: Available, owned by DevOps, medium risk if downtime
- Git repository access: Available, owned by IT, low risk
- Decision on standard test framework: Pending, owned by Tech Lead, high risk if delayed

---

## Non-Goals (what we explicitly won't do)

**Instructions:** Be very explicit about what this project will NOT attempt. Helps avoid misaligned expectations.

**Template:**
- [Non-goal 1]
- [Non-goal 2]
- [Non-goal 3]

**Example:**
- This is not a full SDLC methodology (no requirements gathering, product management)
- This is not a team management framework (no hiring, performance reviews, agile ceremonies)
- This is not a security framework (assumes secure infrastructure exists)
- This is not a specific tool or platform (tool-agnostic approach)

---

## Changelog

**Instructions:** Track major changes to scope over time.

**Template:**
| Date | Version | Changes | Author |
|------|---------|---------|--------|
| YYYY-MM-DD | 1.0 | Initial scope defined | [Name] |
| | | | |

---

## Instructions for Using This Template

1. **Copy this file** to your project's `docs/` directory as `scope.md`
2. **Replace all `[bracketed placeholders]`** with your actual content
3. **Delete instruction paragraphs** after filling in each section
4. **Be specific** — vague goals like "improve quality" aren't useful
5. **Make SLOs measurable** — include numbers, percentiles, thresholds
6. **Update regularly** — scope can evolve, but track changes in Changelog
7. **Reference from other docs** — tracker.md tasks should align with goals here
8. **Review with stakeholders** — get alignment before implementation starts

**This template ensures everyone knows what you're building and what success looks like.**

---

**End of scope_template.md**
