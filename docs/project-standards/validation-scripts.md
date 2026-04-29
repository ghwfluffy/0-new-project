# Validation Scripts

Projects should expose predictable validation entry points.

## Scripts

Provide:

```text
./scripts/validate.sh
./api/lint.sh
./api/test.sh
./web/test.sh
./web/build.sh
```

## Expected Behavior

- `api/lint.sh` runs `mypy`, `flake8`, and `ruff`.
- `api/test.sh` runs `pytest`.
- `web/test.sh` runs Vitest.
- `web/build.sh` runs `vue-tsc --noEmit` and `vite build`.
- `scripts/validate.sh` runs all of the above from the repository root.

## Agent Verification Loop

Agents should run the narrowest useful checks while iterating.

Before declaring completion, agents should run `./scripts/validate.sh` when feasible. If full validation cannot be run, agents must state exactly what was and was not verified.
