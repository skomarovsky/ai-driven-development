# design.md

**Version:** 1.0  
**Last updated:** YYYY-MM-DD  
**Status:** Living document — updated as architecture evolves  
**Authority:** Technical decisions source of truth; must align with `scope.md`

---

## Purpose

[2-3 sentences: What is this document? Who uses it? When should it be referenced?]

**Example:**
> This document defines the technical architecture, design patterns, and implementation guidelines for {{project_name}}. It answers "how are we building this?" and serves as the reference for all code decisions during implementation and PR reviews.

---

## 1. Architecture Overview

### 1.1 System Context

**Instructions:** Describe the system's boundaries and stakeholders in 3-5 bullets.

**What to include:**
- System's primary purpose (1 sentence)
- Who/what uses this system (web app, mobile, APIs, users)
- Key external dependencies (databases, third-party APIs, cloud services)
- What this system does NOT do (boundaries)

**Example starter:**
```
- REST API service for {{primary_function}}
- Integrates with: [Auth provider, Database, Cache, External APIs]
- Serves: [Web app, Mobile app, Third-party integrations]
- Does not handle: [List what's out of scope]
```

---

### 1.2 High-Level Architecture

**Instructions:** Show or describe the system's major components and how they interact.

**What to include:**
- Architecture style (monolith, microservices, serverless, event-driven)
- Component diagram (ASCII art or link to image)
- Data flow between components
- Communication patterns (REST, GraphQL, message queues, events)

**Template:**
```
Architecture style: [Monolith | Microservices | Serverless | Layered | etc.]

Diagram:
[Insert ASCII diagram or link: docs/diagrams/architecture.png]

Example structure:
┌─────────┐
│ Clients │
└────┬────┘
     │
┌────▼────┐
│   API   │
└────┬────┘
     │
┌────▼────────┬─────────┐
│  Database   │  Cache  │
└─────────────┴─────────┘

Component responsibilities:
- [Component 1]: [What it does]
- [Component 2]: [What it does]
```

---

### 1.3 Technology Stack

**Instructions:** List all major technologies with versions and brief rationale.

**What to include:**
- Runtime/language and version
- Framework(s)
- Database(s)
- Caching layer
- Testing tools
- Build/deployment tools
- Any critical libraries

**Template:**
```markdown
| Layer | Technology | Version | Rationale |
|-------|-----------|---------|-----------|
| Runtime | [e.g., Node.js] | [e.g., 20 LTS] | [Why chosen] |
| Framework | [e.g., Express] | [e.g., 4.x] | [Why chosen] |
| Database | [e.g., PostgreSQL] | [e.g., 15] | [Why chosen] |
| Cache | [e.g., Redis] | [e.g., 7.x] | [Why chosen] |
| Testing | [e.g., Jest] | [e.g., 29.x] | [Why chosen] |

Constraints:
- ✅ [What you must use/follow]
- ❌ [What you must avoid]
```

---

## 2. Design Principles

### 2.1 Core Principles

**Instructions:** Define 3-7 fundamental principles that guide all implementation decisions.

**What to include:**
- Principle name + 1-2 sentence description
- How it applies to this project specifically
- Examples of applying the principle

**Common principles to consider:**
- Fail fast / Fail safe
- Single Responsibility
- Dependency Injection
- Configuration over code
- Explicit over implicit
- Least privilege
- Defense in depth

**Template:**
```markdown
**1. [Principle Name]**
- [What it means]: [1-2 sentence description]
- [How we apply it]: [Specific to your project]
- [Example]: [Concrete code example or scenario]

**2. [Principle Name]**
- [What it means]: ...
```

---

### 2.2 Error Handling Strategy

**Instructions:** Define how your system handles different types of errors.

**What to include:**
- Classification of errors (operational vs. programmer errors, or your taxonomy)
- Strategy for each type (catch/log/retry, fail fast, etc.)
- Error response format (JSON structure)
- What never to expose (stack traces, internal details)
- Logging strategy for errors

**Template:**
```markdown
**Error types:**
- **[Type 1 - e.g., Operational]**: [Examples] → Strategy: [How to handle]
- **[Type 2 - e.g., Programmer]**: [Examples] → Strategy: [How to handle]

**Error response format:**
```json
{
  [Your standard error response structure]
}
```

**Never expose:**
- [Thing 1, e.g., stack traces to clients]
- [Thing 2, e.g., database error messages]
```

---

### 2.3 Logging Strategy

**Instructions:** Define what, when, and how to log.

**What to include:**
- Log format (structured JSON vs. plain text)
- Log levels and when to use each (ERROR, WARN, INFO, DEBUG)
- What to always log (user actions, errors, performance)
- What to never log (passwords, tokens, PII per methodology.md §8)
- Correlation/request ID strategy

**Template:**
```markdown
**Format:** [Structured JSON | Plain text | Other]

**Log levels:**
- **ERROR**: [When to use] - [Example]
- **WARN**: [When to use] - [Example]
- **INFO**: [When to use] - [Example]
- **DEBUG**: [When to use] - [Example]

**Always log:**
- ✅ [Category 1, e.g., user actions]
- ✅ [Category 2, e.g., external API calls]

**Never log:**
- ❌ [Thing 1, e.g., passwords, per methodology.md §8]
- ❌ [Thing 2, e.g., PII without hashing]

**Correlation:** [How you track requests end-to-end]
```

---

## 3. Module Design

### 3.1 Directory Structure

**Instructions:** Define the canonical project structure and naming conventions.

**What to include:**
- Directory tree (full paths)
- Purpose of each major directory
- File naming conventions
- Where tests live (co-located vs. separate)

**Template:**
```
project-root/
├── [dir1]/           # [Purpose]
│   ├── [subdir]/     # [Purpose]
├── [dir2]/           # [Purpose]
├── [dir3]/           # [Purpose]
└── [config-files]    # [Purpose]

**Conventions:**
- [Convention 1, e.g., test files next to source]
- [Convention 2, e.g., index.js exports public API]
- [Convention 3, e.g., PascalCase for classes]
```

---

### 3.2 Layer Responsibilities

**Instructions:** Define each architectural layer's responsibilities and boundaries.

**What to include:**
- For each layer: purpose, responsibilities, what it CAN do, what it CANNOT do
- Code example showing the pattern
- How layers communicate (direct calls, events, etc.)

**Common layers to define:**
- API/Controller layer (HTTP handling)
- Service/Business logic layer
- Repository/Data access layer
- Utilities/Helpers

**Template for each layer:**
```markdown
**[Layer Name] (e.g., Service Layer):**

Purpose: [1 sentence]

Responsibilities:
- ✅ [Thing it should do]
- ✅ [Thing it should do]
- ❌ [Thing it should NOT do]
- ❌ [Thing it should NOT do]

Code pattern:
```[language]
[Minimal example showing structure]
```

**[Repeat for each layer]**
```

---

### 3.3 Testing Strategy

**Instructions:** Define what kinds of tests you write and when.

**What to include:**
- Test types (unit, integration, e2e)
- When to write each type
- Coverage expectations (reference methodology.md §7)
- Where tests live
- How to run tests (commands)

**Template:**
```markdown
**Unit Tests:**
- Purpose: [What they test]
- Scope: [One function/class]
- Mocking: [What to mock]
- Coverage target: [X% per methodology.md §7]
- Run: `[command]`

**Integration Tests:**
- Purpose: [What they test]
- Scope: [Multiple layers/components]
- Environment: [Test DB, in-memory, etc.]
- When to run: [Every commit, before merge, etc.]
- Run: `[command]`

**E2E Tests (if applicable):**
- Purpose: [What they test]
- Scope: [Full user flows]
- Environment: [Staging, Docker Compose]
- When to run: [Nightly, before release]
- Run: `[command]`
```

---

## 4. Security Guidelines

### 4.1 Authentication & Authorization

**Instructions:** Define how your system proves identity and enforces permissions.

**What to include:**
- Authentication mechanism (JWT, sessions, OAuth, API keys)
- Token/session lifetime and storage
- Authorization model (RBAC, ABAC, claims-based)
- Where auth checks happen (middleware, service layer)
- Implementation references (file paths)

**Template:**
```markdown
**Authentication:**
- Mechanism: [JWT | Sessions | OAuth | API Keys]
- Token lifetime: [Access: X min, Refresh: Y days]
- Storage: [Where tokens are stored]
- Invalidation: [How to logout/revoke]

**Authorization:**
- Model: [RBAC | ABAC | Claims-based]
- Roles: [List roles: admin, user, guest]
- Permission checks: [Where in code: middleware, service layer]

**Implementation:** See `[file path]` for details.
```

---

### 4.2 Input Validation

**Instructions:** Define how you validate and sanitize all inputs.

**What to include:**
- Validation library/approach (Joi, Zod, manual)
- Where validation happens (API boundary, service layer, both)
- What to validate (types, formats, ranges, business rules)
- Sanitization steps (trim, normalize, escape)

**Template:**
```markdown
**Validation library:** [Joi | Zod | Yup | Custom]

**Where:** [API boundary | Service layer | Both]

**Validate:**
- ✅ [Type 1, e.g., all request body fields]
- ✅ [Type 2, e.g., file uploads - size, type]
- ✅ [Type 3, e.g., query parameters]

**Sanitize:**
- [Step 1, e.g., trim all string inputs]
- [Step 2, e.g., normalize emails to lowercase]

**Example:**
```[language]
[Validation schema or code example]
```
```

---

### 4.3 Data Protection

**Instructions:** Define how you protect sensitive data.

**What to include:**
- Secrets management (where stored, how accessed)
- Encryption (at rest, in transit)
- Hashing (passwords, sensitive fields)
- Database security (parameterized queries, least privilege)
- Reference to methodology.md §8

**Template:**
```markdown
**Secrets:**
- Storage: [.env local | AWS Secrets Manager prod | etc.]
- Access: [Environment variables]
- Rotation: [Frequency and process]
- Per methodology.md §8: Never commit secrets

**Sensitive data:**
- Passwords: [bcrypt cost 10-12 | Argon2 | etc.]
- PII: [Encrypt at rest | Hash before storage]
- Logs: [Redact before logging]

**Database:**
- Use parameterized queries (prevents SQL injection)
- Principle of least privilege (app user permissions)
- SSL/TLS connections: [Yes | No, why]
```

---

### 4.4 Security Headers

**Instructions:** Define required HTTP security headers.

**What to include:**
- List of required headers
- Configuration (code snippet or reference to middleware)
- Rationale for each header

**Template:**
```markdown
**Required headers:**
- `[Header-Name]`: [Purpose]
- `[Header-Name]`: [Purpose]

**Implementation:**
```[language]
[Code snippet showing how headers are set]
```

**Reference:** [OWASP recommendations, security scanning results]
```

---

## 5. Performance Guidelines

### 5.1 Database Optimization

**Instructions:** Define patterns for efficient database usage.

**What to include:**
- Query patterns (indexes, limits, joins)
- Connection management (pooling)
- Common anti-patterns to avoid (N+1, SELECT *)
- Monitoring strategy

**Template:**
```markdown
**Query patterns:**
- ✅ [Pattern 1, e.g., always use indexes on WHERE clauses]
- ✅ [Pattern 2, e.g., use LIMIT for pagination]
- ❌ [Anti-pattern 1, e.g., avoid N+1 queries]
- ❌ [Anti-pattern 2, e.g., no SELECT * in production]

**Connection management:**
- [Strategy: pooling, max connections, timeout]

**Example:**
[Code showing good pattern vs bad pattern]
```

---

### 5.2 Caching Strategy

**Instructions:** Define what, where, and how to cache.

**What to include:**
- What to cache (and what NOT to cache)
- Cache layers (in-memory, Redis, CDN)
- TTL strategy
- Cache invalidation approach

**Template:**
```markdown
**What to cache:**
- ✅ [Type 1, e.g., user profiles - read-heavy, change-light]
- ✅ [Type 2, e.g., config data]
- ❌ [Type 3, e.g., real-time data]
- ❌ [Type 4, e.g., sensitive user data unless encrypted]

**Cache layers:**
1. [Layer 1, e.g., In-memory]: [When to use]
2. [Layer 2, e.g., Redis]: [When to use]
3. [Layer 3, e.g., CDN]: [When to use]

**TTL strategy:**
- [Data type 1]: [TTL duration and why]
- [Data type 2]: [TTL duration and why]

**Invalidation:**
- [When and how cache is cleared]
```

---

### 5.3 Rate Limiting

**Instructions:** Define rate limits to protect against abuse.

**What to include:**
- Limits per endpoint type (general API, login, expensive operations)
- Algorithm (token bucket, leaky bucket, fixed window)
- Response format when limit exceeded
- Implementation reference

**Template:**
```markdown
**Limits:**
- General API: [X requests per Y time per Z identifier]
- Login/auth: [X requests per Y time to prevent brute force]
- Expensive ops: [X requests per Y time for report generation, etc.]

**Algorithm:** [Token bucket | Leaky bucket | Fixed window]

**Response when exceeded:**
```http
HTTP/1.1 429 Too Many Requests
[Headers to include]
```

**Implementation:** See `[file path]`
```

---

## 6. Observability

### 6.1 Monitoring Metrics

**Instructions:** Define what metrics you track and how.

**What to include:**
- Key metrics (request rate, latency, error rate, resources)
- Tools used (Prometheus, DataDog, CloudWatch)
- Dashboards (what's displayed)

**Template:**
```markdown
**Key metrics:**
- [Metric 1, e.g., Request rate (req/sec by endpoint)]
- [Metric 2, e.g., Latency (P50, P95, P99)]
- [Metric 3, e.g., Error rate (4xx, 5xx by endpoint)]
- [Metric 4, e.g., Resource usage (CPU, memory)]

**Tools:** [Prometheus | DataDog | New Relic | CloudWatch]

**Dashboards:** [Link to dashboards or describe what's shown]
```

---

### 6.2 Alerting

**Instructions:** Define when and how to alert on-call.

**What to include:**
- Conditions that trigger alerts
- Alert channels (PagerDuty, Slack, email)
- Escalation policy
- What NOT to alert on (to reduce noise)

**Template:**
```markdown
**Alert on:**
- 🚨 [Condition 1, e.g., Error rate >5% for 5 min]
- 🚨 [Condition 2, e.g., P95 latency >500ms for 5 min]
- 🚨 [Condition 3, e.g., DB connection pool exhausted]

**Channels:** [PagerDuty | Slack | Email | Phone]

**Escalation:**
- [Step 1: Page on-call immediately]
- [Step 2: Escalate to lead after X minutes]
- [Step 3: Escalate to director after Y minutes]

**Don't alert on:**
- [Noise 1, e.g., single failed request]
- [Noise 2, e.g., planned maintenance]
```

---

### 6.3 Health Checks

**Instructions:** Define health check endpoints for orchestrators.

**What to include:**
- Liveness endpoint (is process running?)
- Readiness endpoint (can handle traffic?)
- What each endpoint checks
- Response format

**Template:**
```markdown
**Liveness probe:**
- Endpoint: `[GET /health/live]`
- Checks: [Process is alive]
- Response: [200 OK with simple status]

**Readiness probe:**
- Endpoint: `[GET /health/ready]`
- Checks: [List what's checked: DB, cache, external APIs]
- Response: [200 OK if ready, 503 if not ready]

**Example response:**
```json
[Sample JSON response]
```

**Used by:** [Kubernetes | ECS | Load balancer]
```

---

## 7. Deployment & Operations

### 7.1 Environment Strategy

**Instructions:** Define your environments and their purposes.

**What to include:**
- List of environments (local, test, staging, prod)
- Purpose of each
- When deployments happen
- Data characteristics (fake, anonymized, real)

**Template:**
```markdown
| Environment | Purpose | Deploy Trigger | Data |
|-------------|---------|----------------|------|
| **[Env 1]** | [Purpose] | [When] | [Data type] |
| **[Env 2]** | [Purpose] | [When] | [Data type] |
| **[Env 3]** | [Purpose] | [When] | [Data type] |

**Config differences:**
- [Env 1]: [Config approach, log level, scale]
- [Env 2]: [Config approach, log level, scale]
```

---

### 7.2 Deployment Process

**Instructions:** Document your CI/CD pipeline and deployment steps.

**What to include:**
- CI/CD steps in order (lint, test, build, deploy)
- Quality gates (what must pass)
- Deployment mechanism (kubectl, terraform, etc.)
- Rollback procedure
- Reference to methodology.md §9 for CI expectations

**Template:**
```markdown
**Pipeline steps (per methodology.md §9):**
1. [Step 1, e.g., Lint]
2. [Step 2, e.g., Unit tests with coverage]
3. [Step 3, e.g., Build]
4. [Step 4, e.g., Integration tests]
5. [Step 5, e.g., Security scans]
6. [Step 6, e.g., Build artifact (Docker image)]
7. [Step 7, e.g., Deploy to environment]
8. [Step 8, e.g., Smoke tests]
9. [Step 9, e.g., Monitor for X minutes]

**Rollback:** [How to rollback, SLA for rollback time]

**Artifacts:** [Where images/builds are stored]
```

---

### 7.3 Database Migrations

**Instructions:** Define your database change management strategy.

**What to include:**
- Migration tool (Flyway, Liquibase, framework migrations)
- How migrations are versioned
- Where migration files live
- Testing strategy (staging first)
- Rollback approach

**Template:**
```markdown
**Tool:** [Flyway | Liquibase | Knex | Sequelize | Alembic]

**Versioning:** [How files are named/numbered]

**Location:** [Directory path]

**Process:**
1. [Step 1, e.g., Write migration file]
2. [Step 2, e.g., Test on local DB]
3. [Step 3, e.g., Apply to staging]
4. [Step 4, e.g., Verify staging]
5. [Step 5, e.g., Apply to production]

**Rollback:** [How to reverse migrations]

**Rules:**
- ✅ [Rule 1, e.g., Always backward-compatible]
- ❌ [Rule 2, e.g., Never rename columns, add new and deprecate old]
```

---

## 8. Decision Log (ADRs)

**Instructions:** Document all significant architectural decisions here. Each decision gets a numbered ADR.

**What to include:**
- ADR number, title, date, status
- Context (what problem, what forces)
- Decision made
- Consequences (pros and cons)
- Alternatives considered

**Template for each ADR:**
```markdown
### 8.X ADR-XXX: [Decision Title]

**Date:** YYYY-MM-DD  
**Status:** [Proposed | Accepted | Deprecated | Superseded by ADR-YYY]

**Context:**
[What problem are we solving? What requirements or constraints matter?]

**Decision:**
[What did we decide to do?]

**Consequences:**
- ✅ [Positive consequence 1]
- ✅ [Positive consequence 2]
- ❌ [Negative consequence / tradeoff 1]
- ❌ [Negative consequence / tradeoff 2]

**Alternatives Considered:**
- **[Option A]**: [Why we didn't choose this]
- **[Option B]**: [Why we didn't choose this]
```

**Start with your first few major decisions:**
- ADR-001: Choice of primary language/runtime
- ADR-002: Choice of database
- ADR-003: Authentication approach
- ADR-004: [Your next major decision]

---

## 9. Coding Standards

### 9.1 Language-Specific Conventions

**Instructions:** Define your code style and conventions.

**What to include:**
- Naming conventions (PascalCase, camelCase, UPPER_SNAKE_CASE)
- Language features to use/avoid
- Formatting rules (or reference to .eslintrc, prettier config)
- File organization patterns

**Template:**
```markdown
**Naming:**
- Classes: [Convention]
- Functions/methods: [Convention]
- Variables: [Convention]
- Constants: [Convention]
- Private fields: [Convention]

**Language features:**
- ✅ Use: [Feature 1, e.g., const/let, not var]
- ✅ Use: [Feature 2, e.g., async/await]
- ❌ Avoid: [Anti-pattern 1, e.g., any type]
- ❌ Avoid: [Anti-pattern 2, e.g., non-null assertions]

**Formatting:**
- [Reference to config file: .eslintrc, .prettierrc]
- Or [Manual rules: indent X spaces, max line length Y]
```

---

### 9.2 Comments & Documentation

**Instructions:** Define when and how to write comments and docs.

**What to include:**
- When to comment (why not what)
- JSDoc/docstring format for public APIs
- README requirements
- When to skip comments (self-explanatory code)

**Template:**
```markdown
**Comment when:**
- ✅ [Reason 1, e.g., Non-obvious "why" decisions]
- ✅ [Reason 2, e.g., Gotchas and warnings]
- ✅ [Reason 3, e.g., Complex algorithms]
- ❌ [Not for: obvious "what" code does]

**Documentation format:**
- Public APIs: [JSDoc | Docstrings | TypeDoc]
- Inline: [Brief comments explaining rationale]

**Example:**
```[language]
[Sample documented function]
```
```

---

### 9.3 Git Commit Messages

**Instructions:** Define commit message format and conventions.

**What to include:**
- Format standard (Conventional Commits recommended)
- Types (feat, fix, docs, etc.)
- Scope usage
- Examples of good commits

**Template:**
```markdown
**Format:** [Conventional Commits | Custom format]

```
<type>(<scope>): <subject>

<body>

<footer>
```

**Types:**
- `feat`: [When to use]
- `fix`: [When to use]
- `docs`: [When to use]
- `refactor`: [When to use]
- [Add other types you use]

**Examples:**
```
[Example 1]
[Example 2]
```
```

---

## 10. Extensibility & Future Work

### 10.1 Extension Points

**Instructions:** Document where the system is designed to be extended.

**What to include:**
- Planned extension mechanisms (plugins, hooks, configs)
- How to add new features without modifying core
- Examples of future extensions

**Template:**
```markdown
**Designed for extension:**
- [Area 1, e.g., Validation rules - plugin system]
- [Area 2, e.g., Authentication providers - strategy pattern]
- [Area 3, e.g., Output formats - adapter pattern]

**How to extend:**
- [Instructions or reference to extension guide]

**Planned extensions:**
- [Future feature 1]
- [Future feature 2]
```

---

### 10.2 Tech Debt Tracking

**Instructions:** Define how you track and manage technical debt.

**What to include:**
- How to document debt (TODO comments with ticket IDs)
- Where debt is tracked (tracker.md, JIRA, GitHub issues)
- Review cadence
- Reference to handoff.md for blocking debt

**Template:**
```markdown
**Document debt:**
- In code: `// TODO(T-XXX): [Description]`
- In tracker.md: Task with type "tech-debt"
- In handoff.md: If blocking future work (per methodology.md §4)

**Review cadence:** [Weekly | Monthly | Quarterly]

**Priority criteria:**
- [When debt gets prioritized: blocks features, security risk, etc.]
```

---

## 11. Changelog

**Instructions:** Track major changes to this design document.

**Template:**
```markdown
| Date | Version | Changes | Author |
|------|---------|---------|--------|
| YYYY-MM-DD | 1.0 | Initial design document | [Name] |
| | | | |
```

---

## Appendix A: Useful References

**Instructions:** Link to external resources referenced in this document.

**Template:**
```markdown
**Official Documentation:**
- [Technology 1]: [URL]
- [Technology 2]: [URL]

**Security:**
- OWASP Top 10: [URL]
- [Relevant security guide]: [URL]

**Design Patterns:**
- [Resource 1]: [URL]
- [Resource 2]: [URL]

**Internal:**
- [Related doc 1]: [path or URL]
- [Related doc 2]: [path or URL]
```

---

## Instructions for Using This Template

1. **Copy this file** to your project's `docs/` directory as `design.md`
2. **Replace all `[bracketed placeholders]`** with your actual content
3. **Delete instruction paragraphs** (the "Instructions:" sections) after filling in
4. **Remove sections** that don't apply to your project
5. **Add sections** if you need additional topics not covered here
6. **Reference the example** (`design_example.md`) for detailed content ideas
7. **Update regularly** as your design evolves (it's a living document)
8. **Link from tracker.md** tasks to specific sections (e.g., "See design.md §3.4")

**This template provides the structure; you provide the specifics for your project.**

---

**End of design_template.md**
