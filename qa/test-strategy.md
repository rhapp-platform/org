# Test Strategy

## Testing Vocabulary

Two axes: **level** (how deep) × **unit** (what component). A test request is always a combination — e.g. "vm regression" or "runtime smoke".

### Levels

- `smoke` — Does the changed thing work at all? Single happy path, ~1–2 min. Use after a bug fix lands.
- `regression` — Full test suite + fix verification + adjacent behavior check. Use for PR review and before promoting a build.
- `full` — Regression + edge cases + all flows + visual verification. Use for release candidates and major platform changes.

### Units

- `vm` — VM runtime (TypeScript). Repo: repos/vm. Run: `bun test ./src`
- `css` — Rhythm CSS. Repo: repos/rhythm. Run: `bun run build && bun run test/rhythm-test.mjs`
- `compiler` — flcc WASM compiler (Go). Repo: repos/flcc. Requires Go toolchain + up-to-date WASM binary.
- `action` — Single action set (e.g. .text, .button, .var). Tested via rhepl with WASM binary.
- `RAF` — Rhappsody Action Framework bundle. Tested via rhepl integration. Artifact: repos/actions/RAF.esm.js
- `runtime` — Composite: vm + css + RAF together. Tested via rhepl end-to-end.

---

## Matrix — Runnable Today

✅ Runnable   ⚠️ Partial / limited   ❌ Blocked

| Unit \ Level | smoke | regression | full |
|---|---|---|---|
| vm | ✅ | ✅ | ⚠️ e2e tests hang (WASM init) |
| css | ✅ | ✅ | ✅ |
| compiler | ❌ | ❌ | ❌ |
| action | ⚠️ basic only | ⚠️ basic only | ❌ |
| RAF | ⚠️ via rhepl | ⚠️ via rhepl | ❌ |
| runtime | ⚠️ basic only | ⚠️ basic only | ❌ |

---

## Tool Requirements & Gaps

### vm

- Tools: Bun, linkedom, happy-dom (all installed)
- Scope: `bun test ./src` → 611 pass / 4 skip / 1 known fail (#85 datetime). Full e2e suite hangs at WASM init — scoped out of nightly.
- Gap: e2e tests (`__test__/`) blocked until rhepl WASM issue resolved.

### css

- Tools: Bun (installed)
- Scope: 50/50 tests pass. No gaps — fully runnable at all levels.

### compiler

- Tools needed: Go toolchain, fresh WASM binary from Niko
- 🚫 Gap: Go compiler not available in agent container. Fresh WASM binary pending from Niko (current binary is v0.6.6.1108, predates current RAF). Compiler tests fully blocked.

### action

- Tools: rhepl (Bun + WASM + RAF), syntax (installed)
- Partial: basic actions (.text, .title, .spacer) work. Complex actions (.var, .button) show "Unrecognized action" due to WASM version mismatch.
- ⚠️ Gap: Fresh WASM binary from Niko needed for full action coverage. Until then, only primitive actions are testable.

### RAF

- Tools: rhepl, pre-built RAF.esm.js (repos/actions/RAF.esm.js)
- Partial: RAF bundle loads and basic rendering works. No standalone RAF test suite exists — tested only through rhepl integration.
- ⚠️ Gap: rhun not yet installed (blocked by fw13 / monorepo dep). rhun is needed to run .rh action test suites against the RAF bundle.

### runtime

- Tools: rhepl (composite of vm + css + RAF). syntax (installed), rhun (pending)
- Partial: rhepl compiles and renders basic `.text "hello world"` → correct DOM output. Full action suite blocked by WASM version.
- 🚫 Gap: Fresh WASM binary from Niko unblocks runtime regression + full. rhun needed for structured test execution.

---

## Open Blockers

1. **Fresh WASM binary from Niko** — unblocks: compiler (all), action (full), RAF (full), runtime (full)
2. **rhun install** — unblocks: structured .rh test suite execution (action + RAF). Blocked by fw13 monorepo dep.
3. **Go toolchain in agent container** — unblocks: compiler tests in repos/flcc (Go test suite).
