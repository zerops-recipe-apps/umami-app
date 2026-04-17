# umami-app

Zerops recipe wrapper that clones and builds upstream [umami-software/umami](https://github.com/umami-software/umami) (Next.js analytics) from source at build time, backed by a sibling PostgreSQL service.

## Zerops service facts

- HTTP ports: `3000` (`httpSupport: true`)
- Siblings: `db` (PostgreSQL) — env: `DATABASE_URL` composed from `${db_connectionString}/${db_dbName}`
- Runtime base: `nodejs@22`

## Zerops

No dev iteration loop — the app clones upstream at build time. Changes in this repo only affect `zerops.yaml` (clone target, build wrapper, deploy file list). Each change requires a full build+deploy through the **Zerops development workflow via `zcp` MCP tools**.

## Notes

- Upstream pinned via `UMAMI_RELEASE_TAG` (default `v3.0.1`); override at the project level with `RUNTIME_UMAMI_RELEASE_TAG`.
- Build clones `umami-software/umami` with `--single-branch`, runs `pnpm install --frozen-lockfile && pnpm build`, and deploys the Next.js `standalone` output plus `public/`, `.next/static/`, `prisma/`, `scripts/`, `generated/`.
- This repo contains no application source — it is a pure build wrapper.
