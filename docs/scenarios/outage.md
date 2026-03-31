# Scenario: Service outage

Use when customers or internal users **cannot use a production service** (errors, timeouts, total unavailability). Not for security-only events (use [security-breach.md](security-breach.md)).

## Decision tree

```mermaid
flowchart TD
  START([Alert or report: service impaired]) --> Q1{Customer-visible?}
  Q1 -->|Yes| SEV[Set severity SEV-1/2 per overview]
  Q1 -->|No internal only| SEV3[SEV-3 unless cascade risk]
  SEV --> IC[Assign IC + open war room / channel]
  SEV3 --> IC
  IC --> Q2{Recent change / deploy?}
  Q2 -->|Yes| ROLL[Consider controlled rollback / feature flag off]
  Q2 -->|No| Q3{Single component or unknown?}
  Q3 -->|Known blast radius| FIX[Isolate: scale, restart, drain bad nodes]
  Q3 -->|Unknown| OBS[Collect metrics, traces, recent deploys]
  ROLL --> Q4{Improved?}
  FIX --> Q4
  OBS --> FIX
  Q4 -->|Yes| MON[Monitor SLOs, comms updates]
  Q4 -->|No| ESC[Escalate vendor / platform + widen IC]
  ESC --> Q4
  MON --> Q5{Stable for YOUR_STABILITY_WINDOW?}
  Q5 -->|No| FIX
  Q5 -->|Yes| CLOSE[Declare mitigation, schedule PIR]
```

## Immediate actions (checklist)

- [ ] Create **incident ticket**; pin command channel
- [ ] IC assigned; **severity** recorded
- [ ] **Customer impact** statement (factual, one paragraph)
- [ ] **Comms** next-update time set
- [ ] Identify **change correlation** (deploy, config, DNS, third party)
- [ ] **Mitigate** before root-cause if needed (rollback, scale, bypass)

## Useful splits (parallel workstreams)

| Stream | Tasks |
|--------|--------|
| **App** | Errors, saturation, recent releases |
| **Data** | DB connectivity, replication lag, locks |
| **Network / edge** | DNS, CDN, WAF, TLS |
| **Dependencies** | APIs, queues, payment providers |

## Exit criteria

- Service meets **SLO / error budget** for agreed window, or documented degraded mode with customer comms.
- IC hands off to owner for **PIR** within `YOUR_PIR_SLA`.

## Links

- DR / failover: `YOUR_DR_RUNBOOK_LINK`
- Status page: `YOUR_STATUS_PAGE`
