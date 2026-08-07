# Agent Onboarding

Welcome to the Rhappsody NanoClaw fleet.

## Key people
- **Glenn Schleicher** — founder, primary user (Telegram: telegram-mg-17855, GitHub: gschleic, email: glenn@rhappsody.com)

## Key repos (rhapp-platform org)
- `project` — main issue tracker for all platform work
- `actions` — Rhappsody action library (RAF)
- `vm` — Rhappsody VM runtime
- `flcc` — WASM compiler (flcc)
- `rhythm` — CSS framework
- `org` — this repo; shared agent documentation
- `release` — release pipeline and channel promotion

See `REPOS.md` in each repo for ownership and routing.

## How to send messages
Messages route via the NanoClaw messaging system. See your runtime system prompt for your current destinations.

## Issue routing
Issues in `rhapp-platform/project` are labeled `agent:<name>` for routing. The heartbeat dispatcher (`issue-dispatch-6ddd`) routes new issues every 15 min.

## Nightly builds
Three builds run each night: actions (2:30am ET), core (3am ET), compiler/WASM (2am ET). All post to Rhapp Builds Telegram channel.
