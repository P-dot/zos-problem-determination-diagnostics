# z/OS Problem Determination and Diagnostics

Hands-on z/OS problem-determination engineering on an ADCD laboratory system.

This repository develops a repeatable diagnostic workflow around system messages, SYSLOG, execution context, timeline correlation, failure isolation, corrective reasoning, and post-incident validation.

## Engineering workflow

```text
Detect
  -> Identify
  -> Correlate
  -> Diagnose
  -> Validate
  -> Document
```

The repository is intentionally evidence-driven. A message identifier by itself is not treated as a diagnosis: timestamps, address spaces, related components, surrounding SYSLOG records, documented system action, and observed final state are considered together.

## Historical continuity

Problem-determination foundations were originally validated in the central `zos-adcd-hercules-engineering-lab`, especially:

- `labs/06-system-services-structure-problem-determination-logrec-dumps`

That historical lab remains in Core so its Git history and evidence remain intact. This repository continues future domain-native diagnostics work rather than copying or relocating the historical lab.

## Labs

| Lab | Topic | Status |
|---|---|---|
| 01 | z/OS Messages and SYSLOG Fundamentals | Complete |

## Current capability

Lab 01 establishes:

- SDSF SYSLOG navigation;
- z/OS message-family recognition;
- message-to-component mapping;
- timestamp and execution-context correlation;
- cross-component timeline analysis;
- distinction between correlation and causation;
- analysis of a real NFS/Kerberos initialization condition;
- post-message runtime validation;
- controlled operator-command correlation using `DISPLAY A,L`.

## Scope and safety

The current lab is observational and non-destructive. No `START`, `STOP`, `CANCEL`, `FORCE`, `VARY`, dump capture, trace modification, SLIP creation, or configuration-changing action is performed.

## Repository roadmap

See [ROADMAP.md](ROADMAP.md).

## Ecosystem

See [docs/ECOSYSTEM-INTEGRATION.md](docs/ECOSYSTEM-INTEGRATION.md) and [docs/HISTORICAL-ORIGIN.md](docs/HISTORICAL-ORIGIN.md).
