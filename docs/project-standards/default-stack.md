# Default Stack

This stack is the default unless the project explicitly overrides it.

## Backend

- Python 3.12 on Ubuntu 24.04/Noble containers.
- FastAPI for HTTP APIs.
- Pydantic and `pydantic-settings` for schemas and environment configuration.
- SQLAlchemy ORM for persistence.
- Alembic for schema migrations.
- PostgreSQL as the source-of-truth database.
- Uvicorn for local and containerized development serving.
- `psycopg2-binary` as the default PostgreSQL driver.
- `bcrypt` for password hashing.
- `cryptography` where secure signing or encryption primitives are needed.
- `python-multipart` and Pillow for conservative upload/profile-image handling when uploads are needed.
- `pytest`, FastAPI test tooling, and `httpx` for backend tests.
- `mypy`, `ruff`, and `flake8` for backend quality checks.

## Frontend

- Vue 3.
- Vite.
- TypeScript.
- Pinia for client-side state.
- Vue Router.
- PrimeVue and PrimeIcons as the default vetted component system.
- Vitest and Vue Test Utils for frontend unit/component tests.
- Playwright for end-to-end smoke tests when the project has enough UI workflow surface.

Do not reinvent basic UI controls. Use the component framework for dialogs, buttons, menus, tabs, tables, forms, dropdowns, toasts, avatars, progress indicators, and similar primitives.

If a future project explicitly requires a different vetted component framework such as Carbon, use it consistently as the component system. Do not mix multiple full component libraries without approval.

## Infrastructure

- Docker Compose is the default local and small-deployment orchestrator.
- Nginx is the default ingress/static frontend server.
- PostgreSQL backup automation must be part of the Compose stack.
- A production deployment may use an outer proxy for TLS and routing.

## Dependency Changes

New dependencies should be introduced only when they remove meaningful complexity or provide a vetted capability. Agents should document why a new dependency is necessary when it affects the project standard stack.
