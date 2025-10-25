# ai_patterns.md

**Version:** 1.0  
**Last updated:** 2025-10-21  
**Purpose:** Practical guidance for effective human-AI collaboration in software development sessions.

---

## Overview

This document teaches you **how to work effectively with AI** as a collaborative coding partner. It covers when to trust AI, how to verify outputs, effective prompting patterns, and how to recover from mistakes.

**Target audience:** Human operators using AI assistance for development  
**Scope:** Session-level interactions, not architectural decisions  
**Companion to:** `methodology.md` (process gates), `human_instructions.md` (technical commands)

---

## Table of Contents

1. [Trust & Verification Framework](#1-trust--verification-framework)
2. [AI Capabilities & Limitations](#2-ai-capabilities--limitations)
3. [Session Management Tactics](#3-session-management-tactics)
4. [Prompt Patterns Library](#4-prompt-patterns-library)
5. [Evidence Validation](#5-evidence-validation)
6. [Handling AI Mistakes](#6-handling-ai-mistakes)
7. [Context Window Management](#7-context-window-management)
8. [Quality Signals](#8-quality-signals)

---

## 1. Trust & Verification Framework

### When to Trust AI (High Confidence)

✅ **Boilerplate & Scaffolding**
- CRUD operations, REST endpoints, basic database queries
- Standard test structure (describe/it blocks, setup/teardown)
- Common configuration files (package.json, tsconfig.json, .gitignore)
- Documentation structure and templates

✅ **Pattern Application**
- Applying well-known design patterns (Factory, Strategy, Observer)
- Standard error handling (try/catch, error boundaries)
- Logging and instrumentation (Winston, Pino, console patterns)
- Conventional commits and changelog formatting

✅ **Code Transformations**
- Refactoring for readability (extract function, rename variable)
- Converting between formats (JSON ↔ YAML, callbacks → async/await)
- Adding types to untyped code
- Formatting and linting fixes

**How to use:** Accept AI's output directly, run validation commands, commit if green.

---

### When to Verify Carefully (Medium Confidence)

⚠️ **Business Logic**
- Domain-specific calculations (pricing, taxes, interest rates)
- State machine transitions (order lifecycle, workflow steps)
- Authorization rules (who can access what)
- Data validation rules (what's a "valid" email, phone, address)

⚠️ **External Integrations**
- API contracts (endpoints, request/response shapes)
- Third-party SDK usage (Stripe, Twilio, AWS APIs)
- Database schema migrations (ensure backward compatibility)
- Webhook signatures and verification

⚠️ **Performance-Critical Code**
- Algorithms with O(n²) or worse complexity
- Database queries (check for N+1, missing indexes)
- Caching strategies (eviction, invalidation)
- Resource pooling (connections, threads)

**How to use:** 
1. Read AI's code carefully before applying
2. Ask AI: "What could go wrong with this approach?"
3. Write tests that prove correctness
4. Benchmark if performance matters
5. Cross-reference with official docs

---

### When to Never Trust (Verify Everything)

❌ **Security-Critical Code**
- Authentication mechanisms (password hashing, JWT validation)
- Cryptography (never roll your own, verify library usage)
- Input sanitization (SQL injection, XSS prevention)
- Secret handling (encryption keys, API tokens)
- CORS, CSP, and security headers

❌ **Version-Specific Information**
- Package versions and compatibility
- Breaking changes in APIs (React 17 → 18, Node 14 → 20)
- Deprecated methods (AI may not know about recent deprecations)
- Current best practices (AI's training data has a cutoff)

❌ **Your Codebase Specifics**
- Internal conventions (your team's patterns)
- Existing abstractions (your custom utilities)
- Database schema details (your table structure)
- Environment-specific configs (your infra setup)

**How to use:**
1. Get AI's suggestion as a starting point
2. Consult official docs, security guides, or teammates
3. Use security scanning tools (npm audit, Snyk, Dependabot)
4. Write security-focused tests (penetration test mindset)
5. Have security-critical code reviewed by a human expert

---

## 2. AI Capabilities & Limitations

### What AI Does Well

🎯 **Pattern Recognition**
- Identifying code smells and anti-patterns
- Spotting inconsistencies across files
- Recognizing opportunities for abstraction
- Finding similar code that could be DRY'd

🎯 **Code Generation**
- Writing tests from function signatures
- Generating mocks and fixtures
- Creating boilerplate from templates
- Scaffolding new modules/components

🎯 **Explanation & Documentation**
- Explaining complex code in plain English
- Generating JSDoc/docstring comments
- Writing README sections
- Creating inline comments for non-obvious logic

🎯 **Debugging Assistance**
- Analyzing error messages and stack traces
- Suggesting potential causes for failures
- Proposing debugging steps
- Identifying missing edge cases

---

### What AI Struggles With

⚠️ **Temporal Information**
- Current package versions (training cutoff: January 2025)
- Recent API changes or deprecations
- Latest framework best practices
- Current CVE/security vulnerabilities

**Mitigation:** Always verify package versions with `npm view <pkg> version` or official docs.

---

⚠️ **Your Specific Codebase**
- Undocumented conventions
- Internal abstractions and utilities
- Implicit dependencies between modules
- Historical context ("why was it built this way?")

**Mitigation:** Provide AI with relevant code context (paste existing utilities, point to similar implementations).

---

⚠️ **Non-Deterministic Scenarios**
- Race conditions and concurrency bugs
- Flaky tests (works 90% of the time)
- Environment-specific issues (works on Mac, fails on Linux)
- Timing-dependent behavior

**Mitigation:** Use explicit prompts: "What race conditions could occur here?" Ask AI to add mutex/lock patterns.

---

⚠️ **Complex Stateful Systems**
- Multi-step workflows with partial failures
- Distributed system edge cases
- Transaction isolation levels
- Event sourcing and CQRS patterns

**Mitigation:** Break into smaller pieces. Use AI for individual components, but architect the system yourself.

---

## 3. Session Management Tactics

### Starting a Session

**Always begin with the Universal Prompt** from `methodology_prompt.md`:
```
[Paste entire methodology_prompt.md v1.2 here]
{{project_name}} = your-project-name

Current handoff:
[Paste handoff.md Context Snapshot]
```

**Why this matters:**
- Establishes AI's role and capabilities
- Loads process rules (SSOT) by reference
- Primes AI to ask questions vs. guessing
- Sets expectation for Opening Brief

---

### Mid-Session Check-ins

**Every ~10 exchanges, reality-check with AI:**
```
"Pause. What are we working on right now? What's the acceptance criteria?"
```

**Expected response:**
```
We're working on T-015: Add rate limiting to API
Acceptance: 100 req/min limit enforced, 429 status, tests ≥80% coverage
Currently: writing integration test to prove limit works
```

**If AI's answer is vague or wrong:** Context drift detected. Re-anchor:
```
"Let's refocus. Read handoff.md again. We're on T-015, not T-008."
```

---

### Before Major Changes

**Always ask AI to review its plan against scope/design:**
```
"Before implementing, review your approach against scope.md goals and design.md §3. Does this align?"
```

**Why:** Catches scope creep early, ensures architectural consistency.

---

### Ending a Session

**Always request Closing Report:**
```
"Session complete. Generate the Closing Report and updated handoff.md."
```

**Review checklist:**
- [ ] Closing Report has all 6 sections (What/Validation/Status/Decisions/Risks/Next)
- [ ] handoff.md uses canonical schema (8 sections, exact headings)
- [ ] Status glyph and % are updated
- [ ] Evidence links are specific (not "see PR" but "PR #42, CI run #156")
- [ ] Next Steps are ≤1 day each

---

## 4. Prompt Patterns Library

### Discovery Patterns

**Pattern: Multi-Option Analysis**
```
"Show me 3 different ways to implement [feature]. Include pros/cons for each."
```

**Use when:** Starting a new feature, unsure of best approach  
**Example output:** A vs. B vs. C comparison with honest tradeoffs

---

**Pattern: Edge Case Brainstorm**
```
"What edge cases should I test for [this function]? What could go wrong?"
```

**Use when:** Writing tests, reviewing security-critical code  
**Example output:** List of 5-10 edge cases with test ideas

---

**Pattern: Architecture Review**
```
"Review my approach against SOLID principles. What could be improved?"
```

**Use when:** Refactoring, code review, design validation  
**Example output:** Specific SOLID violations with refactor suggestions

---

### Implementation Patterns

**Pattern: Test-Driven Implementation**
```
"Write failing tests for [feature] first. Then implement to make them pass."
```

**Use when:** TDD workflow, critical features  
**Example output:** Tests first, then minimal implementation

---

**Pattern: Incremental Build**
```
"Implement [feature] in 3 small steps. After each step, give me validation commands."
```

**Use when:** Complex features, want to validate incrementally  
**Example output:** Step 1 + validation → Step 2 + validation → Step 3 + validation

---

**Pattern: Explain Before Implement**
```
"Explain your implementation plan in plain English before writing code."
```

**Use when:** Complex logic, want to review approach first  
**Example output:** Paragraph explaining algorithm/approach, then code

---

### Validation Patterns

**Pattern: Validation First**
```
"What commands should I run to verify this works? What's the expected output?"
```

**Use when:** After every code change  
**Example output:** Exact bash command + expected test count/coverage

---

**Pattern: Red-Green-Refactor**
```
"First show me a test that fails with current code. Then fix the code to pass it."
```

**Use when:** Bug fixes, TDD  
**Example output:** Failing test run → code change → passing test run

---

**Pattern: Performance Proof**
```
"How do I benchmark this? What's acceptable performance for [scale]?"
```

**Use when:** Performance-critical code  
**Example output:** Benchmark command + threshold (e.g., "<100ms for 10K items")

---

### Debugging Patterns

**Pattern: Diagnostic Tree**
```
"I'm seeing [error]. Walk me through 5 possible causes, most likely first."
```

**Use when:** Stuck on a bug  
**Example output:** Ranked list of hypotheses with verification steps

---

**Pattern: Compare & Contrast**
```
"Here's working code [paste A]. Here's broken code [paste B]. What's different?"
```

**Use when:** Regression, "it worked yesterday"  
**Example output:** Diff analysis with likely culprit highlighted

---

**Pattern: Rubber Duck**
```
"I'm going to explain the problem. Stop me when you spot the issue: [explain]"
```

**Use when:** Complex bug, need fresh eyes  
**Example output:** AI interrupts with "Wait, did you check X?"

---

### Code Quality Patterns

**Pattern: Security Audit**
```
"Review this code for security vulnerabilities. Check for: [XSS, injection, auth bypass, secrets]."
```

**Use when:** Before committing security-related code  
**Example output:** List of findings with severity + fix suggestions

---

**Pattern: Code Smell Detection**
```
"What code smells do you see in [this module]? How would you refactor?"
```

**Use when:** Code review, refactoring sessions  
**Example output:** Smells identified (long method, god class, etc.) + refactor plan

---

**Pattern: Testability Check**
```
"Is this code testable? If not, how should I refactor for testability?"
```

**Use when:** Writing tests is hard, code feels coupled  
**Example output:** Dependency injection suggestions, mock points

---

## 5. Evidence Validation

### Never Accept Vague Answers

❌ **Avoid:**
- "Tests should pass"
- "Coverage looks good"
- "This should work"
- "Run the tests"

✅ **Require:**
- "Run `npm test -- --coverage`. Expected: 24/24 passing, 87% coverage"
- "Expect HTTP 429 after 101st request in <5 seconds"
- "Output should show 'Rate limit: 100/min' in logs"

---

### Test Evidence Checklist

When AI says "tests pass," demand:

✅ **Test Count**
- "How many tests? How many suites?"
- Expected: "12 tests across 3 suites"

✅ **Coverage Metrics**
- "Coverage on changed lines?"
- Expected: "87% statements, 82% branches (meets 80% threshold)"

✅ **Test Names**
- "Which tests cover the happy path vs. error cases?"
- Expected: List of specific test names mapped to scenarios

✅ **Failure Modes**
- "What if I break X, which test catches it?"
- Expected: "Test 'should reject invalid token' will fail with [specific assertion]"

---

### Benchmark Evidence Checklist

When AI provides performance claims, demand:

✅ **Dataset Size**
- "Benchmark with 10K, 100K, 1M items"

✅ **Timing Measurements**
- "P50, P95, P99 latencies"
- Expected: "P50: 45ms, P95: 120ms, P99: 380ms"

✅ **Resource Usage**
- "Memory footprint, CPU usage"
- Expected: "Peak heap: 120MB, CPU: 15% single core"

✅ **Comparison Baseline**
- "Faster than what? Previous version? Alternative approach?"
- Expected: "30% faster than previous implementation (baseline: 65ms → new: 45ms)"

---

### Security Scan Evidence Checklist

When AI says "security scan clean," demand:

✅ **Tool Used**
- "Which scanner? npm audit? Snyk? pre-commit?"

✅ **Scan Output**
- "Copy the summary line"
- Expected: "0 critical, 0 high, 2 moderate (false positives documented)"

✅ **Secret Scan**
- "Any high-entropy strings flagged?"
- Expected: "0 findings, checked 47 files"

---

## 6. Handling AI Mistakes

### Types of AI Mistakes

**1. Hallucinated APIs**
```
AI: "Use npm package 'super-fast-json' for parsing"
You: npm install super-fast-json
Result: ERROR 404 Not Found

Fix: "That package doesn't exist. What's the real package name?"
Prevention: Verify package names at npmjs.com before installing
```

**2. Outdated Syntax**
```
AI: "Use React.createClass() for components"
You: [Paste code]
Result: ERROR: createClass is not a function

Fix: "We're on React 18. What's the current syntax?"
Prevention: Ask "What version of [library] does this target?"
```

**3. Logic Errors**
```
AI: "This sorts the array by price descending"
You: [Test with [3,1,2]]
Result: [1,2,3] (ascending, not descending!)

Fix: "This sorts ascending, not descending. Fix the comparator."
Prevention: Write test before applying code
```

**4. Incomplete Error Handling**
```
AI: [Provides code]
You: "What if the API returns 500?"
AI: "Oh, right, we should handle that..."

Fix: Always ask "What could go wrong?"
Prevention: Use "Edge Case Brainstorm" pattern (§4)
```

**5. Test That Never Fails**
```
AI: "This test proves the validator works"
You: [Intentionally break validator]
Result: Test still passes! 

Fix: "This test passes even when validator is broken. Rewrite."
Prevention: Try breaking code to see if test catches it
```

---

### Recovery Protocol

**Step 1: Identify the mistake**
```
"This output doesn't match what you said. Expected X, got Y."
```

**Step 2: Request explanation**
```
"Why did this fail? What did you assume incorrectly?"
```

**Step 3: Get corrected version**
```
"Provide the corrected code with explanation of what changed."
```

**Step 4: Verify the fix**
```
"What command proves this is now correct? Expected output?"
```

**Step 5: Prevent recurrence**
```
"Add a test that would have caught this error."
```

---

### When to Escalate vs. Iterate

**Iterate with AI (try 2-3 times):**
- Syntax errors, typos, simple logic bugs
- Missing imports, wrong package names
- Test failures with clear error messages

**Stop and escalate (don't waste time):**
- Same error after 3+ attempts
- AI is guessing ("try this... or maybe this...")
- Security vulnerabilities (get human expert)
- Architecture decisions (AI shouldn't decide)
- Environment issues (Docker, networking, permissions)

**Escalation targets:**
- Documentation (official docs, Stack Overflow)
- Teammates (Slack, pair programming)
- Search engines (for error messages)
- GitHub issues (for library bugs)

---

## 7. Context Window Management

### Symptoms of Context Loss

🚨 **AI "forgets" earlier decisions:**
- Suggests approach you already rejected
- Contradicts something it said 20 messages ago
- Implements feature you said was out of scope

🚨 **AI becomes repetitive:**
- Gives same answer to different questions
- Restates obvious information
- Loses specificity ("as I mentioned before...")

🚨 **AI misunderstands current task:**
- Works on wrong task (T-015 when you're on T-018)
- Forgets acceptance criteria
- Suggests changes to wrong files

---

### Prevention Tactics

**1. Keep sessions focused (2-3 hours max)**
- One task per session ideal
- End session → update handoff.md → start fresh

**2. Periodically summarize**
```
Every 10-15 exchanges:
"Summarize: what have we accomplished so far this session?"
```

**3. Externalize context to documents**
- Don't rely on AI's memory
- Record decisions in handoff.md
- Update tracker.md incrementally

**4. Start new session when:**
- Switching to different task
- After major milestone
- Context window feels "full" (AI seems confused)

---

### Context Refresh Tactics

**When AI seems confused:**

**Option A: Soft Refresh**
```
"Pause. Reload handoff.md. What's our current task and acceptance criteria?"
```

**Option B: Hard Refresh**
```
"This session is getting long. Let's close with a Closing Report and start fresh."
[Start new session with methodology_prompt.md + updated handoff.md]
```

**Option C: Targeted Context Injection**
```
"Here's the design.md section for this task: [paste]. Review and continue."
```

---

## 8. Quality Signals

### Green Flags (AI is performing well)

✅ **Asks clarifying questions** before implementing
- "Should this fail open or closed if Redis is down?"
- "Do you want to preserve backward compatibility?"

✅ **Provides multiple options** with tradeoffs
- "Here are 3 approaches: A (simple but limited), B (flexible but complex), C (..."

✅ **Self-critiques** before finalizing
- "Wait, this has a race condition. Let me revise..."

✅ **Specific validation steps**
- "Run `npm test -- src/auth.test.js`. Expect: 8/8 passing, 94% coverage."

✅ **References docs properly**
- "Per methodology.md §7, we need 80% coverage on changed lines"

✅ **Admits uncertainty**
- "I'm not sure about the current API for X. Can you check the docs at Y?"

---

### Yellow Flags (Be cautious)

⚠️ **Overly confident** about uncertain things
- "This definitely works" (without testing)
- "X is always better than Y" (no nuance)

⚠️ **Vague validation**
- "Run the tests" (which tests? what's expected?)
- "This should be fine" (based on what?)

⚠️ **No tradeoffs discussed**
- Only provides one option
- Doesn't mention downsides

⚠️ **Ignores constraints**
- Suggests solution that violates scope.md
- Adds dependencies without checking

**Response:** Push back, ask for specifics, demand evidence.

---

### Red Flags (Stop and reassess)

🚨 **Contradicts itself**
- Says A in message 10, says not-A in message 15

🚨 **Hallucinates features**
- "Use the built-in method X" (X doesn't exist)
- References non-existent packages

🚨 **Ignores your feedback**
- You: "Don't use library Y"
- AI: [Provides code using library Y]

🚨 **Can't explain reasoning**
- You: "Why this approach?"
- AI: "It's better" (no specifics)

🚨 **Security anti-patterns**
- Storing secrets in code
- SQL string concatenation
- Rolling custom crypto

**Response:** Stop session, start fresh, or escalate to human expert.

---

## Quick Reference Card

### Session Start
```
1. Paste methodology_prompt.md v1.2
2. Paste handoff.md Context Snapshot
3. Confirm: "What task are we working on?"
4. Wait for Opening Brief before proceeding
```

### During Session
```
- Run every command AI provides
- Paste full outputs (not summaries)
- Ask "What could go wrong?" before committing
- Check in every ~10 exchanges: "What are we doing?"
```

### Validation Every Time
```
- Demand specific numbers (14/14 tests, 87% coverage)
- Request exact commands with expected outputs
- Verify against methodology.md DoD (§6)
- Never accept "should work" without proof
```

### Session End
```
1. Request Closing Report
2. Review handoff.md (canonical schema?)
3. Verify evidence (links to CI, test runs)
4. Commit updated docs
5. Push branch, update PR
```

### When AI Struggles
```
- Same error 3+ times? Escalate (docs, teammates)
- Context loss? Refresh or start new session
- Security-critical? Get human review
- Vague answers? Demand specifics
```

---

## Appendix: Anti-Patterns to Avoid

### Don't Do This

❌ **Blindly accepting code without reading**
```
AI: [100 lines of code]
You: "Thanks!" [Commits without review]
```

**Why bad:** Could contain bugs, security issues, or not match your patterns  
**Instead:** Read code, ask questions, test first

---

❌ **Accepting "trust me" without evidence**
```
AI: "This improves performance by 50%"
You: [Commits]
```

**Why bad:** No benchmark, could be false or context-dependent  
**Instead:** "Show me the benchmark. What dataset? What was baseline?"

---

❌ **Not providing enough context**
```
You: "Fix the bug"
AI: "Which bug? What's the symptom?"
You: "The one in the code"
```

**Why bad:** AI can't help without specifics  
**Instead:** Paste error message, stack trace, failing test

---

❌ **Treating AI like a search engine**
```
You: "What is Redis?"
AI: [Generic explanation]
```

**Why bad:** Wastes session context on info you could Google  
**Instead:** Ask project-specific: "Should we use Redis for this use case given our scale?"

---

❌ **Over-relying on AI for decisions**
```
You: "Should we use microservices or monolith?"
AI: [Suggests approach]
You: [Commits to architecture]
```

**Why bad:** AI doesn't know your team, scale, constraints  
**Instead:** Use AI to analyze tradeoffs, but you decide

---

❌ **Ignoring your instincts**
```
AI: [Suggests complex solution]
You: "This seems over-engineered..."
You: "But AI suggested it, so..." [Implements anyway]
```

**Why bad:** You know your codebase better than AI  
**Instead:** "This feels complex. Show me a simpler approach."

---

**End of ai_patterns.md v1.0**
