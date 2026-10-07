# z/OS Problem Determination and Diagnostics

> Evidence-driven problem determination for IBM z/OS: system messages, SYSLOG correlation, execution context, failure isolation, diagnostic reasoning and post-condition validation.

**Portfolio:** [IBM z/OS Mainframe Engineering Portfolio](https://github.com/P-dot/P-dot)
**Architecture:** [Architecture V2](https://github.com/P-dot/zos-adcd-hercules-engineering-lab/tree/main/docs/architecture/v2)
**Engineering Control:** [Engineering Control](https://github.com/P-dot/zos-adcd-hercules-engineering-lab/tree/main/docs/engineering-control)
**Ecosystem:** [Ecosystem Integration](docs/ECOSYSTEM-INTEGRATION.md)
**Roadmap:** [Diagnostics Engineering Roadmap](ROADMAP.md)

---

## Repository role

This repository is the **Problem Determination and Diagnostics domain** of the wider z/OS engineering portfolio.

It owns the diagnostic method:

```text
Symptom
   |
   v
Observe evidence
   |
   v
Identify component
   |
   v
Correlate time + address space + context
   |
   v
Classify the evidence
   |
   v
Consult release-compatible documentation
   |
   v
Diagnose
   |
   v
Validate final state
   |
   v
Document
```

The repository does **not** replace the technology repositories that generate diagnostic evidence.

| Domain | Primary ownership |
|---|---|
| RACF / SAF | Security and authorization |
| Communications Server | TCP/IP and network services |
| CICS | Transaction processing |
| Db2 | Database services |
| USS | UNIX System Services |
| JCL / JES2 | Batch execution |
| Core z/OS | Platform baseline and system integration |
| **This repository** | **Cross-domain diagnostic method and evidence correlation** |

Messages and runtime observations from those technologies may be analyzed here without recreating their curricula.

---

## Diagnostic engineering principle

A message identifier is an **entry point**, not a diagnosis.

Reliable problem determination requires correlation between:

```text
MESSAGE
   +
TIMESTAMP
   +
ADDRESS SPACE / WORKLOAD
   +
OWNING COMPONENT
   +
SURROUNDING EVENTS
   +
DOCUMENTED SYSTEM ACTION
   +
OBSERVED FINAL STATE
```

This prevents common diagnostic errors such as:

- treating temporal proximity as causation;
- interpreting a warning or failure-looking message as proof of service failure;
- matching the correct message ID from the wrong event;
- diagnosing a component without checking its final runtime state.

---

## Evidence states

The portfolio distinguishes evidence from intention.

| State | Meaning |
|---|---|
| **VALIDATED LOCALLY / PASS** | Executed and supported by reviewable evidence in this repository |
| **HISTORICAL FOUNDATION** | Previously validated work retained in its original repository |
| **PARTIAL / CONTROLLED STOP** | Useful diagnostic progress was demonstrated without claiming the intended final state |
| **PLANNED** | Capability belongs to the roadmap but has not yet been validated |

Configuration presence, documentation knowledge and roadmap intent are not presented as completed operational capability.

---

## Labs

| Lab | Capability | Evidence state |
|---|---|---|
| [01 · z/OS Messages and SYSLOG Fundamentals](labs/01-zos-messages-syslog-fundamentals/) | SYSLOG navigation, message families, timeline correlation, execution context, diagnostic reasoning and controlled-event validation | **VALIDATED LOCALLY / PASS** |
| [02 · IBM MQ CSQ7 BSDS and Active Log Recovery](labs/02-ibm-mq-csq7-bsds-active-log-recovery/) | MQ restart failure isolation, BSDS/RBA correlation, conditional recovery, archive/offload validation, active-log sanitation and clean restart proof | **VALIDATED LOCALLY / PASS** |

### Lab 01 evidence chain

Lab 01 demonstrates:

```text
Observe -> Identify -> Correlate -> Diagnose -> Validate -> Document
```

Validated examples include:

- SDSF SYSLOG navigation;
- TSO/JES/MVS message correlation;
- CICS and Db2 initialization timeline analysis;
- NFS/Kerberos diagnostic interpretation;
- controlled `DISPLAY A,L` observation;
- operator-command-to-SYSLOG correlation;
- false-positive message-ID detection;
- publication security review and evidence redaction.

The lab contains eight published screenshots plus dedicated command, workflow, course-mapping, evidence and security-review documentation.

---

## Diagnostic lessons already demonstrated

### Correlation is not causation

CICS Db2 attachment completion and Db2 DDF TCP/IP availability were observed close together, but represent different initialization paths.

The timeline is useful evidence; it is not sufficient by itself to establish causality.

### A failure-looking message is not automatically service failure

The NFS started task reported a Kerberos-related initialization condition and subsequently reported that the z/OS Network File System Server had started.

The observed final state therefore matters to the diagnosis.

### Message ID alone is insufficient

During Lab 01, an earlier `IEE114I` was initially found. The identifier matched, but the date and time did not.

Reliable correlation must therefore include message ID, date, time, origin, execution context and surrounding records.

---

## Historical foundation

Problem-determination work existed before this specialized repository.

The validated historical foundation remains in:

[Core z/OS — Lab 06: System Services Structure, Problem Determination, LOGREC and Dumps](https://github.com/P-dot/zos-adcd-hercules-engineering-lab/tree/main/labs/06-system-services-structure-problem-determination-logrec-dumps)

That work established visibility into areas including SYSLOG, LOGREC evidence, dump configuration, SLIP state, trace state, pending messages, console state and active address spaces.

Architecture V2 uses a **MIGRATE-FUTURE** model:

```text
Historical evidence remains in Core
              |
              v
Referenced as domain foundation
              |
              v
New diagnostics work is created here
```

The historical lab is not copied or relocated, preserving Git history and avoiding duplicate ownership.

See [Historical Origin](docs/HISTORICAL-ORIGIN.md).

---

## Cross-domain diagnostics

Problem determination is inherently cross-domain:

```text
                    SYSLOG / runtime evidence
                              |
                              v
             Problem Determination & Diagnostics
                /        |        |        \
               /         |        |         \
            RACF       CICS      Db2      Comm Server
          Security   Transactions Data      Network
```

Examples already demonstrated in Lab 01:

| Message family | Diagnostic context |
|---|---|
| `$HASP...` | JES2 activity |
| `IEF...` / `IEE...` / `IEA...` | MVS execution, operator and diagnostic context |
| `DFH...` | CICS behavior |
| `DSNL...` | Db2 distributed communications |
| `GFSA...` | z/OS NFS behavior |
| `IEC...` | Data management / I/O context |

The originating technology remains owned by its domain repository. This repository owns the **diagnostic reasoning applied across those boundaries**.

See [Ecosystem Integration](docs/ECOSYSTEM-INTEGRATION.md).

---

## Architecture V2 lifecycle

Diagnostics contributes especially to the middle and recovery stages of the portfolio lifecycle:

```text
Discover -> Baseline -> Configure -> Operate -> Observe -> Diagnose -> Recover -> Improve -> Automate -> Integrate
```

A mature diagnostic lab should answer, where its scope permits:

1. What happened?
2. How was it detected?
3. Which component emitted the evidence?
4. Which address space or workload was involved?
5. What happened immediately before and after it?
6. Is the relationship temporal, causal or coincidental?
7. What does release-compatible documentation say?
8. What was the observed final state?
9. How was the conclusion independently validated?

---

## Capability roadmap

| ID | Capability | State |
|---|---|---|
| PDD-001 | Messages and SYSLOG fundamentals | **COMPLETE** |
| PDD-002 | Message-to-documentation workflow | **PLANNED** |
| PDD-003 | LOGREC operational analysis | **HISTORICAL FOUNDATION** |
| PDD-004 | Dump configuration and safe capture planning | **PLANNED** |
| PDD-005 | IPCS fundamentals | **PLANNED** |
| PDD-006 | ABEND identification and first-pass analysis | **PLANNED** |
| PDD-007 | Failure classification | **PLANNED** |
| PDD-008 | Incident evidence package | **PLANNED** |
| PDD-009 | Root-cause workflow | **PLANNED** |
| PDD-010 | Recovery and post-incident validation | **PLANNED** |

Detailed planning remains in [ROADMAP.md](ROADMAP.md).

---

## Scope and safety

The current domain-native lab is primarily observational. Lab 01 uses read-only analysis plus the non-destructive `DISPLAY A,L`.

It does not claim validation of dump capture, IPCS analysis, SLIP creation, trace modification or destructive recovery operations.

Published evidence is curated before publication. Network endpoint information visible during the working session was redacted from the public evidence set.

---

## Repository documentation

- [Architecture Decision](docs/ARCHITECTURE-DECISION.md)
- [Ecosystem Integration](docs/ECOSYSTEM-INTEGRATION.md)
- [Historical Origin](docs/HISTORICAL-ORIGIN.md)
- [Diagnostics Engineering Roadmap](ROADMAP.md)
- [Lab 01](labs/01-zos-messages-syslog-fundamentals/)
- [Lab 01 Diagnostic Workflow](labs/01-zos-messages-syslog-fundamentals/docs/DIAGNOSTIC-WORKFLOW.md)
- [Lab 01 Evidence Index](labs/01-zos-messages-syslog-fundamentals/evidence/README.md)
- [Lab 01 Security Review](labs/01-zos-messages-syslog-fundamentals/docs/SECURITY-REVIEW.md)

---

## Portfolio position

```text
IBM z/OS Engineering Portfolio
              |
              +-- Core z/OS
              +-- Security
              +-- Networking
              +-- Data
              +-- Applications
              +-- Automation
              |
              +-- Problem Determination & Diagnostics
                         |
                         +-- Observe
                         +-- Correlate
                         +-- Diagnose
                         +-- Validate
                         +-- Recover
```

**Back to portfolio:** [P-dot IBM z/OS Mainframe Engineering Portfolio](https://github.com/P-dot/P-dot)
