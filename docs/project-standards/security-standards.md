# Security Standards

## Defaults

Security defaults must be conservative.

Applications must:

- read secrets from environment variables
- avoid silently using unsafe production secrets
- log warnings in development when default or generated temporary secrets are used
- use HTTP-only session cookies
- use secure session cookies outside development/test
- set CORS origins explicitly
- validate and sanitize user input
- avoid returning secrets, password hashes, session tokens, internal paths, or raw stack traces
- generate audit events for auth flows and privileged writes where practical

## Ingress

Nginx should:

- apply a general per-IP rate limit for `/api/`
- apply stricter per-IP limits for bootstrap, register, and login
- return `429 Too Many Requests` before abusive traffic reaches FastAPI
- forward `Host`, `X-Forwarded-For`, and `X-Forwarded-Proto`

Applications should be compatible with operation behind a trusted outer proxy.

## Uploads

Upload handling must be explicit and conservative.

Applications should:

- validate file type and size
- re-render user images into a safe format such as PNG before storing or serving
- avoid serving arbitrary uploaded bytes directly
- document where uploaded files live and whether backups include them

## Destructive Operations

Restore and destructive admin actions must be:

- administrator-only
- explicitly confirmed
- server-side validated
- auditable
- implemented through controlled server-side execution, not arbitrary browser-side shell access

## Threat Modeling

Each project should keep a lightweight threat-model checklist in `docs/architecture/` once the main workflows are known. See [Missing Decisions To Consider](./missing-decisions-to-consider.md).
