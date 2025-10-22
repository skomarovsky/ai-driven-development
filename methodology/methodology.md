# AI-Assisted Implementation Methodology (SSOT)
**Version:** 1.1  
**Last updated:** 2025-10-20  
**Status:** Single Source of Truth (SSOT). If any other doc conflicts with this, this file wins.

## 1. Purpose
Establish a deterministic, document-driven way to run multi-session, AI-assisted development with preserved context, explicit gates, and reproducible outputs.

## 2. Principles
1) **Document-driven**: scope → design → tracker → todo → handoff.  
2) **Continuity first**: every session ends with a canonical `handoff.md`.  
3) **Small slices**: ship in ≤1-day steps with measurable acceptance criteria.  
4) **Gated quality**: code is “done” only when it passes all gates in §6–§10.  
5) **Traceability**: each change maps to a tracker item and design section.  
6) **Security by habit**: secrets hygiene is mandatory (see §8).  

## 3. Session workflow (high level)
1) Read docs in order (authority in §4).  
2) Produce an **Opening Brief** (§12 template).  
3) Plan a ≤1-day slice tied to acceptance criteria.  
4) Implement with diffs/full files ready to paste.  
5) Validate: commands + expected results.  
6) Close with a **Closing Report** (§12) and update `handoff.md` (§4 schema).

## 4. Handoff schema (canonical)
Every session must leave a `handoff.md` with exactly these sections:
- **Context Snapshot** – 3–7 bullets of current state.  
- **Active Task(s)** – ID + title + acceptance criteria.  
- **Decisions Made** – brief item + link to design/PR.  
- **Changes Since Last Session** – file path, ±LOC, one-line rationale.  
- **Validation & Evidence** – tests/coverage/benchmarks + where to find logs.  
- **Risks & Unknowns** – each with owner and review date.  
- **Next Steps** – 1–3 steps, each ≤1 day.  
- **Status Summary** – glyph (⚪/🔵/✅/⚠️) + % complete.

> Use the exact headings above. No custom sections.

## 5. Tasks & acceptance
- Each tracker task must include: scope link, design link, acceptance checks, owner, status glyph, % complete, and date(s).  
- Acceptance checks are **objective** (e.g., “unit coverage on changed lines ≥80% and all green CI”).

## 6. Definition of Done (DoD)
A change is **Done** only if all are true:
- Implements the design section referenced by the task.  
- Lints clean; unit tests pass; integration/e2e (if in scope) pass.  
- Coverage target on changed lines met (default 80% unless project overrides).  
- Security scans clean (secrets/SCA) or documented exception with owner/date.  
- `tracker.md` updated (status, %, evidence links).  
- `handoff.md` updated using §4 schema.  
- PR checklist in §10 completed.

## 7. Testing & quality
- **Unit** for every function with nontrivial logic.  
- **Integration** where interfaces meet.  
- **Changed-lines coverage** ≥80% (adjust by project policy).  
- **Benchmarks** only where perf is a requirement; record method + dataset.  
- **Determinism**: tests must be seed-stable or record seeds in `handoff.md`.

## 8. Security & secrets
- Never commit secrets; keep `.env` local; maintain `.env.example` redacted.  
- Run local secret/SCA scans before pushing.  
- If a secret leaks: rotate immediately, scrub history if required, record incident in `handoff.md` (Risks).

## 9. CI expectations
A standard pipeline (order may vary by stack):
1) Lint/format  
2) Build  
3) Unit tests (with coverage output)  
4) Secret/SCA scans  
5) Integration/e2e (if defined)  
Failures must be copied into the session and summarized in `handoff.md`.

## 10. Branching & PRs
- Branch pattern: `feature/<slug>`; urgent fixes use `hotfix/<slug>`.  
- Conventional commits recommended.  
- Open a **draft PR** early; keep it small and cohesive.  
- **PR checklist** (must be ticked before merge):
  - [ ] Lint/build/tests pass in CI  
  - [ ] Coverage target on changed lines met  
  - [ ] Secret/SCA scans clean (or approved exception)  
  - [ ] `tracker.md` updated with evidence links  
  - [ ] `handoff.md` updated (Closing Report is pasted in PR)

## 11. Error recovery & blockers
- When ambiguous, pick the safest default, proceed, and flag in `handoff.md`.  
- When blocked, mark ⚠️ with owner/unblocker, choose a parallel task, and document in `handoff.md`.

## 12. Templates (render exactly)

### Opening Brief
```
## Opening Brief
**Context (from handoff):** …
**Active task(s) & acceptance criteria:** …
**Plan for this session (≤1 day):** …
**Risks/assumptions:** …
**Expected artifacts:** code/tests/docs …
```

### Closing Report
```
## Closing Report
**What changed:** …
**Validation & Evidence:** …
**Status update:** (glyph + %)
**Decisions made:** …
**Risks & Unknowns:** …
**Next steps (≤1 day each):** …
```

## 13. Versioning & governance
- This file is the **SSOT**. Other docs (prompts, runbooks) must point here.  
- Any process change occurs **here first**, then is referenced elsewhere.
