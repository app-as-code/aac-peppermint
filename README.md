<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="assets/icon-dark.svg">
    <img src="assets/icon.svg" alt="Peppermint logo" width="128">
  </picture>
</p>

<h1 align="center">aac-peppermint</h1>

<p align="center">Self-hosted deployment definition for Peppermint, an open source issue management and help desk tool.</p>

<p align="center">
  <a href="https://github.com/Peppermint-Lab/peppermint"><img alt="Upstream" src="https://img.shields.io/badge/upstream-Peppermint--Lab%2Fpeppermint-blue?logo=github"></a>
  <img alt="Image" src="https://img.shields.io/badge/image-pepperlabs%2Fpeppermint%3Anightly-2496ED?logo=docker&logoColor=white">
  <a href="https://github.com/Peppermint-Lab/peppermint"><img alt="License" src="https://img.shields.io/badge/license-see%20upstream-lightgrey"></a>
</p>

## About

Peppermint is a self-hosted ticket and help desk system. It is positioned as a simple
alternative to larger ticketing suites and offers ticket creation with a markdown editor and
file uploads, a client history log, and a markdown notebook with to-do lists. The web
interface is responsive and can be used from mobile devices up to large desktop screens.

Key features (as described by upstream):

- Ticket creation with a markdown editor and file uploads
- Client history log
- Markdown notebook with to-do lists
- Responsive layout for screen sizes from mobile to 4K
- Deployable with Docker or pm2

## Upstream

- Project: <https://github.com/Peppermint-Lab/peppermint>
- Website: <https://peppermint.sh>
- Container image: `pepperlabs/peppermint:nightly` (Docker Hub)
- Database: `postgres:18` (deployed as `aac-peppermint-postgres`)
- License: see the upstream repository

## Quick start (Docker Compose)

The Compose file is rendered from `service.yml` by the App-as-Code template engine. For a
manual run, the external networks used for routing must exist first:

```bash
docker network create services-secured
docker network create services-exposed

# Provide the values listed under "Configuration" in .env / stack.env
docker compose up -d
```

Secrets are not stored in this repository. Provide them through the deployment pipeline
or create the `.env` and `stack.env` files yourself (see below). Do not commit them.

## Configuration

Environment variables (defaults from `service.yml`; `CHANGE_ME` must be set per deployment):

| Variable | Default | Description |
|---|---|---|
| `DB_PASSWORD` | `CHANGE_ME` (secret) | Password of the database user used by the application |
| `SECRET` | `CHANGE_ME` (secret) | Application secret |
| `DATABASE_URL` | `CHANGE_ME` | Database connection string used by the application |
| `DB_HOST` | `CHANGE_ME` | Database host name |
| `DB_PORT` | `5432` | Database port |
| `DB_USERNAME` | `peppermint` | Database user name |
| `DB_NAME` | `peppermint` | Database name |
| `POSTGRES_USER` | `peppermint` | User for the `postgres` dependency |
| `POSTGRES_PASSWORD` | `CHANGE_ME` (secret) | Password for the `postgres` dependency |
| `POSTGRES_DB` | `peppermint` | Database created by the `postgres` dependency |
| `APP_URL` | `CHANGE_ME` | Public URL of the web interface |
| `API_URL` | `CHANGE_ME` | Public URL of the API |
| `HOST` | `CHANGE_ME` | Host name the application binds to or advertises |
| `PUID` / `PGID` | `1000` | User and group ID the container runs as |
| `TZ` | `Europe/Berlin` | Time zone |

Ports:

| Port | Protocol | Purpose |
|---|---|---|
| `3000` | TCP | Web interface (`web`, routed via Traefik) |
| `5003` | TCP | API (`api`) |

Volumes:

| Mount | Description |
|---|---|
| `config` -> `/config` | Peppermint application configuration |
| `db` -> `/var/lib/postgresql` | PostgreSQL data (database service) |

## Deploy with App-as-Code

1. Edit `service.yml`. It is the single source of truth for image, ports, environment,
   volumes, dependencies and per-stage overrides. Do not edit the rendered output.
2. Pushing to `main` (or a configured stage branch) runs the service pipeline, which renders
   the manifests with `aac-template-engine` and triggers the deployment.
3. Releases of this definition are published under <https://github.com/app-as-code/aac-peppermint>.

## Credits and license

Peppermint is developed by the Peppermint project. This repository only contains the
deployment definition for it and is not a fork of the application. The upstream license
applies to the software; see the upstream repository. The logo is provided by the
Peppermint project; see [`assets/ICON_SOURCE.md`](assets/ICON_SOURCE.md) for icon sources and
licenses.
