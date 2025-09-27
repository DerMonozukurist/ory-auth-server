# Ory Auth Server
This project provides a ready-to-run authentication and authorization server. With a single command, it spins up a fully integrated environment that lets you build, prototype, and test identity, login, consent, and token-based access flows end-to-end without needing to wire up cloud services or bespoke infrastructure.

## Installation
- Clone the project (or require it via Composer if you’re integrating with another project).
- Ensure the environment file is set:
    - A default `.env` file is provided in the project root; review and adjust values (especially secrets) before starting.

## Quick Start
1. Adjust secrets in `.env` for local use (do not use defaults in production).
2. Start the stack:
    - Using Docker Compose:
        - docker compose up -d
3. Access the services:
    - Kratos Public API: http://localhost:4433
    - Kratos Admin API: http://localhost:4434
    - Kratos Self-service UI: http://localhost:4435
    - Hydra Public API (OAuth2/OIDC): http://localhost:4444
    - Hydra Admin API: http://localhost:4445
    - Postgres: localhost:5432

### Data persistence:
- By default, PostgreSQL data persists under `./var/postgres`

## Configuration
- Central configuration is via the `.env` file in the project root. Key groups:
    - PostgreSQL: `OAS_POSTGRES_*`
    - Kratos: `OAS_KRATOS_*` and UI-related secrets (`OAS_UI_*`)
    - Hydra: `OAS_HYDRA_*`
- Service configs are mounted from:
    - `config/ory-auth-server/kratos/kratos.yml`
    - `config/ory-auth-server/kratos/identity.schema.json`
    - `config/ory-auth-server/hydra/hydra.yml`

You can customize ports, log levels, and service URLs through `.env`. The Compose file reads those values when starting the containers.

### Docker compose override
- The suggested way to override the default docker compose configuration is to create a `docker-compose.override.yaml` file in the project root.

## Verify
1. Check if all containers are running:
   ```shell
   $ docker ps
   ```
   All services should show (`postgres`, `kratos`, `hydra`, `kratos-selfservice-ui`) should show **"Up"** status.
2. Verify Kratos API is responding
   ```shell
   $ curl http://localhost:4433/health/alive
   {"status":"ok"}
   ```
3. Verify Hydra API is responding
   ```shell
   $ curl http://localhost:4444/health/alive
   {"status":"ok"}
   ```
4. Check that OAuth 2.0 discovery works
   ```shell
   curl http://localhost:4444/.well-known/openid-configuration
   ```
   You should see a JSON response suggesting that the server is running.
5. Test Kratos Self-Service UI (browser)\
   In a browser, navigate to http://localhost:4435. You should see a page indicating that everything is working correctly.

## Common Tasks
- Start services: docker compose up -d
- Viewing logs: 
  - `docker compose logs -f kratos`
  - `docker compose logs -f hydra`
  - `docker compose logs -f postgres`
  - `docker compose logs -f` (all services)
- Stop services: `docker compose down`
- Recreate (fresh DB): `docker compose down -v` (removes volumes, including Postgres data)

## Security Notes
- Change all secrets in `.env` before use (e.g., OAS_HYDRA_SECRETS_SYSTEM, OAS_UI_* secrets).
- The stack runs with development defaults and should not be used as-is in production.

## License
MIT License. See [LICENSE.txt](./LICENSE.txt) for details.