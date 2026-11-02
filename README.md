# Spec-Driven Context Generator Skills

Two production-ready skills for AI coding assistants (Antigravity, Cursor, Gemini CLI, Claude Code) to inspect an existing codebase and automatically generate spec-driven context files.

- `frontend-context-files` — For web applications, mobile applications, or client-side libraries.
- `backend-context-files` — For API servers, microservices, CLI tools, or data pipelines.

---

## What It Does

The AI agent scans your codebase (package manifest, folder hierarchy, existing documentation), proposes a compact, modular file list with section summaries, and writes the context files upon your approval.

These files serve as the single source of truth for AI agents in future coding sessions, preventing architectural drift, hallucinations, and code churn.

---

## Repository Structure

```text
spec-driven-context-skills/
├── README.md                           # Developer guide & prompt templates (for humans)
├── LICENSE                             # MIT License
└── skills/
    ├── frontend-context-files/
    │   └── SKILL.md                    # Agent instructions for frontend codebases
    └── backend-context-files/
        └── SKILL.md                    # Agent instructions for backend codebases
```

---

## Installation

### Option 1: Global (Available across all projects on your machine)

Copy the skills to your global agent customization directory:

**Windows (PowerShell):**
```powershell
Copy-Item -Recurse .\skills\* "$HOME\.gemini\config\skills\"
```

**macOS / Linux:**
```bash
cp -r ./skills/* ~/.gemini/config/skills/
```

### Option 2: Project-Specific (Committed with your team's repository)

Copy the skills directly into your project's workspace agent directory:

```bash
mkdir -p .agents/skills
cp -r /path/to/spec-driven-context-skills/skills/* .agents/skills/
```

---

## How It Works

1. **Provide a prompt** describing your project using one of the templates below.
2. **Agent scans the codebase** (package manifest, directory structure, existing documentation).
3. **Agent proposes a file list** with sections and descriptions for your approval before writing anything.
4. **Agent generates the files** following strict brevity and spec-driven rules (under 80 lines/file, compact directive format, no duplication).
5. **Review and commit** the context files to version control.

---

## Frontend Prompt Template

Use when generating context for a web, mobile, or client application.

```markdown
Generate context files for my frontend project.

Project: [one sentence — what the app does]
Framework: [e.g., Next.js 15, Expo SDK 58, React 19, Vue 3, Flutter]
Language: [e.g., TypeScript, Dart, JavaScript]
Backend: [e.g., REST API with OpenAPI spec, GraphQL, Supabase, Firebase]
Auth: [e.g., SuperTokens, Auth0, Firebase Auth, NextAuth, none]
Users: [e.g., "graduates only (Phase 1)", "customers and admins"]
Phase: [e.g., "MVP", "Phase 1", "production maintenance"]

Notes:
- [e.g., "RTL Arabic app", "uses Hey-API generated client", "WebSocket real-time chat", "NativeWind v4 styling"]

Output directory: [e.g., docs/context/ or .agent-guides/]
```

### Minimal Frontend Prompt
```markdown
Generate context files for my frontend project at [path/to/project].
It's an [Expo/Next.js/React] app that [one sentence description].
```

---

## Backend Prompt Template

Use when generating context for an API server, microservice, or backend pipeline.

```markdown
Generate context files for my backend project.

Project: [one sentence — what the service does]
Language/Framework: [e.g., Go/Iris, Node/Express, Python/FastAPI, Rust/Axum]
Database: [e.g., PostgreSQL, MongoDB, DynamoDB]
API Style: [e.g., REST with OpenAPI spec, GraphQL, gRPC]
Auth: [e.g., JWT, OAuth2, API keys, SuperTokens, none]
Consumers: [e.g., "React frontend + mobile app", "internal microservices"]
Phase: [e.g., "active development", "production v1"]

Notes:
- [e.g., "uses OpenAPI code generation", "Redis background worker", "multi-tenant with org isolation"]

Output directory: [e.g., docs/context/ or .agent-guides/]
```

### Minimal Backend Prompt
```markdown
Generate context files for my backend project at [path/to/project].
It's a [Go/Node/Python] service that [one sentence description].
```

---

## Developer Tips

1. **Be specific about scope:** Clarify what is in Phase 1 vs. future phases so the agent does not document features that should not be built yet.
2. **Highlight complex subsystems:** If your auth flow, real-time messaging, or query setup has non-obvious nuances, mention it in `Notes:`. The agent will dedicate a separate candidate file for it.
3. **Review the proposal:** The agent will output a proposed file list with sections before generating. Adjust or prune sections at this step.
4. **Keep them living:** Update `progress-tracker.md` and your context files as your architecture evolves.

