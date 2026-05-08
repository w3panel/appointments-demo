# Appointments demo

A minimal [Payload CMS](https://payloadcms.com/) + [Next.js](https://nextjs.org/) application that demonstrates [`payload-appointments-plugin`](https://github.com/w3panel/payload-appointments-plugin) — scheduling, booking flows, and related admin UI.

This repository is normally pulled into the [`doctors-app`](https://github.com/w3panel/doctors-app) monorepo as a Git submodule at `apps/appointments-demo`.

## Prerequisites

- **Node.js** `^18.20.2` or `>=20.9.0`
- **PostgreSQL** — the app uses `@payloadcms/db-postgres`
- **Bun** (recommended for the parent monorepo) or another package manager if you adapt dependency pins yourself

## Environment

Copy `.env.example` to `.env` and set:

| Variable          | Required | Description |
|-------------------|----------|-------------|
| `DATABASE_URI`    | Yes      | Postgres connection string |
| `PAYLOAD_SECRET`  | Yes      | Secret for Payload sessions and crypto |
| `SMTP_USER`       | No       | If set with `SMTP_PASS`, uses iCloud SMTP for outbound mail |
| `SMTP_PASS`       | No       | Without SMTP creds, email uses JSON transport (fine for local dev) |

## Running inside `doctors-app`

From the monorepo root:

```bash
git submodule update --init --recursive
bun install
```

Start only this app:

```bash
cd apps/appointments-demo
bun run dev
```

Or from the root using the workspace package name:

```bash
bun run --filter appointments-demo dev
```

Payload admin is served alongside Next.js (see your Next.js app routes under `app/(payload)/`). Generate types and the admin import map after schema or plugin changes:

```bash
cd apps/appointments-demo
bun run generate:types
bun run generate:importmap
```

Lint/format scripts assume paths relative to the monorepo (`../../tsconfig.oxlint.json`). Run them from this directory while it lives under `apps/appointments-demo`.

## CI / production build

The default `build` script is a no-op in the monorepo (`skip (demo app)`). For a real build:

```bash
bun run build:demo
```

Use `bun run start` after a successful build.

## Standalone clone

This package declares `catalog:` dependencies and `payload-appointments-plugin` as `workspace:*`, which resolve when installed from the **parent** Bun workspace. If you clone **only** this repository, replace those with concrete semver ranges (or a git/npm dependency on `payload-appointments-plugin`) and duplicate any catalog versions you need in `package.json` before installing.

## License

MIT
