# Frontend Standards

## Framework

Use Vue single-file components with TypeScript, Vite, Pinia, Vue Router, PrimeVue, and PrimeIcons unless the project explicitly overrides the frontend stack.

## Layout

Recommended layout:

- `web/src/main.ts`: application bootstrap, PrimeVue setup, router, Pinia, and global CSS.
- `web/src/App.vue`: top-level shell.
- `web/src/router`: route definitions and auth-aware navigation.
- `web/src/stores`: Pinia stores for auth, profile, domain resources, admin resources, notifications, and status.
- `web/src/lib`: API client, base-path helpers, date/time helpers, theme bridge, toast helpers, and presentation-only domain helpers.
- `web/src/components`: reusable components split by domain or shell area.
- `web/src/views`: route-level composition components.

## Rules

Frontend code should:

- keep route views thin and composition-oriented
- extract reusable management shells, toolbars, tables, dialogs, and card/list patterns
- use Pinia stores for shared server state and cross-view actions
- centralize API fetch logic in `web/src/lib/api.ts` or focused API modules
- use a shared toast service for user feedback
- restore auth state from `/auth/me` on app load
- use browser timezone for timestamp display unless the project defines another display timezone
- include mobile and desktop layouts from the start for core workflows

Frontend code must not:

- implement authorization as a frontend-only concern
- duplicate API fetch logic across components
- let route views become large mixed-responsibility files
- rebuild framework controls that PrimeVue already provides

## Timezone Boundary

The frontend may display timestamps in the browser timezone. Server-side day-boundary, schedule, and compliance semantics should use the saved user profile timezone.
