---
name: backend-context-files
description: >-
  Generate spec-driven context files for an existing backend project (API, service, or server).
  Use when the user wants to create project documentation for AI-assisted development,
  including project overview, architecture, coding standards, API design context, AI workflow rules,
  and progress tracking. The project should already exist with some implementation.
---

# Backend Context Files Generator

Generate context files that give AI agents the full picture of a backend project. These files are the single source of truth for requirements, architecture, and conventions — keeping AI work consistent across sessions.

## When To Use

- The user has an existing backend project (API server, microservice, CLI tool, data pipeline) and wants structured context files for AI-assisted development.
- The user says something like "create context files", "set up AI documentation", "add spec files", or "prepare docs for spec-driven development".

## Prerequisites

Before generating any file, understand the project:

1. **Read the manifest** (`go.mod`, `package.json`, `Cargo.toml`, `requirements.txt`, `pom.xml`, or equivalent) to identify language, framework, and key dependencies.
2. **Scan the directory structure** to understand package/module organization, entry points, and layering.
3. **Read existing docs** (README, CONTRIBUTING, OpenAPI spec, any `docs/` folder) for stated goals and conventions.
4. **Ask the user** to fill gaps: what does the service do, who consumes it, what's the database, what's in scope vs. out of scope.

Do not guess. If information is missing, ask.

---

## Writing Rules (Apply to Every File)

These rules define the tone, density, and structure of every context file:

1. **Brief and directive.** Write short imperative statements ("Use X", "Never do Y"). Do not write long descriptive paragraphs or tell a story. Each line should be a rule, a fact, or a reference — not an explanation.
2. **Under 80 lines per file.** Exceed only when the content genuinely requires it (e.g., a large endpoint catalog). If a file grows beyond 80 lines, split a section into its own file.
3. **Compact lines.** Keep each line under ~120 characters. Prefer bullet points and tables over prose paragraphs.
4. **Concrete and specific.** Name exact resources, tables, roles, technologies, and endpoints. Never use vague words like "as needed", "various endpoints", or "appropriate patterns".
5. **No duplication.** Every fact lives in exactly one file. When a topic has a dedicated file (e.g., `authentication.md`), the parent file (e.g., `architecture.md`) must contain only a 1–2 line summary with a cross-reference — not a full explanation.
6. **Cross-reference format.** Use: `> Full details → \`authentication.md\`.` or in lists: `- **Auth and RBAC** → \`authentication.md\``
7. **Self-contained.** Each file covers its own topic completely. No file depends on reading another to make sense.
8. **No generic advice.** Do not state things the model already knows ("handle errors properly", "use meaningful names"). Only document project-specific rules and decisions.

---

## Proposal Step (Before Writing)

After scanning the project, **present the user with a proposed file list before generating any content.** For each file, show:
- The filename.
- The sections it will contain, each with a one-line description.

Example format:
```
Proposed context files:

1. `project-overview.md`
   - Service Summary — one-paragraph description of the service
   - Consumer Roles — who calls this service and their access scope
   - Domain Model — key entities and relationships
   - In-Scope / Out-of-Scope — explicit boundaries

2. `architecture.md`
   - Stack Table — layer, technology, role
   - Data Model — where each data type lives
   - External Integrations — third-party APIs and downstream services
   - Invariants — rules that must never be violated
   ...
```

Ask the user to confirm, add, remove, or restructure before writing.

---

## Core Files (Always Generate)

### 1. `AGENTS.md`

The root entry point for AI agents. Must be as small as possible — every token here is consumed on every LLM call.

**Must include:**
- Project name, stack, and current phase (one line each).
- Bullet list of all context files with a 5–10 word description each.
- Instruction: "Read the file relevant to the current task. Do not load all files at once."
- 5–8 critical rules (the project's most important invariants, extracted from other files).

**Must not include:** Details that belong in other files. No code examples, no architecture, no explanations.

**Target: 30–50 lines.**

---

### 2. `project-overview.md`

What the service does, who consumes it, what's in scope.

**Must include:**
- One-paragraph service summary (what it does, who consumes it, how it fits into the larger system).
- Consumer roles and access scope (frontend apps, admin dashboards, other services, CLI tools).
- Core domain model overview (key entities and relationships — brief table or list, not a full ERD).
- API surface summary (REST/GraphQL/gRPC/WebSocket — major resource groups and their purpose).
- In-scope list (what this service is responsible for).
- Out-of-scope list (what belongs elsewhere or is deferred).

**Must not include:** Implementation details, code patterns, task status, or deployment procedures.

---

### 3. `architecture.md`

Technical structure, boundaries, and constraints.

**Must include:**
- Stack table (layer | technology | role — one row per major dependency).
- Data model table (tier | technology | contents — DB, cache, object storage, message queue).
- Auth summary (1–3 lines + cross-ref to `authentication.md` if it exists).
- External integrations list (third-party APIs, downstream services — name + purpose).
- Background processing summary (1–3 lines + cross-ref if applicable).
- Invariants (numbered list of 5–8 rules that must never be violated).

**Must not include:** Coding style, progress, or content that belongs in split-out files.

---

### 4. `project-layout.md`

Package/module organization and directory responsibilities.

**Must include:**
- ASCII directory tree showing the top 2–3 levels of the source directory.
- One-line responsibility description for each major package/directory.
- Architecture layer mapping (which directories are handlers, services, repositories, etc.).
- Naming conventions (packages, files, types, database tables/columns).

**Must not include:** Code examples beyond the directory tree, or architecture decisions.

---

### 5. `code-standards.md`

Coding conventions and patterns. Adapt sections to the project's language and framework.

**Must include (select what applies):**
- General principles (package size, separation of concerns, error handling philosophy).
- Language-specific rules (type safety, naming, import organization, error types, logging).
- Architecture layer rules (what handlers, services, and repositories can and cannot do).
- API design conventions (URL naming, request/response format, pagination, status codes).
- Database conventions (migration strategy, query patterns, transaction boundaries).
- Testing conventions (test structure, integration approach, test data, coverage expectations).
- A "Related Context Files" section listing all split-out files.

**Splitting rule:** When any section exceeds ~25 lines of rules + code examples, split it into a dedicated file. Add a 1-line summary + cross-reference in `code-standards.md`.

**Must not include:** Product requirements, architecture decisions, or progress.

---

### 6. `ai-workflow-rules.md`

How AI agents should work on this project.

**Must include:**
- Overall approach (spec-driven, incremental, one unit at a time).
- Scoping rules (no speculative changes, no placeholders, no scope creep).
- When to split work (concrete triggers: 3+ packages, both migration and handler, new integration).
- Missing requirements handling (stop and ask, propose options, do not guess).
- Protected files (auto-generated code, applied migrations, vendored dependencies).
- Migration safety (never modify applied migrations, always create new migration files).
- Documentation sync rules (when to update which context file).
- Verification checklist (compilation, tests, linter, API contract, no breaking changes).

**Must not include:** Product definitions, architecture, or coding style.

---

### 7. `progress-tracker.md`

**Do not generate content.** Create with this empty template:

```markdown
# Progress Tracker

Update after every meaningful implementation change.

## Current Phase
- (Planning / Setup / Feature Build / Polish / Release)

## Current Goal
- (Immediate objective and specific deliverables)

## Completed
- None yet.

## In Progress
- None yet.

## Next Up
- (Next 3-5 tasks in priority order)

## Blockers
- None.

## Decisions Log
| Date | Decision | Rationale |
|---|---|---|
```

Explain to the user: this file restores context between sessions. The Decisions Log prevents re-debating resolved questions.

---

## Candidate Files (Generate Based on Complexity)

Not every project needs these. Generate a candidate file when its topic is mid-to-complex in the project — meaning the rules and details for that topic would exceed ~25 lines if kept inline in a core file.

**Decision guide:** If the topic can be covered in 5–10 lines inside `architecture.md` or `code-standards.md`, keep it there. If it needs its own examples, tables, or detailed rules, split it out.

| Candidate File | Generate When |
|---|---|
| `api-design.md` | The project has an OpenAPI/GraphQL/protobuf spec, 15+ endpoints, or uses API code generation. Covers URL conventions, envelope format, pagination, error taxonomy, versioning, validation. |
| `authentication.md` | Auth involves JWT, OAuth, RBAC, multi-tenant isolation, API keys, or service-to-service auth. |
| `database-patterns.md` | Complex query patterns, multi-database setup, advanced migration workflows, or specific ORM/query builder conventions. |
| `error-handling.md` | Structured error hierarchies, domain-specific error codes, error response formatting, or logging/monitoring patterns. |
| `testing-strategy.md` | Multiple test types (unit, integration, e2e), complex test infrastructure, test database management, or contract testing. |
| `deployment.md` | Dockerfile, docker-compose, CI/CD pipelines, environment variables, infrastructure-as-code, or multi-environment setup. |
| `event-system.md` | Message queues (RabbitMQ, Kafka, SQS), event sourcing, pub/sub patterns, or async job processing. |
| `definition-of-done.md` | Testable completion criteria beyond "it compiles". 5–10 bullet points. |

When generating a candidate file, add its cross-reference to `code-standards.md` (Related Context Files section) and `AGENTS.md` (file list).

---

## After Generation

1. Review all generated files with the user. Ask if any section needs adjustment.
2. Verify no information is duplicated across files.
3. Suggest the user commit these files to version control.

