# Audit Events And Jobs

## Audit Events

Applications should add durable audit logging for meaningful writes.

Audit records should answer:

- who acted
- whether the actor was a user, administrator, system process, or background job
- what action occurred
- what entities were affected
- when it occurred
- enough context to explain the write without storing secrets

## Common Audited Actions

Audit events should cover:

- account creation
- login/logout where practical
- password changes
- admin actions
- registration-code creation/revocation
- backup creation and restore requests
- domain object creation/update/delete/archive
- source-data entry creation/correction
- public share-link creation/revocation
- seed-data upgrades
- notification generation/dismissal
- background jobs that persist derived state

Not every read action needs auditing. The goal is meaningful traceability, not noise.

## Background Jobs

Background jobs should be:

- deterministic where possible
- diagnosable when they fail
- safe to rerun
- auditable when they write persistent state

Likely job categories include reminder evaluation, notification cleanup, derived summary refresh, forecast recalculation, share-link expiration cleanup, backup retention, restore execution, and seed-data upgrades.
