# Support Tier Framework

## Categories

| Category | When to apply |
|---|---|
| `billing` | Payment, charge, subscription, invoice, refund, plan upgrade/downgrade, pricing questions |
| `technical` | Broken feature, error, bug, cannot access, not working, slow, crash, data loss, outage |
| `feature_request` | "would be great", "can you add", "request", "suggest", "wish", missing UI element |
| `other` | Anything else, tests, unclear submissions |

---

## Priority Levels

### Signals (both apply at triage)

**Customer tier** — higher tier raises priority weight:

| Tier | Weight |
|---|---|
| Studio | High |
| Pro | Medium |
| Starter | Standard |

**Keywords** — any match elevates priority: "can't access", "cannot access", "lost data", "data loss", "charge", "charged", "outage", "down"

### Priority definitions

| Priority | When to apply |
|---|---|
| `critical` | Platform inaccessible, data loss, full outage — with urgent tone |
| `high` | Technical issue from Pro or Studio customer; billing dispute; multiple users affected |
| `normal` | Standard technical or billing issue from Starter customer; no data loss |
| `low` | Feature request, general question, internal/Glenn testing |

---

## SLA Targets

| Priority | Auto-ack | Substantive response | Resolution target |
|---|---|---|---|
| `critical` | Immediate | 1 hour | 4 hours |
| `high` | Immediate | 4 hours | 24 hours |
| `normal` | Immediate | 24 hours | 72 hours |
| `low` | Immediate | 72 hours | Best effort / next sprint |

---

## Escalation Paths by Category

### billing
1. Auto-ack on intake
2. **All billing → Glenn immediately** via Nano, regardless of priority
3. Sup holds reply until Glenn's direction; drafts after Glenn decides

### technical

| Priority | Sup action |
|---|---|
| `critical` | Individual Nano alert + GitHub issue + ping Fix + holding reply to customer |
| `high` | Individual Nano alert + GitHub issue + ping Fix + holding reply |
| `normal` | Batch Nano summary + GitHub issue if confirmed bug |
| `low` | Batch Nano summary only |

Client-specific: loop in relationship owner (e.g. Ren for `client-renner`) + label `agent:<owner>`.

### feature_request
1. Auto-ack + log to `rhapp-platform/project` with `feature_request` label
2. Batch Nano summary; Glenn reviews in sprint planning
3. Customer reply: "Thanks for the suggestion — we've noted it for a future update"

### other
1. Auto-ack; if test/internal → close silently
2. If unclear → ask Nano before replying

---

## Routing Summary

| Condition | Action |
|---|---|
| Any ticket | Auto-ack email on intake |
| `critical` or `high` | Individual alert to Nano |
| `normal` or `low` | Batched triage summary to Nano |
| Confirmed bug | GitHub issue → ping Fix |
| Billing | Escalate to Glenn via Nano immediately |
| Client-specific | Label + message relationship owner |
