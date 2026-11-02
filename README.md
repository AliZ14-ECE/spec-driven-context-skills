# Spec-Driven Context Skills

AI agent skills that generate structured context files for AI-assisted development. One skill for frontend projects, one for backend.

## Skills

| Skill                                                                | Description                                                                      |
| -------------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| [`backend-context-files`](./skills/backend-context-files/SKILL.md)   | Generate spec-driven context files for backend projects (APIs, services, CLIs)   |
| [`frontend-context-files`](./skills/frontend-context-files/SKILL.md) | Generate spec-driven context files for frontend projects (web apps, mobile apps) |

## Installation

Install all skills interactively:

```bash
npx skills add AliZ14-ECE/spec-driven-context-skills
```

Install a specific skill:

```bash
# Backend context files
npx skills add AliZ14-ECE/spec-driven-context-skills --skill backend-context-files -a claude-code

# Frontend context files
npx skills add AliZ14-ECE/spec-driven-context-skills --skill frontend-context-files -a claude-code
```

Install globally (available in all your projects):

```bash
npx skills add -g AliZ14-ECE/spec-driven-context-skills
```

> **Requirements:** Node.js 22.20 or later

## What These Skills Do

The AI agent scans your existing project, proposes a list of context files, and writes them after your approval. These files give any AI agent (Cursor, Copilot, Claude, Gemini) the full picture of your project so it writes code that matches your architecture, conventions, and scope.

### How it works

1. You provide a prompt describing your project
2. The agent scans your codebase (manifest, folder structure, existing docs)
3. The agent proposes a file list with sections — you approve, add, or remove
4. The agent generates the files
5. You review and commit them to version control

## Usage

### Backend projects

```
Generate context files for my backend project.

Project: [one sentence — what the service does]
Language/Framework: [e.g., Go/Iris, Node/Express, Python/FastAPI, Rust/Axum]
Database: [e.g., PostgreSQL, MongoDB, DynamoDB]
API Style: [e.g., REST with OpenAPI spec, GraphQL, gRPC]
Auth: [e.g., JWT, OAuth2, API keys, SuperTokens, none]
Consumers: [who calls this service — e.g., "React frontend + mobile app", "internal microservices"]
Phase: [e.g., "active development", "adding new features to production service"]

Notes:
- [anything the agent should know]

Output directory: [e.g., docs/context/ or .agent-guides/]
```

**Minimal version:**

```
Generate context files for my backend project at [path/to/project].
It's a [language/framework] service that [one sentence description].
```

### Frontend projects

```
Generate context files for my frontend project.

Project: [one sentence — what the app does]
Framework: [e.g., Next.js 15, Expo SDK 58, Vue 3, Angular 18]
Language: [e.g., TypeScript, JavaScript]
Backend: [e.g., REST API at api.example.com, GraphQL, Firebase, Supabase]
Auth: [e.g., SuperTokens, Auth0, Firebase Auth, NextAuth, none]
Users: [who uses the app — e.g., "customers and admins", "students only"]
Phase: [e.g., "MVP", "Phase 1 — customer features only", "production maintenance"]

Notes:
- [anything the agent should know]

Output directory: [e.g., docs/context/ or .agent-guides/]
```

**Minimal version:**

```
Generate context files for my frontend project at [path/to/project].
It's a [framework] app that [one sentence description].
```

## Generated Files

Both skills generate these core context files:

| File                   | Purpose                                             |
| ---------------------- | --------------------------------------------------- |
| `AGENTS.md`            | Root entry point — loaded on every agent session    |
| `project-overview.md`  | What the project does, who uses it, what's in scope |
| `architecture.md`      | Tech stack, data model, invariants                  |
| `project-layout.md`    | Directory structure and naming conventions          |
| `code-standards.md`    | Coding conventions and patterns                     |
| `ai-workflow-rules.md` | How the AI agent should work on this project        |
| `progress-tracker.md`  | Session continuity and decision log                 |

Additional files are generated based on project complexity (e.g., `authentication.md`, `api-design.md`, `testing-strategy.md`).

## Tips

- **Be specific about scope.** Tell the agent what's in Phase 1 vs. Phase 2 so it doesn't document features you're not building yet.
- **Mention the complex parts.** If your auth flow, real-time system, or API client has non-obvious details, say so — the agent will create dedicated files for them.
- **Review the proposal.** The agent will show you the file list before writing. This is your chance to add or remove files.
- **Iterate.** After generation, ask the agent to refine specific files. These are living docs — update them as the project evolves.

## License

MIT
