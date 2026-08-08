# Workflow: New Client Onboarding

## Trigger
Glenn closes a new client deal and instructs Nano to kick off onboarding.

## Steps

| Step | Owner | Action |
|------|-------|--------|
| 1. Deal close | Glenn / Sal | Glenn confirms terms; Sal hands off to Nano with client brief (name, tier, contacts, contracted scope) |
| 2. Billing setup | Bill | Creates client in billing system; sets up subscription per agreed terms; confirms with Glenn before first invoice |
| 3. Contract | Leg | Files executed contract; flags any non-standard terms to Glenn |
| 4. Repo + label setup | Sys | Creates client repo if needed; adds `client-<name>` label to GitHub; wires label routing |
| 5. Agent assignment | Nano | Assigns relationship owner agent (e.g. Ren for Renner Dance, Son for other clients); if no dedicated agent, App takes it |
| 6. App provisioning | Stu / App/Son | Stu provisions account-facing page and Studio access; relationship owner sets up client app |
| 7. Design brief | Des | Relationship owner briefs Des on brand assets needed; Des produces initial design pack |
| 8. Welcome | Relationship owner | Sends welcome email to client (from `nano@rhappsody.com`, BCC glenn@rhappsody.com); provides access credentials and next steps |
| 9. Handoff confirmation | Nano → Glenn | Nano confirms onboarding complete; flags any open items |

## Handoff conditions

- **Sal → Nano**: Deal confirmed, client brief provided (contacts, tier, scope)
- **Nano → Bill**: Client brief for billing setup
- **Nano → Leg**: Signed contract for filing
- **Nano → Sys**: Client name + label for repo/routing setup
- **Nano → Relationship owner**: Full client brief; Stu provisioning confirmed
- **Relationship owner → Des**: Brand brief (colors, fonts, existing assets, intended use)
- **Relationship owner → Glenn**: Welcome sent; any access or scope questions

## Relationship owner assignment

| Client | Agent |
|--------|-------|
| Renner Dance | Ren |
| New clients (default) | Son |
| If Son at capacity | App |

## Notes
- Glenn must confirm billing terms before Bill sets up the subscription — Bill does not self-start on pricing
- Leg holds the contract; Renner (or the relationship owner) flags any client-side legal questions to Leg
- All client comms: from `nano@rhappsody.com`, BCC `glenn@rhappsody.com`
- See `support/tier-framework.md` for how ongoing support is routed once the client is live
