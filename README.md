# Incident response playbook

This is a documented, **versioned** runbooks for common incidents: **outage**, **security breach**, and **data loss**. Decision flows use [Mermaid](https://mermaid.js.org/) diagrams (visible on GitHub and in many editors).

## Version

Current release: **`1.0.0`** (see [`VERSION`](VERSION) and [`CHANGELOG.md`](CHANGELOG.md)).

## Contents

| Document | Purpose |
|----------|--------|
| [docs/overview.md](docs/overview.md) | Scope, severities, lifecycle, legal/PR boundaries |
| [docs/roles-and-escalation.md](docs/roles-and-escalation.md) | IC, comms, security, exec path |
| [docs/communications.md](docs/communications.md) | Status page and internal update templates |
| [docs/scenarios/outage.md](docs/scenarios/outage.md) | Service outage — decision tree + actions |
| [docs/scenarios/security-breach.md](docs/scenarios/security-breach.md) | Suspected or confirmed breach |
| [docs/scenarios/data-loss.md](docs/scenarios/data-loss.md) | Corruption, deletion, restore |

## How to use

1. **Customize** placeholders (`YOUR_*`, contacts, tooling names) for your org.
2. **Version** changes: bump `VERSION`, add a `CHANGELOG.md` entry, tag in git (`git tag v1.1.0`).
3. **Exercise** with tabletops; note gaps in `CHANGELOG.md` or an internal tracker.
4. During an incident, **follow one scenario doc** and record decisions in your ticket (Jira, etc.).


## License

Use your org’s default license.
