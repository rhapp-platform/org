---
type: framework
owner: Sup
updated: 2026-08-07
---

# Support Tier Framework

## Categories

| Value | Description |
|-------|-------------|
| `billing` | Billing, charges, invoices, subscription |
| `technical` | Platform errors, bugs, access issues |
| `feature_request` | New feature or enhancement asks |
| `other` | General questions, misc |

## Priority Levels

| Priority | Criteria |
|----------|----------|
| **critical** | Cannot access platform, lost data, outage |
| **high** | Pro/Studio technical issue, billing dispute, multiple users affected |
| **normal** | Everything else |
| **low** | Feature requests, general questions, Glenn testing |

### Priority signals

- **Customer tier**: Studio > Pro > Starter (higher tier = escalate faster)
- **Keywords**: "can't access", "lost data", "charge", "outage" → treat as critical/high regardless of tier

## Routing

| Condition | Action |
|-----------|--------|
| All tickets | Auto-ack email on intake |
| `critical` or `high` | Immediate alert to Nano |
| `normal` or `low` | Batched triage summary to Nano |
| Confirmed bug | Open GitHub issue → ping Fix agent |
| Client-specific issue | Loop relationship owner (e.g. Ren for `client-renner`) |
