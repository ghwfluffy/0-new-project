# API Standards

## Prefix

Use a versioned API prefix:

```text
/api/v1
```

## Default Route Areas

Most applications should start with:

- `/api/v1/status`
- `/api/v1/auth/bootstrap-status`
- `/api/v1/auth/bootstrap`
- `/api/v1/auth/login`
- `/api/v1/auth/logout`
- `/api/v1/auth/register`
- `/api/v1/auth/me`
- `/api/v1/users`
- `/api/v1/invitation-codes`
- `/api/v1/admin/backups`
- `/api/v1/admin/backups/{backup_id}/restore`
- domain routes from `project-description.md`
- share/public routes if the product exposes public tokens

## Design Rules

APIs should:

- use resource-oriented routes
- keep admin routes visibly under an admin namespace where practical
- use explicit request and response schemas
- return stable identifiers and timestamps
- validate ownership in every route that touches user-owned data
- use deterministic, typed service errors and map them to clear HTTP statuses

APIs must not:

- expose internal database identifiers through public URLs when share tokens are intended
- rely on frontend checks for authorization
- leak secrets, password hashes, session tokens, internal paths, or raw stack traces

## Public Access

Public access must go through explicit token/share records. Do not make private entities public by exposing normal internal IDs.

## Error Contract

Every project should define its concrete API error response contract in `docs/architecture/`. See [Missing Decisions To Consider](./missing-decisions-to-consider.md) for the reusable default recommendation.

Error responses should include a safe, human-readable message suitable for frontend toast display. Do not expose raw exception text, stack traces, internal paths, secrets, or implementation details in this message.
