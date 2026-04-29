# Backend Standards

## Layers

Backend code should use these layers:

- `api/app/api/routes`: FastAPI routers, request handling, dependency wiring, and HTTP error translation.
- `api/app/api/schemas`: Pydantic request/response models and serializer helpers.
- `api/app/services`: domain rules, validation, calculations, orchestration, shared business logic, and transaction coordination.
- `api/app/db`: SQLAlchemy models and session setup.
- `api/app/core`: settings, security helpers, and cross-cutting app configuration.

## Route Modules

Route modules should:

- parse validated request payloads
- load authenticated user and database session dependencies
- call service-layer functions
- translate service errors into HTTP errors

Route modules must not become the home for business rules, progress calculations, forecasting, serialization shared across domains, or cross-endpoint validation.

## Services

Service modules must own domain behavior.

Services should:

- validate domain invariants
- normalize user input for persistence
- calculate progress, forecasts, compliance, or other domain outputs
- coordinate related writes in transactions
- provide shared behavior used by routes, jobs, and rendering code

Services must not return FastAPI response objects or HTTP-specific errors.

## Database Layer

SQLAlchemy models should start in `api/app/db/models.py`. Split by domain only when the file becomes hard to navigate, and preserve a clear import surface through `api/app/db/__init__.py`.

Database modules must not absorb endpoint serialization, request validation, or business workflows.

## Module Size And Reuse

Avoid catch-all modules named `utils.py`, `helpers.py`, `misc.py`, or `common.py`.

Prefer:

1. an existing domain service helper
2. a new focused shared service module
3. a local helper inside one module

Review files over roughly 300 lines for mixed responsibilities. Treat files over roughly 500 lines as refactor targets unless they are one coherent unit.

## Python Style

- Use full type hints.
- Keep backend line length at 110 columns.
- Prefer small typed functions with explicit parameters.
- Use descriptive names over generic helpers.
- Add comments only when intent is not obvious from the code.
