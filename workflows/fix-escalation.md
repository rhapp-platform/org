# Workflow: Fix Escalation

## Trigger
A bug is confirmed — either via a GitHub issue labeled `bug`, a support ticket escalated by Sup, or a QA test failure.

## Steps

| Step | Owner | Action |
|------|-------|--------|
| 1. Intake | Fix | Receives bug report; assesses scope (isolated vs. systemic, severity) |
| 2. Triage | Fix | Classifies: implementation bug → Fix owns; design/architecture issue → escalate to Arch; security issue → loop Sec immediately |
| 3. Reproduce | Fix | Confirms repro in appropriate environment; documents steps in GitHub issue |
| 4. Fix + PR | Fix | Implements fix on a branch; requests QA smoke before merge |
| 5. QA validation | QA | Confirms fix resolves the issue; checks for regressions |
| 6. Merge | Glenn | Glenn merges after QA pass |
| 7. Customer close | Sup | If ticket originated from a customer, Sup sends resolution email |

## Escalation paths

| Condition | Escalate to | Action |
|-----------|-------------|--------|
| Root cause is architectural | Arch | Fix flags to Arch; Arch leads resolution with Fix assisting |
| Security vulnerability | Sec | Sec assesses immediately; may escalate to Glenn for go/no-go on disclosure |
| Fix requires product decision | Nano → Glenn | Fix flags to Nano; Glenn decides before Fix proceeds |
| Critical severity (platform down) | Nano immediately | Follows `process/incident-response.md` |

## Handoff conditions

- **Sup → Fix**: Confirmed bug (not a support question); GitHub issue opened with repro steps
- **Fix → Arch**: Fix cannot resolve without a design change; Arch receives issue + Fix's assessment
- **Fix → Sec**: Any potential security implication, however small
- **Fix → QA**: Branch ready, fix implemented, basic self-check done
- **QA → Sup**: Resolution confirmed; Sup notified to close customer ticket

## Notes
- Fix does not merge directly — all fixes go through Glenn
- For `critical` bugs, skip the normal queue: alert Nano before investigating
- See `support/tier-framework.md` for priority definitions that determine urgency
