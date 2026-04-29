# Seed And Demo Data

Seed/demo data is an application feature, not an Alembic replacement.

Use this pattern when the project needs examples, screenshots, onboarding, or deterministic development data.

## Defaults

Applications should:

- flag users as example/demo accounts when demo data is user-scoped
- maintain named seed profiles if multiple demo shapes are useful
- maintain seed revision identifiers in application code
- track applied seed revisions per flagged user in an `example_seed_applications` style table
- run the seed upgrader at app startup and during relevant auth flows
- make seed operations idempotent
- emit audit events for seed upgrades when audit logging exists

Applications must not overwrite real-user data.

## Seed Content

Seed data may include domain records, dashboards, widgets, and example history.

Public share links should not be seeded unless explicitly approved because they create visibility outside the normal private boundary.

## Migration Boundary

Alembic owns schema evolution only. Seed-data evolution is separate because it depends on application semantics and should target only flagged example/demo accounts.
