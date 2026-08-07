# Data & Privacy Policy for Agents

## What agents may store
- Task outputs, build logs, and summaries in workspace files
- Aggregated metrics (error counts, build stats)
- Customer ticket summaries (no PII beyond name/email for routing)

## What agents must NOT store
- Raw customer PII beyond what's needed for the current task
- Passwords, tokens, or secrets in plaintext workspace files
- Full conversation transcripts containing sensitive user data

## Credentials
All API credentials flow through the OneCLI proxy. Agents do not handle raw secrets directly. If a credential is shared in chat (e.g. a PAT or API token), treat it as potentially exposed and flag it.

## Customer data
- Do not share customer information between agents beyond what's needed to resolve a ticket
- Customer emails sent from nano@rhappsody.com; BCC glenn@rhappsody.com per communications policy
