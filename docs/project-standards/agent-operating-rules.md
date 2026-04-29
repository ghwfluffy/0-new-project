# Agent Operating Rules

## Default Behavior

Agents must treat these standards as pre-approved defaults. A project-specific document may override them, but the override should be explicit.

Agents should:

- Read `project-description.md` first when present.
- Use `docs/project-standards/README.md` as the reusable standards entrypoint.
- Use `docs/architecture/` for project-specific architecture.
- Prefer boring, maintainable implementations.
- Implement in phases: foundation, core domain, admin/operations, sharing/integrations when needed, then polish.
- Add tests as part of the feature work.
- Keep documentation current when a decision becomes durable.

Agents must not:

- Ask about routine details that these standards already decide.
- Put reusable cross-project defaults in `docs/architecture/`.
- Put project-specific business rules in `docs/project-standards/`.
- Claim work is complete without running verification or explaining why verification was not run.

## Feedback Points

Agents should ask for feedback before locking in decisions that are expensive to undo, including:

- database schema and ownership boundaries
- authentication or authorization model changes
- destructive operations
- backup or restore behavior
- dependency changes outside the approved stack
- major UI framework changes
- domain workflows that are unclear in `project-description.md`
