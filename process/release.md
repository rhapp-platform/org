# Release Process

Rhappsody ships through two independent pipelines — automated nightly builds
for continuous validation, and a manual/weekly pipeline for actual channel
promotion. Both are driven from `rhapp-platform/release`.

## Versioning

`MAJOR.MINOR.PATCH.BUILD` (e.g. `v0.7.3.32`). Single source of truth is
`release/package.json`; `just propagate-version` pushes it out to all
components at build time. Weekly builds bump BUILD (week number); PATCH/MINOR
are hand-bumped for real feature/fix batches; MAJOR is hand-bumped for
breaking changes.

## Channels

```
dev (live tunnel) → edge → preview → stable
```

- **dev** — served live from a dev machine's `dist/` via cloudflared tunnel. Not a snapshot.
- **edge / preview / stable** — deployed R2 snapshots. Current pointer + promotion history per channel lives in `channels.json`, updated by `just deploy` (edge) and `just promote <version> <channel>` (preview/stable).
- Promoting to **stable** also redeploys the embedded-stable Workers (`rhapp-app`, `rhapp-review`).

## Nightly Builds

Two nightly jobs validate that the platform builds clean off `main` HEAD — they do not touch channels.

- **Actions/RAF build** (`nightly-actions-build`) — action-set + chunk bundles from `actions`.
- **Core/VM build** (`nightly-core-build`) — all 5 VM/core bundles from `vm` + `decode`.

Both: `git pull` → build → publish to `nightly/<date>/` and `nightly/latest/` in R2 → generate `CHANGES.md` → notify Rhapp Builds + Rhapp Test Results Telegram channels.

## Weekly / Manual Release

```bash
just bump-build            # or bump-patch / bump-minor / bump-major
just weekly-release v0.7.3.33
```

`weekly-release` runs:
1. `generate-notes` — changelog from commits across source repos since last release
2. `build` — propagate-version → conditional WASM compile → build all components → collect into `dist/v$VERSION/`
3. `tag-release` — tag `release` + core repos (`vm`, `flcc`, `actions`, `rhythm`) at exact commit shipped
4. `deploy` — upload to `/v$VERSION/`, sync to `/edge/`, update `channels.json`, purge CDN

Then manually, after testing:

```bash
release promote v0.7.3.33 preview   # after edge looks good
release promote v0.7.3.33 stable    # after preview soaks
just create-github-release v0.7.3.33
```

### Conditional WASM compilation

| Flag | Behavior |
|---|---|
| *(default)* | rebuild if signal set, OR if `rhcc.wasm` missing/older than `OPDEFS.go` |
| `--skip-wasm` | rebuild only if signal set (fast JS-only iteration) |
| `--force-wasm` | always rebuild (use for stable releases) |

## Tagging

Every shipped version gets an annotated git tag (`v0.7.3.33`) on `rhapp-platform/release` and each of the 4 core repos (`vm`, `flcc`, `actions`, `rhythm`) at the exact commit that was checked out during the build. Tags are immutable — `channels.json` remains the system of record for what's live on each channel. No tag is written at promotion time.

## Major Releases

For a `MAJOR` bump (e.g. `v0.8.0.0`), cut a temporary `release/0.8` branch from `main` at feature freeze. Fix + test on that branch; cherry-pick critical fixes back to `main`. Tag and ship from the release branch, then delete it. As a solo dev with short freeze windows, you may skip the branch entirely — freeze `main`, stabilize, tag, ship — if you don't need to keep landing next-cycle work in parallel.

## Team Coordination

- **DevOps** — WASM rebuilds, CF Worker deploys. Not in the critical path for a normal weekly release.
- **QA** — test gates before promotion (especially preview → stable).
- **Dev** — code changes.

## Troubleshooting

- `just channels` — show current channel assignments
- `release show` — comprehensive status
- R2 `NotImplemented: 501` on `rclone sync` is a known transient — retries self-heal.
