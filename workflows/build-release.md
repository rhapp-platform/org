# Workflow: Build & Release

## Trigger
Weekly release cycle, or Glenn initiating a manual release.

## Steps

| Step | Owner | Action |
|------|-------|--------|
| 1. Version bump | Bar | `just bump-build` (or `bump-patch`/`bump-minor` for feature/fix batches) |
| 2. Build | Bar | `just weekly-release v$VERSION` — propagates version, compiles, collects artifacts |
| 3. QA gate | QA | Runs regression against built `dist/v$VERSION/`; posts results |
| 4. Fix loop (if needed) | Fix / Dev | Resolves any QA failures; Bar rebuilds |
| 5. Deploy to edge | Bar | `just deploy` — uploads to R2, updates `channels.json`, purges CDN |
| 6. Edge soak | QA / Glenn | Glenn reviews edge; QA runs smoke if needed |
| 7. Promote to preview | Glenn | `release promote v$VERSION preview` |
| 8. Preview soak | QA | Regression pass on preview channel |
| 9. Promote to stable | Glenn | `release promote v$VERSION stable` — also redeploys Workers |
| 10. GitHub release | Bar | `just create-github-release v$VERSION` |
| 11. Announce | Nano / Mark | Build posted to Rhapp Builds channel; Mark handles external comms if warranted |

## Handoff conditions

- **Bar → QA**: Build complete, artifacts in `dist/v$VERSION/`; Bar posts build summary
- **QA → Bar (pass)**: Regression green; Bar proceeds to deploy
- **QA → Fix (fail)**: QA posts failures; Fix resolves; Bar rebuilds
- **Bar → Glenn**: Edge deployed; Glenn link for review
- **Glenn → Bar**: Promotion decisions at each channel step
- **Bar → Nano/Mark**: Stable promoted; announce

## Nightly builds (automated, no Glenn input needed)

- `nightly-actions-build` — 2:30am ET from `actions` main
- `nightly-core-build` — 3:00am ET from `vm` + `decode` main
- Both post to Rhapp Builds and Rhapp Test Results Telegram channels
- Do not touch channels — validation only

## Notes
- WASM: use `--force-wasm` for stable releases; `--skip-wasm` for fast JS-only iteration
- All promoted versions get annotated git tags on `release` + 4 core repos
- See `process/release.md` for full versioning and channel details
- Pending: QA gate recipe to be wired into `just weekly-release` (rhapp-platform/release #17)
