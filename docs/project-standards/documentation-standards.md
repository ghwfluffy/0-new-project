# Documentation Standards

Documentation should act as durable memory for future agents and maintainers.

## Documentation Locations

- `README.md`: public overview for humans evaluating the repository.
- `AGENTS.md`: short routing and working rules for agents.
- `docs/project-standards/`: reusable cross-project defaults.
- `docs/project-standards/project-init.md`: setup playbook for creating project-facing docs from copied standards.
- `docs/architecture/`: project-specific architecture, business rules, and approved overrides.
- `docs/development.md`: local run commands, validation, and development runbook.
- `docs/style/`: optional project-specific style guidance when a project needs more detail than the reusable standards.

## Architecture Docs

Project-specific architecture docs should capture:

- system overview
- approved decisions and open questions
- domain model
- auth/access deviations from the standards
- operations deviations from the standards
- testing expectations specific to the project

Reusable defaults must not be copied into `docs/architecture/` unless the project intentionally overrides them.

## When To Update Docs

Update docs when:

- a reusable convention changes
- a project-specific architecture decision is approved
- a dependency is added or replaced
- schema ownership boundaries change
- operations, backup, restore, or security behavior changes
- validation commands change

Prefer updating an existing focused doc over creating overlapping documentation.
