# Testing And Quality

Testing is part of the project design, not cleanup after implementation.

## Validation Levels

Projects should maintain confidence at these levels:

- backend logic and APIs
- migration and upgrade safety
- frontend components and flows
- end-to-end smoke coverage

## Backend Tests

Use `pytest`, FastAPI test tooling, and `httpx`.

Coverage should include:

- auth and sessions
- registration codes
- ownership boundaries
- admin permissions
- domain CRUD and validation
- backup and restore authorization/safety flows
- seed-data application logic
- audit-event generation for important writes

## Migration Tests

High-risk schema changes should be tested on disposable PostgreSQL databases.

Coverage should include:

- upgrade from prior revisions
- compatibility with existing data
- interaction between schema upgrades and seed-data upgrades
- operational tables such as backup and restore records

## Frontend Tests

Use Vitest and Vue Test Utils.

Coverage should include:

- auth-aware navigation
- main forms
- management UI
- dashboard/widget composition when present
- notification UI when present
- share-link management when present
- admin backup/restore confirmation flows

## End-To-End Tests

Use Playwright for smoke coverage after the main app skeleton exists.

Smoke coverage should include login, reaching an admin screen, creating a domain object, editing or submitting source data, viewing the main app surface, creating/revoking share links when supported, and creating an on-demand backup.

## Quality Rule

When a feature adds persistent behavior, add backend logic/API tests and migration coverage when schema changes.

When a feature adds user-facing behavior, add frontend tests for the main interaction.
