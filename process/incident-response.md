# Incident Response Playbook

## What counts as an incident
- Platform down or returning errors for users
- Data not being written / data loss
- Auth tokens expired affecting multiple users
- Build pipeline failure on stable channel

## Steps
1. **Detect** — Ops monitors logs and error rates. Sup flags tickets marked `critical`.
2. **Verify** — Confirm the scope: one user or many? One feature or systemic?
3. **Notify** — Alert Nano immediately. Nano loops in Glenn if high severity.
4. **Contain** — Identify workaround or rollback option.
5. **Communicate** — Sup sends customer acknowledgment email within 15 min of confirmed incident.
6. **Resolve** — Fix agent or relevant engineer implements fix.
7. **Close** — Update GitHub issue, notify customer of resolution, post to Rhapp Builds if build-related.

## Recent examples
- Aug 1–7, 2026: Google OAuth token expiry broke comp app sheet writes (RSC-1020 / project #124)
