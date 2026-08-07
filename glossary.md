# Rhappsody Glossary

Canonical terms used across all agents and repos.

## Build types
- **actions build** — nightly RAF bundle build from `rhapp-platform/actions` → release.rhapp.cc/nightly/DATE/actions/
- **core build** — nightly VM bundle build from `rhapp-platform/vm` + `decode` → release.rhapp.cc/nightly/DATE/core/
- **compiler build** — nightly WASM compiler build from `rhapp-platform/flcc` → release.rhapp.cc/nightly/DATE/wasm/

## Release channels
- **edge** — latest nightly, auto-promoted
- **preview** — manually promoted from edge, pre-release
- **stable** — production, manually promoted from preview

## Testing levels
- **smoke** — single happy path, ~2 min
- **regression** — full suite + adjacent checks
- **full** — regression + edge cases + visual

## Testing units
- **vm** — VM runtime (TypeScript, repos/vm)
- **css** — Rhythm CSS (repos/rhythm)
- **compiler** — flcc WASM compiler (repos/flcc)
- **action** — single action set
- **RAF** — Rhappsody Action Framework bundle
- **runtime** — vm + css + RAF composite

## Agents
See `roles/` for full role descriptions.

## Other terms
- **rhepl** — Rhappsody REPL, used for action testing
- **rhun** — structured .rh test suite runner (pending install)
- **R2** — Cloudflare R2 object storage used for build artifacts
- **RAF.esm.js** — compiled action framework bundle
