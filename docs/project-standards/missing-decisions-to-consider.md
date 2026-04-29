# Missing Decisions To Consider

These are reusable default recommendations for decisions that each project should eventually make explicit. Document project-specific answers in `docs/architecture/`.

## API Error Response Contract

Default recommendation: use one stable JSON error envelope with a machine-readable code, human-readable message, optional field errors, and request ID.

## Pagination, Filtering, And Sorting

Default recommendation: use limit/offset or cursor pagination consistently per resource class. Document allowed filters and sort keys per endpoint.

## Archive, Soft Delete, And Hard Delete

Default recommendation: use archive for user-facing reversible removal, soft delete for audit-sensitive records, and hard delete only for records that are safe to destroy or required to be deleted.

## Timestamp, Date, And Timezone Details

Default recommendation: store instants as UTC timestamps, store user timezone as an IANA name, and document whether date-only values resolve to start-of-day or end-of-day for each domain rule.

## Idempotency Keys

Default recommendation: require idempotency keys for expensive, externally triggered, payment-like, import, backup, restore, or repeated write operations.

## Background Worker Strategy

Default recommendation: start with simple in-process or Compose-managed scheduled jobs. Move to a dedicated queue only when retry, concurrency, or throughput needs justify it.

## Logging And Observability

Default recommendation: use structured logs with request IDs, user/actor IDs where safe, route names, status codes, and durations. Do not log secrets or session tokens.

## Dependency Pinning And Approval

Default recommendation: pin dependency versions through lockfiles where the ecosystem supports it. Require a short rationale for new runtime dependencies.

## Production Config Validation

Default recommendation: fail fast in production when required secrets, database settings, public URLs, or security-sensitive config values are missing or unsafe.

## File Upload Storage

Default recommendation: decide whether uploads live in PostgreSQL, local mounted storage, or object storage. Document backup implications and maximum file sizes.

## Email And Outbox

Default recommendation: do not call external email providers directly from request handlers once reliability matters. Use an outbox table or job-mediated delivery.

## Accessibility Baseline

Default recommendation: target semantic HTML, keyboard navigation for core flows, visible focus states, form labels, sufficient contrast, and no critical color-only indicators.

## Browser Support

Default recommendation: support current stable Chrome, Firefox, Safari, and Edge. Document any mobile browser requirements for the product.

## Internationalization

Default recommendation: keep copy centralized enough to migrate later, but do not add a full i18n framework until the project needs multiple languages.

## OpenAPI And Client Generation

Default recommendation: keep FastAPI OpenAPI output accurate. Generate clients only when it reduces real duplication or supports an external consumer.

## Data Export And Import

Default recommendation: provide explicit export/import formats for user-owned data when the product stores meaningful user history. Validate imports as untrusted input.

## Lightweight Threat Model

Default recommendation: document actors, protected data, public surfaces, privileged actions, destructive actions, and likely abuse paths before production use.
