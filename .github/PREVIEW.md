# Preview & CI

This repository is a **Node.js / TypeScript CLI** orchestration tool (`claude-flow` / `ruflo`).
There is **no previewable web/app artifact** — no frontend (`vite`/`next`/CRA), no `app.json`/Expo
config, and no `android/` directory. The meaningful "preview" for a CLI is proving that it
**installs, type-checks, compiles, and tests** cleanly.

## Tiers in this repo

| Tier | Workflow | What it proves |
|------|----------|----------------|
| Test/Build CI | `.github/workflows/ci.yml` ("CI") | `npm install` → `npm run build` (tsc) → `npx vitest run`. Verifies the package compiles and tests pass. |

## Triggers

- Push to `main` or `claude/repo-organization-artifacts-tnwgtb`
- Pull requests targeting `main`
- Manual `workflow_dispatch`

## Notes & caveats

- **No live preview / deploy.** A CLI tool has no served artifact, so there is no GitHub Pages
  deploy and no base-path configuration.
- **Tests run non-interactively** via `npx vitest run` (the repo's `test` script is bare `vitest`,
  which defaults to watch mode in a TTY and would hang CI). The build step (`npm run build` → `tsc`)
  is the primary gate; the test step is best-effort and surfaces a warning rather than failing the
  run, because this monorepo snapshot may carry partial/in-progress suites.
- **No secrets required to build or test.** Runtime usage of the CLI would want an
  `ANTHROPIC_API_KEY` (and optionally other provider keys), but none are needed to compile or test,
  so this pipeline uses **zero secrets**.
- **Monorepo note.** The primary CLI source also lives under `v3/@claude-flow/cli`, which has its
  own `build`/`lint` scripts. The root `npm run build` covers the root `tsconfig.json`; extend the
  workflow with a matrix or extra steps if you want to gate the nested package independently.
