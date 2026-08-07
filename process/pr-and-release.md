# PR & Release Process

## Branch strategy
All four core repos (vm, flcc, actions, rhythm) use trunk-based development — one `main` branch per repo. Short-lived PR branches for all changes; no permanent channel branches.

## PR rules
- All PRs targeting `main` must have `gschleic` explicitly added as a reviewer via the GitHub API before requesting merge.
- Use a comment ping (`@gschleic ready for your merge`) rather than a formal review request to avoid mobile queue buildup.
- PRs must pass QA (smoke at minimum) before merge.

## Release promotion
Artifacts are promoted through channels via `channels.json` in the release repo:
`edge` → `preview` → `stable`

Tag `main` in each source repo at each promotion (e.g. `git tag v0.7.3.32`) for traceability.

## Hotfixes
Branch from the promoted tag: `hotfix/<description>`. Patch, re-tag, merge back to `main`.

## Nightly builds
See nightly task schedule in the release repo. All three builds run off `main`, post to Rhapp Builds channel, publish to CDN, and generate `CHANGES.md`.
