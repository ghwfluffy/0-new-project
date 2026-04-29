# Auth And Authorization

## Authentication

Use server-side sessions stored in PostgreSQL. Do not use fully stateless signed-cookie sessions as the default.

Default behavior:

- The first account can bootstrap without a registration code and becomes an administrator.
- Later registration requires an admin-managed registration code.
- Registration codes have expiration timestamps, optional revocation, creator metadata, and traceability to created users.
- Users may optionally be flagged as example/demo users when the product supports seeded data.
- Passwords are stored with bcrypt-backed hashes using production-safe work factors.
- Login lockout/rate-limit protections should exist at both app and ingress layers where appropriate.

## Session Model

Sessions should:

- store a random session token hash in the database
- send an opaque signed cookie containing the raw token plus signature
- use HTTP-only cookies
- use secure cookies outside development/test
- store created, last-seen, expiration, revocation, user-agent, and source-IP metadata where practical
- validate expiration and revocation server-side
- support password-change invalidation

## Authorization

Every API operation must choose one access category:

- anonymous public
- authenticated user-owned
- administrator-only
- system/background job

Default access rules:

- Users can access and manage only their own private records.
- Administrators can perform administrative account, registration-code, backup, restore, and operational actions.
- Administrator status does not automatically mean public visibility into private user content unless the project explicitly designs that ability.
- Public access exists only through explicit token/share records.
- Restore and destructive admin actions require explicit confirmation and server-side validation.

## Profiles

User profiles should include:

- username or login identifier
- display name
- timezone
- administrator flag
- example/demo-data flag when applicable
- profile image/avatar if needed

## Registration Codes

Registration codes are a first-class administrative feature.

They should support:

- secure random code generation
- expiration timestamp
- revocation timestamp
- creating administrator reference
- created-user traceability
- list/create/revoke/update admin APIs
- frontend admin UI
- tests for expiration, revocation, and role enforcement
