# human_instruction.md

**Version:** 1.1  
**Last updated:** 2025-10-20  
**Source of Truth:** Aligns with `methodology.md` v1.1 (SSOT). If anything conflicts, the SSOT wins.

> **Purpose.** This is your practical, step-by-step runbook as the human operator. It tells you what to run, what to capture, and what to commit—without redefining process. For workflow, gates, and templates, refer to `methodology.md`.

---

## TL;DR (fast lane)
1. **Sync & branch**
   ```bash
   git fetch origin && git switch main && git pull
   git switch -c feature/<slug>
   ```
2. **Read** `handoff.md` first (canonical schema), then skim `scope.md`, `design.md`, `tracker.md`, `todo.md`.  
3. **Status sweep** (build/test/lint/scan) to verify you can run the project locally.  
4. **Start session with the prompt** in `methodology_prompt.md`.  
5. **Run commands** the AI provides; paste full outputs back.  
6. **Update docs** (AI should draft them): commit `tracker.md` + `handoff.md` updates.  
7. **Open/refresh PR**, push branch, and ensure CI is green.  
8. **Close** using the SSOT “Closing Report” and push the updated `handoff.md`.

---

## 1) Roles & Handoffs (what you own)
- You **run** builds/tests/linters/scans and **provide raw outputs** back to the AI.
- You **commit/push** code & doc changes, open PRs, and **shepherd CI**.
- You **keep secrets safe** (.env, tokens) and **never** commit them.
- You **enforce gates** by referencing `methodology.md`:
  - DoD (§6), Testing (§7), Security (§8), CI (§9), Branching/PR (§10).
- You **ensure continuity** by updating `handoff.md` using the SSOT schema (§4).

---

## 2) One-time environment setup
> Adjust per stack; the AI will specify exact tools/versions as needed.

- **Git & signing (optional):**
  ```bash
  git config --global pull.rebase true
  git config --global commit.gpgsign true   # if you use signing
  ```
- **Pre-commit hooks (recommended):**
  ```bash
  pipx install pre-commit || pip install pre-commit
  pre-commit install
  ```
- **Language/toolchain managers** (pick what you use):  
  - Node: `nvm install && nvm use` → `npm ci`  
  - Python: `pyenv install -s <ver>` → `python -m venv .venv && source .venv/bin/activate && pip install -r requirements.txt`  
  - Go: `go env -w GOINSECURE=off` (then `go mod download`)  
- **Secrets:** Create `.env` from `.env.example` and **do not commit** `.env`.

---

## 3) Start-of-session checklist (repeat every session)
1. **Sync** and **branch** (see TL;DR).  
2. **Read docs in SSOT order:** `handoff.md` → `scope.md` → `design.md` → `tracker.md` → `todo.md`.  
3. **Status sweep** (examples below).  
4. **Launch the session** by pasting the Universal Prompt from `methodology_prompt.md`.  
5. **Confirm acceptance criteria** for the active task(s) in `tracker.md`.

**Status sweep — generic commands**
> Replace with project-specific commands; the AI will refine.
```bash
# Build
npm run build || make build || ./gradlew build

# Tests (aim to see coverage too)
npm test -- --coverage || pytest -q --maxfail=1 --disable-warnings --cov || go test ./... -count=1

# Lint/format
npm run lint || ruff check . || golangci-lint run

# Secret scan (local pre-flight)
pre-commit run --all-files || true
```

---

## 4) Executing AI-produced changes
- Apply patches/diffs exactly as provided (or ask the AI to reprint the full file).
- **Run** the commands it lists; **paste full outputs** back (not summaries).
- If something fails, include:
  - The **exact command**, **exit code**, and the **last 100 lines** of output.
  - Your OS/runtime versions: `node -v`, `python --version`, `go version`, etc.

---

## 5) Evidence capture (so the AI can validate)
- **Tests:** copy counts and failing test names; attach coverage % (changed lines threshold is in SSOT §7/§6).
- **Benchmarks:** include the dataset/fixture used and the numeric results.
- **Logs:** paste text blocks; for long logs, attach the tail/head and mention the location (`artifacts/`, `logs/`).

---

## 6) Tracker & Handoff updates (continuity)
- The AI will **draft updates**; you **commit** them. If absent, ask the AI for the exact diffs to paste.
- Follow SSOT **Handoff Schema** (§4) strictly (no custom headings).
- In `tracker.md`, ensure each active task has:
  - **Status glyph** + **% complete**, **dates**, and **measurable acceptance criteria** (§5).
  - **Evidence links** (test runs, PRs, logs) when available.

---

## 7) Git workflow (concise)
- **Branching:** `feature/<slug>`; use `hotfix/<slug>` for urgent fixes (SSOT §10).
- **Conventional commits:**
  ```text
  feat(component): short description
  fix(api): handle null payloads in v2 adapter
  docs(methodology): update handoff schema reference
  ```
- **Small commits,** cohesive diffs. Avoid mixing refactors with feature changes.

---

## 8) Pull Requests (PR) — open early, update often
- Open a **draft PR** once you have the first meaningful diff.  
- PR body: link to tracker item(s), reference design sections, and paste the **Closing Report** summary when finishing.
- Use the **PR Checklist** in SSOT §10. The PR should show:
  - Passing **CI** (SSOT §9), coverage gate met, secret scan clean.
  - Updated `tracker.md` and `handoff.md`.

---

## 9) Security & secrets (you enforce locally)
- Never commit secrets; `.env` stays local. Keep `.env.example` redacted and current.
- Run **pre-commit** and any **SCA/secret scans** locally before pushing (SSOT §8).
- If a secret leak is suspected: rotate credentials, purge history if necessary, and document in `handoff.md` → “Risks & Unknowns”.

---

## 10) CI baseline (what to expect)
> The repo’s CI should reflect SSOT §9. Locally you can approximate:

```bash
# 1) Lint/format → 2) Build → 3) Unit tests (coverage) → 4) Secret scan/SCA → 5) Integration tests
npm run lint && npm run build && npm test -- --coverage
# or
ruff check . && pytest --cov && pre-commit run --all-files
# or
golangci-lint run && go build ./... && go test ./... -count=1
```

If CI fails on PR:
- Click through to the failing job → copy failing step log → send to AI.
- Re-run after `git pull --rebase` if the base moved.

---

## 11) Troubleshooting (greatest hits)
- **Build won’t start:** confirm toolchain versions (`node -v`, `python --version`, `java -version`), compare to what AI expects.
- **Dependency hell:** clear caches (`npm ci` vs `npm install`; `pip install --no-cache-dir`; `go clean -modcache`); confirm lockfiles.
- **Flaky tests:** re-run with `-k`/`--runInBand`/`-count=1`; capture seed/randomization flags.
- **Coverage shortfall:** ask AI for **targeted unit tests** for uncovered lines (SSOT §7).
- **Secret scan hits:** identify file, rotate secrets, and force-push scrubbed history **only if instructed**.

---

## 12) End-of-session checklist (map to SSOT §12)
- [ ] All commands run; outputs pasted to AI.  
- [ ] **`tracker.md` updated** (status/%/dates/evidence).  
- [ ] **`handoff.md` updated** using the canonical schema (Context, Active Tasks, Decisions, Changes, Validation, Risks, Next Steps, Status).  
- [ ] Branch pushed; PR updated (or created) and CI green (or failing logs captured).  
- [ ] **Closing Report** posted in the session and copied into the PR if appropriate.

---

## 13) Templates & snippets (copy/paste)

**Create/refresh branch**
```bash
git fetch origin
git switch main && git pull
git switch -c feature/<slug>   # or: git switch feature/<slug>
```

**Conventional commit (examples)**
```text
feat(auth): add token refresh endpoint
fix(adapter): correct v2->v1 mapping for late events
test(adapter): add golden case for backpressure
docs(handoff): record throughput benchmark evidence
```

**Common package chores**
```bash
# Node
nvm use && npm ci && npm run build && npm test -- --coverage && npm run lint

# Python
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
pytest -q --maxfail=1 --disable-warnings --cov
ruff check .

# Go
go mod download
go build ./...
go test ./... -count=1
```

---

## 14) What NOT to do
- Don’t alter the process or checklists here—**this file is not the SSOT**.
- Don’t bypass tests/CI to “get it in”—use the gates in `methodology.md`.
- Don’t improvise handoff sections—use the exact SSOT schema.

---

## 15) Versioning
- Update **this file** only to refine operator steps/snippets.
- Any change to process, gates, or templates **must** be done in `methodology.md` and then referenced here.

---

**End of human_instruction.md**
