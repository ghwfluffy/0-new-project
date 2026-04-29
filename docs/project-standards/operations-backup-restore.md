# Operations, Backup, And Restore

## Runtime Components

Use Docker Compose at the repository root as the primary operational entry point.

Default services:

- `db`: PostgreSQL.
- `api`: FastAPI application.
- `web`: Vite dev server for hot-reload development.
- `nginx`: production-style static frontend server and API reverse proxy.
- `db-backup`: periodic PostgreSQL backup service writing to mounted backup storage.

Development expectations:

- Source directories are mounted into containers.
- Backend reload uses `uvicorn --reload`.
- Frontend reload uses the Vite dev server.
- The API runs Alembic migrations at container startup in development.
- Nginx serves built frontend assets and proxies only API requests in production-style local runs.

PostgreSQL 18+ containers should mount the named volume at `/var/lib/postgresql`, not the older `/var/lib/postgresql/data` layout.

## Docker Compose Skeleton

Start new projects from this service shape and adjust names/ports only when the project description requires it:

```yaml
services:
  db:
    image: postgres:18
    restart: unless-stopped
    env_file:
      - .env
    environment:
      POSTGRES_USER: ${POSTGRES_USER}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
      POSTGRES_DB: ${POSTGRES_DB}
    volumes:
      - postgres-data:/var/lib/postgresql
      - ./db/init.sql:/docker-entrypoint-initdb.d/init.sql:ro
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U ${POSTGRES_USER} -d ${POSTGRES_DB}"]
      interval: 1s
      timeout: 2s
      retries: 10
      start_period: 1s

  api:
    build:
      context: .
      dockerfile: api/Dockerfile
    restart: unless-stopped
    env_file:
      - .env
    environment:
      APP_ENV: ${APP_ENV:-development}
      APP_VERSION: ${APP_VERSION:-0.1.0}
      POSTGRES_HOST: db
      POSTGRES_PORT: 5432
    working_dir: /workspace/api
    command: sh -c "alembic upgrade head && uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload"
    volumes:
      - .:/workspace
    depends_on:
      db:
        condition: service_healthy

  db-backup:
    image: postgres:18
    restart: unless-stopped
    env_file:
      - .env
    environment:
      POSTGRES_HOST: db
      POSTGRES_PORT: 5432
      BACKUP_DIR: /backups
      BACKUP_UID: ${BACKUP_UID}
      BACKUP_GID: ${BACKUP_GID}
      BACKUP_INTERVAL_MINUTES: ${BACKUP_INTERVAL_MINUTES:-360}
    command: ["/bin/sh", "/opt/backup/backup-loop.sh"]
    volumes:
      - ./db:/opt/backup:ro
      - ./backups:/backups
    depends_on:
      db:
        condition: service_healthy

  web:
    build:
      context: .
      dockerfile: web/Dockerfile
    restart: unless-stopped
    environment:
      VITE_API_BASE_URL: ${VITE_API_BASE_URL:-/api/v1}
      VITE_API_PROXY_TARGET: ${VITE_API_PROXY_TARGET:-http://api:8000}
      VITE_APP_BASE_PATH: ${VITE_APP_BASE_PATH:-}
    working_dir: /workspace/web
    command: sh -c "npm install && npm run dev -- --host 0.0.0.0 --port 8081"
    volumes:
      - .:/workspace
    depends_on:
      - api
    ports:
      - "${BIND_ADDRESS-0.0.0.0}:8081:8081"

  nginx:
    build:
      context: .
      dockerfile: nginx/Dockerfile
      args:
        VITE_API_BASE_URL: ${VITE_API_BASE_URL:-/api/v1}
        VITE_APP_BASE_PATH: ${VITE_APP_BASE_PATH:-}
    restart: unless-stopped
    depends_on:
      - api
    ports:
      - "${BIND_ADDRESS-0.0.0.0}:8082:80"

volumes:
  postgres-data:
```

Production deployments may add an ACME/certificate service if this stack owns TLS. If an outer proxy owns TLS, keep this stack HTTP-only internally.

## Environment Configuration

Keep deploy-time configuration in `.env` and provide `.env.example`.

Expected variables:

```text
POSTGRES_USER=
POSTGRES_PASSWORD=
POSTGRES_DB=
POSTGRES_HOST=
POSTGRES_PORT=

APP_ENV=
APP_VERSION=
APP_BASE_PATH=
PUBLIC_URL=
BIND_ADDRESS=

SESSION_KEY=
SESSION_COOKIE_NAME=
SESSION_DURATION_MINUTES=
AUTH_MAX_FAILED_ATTEMPTS=
AUTH_LOCKOUT_MINUTES=

BACKUP_DIR=
BACKUP_UID=
BACKUP_GID=
BACKUP_INTERVAL_MINUTES=
BACKUP_COMMAND_TIMEOUT_SECONDS=

VITE_API_BASE_URL=
VITE_API_PROXY_TARGET=
VITE_APP_BASE_PATH=
```

`PUBLIC_URL` should be the scheme and host, without an ingress subpath. `APP_BASE_PATH` and `VITE_APP_BASE_PATH` represent the ingress subpath when deployed behind a path prefix.

## Behind-Proxy And Base-Path Support

Applications must support deployment behind an outer ingress path such as `/myapp`.

Expected behavior:

- Vue Router and built asset URLs resolve under the base path.
- Frontend API calls resolve under the same base path unless an explicit external API URL is configured.
- Vite dev server rewrites base-path API requests to backend `/api/*`.
- Backend-generated public links include the base path.
- The outer ingress strips the base path before forwarding to this Compose stack.
- Do not configure both the outer ingress and inner Nginx to independently apply the same prefix.

## Automated Backups

Run backups as a Compose-managed `db-backup` service using a PostgreSQL container.

Default behavior:

- Backups are written under `./backups`.
- Backup interval is configured by `BACKUP_INTERVAL_MINUTES`, defaulting to 360 minutes.
- `pg_dump --format custom` creates dump artifacts.
- Each backup has a manifest JSON with storage key, filename, created timestamp, source, size, and SHA-256.
- Backup files and manifests use configured `BACKUP_UID` and `BACKUP_GID` ownership.
- Automatic backups use source `automatic`.
- On-demand backups use source `manual` or another explicit source.

Application behavior:

- Persist backup metadata in a `backup_records` style table.
- Admins can list known backups.
- Admins can trigger an on-demand backup.
- Backup records include storage key, filename/path reference, trigger source, file size, checksum, actor when user-triggered, and timestamps.

## Restore

Restore is high-risk. Implement simple full-instance restore first unless the project explicitly approves granular restore.

Restore must be:

- administrator-only
- explicitly confirmed in the UI
- server-side validated
- modeled with a `restore_operations` style table
- visible to administrators through status/history
- auditable

Prefer creating a fresh pre-restore backup before executing restore.

## Manual Backup And Restore

The admin UI should be the normal path for on-demand backups and controlled restore requests. Also document emergency shell procedures for operators.

Manual backup command pattern:

```bash
docker compose exec db-backup /opt/backup/create_backup.sh manual
```

Manual restore procedure:

1. Identify the target backup manifest and verify its SHA-256 against the dump file.
2. Create a fresh pre-restore backup.
3. Stop application writers such as `api`, `web`, and `db-backup`.
4. Restore with `pg_restore` from a PostgreSQL client container that can reach the database.
5. Restart services and verify `/healthz`, `/api/v1/status`, login, and the main domain workflow.
6. Record what happened in restore-operation history or operational notes if the app database was restored outside the normal UI path.

Command pattern for emergency full-instance restore:

```bash
docker compose stop api web db-backup
docker compose run --rm db-backup sh -c 'PGPASSWORD="$POSTGRES_PASSWORD" pg_restore --host "$POSTGRES_HOST" --port "$POSTGRES_PORT" --username "$POSTGRES_USER" --dbname "$POSTGRES_DB" --clean --if-exists --no-owner "/backups/<backup-file>.dump"'
docker compose up -d
```
