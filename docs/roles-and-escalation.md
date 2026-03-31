# Roles and escalation

Fill in names, phone numbers, and backups in your internal directory or secure wiki; keep **this repo free of PII**.

## Core roles

| Role | Responsibility |
|------|----------------|
| **Incident Commander (IC)** | Single authority for scope, priority, and external comms approval |
| **Technical lead** | Drives mitigation, coordinates engineers |
| **Comms lead** | Internal + customer messaging per approved templates |
| **Security lead** | Breach assessment, forensics coordination, regulator/customer security notices |
| **Legal / Privacy** | Regulatory obligations, contracts, breach notification |
| **Executive sponsor** | Escalation for SEV-1, brand, or legal exposure |

## Escalation ladder (example)

```mermaid
flowchart TD
  O[On-call engineer] -->|SEV-3 stable| O
  O -->|SEV-2 or unclear| IC[Incident Commander]
  IC -->|SEV-1 or breach suspected| SEC[Security + Legal]
  IC -->|Customer-visible major outage| COMMS[Comms + Exec]
  SEC -->|Confirmed data exposure| LEGAL[Legal / DPO notification path]
```

## Handoff

- IC handoff: **verbal + ticket** with current state, open decisions, and next update time.
- After resolution: IC remains owner until **post-incident review** is scheduled.
