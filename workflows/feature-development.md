# Workflow: Feature Development

## Trigger
Glenn has a new feature idea, or a feature request from a client/user has been approved for development.

## Steps

| Step | Owner | Action |
|------|-------|--------|
| 1. Idea capture | Glenn | Glenn describes the feature to Nano (scope, motivation, any constraints) |
| 2. Scoping | Arch | Arch produces a design proposal: affected components, approach options, tradeoffs, estimated complexity |
| 3. Design review | Glenn | Glenn reviews Arch's proposal; approves or redirects |
| 4. Issue creation | Arch / Nano | Arch opens GitHub issue in `rhapp-platform/project` with: agreed design, acceptance criteria, label `agent:dev` (or appropriate agent) |
| 5. Implementation | Dev | Dev picks up the issue; implements per Arch's design; flags to Arch if scope changes emerge |
| 6. QA gate | QA | Dev requests QA test run (level depends on scope: smoke for small, regression for significant); QA posts results |
| 7. Fix loop (if needed) | Fix / Dev | Dev or Fix resolves failures; QA re-validates |
| 8. PR + merge | Glenn | Dev opens PR; posts `@gschleic ready for your merge`; Glenn merges |
| 9. Release | Bar | Feature included in next weekly release cycle per `workflows/build-release.md` |

## Handoff conditions

- **Glenn → Arch**: Feature idea with enough context to scope (motivation, any known constraints)
- **Arch → Glenn**: Design proposal with options and a recommendation; Glenn approves before work starts
- **Arch → Dev**: Approved design + GitHub issue with acceptance criteria
- **Dev → Arch**: If implementation reveals a design gap or scope change — Arch decides before Dev continues
- **Dev → QA**: Implementation complete; Dev has done a basic self-check
- **QA → Dev (pass)**: All requested levels green
- **QA → Dev/Fix (fail)**: Failures documented in PR comment
- **Dev → Glenn**: QA passed; PR ready for merge

## Notes
- No implementation starts without an approved Arch design for features of any real scope — this avoids rework
- Small/cosmetic changes (copy, minor UI tweaks) may skip the Arch scoping step at Glenn's discretion
- Arch's design proposal lives in the GitHub issue; significant decisions also get a file in `decisions/`
- See `process/pr-and-release.md` for PR reviewer and merge conventions
