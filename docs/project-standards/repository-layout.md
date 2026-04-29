# Repository Layout

Use a single repository with this shape unless the project explicitly overrides it.

```text
.
|-- api/
|   |-- alembic/
|   |-- app/
|   |   |-- api/
|   |   |   |-- routes/
|   |   |   `-- schemas/
|   |   |-- core/
|   |   |-- db/
|   |   |-- services/
|   |   `-- main.py
|   |-- tests/
|   |-- Dockerfile
|   |-- lint.sh
|   |-- pyproject.toml
|   |-- requirements.txt
|   `-- test.sh
|-- db/
|   |-- backup-loop.sh
|   |-- create_backup.sh
|   `-- init.sql
|-- docs/
|   |-- architecture/
|   |-- project-standards/
|   |-- style/
|   `-- development.md
|-- nginx/
|   |-- Dockerfile
|   `-- default.conf
|-- scripts/
|   `-- validate.sh
|-- web/
|   |-- public/
|   |-- src/
|   |   |-- components/
|   |   |-- lib/
|   |   |-- router/
|   |   |-- stores/
|   |   |-- views/
|   |   |-- main.ts
|   |   `-- style.css
|   |-- Dockerfile
|   |-- build.sh
|   |-- package.json
|   |-- test.sh
|   `-- vite.config.mjs
|-- backups/
|-- docker-compose.yml
|-- .env.example
|-- go.sh
`-- README.md
```

## Boundaries

- Backend route modules must not absorb business logic.
- Frontend views should stay thin and composition-oriented.
- Reusable cross-project standards belong in `docs/project-standards/`.
- Project-specific architecture belongs in `docs/architecture/`.
- Public project description belongs in `README.md`.
- Local development and runbook instructions belong in `docs/development.md`.

The exact domain module names may vary, but these ownership boundaries should remain stable.
