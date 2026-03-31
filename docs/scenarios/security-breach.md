# Scenario: Security breach or suspected compromise

Use for **unauthorized access**, **credential leak**, **malware on prod systems**, **suspicious admin activity**, or **report of data exposure**. **Preserve evidence**; avoid destructive actions until Security advises.

## Decision tree

```mermaid
flowchart TD
  START([Signal: possible security incident]) --> Q1{Active ongoing attack?}
  Q1 -->|Yes| CONTAIN[Contain: block IPs, disable accounts, isolate systems]
  Q1 -->|No / unknown| PRES[Preserve logs; avoid wiping disks]
  CONTAIN --> SEC[Notify Security lead + IC]
  PRES --> SEC
  SEC --> Q2{Personal or regulated data at risk?}
  Q2 -->|Yes| LEGAL[Engage Legal / Privacy same day]
  Q2 -->|No / unclear| INV[Assume worst until ruled out]
  LEGAL --> INV
  INV --> Q3{Credentials or tokens exposed?}
  Q3 -->|Yes| ROT[Rotate secrets; invalidate sessions; review audit logs]
  Q3 -->|No| Q4{Evidence of data exfiltration?}
  ROT --> Q4
  Q4 -->|Yes| FORENS[Forensics plan; chain of custody]
  Q4 -->|No| MON2[Enhanced monitoring; hunt]
  FORENS --> NOTIFY{Q2 was Yes or regulator timeline?}
  MON2 --> NOTIFY
  NOTIFY -->|Required| REG[Legal-owned customer/regulator notifications]
  NOTIFY -->|Not required per counsel| DOC[Document decision + close criteria]
  REG --> PIR[Security PIR; playbook update]
  DOC --> PIR
```

## Immediate actions (checklist)

- [ ] **Do not** announce details publicly until Comms + Legal align
- [ ] IC + **Security lead** engaged; incident **classified** (suspected / confirmed)
- [ ] **Audit logs** from identity, cloud, app, CDN — export to secure storage if advised
- [ ] **Scope:** which systems, data classes, time window
- [ ] **Containment** that does not destroy evidence (snapshot volumes per policy, not ad-hoc deletes)
- [ ] **Legal / Privacy** if PHI, financial, children’s data, or contract triggers apply

## Coordination

| If… | Then… |
|-----|--------|
| Ransom demand received | **Legal + law enforcement path** per policy; do not pay without exec/legal |
| Customer reports abuse of their data | Validate claim; **support + security** joint ticket |
| Supply chain / dependency compromise | Map blast radius; pin versions; notify downstream |

## Exit criteria

- Security and Legal agree **containment** and **notification** obligations are addressed (or formally documented as N/A with rationale).
- **PIR** completed with remediation items tracked.

## Links

- Vendor security contacts: `YOUR_VENDOR_LIST`
- Data classification: `YOUR_DATA_POLICY_LINK`
