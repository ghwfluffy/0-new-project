# Data Modeling And Migrations

## Database

Use PostgreSQL as the durable source of truth.

Use Alembic as the only schema version tracker. Do not create a second custom database-version table.

Application settings, seed revisions, feature flags, backup metadata, and operational state should live in normal application tables.

## Schema Principles

Schemas should:

- make ownership explicit for every user-owned table
- use UUID-style string primary keys unless the project has a reason to use database integers
- store timestamps as timezone-aware UTC datetimes
- store user/profile timezone as an IANA timezone string when day-boundary semantics matter
- use relational constraints for ownership, uniqueness, and mutually exclusive relationships where practical
- prefer normalized relational structure for core domain records
- use JSON only for genuinely flexible display/configuration payloads
- keep source-of-truth records separate from rebuildable derived records
- model high-risk operational actions, such as restore, as first-class records

## Migrations

Every persistent feature requires an Alembic migration.

Migration revision IDs must be short enough for PostgreSQL's `alembic_version.version_num` column. Keep revision IDs at 32 characters or fewer.

High-risk migrations should be tested on disposable PostgreSQL databases.

Schema migrations must remain separate from seed/demo data upgrades.

## Source Data And Derived State

The database should preserve source data first. Derived views, summaries, forecasts, dashboards, snapshots, and cached calculations should be rebuildable from source records plus deterministic rules.

If derived state is persisted, it should record when and why it was computed and keep enough source references to explain it.

Project-specific entity boundaries and business rules belong in `docs/architecture/`.
