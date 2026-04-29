# Delivery Checklist

A new project should be left with these pieces unless the project explicitly scopes them out.

## Foundation

- Docker Compose stack.
- Backend FastAPI skeleton.
- Frontend Vue/Vite skeleton.
- PostgreSQL integration.
- Alembic configured and at least one migration.
- `.env.example`.
- Base-path/proxy support.

## Auth And Admin

- Server-side session auth.
- First-admin bootstrap.
- Registration-code flow if more than one user is allowed.
- User profile with timezone.
- Admin-only APIs for registration codes and backups.

## Operations

- Backup scripts.
- `db-backup` service.
- Backup metadata persistence.
- Restore-operation model and guarded admin flow when restore is in scope.

## Frontend

- Token-driven frontend CSS.
- Shared component patterns instead of copied UI.
- Mobile and desktop handling for core workflows.

## Quality

- Backend, frontend, and validation scripts.
- Tests for auth, ownership, core domain behavior, and main UI flows.
- Documentation that captures project-specific architecture decisions.
