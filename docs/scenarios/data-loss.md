# Scenario: Data loss or corruption

Use for **accidental deletion**, **bad migration**, **replication failure**, **ransomware**, or **integrity errors** (wrong data shown to users). Pair with **backup / DR** procedures.

## Decision tree

```mermaid
flowchart TD
  START([Report: missing or wrong data]) --> Q1{Still writing to affected store?}
  Q1 -->|Yes| STOP[Stop-the-bleeding: read-only, feature off, queue drain]
  Q1 -->|No / read-only| SNAP[Identify last known good timestamp]
  STOP --> SNAP
  SNAP --> Q2{Backup or PITR available?}
  Q2 -->|No| DAM[Assess damage scope; legal/comms if customer data]
  Q2 -->|Yes| Q3{Restore overwrites current data?}
  Q3 -->|Yes| COPY[Restore to clone / secondary; validate before cutover]
  Q3 -->|No| RESTORE[Execute restore with IC approval]
  DAM --> ESC{Regulatory or contract breach?}
  COPY --> VAL[Validate row counts, checksums, app smoke tests]
  RESTORE --> VAL
  VAL --> Q4{Application schema compatible?}
  Q4 -->|No| MIG[Run migrations or restore matching binary snapshot]
  Q4 -->|Yes| CUTOVER[Controlled cutover or merge strategy]
  MIG --> CUTOVER
  CUTOVER --> COMMS[Notify affected users if required]
  ESC -->|Yes| LEGAL[Legal / Privacy path]
  ESC -->|No| COMMS
  LEGAL --> PIR[PIR: root cause + backup gap review]
  COMMS --> PIR
```

## Immediate actions (checklist)

- [ ] **Time-bound** the event: when did data last look correct?
- [ ] **Scope:** which tenants, tables, buckets, regions
- [ ] **Freeze** destructive jobs (migrations, GC, retention policies) until IC approves
- [ ] Locate **backup ID** or **PITR** point; test restore in **non-prod** slice if possible
- [ ] Document **RPO** actually achieved vs target

## Restore strategies

| Situation | Typical approach |
|-----------|------------------|
| Bad deploy wrote bad rows | Roll forward fix vs restore subset |
| Dropped table / bucket | PITR or backup restore to staging then merge |
| Replica lag / split-brain | Stop writes, elect source of truth, resync |

## Comms

- If users saw **wrong** data: coordinate **Comms + Legal** (may be breach-class).
- If **loss** only (data gone): message per policy; avoid technical blame in external text.

## Exit criteria

- Data **validated** against business checks; monitoring green.
- **Backup/restore test gap** filed if restore was untested or failed first attempt.

## Links

- Backup and DR runbook: `YOUR_BACKUP_DR_LINK`
- Database ownership: `YOUR_DBA_CONTACT`
