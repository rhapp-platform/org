# Workflow: Support Triage

## Trigger
A customer ticket arrives via the support channel.

## Steps

| Step | Owner | Action |
|------|-------|--------|
| 1. Intake | Sup | Auto-ack email sent immediately on receipt |
| 2. Classify | Sup | Assigns category (`billing`, `technical`, `feature_request`, `other`) and priority (`critical`/`high`/`normal`/`low`) per `support/tier-framework.md` |
| 3. Route | Sup | Follows routing rules below |
| 4. Resolution | Fix / Dev / Glenn | Resolves the underlying issue |
| 5. Customer reply | Sup | Drafts and sends resolution email (BCC glenn@rhappsody.com per policy) |
| 6. Close | Sup | Closes ticket; logs resolution |

## Routing by category

### billing
1. Sup alerts Nano → Nano loops Glenn immediately
2. Sup holds customer reply until Glenn's direction
3. Glenn decides; Sup drafts reply based on Glenn's call

### technical — critical/high
1. Sup alerts Nano immediately (individual message, not batch)
2. Sup opens GitHub issue with `bug` label → pings Fix
3. If client-specific: Sup labels `agent:<owner>` and messages the relationship owner (e.g. Ren for `client-renner`)
4. Sup sends holding reply to customer: "We're on it"
5. Fix resolves → QA validates → Sup sends resolution email

### technical — normal/low
1. Sup includes in next batched triage summary to Nano
2. Sup opens GitHub issue if confirmed bug
3. Resolution follows standard fix-escalation workflow

### feature_request
1. Sup logs to `rhapp-platform/project` with `feature_request` label
2. Sup sends customer reply: "Thanks for the suggestion — we've noted it for a future update"
3. Included in next batch summary to Nano; Glenn reviews in sprint planning

### other
1. If test/internal: close silently
2. If unclear: Sup asks Nano before replying

## Handoff conditions

- **Customer → Sup**: Any inbound ticket; Sup owns classification
- **Sup → Fix**: Confirmed technical bug with repro steps
- **Sup → Nano**: Billing escalation, or critical/high technical issue
- **Fix → Sup**: Resolution confirmed; Sup owns customer communication
- **Nano → Glenn**: Any billing issue or critical severity

## Notes
- All customer emails sent from `nano@rhappsody.com`; BCC `glenn@rhappsody.com` on every outbound
- SLA targets in `support/tier-framework.md`
- Sup does not self-start on substantive fixes — Fix or Dev own the resolution
