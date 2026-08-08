# Agent Dev Workspace

Setup reference for dev agents that need to run platform tools (`rhepl`, `rtest`, `syntax`, `genaction`, etc.).

> For platform architecture, Rhyme language docs, ADRs, and the full repo map, see `AGENTS.md` at the monorepo root (`rhapp-platform/repo-root`). This doc covers only what an agent container needs to set up a working dev workspace.

---

## Environment Variables

All three names point to the same root. Set whichever your toolchain expects; all three should be consistent.

```bash
export RHAPPSODY_ROOT=/workspace/rh
export XPDEV_ROOT=/workspace/rh
export r=/workspace/rh
```

Derived conventions (from `repo-root/AGENTS.md`):
- `$a` = `$r/actions/sets` — action catalog root
- `$t` = `$r/tools` — tools root

---

## Required Repos

Clone as direct children of `$r`. All are in `rhapp-platform/`.

| Repo | Purpose |
|------|---------|
| `actions` | RAF / action resolution |
| `components` | Full clone required; `components/rh-busy` is the critical dependency for rhepl |
| `flcc` | Rhyme compiler (alias: `compiler`) |
| `vm` | Rhappsody VM runtime |
| `decode` | VM dependency |
| `varmgr` | VM dependency |
| `rhdom` | VM dependency |
| `netrh` | VM dependency |
| `rhimg` | VM dependency |
| `rhepl` | Rhyme REPL — primary interactive dev tool |
| `rhun` | Rhyme runtime / app runner |
| `rhythm` | CSS framework |
| `rtest` | Test harness |
| `appcert` | Cert generation (pod) |
| `app-serving-core` | Cert delivery patterns |
| `quicktest` | Quick test tooling |
| `tools` | Platform tools (owner: Sys) |

### RAF files

`RAF.esm.ts`, `RAF.master.esm.ts`, and related RAF files live at `$r/` root — **not** inside a sub-repo. These must be present for action resolution to work.

---

## Directory Layout

```
/workspace/rh/                    ← $r / $RHAPPSODY_ROOT / $XPDEV_ROOT
├── RAF.esm.ts                    ← RAF entry point (root level, not in a repo)
├── RAF.master.esm.ts
├── actions/                      ← git repo: rhapp-platform/actions
│   └── sets/                     ← $a (action catalog)
├── app-serving-core/             ← git repo
├── appcert/                      ← git repo
├── components/                   ← git repo
│   └── rh-busy/                  ← critical rhepl dependency
├── decode/                       ← git repo (VM dep)
├── flcc/                         ← git repo (compiler)
├── netrh/                        ← git repo (VM dep)
├── quicktest/                    ← git repo
├── rhepl/                        ← git repo (REPL)
├── rhdom/                        ← git repo (VM dep)
├── rhimg/                        ← git repo (VM dep)
├── rhun/                         ← git repo
├── rhythm/                       ← git repo (CSS framework)
├── rtest/                        ← git repo (test harness)
├── tools/                        ← git repo ($t) — owner: Sys
├── varmgr/                       ← git repo (VM dep)
└── vm/                           ← git repo (VM runtime)
```

---

## Setup Script

Run once to clone the required repos into `$r`. Adjust `RHAPPSODY_ROOT` as needed.

```bash
#!/usr/bin/env bash
set -euo pipefail

export RHAPPSODY_ROOT="${RHAPPSODY_ROOT:-/workspace/rh}"
export XPDEV_ROOT="$RHAPPSODY_ROOT"
export r="$RHAPPSODY_ROOT"

ORG="https://github.com/rhapp-platform"

mkdir -p "$r"
cd "$r"

REPOS=(
  actions
  components
  flcc
  vm
  decode
  varmgr
  rhdom
  netrh
  rhimg
  rhepl
  rhun
  rhythm
  rtest
  appcert
  app-serving-core
  quicktest
  tools
)

for repo in "${REPOS[@]}"; do
  if [ -d "$r/$repo/.git" ]; then
    echo "→ $repo: already cloned, pulling"
    git -C "$r/$repo" pull --ff-only
  else
    echo "→ $repo: cloning"
    git clone "$ORG/$repo.git" "$r/$repo"
  fi
done

echo ""
echo "Workspace ready at $r"
echo "Remaining step: place RAF.esm.ts and RAF.master.esm.ts at $r/"
echo "  (pull from rhapp-platform/actions build artifacts or a known-good nightly)"
```

> **RAF files**: the setup script does not place `RAF.esm.ts` / `RAF.master.esm.ts` automatically because they are build outputs, not source repos. Pull them from the latest nightly (`nightly/latest/actions/`) or ask Bar to provide a current build.

---

## Verification

After setup, confirm the critical chain works:

```bash
# Compiler available
rhcc --version

# REPL launches
rhepl

# Action syntax lookup
syn .text

# Run a minimal app
echo '.text "hello"' | rhun -
```

If any of these fail, check that the relevant repo is cloned and that `$r` env vars are exported in your shell environment.

---

## Notes

- Each subdirectory under `$r` is an independent git repo. Run `git -C <dir> status` before making changes.
- See `qa/test-strategy.md` for test levels and known blockers (WASM binary, `rhun` install state).
- `tools` is owned by Sys — file issues or tool requests against `rhapp-platform/tools` labeled `agent:sys`.
