# Example YORIENT.md

A fully worked example of a `YORIENT.md` for a hypothetical Node/TypeScript monorepo. Use it as a calibration target for length, density, and voice.

The repo this example describes does not exist; it is a representative shape. Treat structure and tone as the lesson, not the specific names.

---

```md
# YORIENT.md

## Purpose

`acme` is a TypeScript monorepo holding two user-facing apps (`web`, `mobile`),
three shared packages (`ui`, `api-client`, `config`), and the public REST API
in `services/api`. Builds run on pnpm + Turbo. Tests run on Vitest.

## Start Here

- Touching a UI component used in both apps -> `packages/ui/src/` first; check
  `apps/web` and `apps/mobile` for adoption before changing exports.
- Adding a REST endpoint -> `services/api/src/routes/`, then update
  `packages/api-client/src/` so callers stay typed.
- Bumping a dependency -> root `package.json` and `pnpm-lock.yaml`; never edit
  per-package lockfile fragments by hand.
- Investigating a CI failure -> `.github/workflows/ci.yml` + most recent run in
  the Actions tab; logs are clearer than re-running locally.

## Tree Map

```text
acme/
|___apps/
|   |___web/                 # Next.js app (App Router)
|   |   |___app/             # routes
|   |   |___components/      # web-only components
|   |___mobile/              # Expo React Native app
|       |___app/             # expo-router routes
|       |___components/      # mobile-only components
|___packages/
|   |___ui/                  # shared design-system components
|   |___api-client/          # typed fetch client for services/api
|   |___config/              # eslint/tsconfig/tailwind presets
|___services/
|   |___api/                 # Fastify REST API
|       |___src/routes/      # one file per resource
|       |___src/db/          # Drizzle schema + migrations
|___docs/                    # human-facing docs (mdx)
|___.github/workflows/       # CI definitions
|___turbo.json               # task graph
|___pnpm-workspace.yaml      # workspace globs
```

## Routing Map

| If you need to... | Start at | Then check | Avoid |
|---|---|---|---|
| Add a UI component shared by both apps | `packages/ui/src/` | `packages/ui/src/index.ts` exports, both apps' imports | duplicating into `apps/*/components/` |
| Add a REST endpoint | `services/api/src/routes/` | `packages/api-client/src/`, `services/api/src/db/schema.ts` | mutating `db/migrations/*.sql` directly |
| Change auth | `services/api/src/auth/` | `packages/api-client/src/auth.ts`, `apps/web/middleware.ts` | secrets, `.env*` files, prod configs |
| Adjust a build/test command | `turbo.json`, target package's `package.json` | root `package.json` scripts | per-package custom runners |
| Bump a dep | root `package.json` | `pnpm-lock.yaml`, peer constraints in `packages/*/package.json` | hand-editing the lockfile |
| Add a Tailwind utility | `packages/config/tailwind/` | both apps' `tailwind.config.ts` | per-app forks of preset values |

## Commands

- Install: `pnpm install`
- Build everything: `pnpm build` (Turbo, cached)
- Dev (parallel): `pnpm dev`
- Test: `pnpm test` (Vitest, all workspaces)
- Lint: `pnpm lint`
- Typecheck: `pnpm typecheck`
- DB migration (api): `pnpm --filter @acme/api migrate:dev`

## Conventions

- Package manager: pnpm 9.x (root `packageManager` field is authoritative).
- Language: TypeScript strict mode in every package.
- Test placement: colocated `*.test.ts` next to source files.
- Imports across workspace packages: use the workspace alias (`@acme/ui`), never deep relative paths.
- React components: function components + hooks; class components are not allowed.
- Commit style: Conventional Commits enforced via `commitlint`.

## No-Go / High-Risk Areas

- `services/api/src/db/migrations/` — append-only; never edit historical migrations.
- `.env*` and `services/api/secrets/` — secrets, never committed in cleartext.
- `pnpm-lock.yaml` — generated; only modify via `pnpm install` or `pnpm update`.
- `apps/mobile/ios/` and `apps/mobile/android/` — Expo prebuild output; do not hand-edit.
- `.turbo/` and `node_modules/` — caches; safe to delete, never to commit.

## Subsystem Notes

- `services/api/` has its own `YORIENT.md` covering DB schema layout and route conventions.
- `packages/ui/` has a Storybook config; component changes should land with a story update.

## Maintenance

- Update this file when top-level directories, workspace globs, or root commands change.
- Run the drift check from the `yorient` skill quarterly or after large refactors.

Last reviewed: 2026-05-25.
```

---

## What to imitate

- **Lead with Purpose, then Start Here.** Agents skim the first two sections to decide where to read next.
- **Tree Map is approximate, not exhaustive.** Show top three levels at most; let nested `YORIENT.md` files cover deeper structure.
- **Routing Map is the workhorse.** Five to eight rows of "if you need X, start at Y" beats any prose explanation.
- **Commands are verified, not guessed.** If a command is unconfirmed, label it `(unverified)` or omit it.
- **No-Go is concrete.** Name actual directories and files, not categories.
- **Maintenance has a date.** `Last reviewed:` makes drift visible at a glance.

## What to avoid

- Marketing prose ("the elegant monorepo for modern teams").
- Architecture diagrams in ASCII — keep them in `docs/`.
- Per-file documentation; that belongs in JSDoc/Tsdoc or `docs/`.
- Restating things every contributor already knows about the language ("TypeScript is a superset of JavaScript").
- Linking out to issue trackers or chat — `YORIENT.md` lives with the code, links rot.
