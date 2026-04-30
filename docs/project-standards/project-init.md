# Project Init

Use this when a new repository starts with only `docs/project-standards/` copied in.

The goal is to let an agent set up the project-facing files without duplicating the reusable standards. The standards stay in this directory. The new repository gets short routing docs, public overview docs, and project-specific architecture placeholders.

## Recommended User Prompt

After copying `docs/project-standards/` into a new repository, prompt the agent with:

```text
I want to build <project idea>. Reference ./docs/project-standards for design decisions.
Read the standards, make a setup plan, then create the initial project docs and scaffold.
Keep reusable defaults in docs/project-standards and project-specific decisions in docs/architecture.
```

If you already have a project description, create `project-description.md` before asking the agent to scaffold.

## Agent Setup Order

An agent should:

1. Read `project-description.md` when present.
2. Read `docs/project-standards/README.md`.
3. Create or update the top-level `README.md`.
4. Create a sparse top-level `AGENTS.md`.
5. Create `docs/architecture/README.md`.
6. Create `docs/development.md`.
7. Create any additional project-specific architecture docs needed for the initial plan.
8. Scaffold the application only after project-specific assumptions are clear enough.
9. Set up validation tooling as part of the scaffold: backend lint/test scripts, frontend test/build scripts, and a root `scripts/validate.sh` when those project areas exist.

## Top-Level README Template

Create a public-facing README. It should help a GitHub evaluator quickly decide whether the project is relevant.

Use this shape:

```markdown
# Project Name

Short description of what the project does and who it is for.

## Overview

High-level explanation of the problem and solution.

## Features

- Feature 1
- Feature 2
- Feature 3

## Tech Stack

- Backend: FastAPI, PostgreSQL, SQLAlchemy, Alembic
- Frontend: Vue 3, Vite, TypeScript, PrimeVue
- Infrastructure: Docker Compose, Nginx
- Testing: pytest, Vitest, Playwright

## Screenshots

Screenshots or demo links will be added after the first usable app surface exists.

## Getting Started

Development instructions live in [docs/development.md](./docs/development.md).

## Documentation

- Development: [docs/development.md](./docs/development.md)
- Project architecture: [docs/architecture/](./docs/architecture/)
- Reusable project standards: [docs/project-standards/README.md](./docs/project-standards/README.md)

## Status

Experimental, active, production-ready, archived, or another accurate status.

## License

License TBD.
```

The README must not duplicate the architecture standards or read like an AI prompt.

## AGENTS.md Template

Create a sparse top-level `AGENTS.md`. It should route agents to the right docs and enforce the verification loop.

Use this shape:

```markdown
# Agent Instructions

## Project Basics

- Briefly identify the project type and stack.
- Read `project-description.md` first when present.

## Documentation Map

- Reusable cross-project defaults: `docs/project-standards/README.md`
- Project-specific architecture: `docs/architecture/`
- Development/runbook: `docs/development.md`
- Public overview: `README.md`

## Working Rules

- Follow reusable standards unless project-specific docs explicitly override them.
- Keep project-specific decisions in `docs/architecture/`, not in `docs/project-standards/`.
- Update docs when introducing a reusable project convention or architecture decision.
- Prefer boring, maintainable implementations.

## Verification Loop

- Run the narrowest relevant tests while iterating.
- Before declaring completion, run `./scripts/validate.sh` when feasible.
- If full validation cannot be run, state exactly what was and was not verified.
- Do not claim success without running or explaining verification.
```

Keep this file short. It is a routing and enforcement file, not a design document.

## docs/architecture/README.md Template

Create `docs/architecture/README.md` as a project-specific architecture entrypoint.

Use this shape:

```markdown
# Project Architecture

This directory is reserved for project-specific architecture.

Use it for domain models, product workflows, project-specific technical decisions, and approved deviations from the reusable standards in `docs/project-standards/`.

Do not copy the reusable defaults into this directory unless this project intentionally overrides them.

## Current Architecture Docs

- System overview: TBD
- Decisions and open questions: TBD
- Domain model: TBD
- Operations notes: TBD
- Testing expectations: TBD
```

Project-specific docs should explain the product, not restate cross-project defaults.

## docs/development.md Template

Create `docs/development.md` once there are real run commands.

Use this starting shape:

````markdown
# Development

## Prerequisites

- Docker and Docker Compose
- Python 3.12 when running backend tools outside containers
- Node.js when running frontend tools outside containers

## Local Run

Document the exact commands after the scaffold exists.

## Validation

Run the full validation flow from the repository root:

```bash
./scripts/validate.sh
```

During implementation, run the narrowest relevant backend or frontend checks while iterating. If full validation cannot be run, state what was and was not verified.

Validation is required project infrastructure. As soon as backend or frontend code exists, create the matching scripts from `docs/project-standards/validation-scripts.md`, make them executable, and keep this document pointed at the root validation command.
````

## First Project-Specific Architecture Docs

After setup, create focused docs under `docs/architecture/` when the project needs them:

- `10-system-overview.md`
- `20-decisions-and-open-questions.md`
- `30-domain-model.md`
- `90-testing-and-quality.md`
- `110-operations-and-infrastructure.md`

These files are optional until there is project-specific content to record.

## What Must Stay In Project Standards

Keep reusable cross-project defaults in `docs/project-standards/`, including:

- default stack
- repository layout
- backend and frontend conventions
- auth and authorization defaults
- security defaults
- operations, backup, and restore defaults
- testing expectations
- documentation standards

## What Must Stay Project-Specific

Keep these in `docs/architecture/` or `project-description.md`:

- product behavior
- domain model
- business rules
- user workflows
- approved deviations from the standards
- project-specific security concerns
- project-specific operational constraints

## Initial Scaffold Checklist

The agent should leave a new project with:

- top-level `README.md`
- top-level `AGENTS.md`
- `docs/architecture/README.md`
- `docs/development.md`
- copied `docs/project-standards/`
- validation tooling following `docs/project-standards/validation-scripts.md`
- clear project-specific open questions
- a plan for application scaffold, schema, auth, and validation

Do not scaffold large amounts of code before the domain model and first implementation phase are clear.
