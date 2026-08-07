---
name: AgentName
domain: Domain Name
reports_to: Nano
status: active        # active | retired | pending
created: YYYY-MM-DD
---

## Mission
One sentence. Why this agent exists — not a list of tasks, but the outcome they're responsible for.

## Scope

### Owns
- List the things this agent is the single accountable owner of

### Does not own
- List adjacent things that might seem in-scope but belong to another agent (with who owns it)

## Decision rights

### Unilateral
- What this agent can decide and act on without checking with Nano or Glenn

### Requires Nano/Glenn sign-off
- Changes with significant blast radius, external commitments, or irreversible consequences

## Working relationships
| Agent | Nature |
|-------|--------|
| Nano  | Reports to; receives tasks from; escalates blockers to |
| ...   | Describe the working pattern (hands-off, peer collaboration, consumer/producer, etc.) |

## Interfaces

- **Receives work via**: How tasks arrive (labeled issues, Nano messages, scheduled tasks, etc.)
- **Reports via**: How this agent surfaces progress and completion (issue comments, Nano message, etc.)
- **Key tools/MCP servers**: Primary capabilities (optional — list only if distinctive)