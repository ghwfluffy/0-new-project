# Validation Scripts

Projects must set up good validation tooling during the initial scaffold, not after feature work has already started.

Validation tooling is considered part of the baseline project infrastructure. A project is not properly scaffolded until it has repeatable commands for backend linting, backend tests, frontend tests, frontend production builds, and one root-level command that runs the full validation flow.

These scripts should be reliable in a fresh checkout, should install or refresh local dependencies when practical, and should fail fast on errors. They are the commands agents and humans use to decide whether a change is complete.

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

## Setup Requirements

Set up validation tooling as soon as the corresponding project area exists:

- Backend scaffolds must include dependency declarations for test and lint tooling.
- Backend scaffolds must include `api/lint.sh` and `api/test.sh`.
- Frontend scaffolds must include `web/test.sh` and `web/build.sh`.
- Repositories with both backend and frontend code must include `scripts/validate.sh`.
- Scripts must be executable and usable from a clean checkout after installing normal system prerequisites.
- Scripts should manage local virtualenv or `node_modules` setup when that keeps validation predictable.
- Scripts must return non-zero exit codes when linting, type checks, tests, or builds fail.
- `scripts/validate.sh` must run every required check and must be the command documented in `docs/development.md`, `AGENTS.md`, and project-specific testing/operations docs.

Do not leave validation as informal prose such as "run pytest" or "run npm build". The project must expose stable script entry points.

## Agent Verification Loop

Agents should run the narrowest useful checks while iterating.

Before declaring completion, agents should run `./scripts/validate.sh` when feasible. If full validation cannot be run, agents must state exactly what was and was not verified.

## Example Files

The following files are examples from a working FastAPI plus Vue project. New projects should adapt these patterns to their exact stack while preserving the same stable entry points and failure behavior.

### `api/requirements.txt`

Example source: `./goals/api/requirements.txt`

```text
alembic
bcrypt
cryptography
fastapi
flake8
httpx
mypy
pillow
psycopg2-binary
pydantic
pydantic-settings
pytest
python-multipart
ruff
sqlalchemy
types-PyYAML
uvicorn
```

### `api/test.sh`

Example source: `./goals/api/test.sh`

```bash
#!/bin/bash

set -eu -o pipefail

cd "$(dirname "$0")"

REQ_HASH="$(sha256sum requirements.txt pyproject.toml | sha256sum | awk '{print $1}')"
DIGEST_FILE=".venv-requirements.sha256"

if [ ! -d venv ]; then
  python3 -m venv venv
fi

source venv/bin/activate

if ! python3 - <<'PY'
import importlib.util
import sys

required_modules = [
    "alembic",
    "fastapi",
    "pydantic_settings",
    "pytest",
    "sqlalchemy",
]

missing = [module for module in required_modules if importlib.util.find_spec(module) is None]
sys.exit(1 if missing else 0)
PY
then
  python3 -m pip install -r requirements.txt
  printf '%s\n' "${REQ_HASH}" > "${DIGEST_FILE}"
elif [ ! -f "${DIGEST_FILE}" ]; then
  printf '%s\n' "${REQ_HASH}" > "${DIGEST_FILE}"
elif [ "${REQ_HASH}" != "$(cat "${DIGEST_FILE}")" ]; then
  python3 -m pip install -r requirements.txt
  printf '%s\n' "${REQ_HASH}" > "${DIGEST_FILE}"
fi

python3 -m pytest
```

### `api/lint.sh`

Example source: `./goals/api/lint.sh`

```bash
#!/bin/bash

set -eu -o pipefail

cd "$(dirname "$0")"

REQ_HASH="$(sha256sum requirements.txt pyproject.toml | sha256sum | awk '{print $1}')"
DIGEST_FILE=".venv-requirements.sha256"

if [ ! -d venv ]; then
  python3 -m venv venv
fi

source venv/bin/activate

if ! python3 - <<'PY'
import importlib.util
import sys

required_modules = [
    "alembic",
    "fastapi",
    "flake8",
    "mypy",
    "pydantic_settings",
    "pytest",
    "ruff",
    "sqlalchemy",
]

missing = [module for module in required_modules if importlib.util.find_spec(module) is None]
sys.exit(1 if missing else 0)
PY
then
  python3 -m pip install -r requirements.txt
  printf '%s\n' "${REQ_HASH}" > "${DIGEST_FILE}"
elif [ ! -f "${DIGEST_FILE}" ]; then
  printf '%s\n' "${REQ_HASH}" > "${DIGEST_FILE}"
elif [ "${REQ_HASH}" != "$(cat "${DIGEST_FILE}")" ]; then
  python3 -m pip install -r requirements.txt
  printf '%s\n' "${REQ_HASH}" > "${DIGEST_FILE}"
fi

python3 -m mypy app tests
python3 -m flake8 app tests
python3 -m ruff check app tests
python3 -m ruff format --check app tests
```

### `web/build.sh`

Example source: `./goals/web/build.sh`

```bash
#!/bin/bash

set -eu -o pipefail

cd "$(dirname "$0")"

DIGEST_SOURCE="package-lock.json"
if [ ! -f "${DIGEST_SOURCE}" ]; then
  DIGEST_SOURCE="package.json"
fi

REQ_HASH="$(sha256sum ${DIGEST_SOURCE} | sha256sum | awk '{print $1}')"
DIGEST_FILE=".node-deps.sha256"
NEEDS_INSTALL=0

if [ ! -d node_modules ]; then
  NEEDS_INSTALL=1
elif [ ! -f "${DIGEST_FILE}" ]; then
  printf '%s\n' "${REQ_HASH}" > "${DIGEST_FILE}"
elif [ "${REQ_HASH}" != "$(cat "${DIGEST_FILE}")" ]; then
  if [ -w node_modules ]; then
    NEEDS_INSTALL=1
  else
    printf 'Skipping npm install because node_modules is not writable; using existing dependencies.\n' >&2
    printf '%s\n' "${REQ_HASH}" > "${DIGEST_FILE}"
  fi
fi

if [ "${NEEDS_INSTALL}" -eq 1 ]; then
  npm install
  printf '%s\n' "${REQ_HASH}" > "${DIGEST_FILE}"
fi

npm run build
```

### `scripts/validate.sh`

Example source: `./goals/scripts/validate.sh`

```bash
#!/bin/bash

set -eu -o pipefail

ROOT_DIR="$(cd "$(dirname "$0")/.." && pwd)"

"${ROOT_DIR}/api/lint.sh"
"${ROOT_DIR}/api/test.sh"
"${ROOT_DIR}/web/test.sh"
"${ROOT_DIR}/web/build.sh"
```
