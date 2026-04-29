# Project Standards

This directory is the entrypoint for reusable cross-project defaults. Copy it into new web app projects so agents know the architecture and engineering decisions that have already been made.

Use these standards together with a project-specific `project-description.md` and any files under `docs/architecture/`.

## Scope

These files contain defaults that should apply across projects. Project-specific domain models, workflows, business rules, and approved deviations must live in `docs/architecture/`, not here.

Agents must follow these standards unless project-specific docs explicitly override them.

## Agent Reading Order

1. Read `project-description.md` when present.
2. Read this README.
3. For a new repository setup, read [Project Init](./project-init.md).
4. Read the relevant standards files below.
5. Read `docs/architecture/` for project-specific decisions when that directory exists.
6. Read `docs/development.md` for local run and validation instructions when that file exists.

## Standards Map

- [Project Init](./project-init.md)
- [Agent Operating Rules](./agent-operating-rules.md)
- [Default Stack](./default-stack.md)
- [Repository Layout](./repository-layout.md)
- [Backend Standards](./backend-standards.md)
- [API Standards](./api-standards.md)
- [Frontend Standards](./frontend-standards.md)
- [UI, CSS, And Theming](./ui-css-and-theming.md)
- [Data Modeling And Migrations](./data-modeling-and-migrations.md)
- [Auth And Authorization](./auth-and-authorization.md)
- [Security Standards](./security-standards.md)
- [Operations, Backup, And Restore](./operations-backup-restore.md)
- [Testing And Quality](./testing-and-quality.md)
- [Seed And Demo Data](./seed-demo-data.md)
- [Audit Events And Jobs](./audit-events-and-jobs.md)
- [Notifications And Reminders](./notifications-reminders.md)
- [Dashboard, Widget, And Sharing](./dashboard-widget-sharing.md)
- [Documentation Standards](./documentation-standards.md)
- [Validation Scripts](./validation-scripts.md)
- [Delivery Checklist](./delivery-checklist.md)
- [Missing Decisions To Consider](./missing-decisions-to-consider.md)

## Override Rule

Reusable standards should not be edited to satisfy one project's unusual domain requirement. Document that requirement in `docs/architecture/` and state the reason for the override.
