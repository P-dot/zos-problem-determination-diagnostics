# Ecosystem Integration — Problem Determination and Diagnostics

> Cross-domain diagnostic ownership within the P-dot IBM z/OS engineering portfolio.

[Back to repository](../README.md) · [Portfolio](https://github.com/P-dot/P-dot) · [Architecture V2](https://github.com/P-dot/zos-adcd-hercules-engineering-lab/tree/main/docs/architecture/v2)

---

## 1. Domain ownership

Problem determination is a cross-domain engineering capability. This repository owns the **diagnostic process**:

```text
Observe -> Identify -> Correlate -> Classify -> Diagnose -> Validate -> Document
```

It does not take ownership of the technologies being diagnosed.

| Technology domain | Owning responsibility | Diagnostic relationship |
|---|---|---|
| RACF / SAF | Security and authorization | Security evidence can feed diagnostic correlation |
| Communications Server | TCP/IP and network services | Network evidence can feed diagnostic correlation |
| CICS | Transaction processing | CICS messages can feed diagnostic correlation |
| Db2 | Database services | Db2 messages can feed diagnostic correlation |
| USS | UNIX execution environment | Process and service evidence can feed diagnostic correlation |
| JCL / JES2 | Batch execution | Batch/JES evidence can feed diagnostic correlation |
| Core z/OS | Platform baseline and integration | Historical and system-level evidence foundation |
| Problem Determination | Diagnostic process | Correlates evidence across domains |

A failure remains owned technically by its originating domain. This repository demonstrates how evidence from those domains is correlated and reasoned about.

---

## 2. Evidence model

A diagnostic conclusion should be supported by multiple dimensions of evidence:

```text
Message ID
   +
Timestamp
   +
Component
   +
Address space / workload
   +
Surrounding events
   +
Documented system action
   +
Observed final state
   =
Bounded diagnostic conclusion
```

A single message identifier is not treated as sufficient proof of root cause.

---

## 3. Evidence states

| State | Interpretation |
|---|---|
| **VALIDATED LOCALLY / PASS** | Demonstrated with evidence in this repository |
| **HISTORICAL FOUNDATION** | Validated previously and retained in its original repository |
| **PARTIAL / CONTROLLED STOP** | Useful diagnostic progress without an unsupported final claim |
| **PLANNED** | Intended capability not yet demonstrated |

This prevents architectural diagrams or roadmap entries from being mistaken for validated runtime capability.

---

## 4. Current local evidence

### Lab 01 — z/OS Messages and SYSLOG Fundamentals

**State: VALIDATED LOCALLY / PASS**

The lab demonstrates SDSF SYSLOG navigation, message-family recognition, address-space correlation, timeline reconstruction, component-boundary reasoning, correlation-versus-causation analysis, final-state validation, controlled operator-event correlation and evidence publication review.

```text
Real system event
      |
      v
SYSLOG evidence
      |
      v
Message family
      |
      v
Timestamp + execution context
      |
      v
Component reasoning
      |
      v
Observed final state
      |
      v
Bounded diagnostic conclusion
```

---

## 5. Core z/OS relationship

Repository: [zos-adcd-hercules-engineering-lab](https://github.com/P-dot/zos-adcd-hercules-engineering-lab)

Core owns the platform baseline, system-level integration, ADCD/Hercules engineering and historical platform evidence.

The historical diagnostics seed remains in [Core Lab 06 — System Services Structure, Problem Determination, LOGREC and Dumps](https://github.com/P-dot/zos-adcd-hercules-engineering-lab/tree/main/labs/06-system-services-structure-problem-determination-logrec-dumps).

Its validated history is not copied here.

```text
Core historical evidence
        |
        | MIGRATE-FUTURE
        v
Diagnostics specialization
        |
        v
New domain-native labs
```

This preserves provenance and avoids duplicate ownership.

---

## 6. TSO / ISPF / SDSF relationship

Repository: [MVS_TSO_ISPF](https://github.com/P-dot/MVS_TSO_ISPF)

TSO/ISPF owns the interactive operating environment and operator-productivity skills. This diagnostics repository may use TSO, ISPF and SDSF as access paths, but the engineering responsibility determines ownership.

Learning how to navigate SDSF belongs to the interactive-operations path. Using SYSLOG evidence to reconstruct an incident belongs to **Problem Determination and Diagnostics**.

---

## 7. JCL / JES2 relationship

Repository: [JCL_LABS](https://github.com/P-dot/JCL_LABS)

JCL and JES2 own batch execution behavior. Diagnostics may consume `$HASP...`, `IEF...`, `IEA...` and `IEE...` evidence to correlate workload lifecycle, execution context, system actions and surrounding events.

The diagnostic repository does not duplicate JCL construction or JES2 operational training.

---

## 8. RACF / SAF relationship

Repository: [mainframe-racf-security-evidence](https://github.com/P-dot/mainframe-racf-security-evidence)

RACF owns identities, groups, profiles, permissions, SAF authorization, certificates, key rings and security-policy evidence.

Diagnostics may consume `ICH...`, `IRR...`, SAF-related messages, authorization failures and started-task identity evidence when investigating an incident. RACF policy ownership remains in the RACF repository.

---

## 9. Communications Server relationship

Repository: [zos-communications-server-network-lab](https://github.com/P-dot/zos-communications-server-network-lab)

Communications Server owns TCP/IP runtime, network services, LCS/ETH1, FTP/HTTP/TN3270/SSH network behavior, Policy Agent, AT-TLS and transport-security implementation.

Diagnostics consumes network-service messages and runtime observations when reconstructing failures. The Communications repository owns the network correction; this repository owns the reusable diagnostic method.

---

## 10. CICS relationship

Repository: [CICS](https://github.com/P-dot/CICS)

CICS owns transaction-processing behavior. Lab 01 already consumes `DFH...` messages to reason about CICS initialization and its Db2 attachment.

The evidence is used here for message-family recognition, timeline correlation, address-space context and subsystem-boundary reasoning; it does not replace CICS application or region engineering.

---

## 11. Db2 relationship

Repository: [DB2-](https://github.com/P-dot/DB2-)

Db2 owns database services and Db2 runtime behavior. Lab 01 uses `DSNL...` evidence from Db2 DDF.

An important demonstrated lesson is that CICS Db2 attachment completion and Db2 DDF TCP/IP availability are distinct initialization paths even when their events occur close together. Temporal correlation must not be automatically promoted to causation.

---

## 12. USS relationship

Repository: [UNIX_System_Services-](https://github.com/P-dot/UNIX_System_Services-)

USS owns the UNIX execution environment. Diagnostics may consume process state, filesystem evidence, USS logs, service startup failures and environment/configuration evidence.

A future incident may cross both repositories, while component-specific correction remains in the owning domain.

---

## 13. NFS diagnostic evidence

Lab 01 provides a useful cross-component diagnostic case. The NFS started task emitted a Kerberos-related condition and subsequently reported that the server had started.

The conclusion is deliberately bounded to the observed evidence. The lab does not infer a complete Kerberos root cause from the return code alone.

---

## 14. Controlled-event correlation

Lab 01 also establishes a known-action diagnostic pattern. A non-destructive `DISPLAY A,L` was issued and then correlated with its SYSLOG record and response:

```text
Known action -> Command record -> Timestamp -> System response -> Observed runtime state
```

Known actions provide a controlled reference when learning event correlation.

---

## 15. Historical versus current evidence

Architecture V2 distinguishes provenance.

### Historical foundation — Core Lab 06

- SYSLOG;
- LOGREC;
- dump configuration;
- SLIP state;
- trace state;
- pending messages;
- console state.

### Current domain-native evidence — Diagnostics Lab 01

- message anatomy;
- component recognition;
- timeline correlation;
- correlation versus causation;
- controlled-event validation;
- final-state reasoning.

Historical capability is referenced, not relabeled as newly validated local work.

---

## 16. Architecture V2 lifecycle

The portfolio lifecycle is:

```text
Discover -> Baseline -> Configure -> Operate -> Observe -> Diagnose -> Recover -> Improve -> Automate -> Integrate
```

This repository specializes primarily in:

```text
Observe -> Diagnose -> Recover -> Improve
```

Current local evidence is strongest in:

```text
Observe -> Diagnose
```

Recovery and post-incident validation remain roadmap capabilities except where historical or domain-specific evidence is explicitly referenced.

---

## 17. Diagnostic maturity

A useful maturity progression for this domain is:

```text
M0  Message recognition
 |
M1  Context and timeline correlation
 |
M2  Structured diagnostic workflow
 |
M3  Failure isolation and recovery validation
 |
M4  Repeatable evidence / automation
 |
M5  Cross-domain production-like diagnosis
```

The model is directional; maturity is not claimed merely because a level appears in documentation.

---

## 18. Production-track integration

Diagnostics supports production tracks because failures can cross domain boundaries:

```text
Workload
   |
   v
Unexpected state
   |
   v
SYSLOG / runtime evidence
   |
   v
Diagnostics
   |
   +--> Security
   +--> Network
   +--> Batch
   +--> Data
   +--> Transaction processing
   +--> USS
   |
   v
Correction in owning domain
   |
   v
Post-condition validation
```

The diagnostic repository acts as an evidence-correlation layer across the portfolio rather than as another technology silo.

---

## 19. Publication security

Diagnostic evidence can expose sensitive operational information.

Before publication, evidence should be reviewed for private IP addresses, MAC addresses, host-specific network identifiers, credentials or secrets, certificate/private-key material and internal infrastructure details not required by the learning objective.

Redaction must preserve enough context for the engineering conclusion to remain reviewable. Lab 01 already applies this principle to network endpoint information visible in Db2 DDF evidence.

---

## 20. Current integration frontier

### Validated today

- SYSLOG observation;
- message-family recognition;
- execution-context correlation;
- timeline reasoning;
- final-state validation;
- controlled-event correlation.

### Historical foundation

- LOGREC / dumps / SLIP / trace visibility.

### Not yet claimed as domain-native validated capability

- message-to-documentation workflow;
- LOGREC operational analysis;
- safe dump capture;
- IPCS;
- ABEND first-pass analysis;
- formal failure classification;
- incident evidence package;
- root-cause workflow;
- recovery and post-incident validation.

Those capabilities remain controlled roadmap items until evidence exists.

---

## 21. Navigation

### Portfolio

[P-dot IBM z/OS Mainframe Engineering Portfolio](https://github.com/P-dot/P-dot)

### Core

[zos-adcd-hercules-engineering-lab](https://github.com/P-dot/zos-adcd-hercules-engineering-lab)

### Related domains

- [RACF Security](https://github.com/P-dot/mainframe-racf-security-evidence)
- [Communications Server](https://github.com/P-dot/zos-communications-server-network-lab)
- [TSO / ISPF](https://github.com/P-dot/MVS_TSO_ISPF)
- [JCL / JES2](https://github.com/P-dot/JCL_LABS)
- [CICS](https://github.com/P-dot/CICS)
- [Db2](https://github.com/P-dot/DB2-)
- [USS](https://github.com/P-dot/UNIX_System_Services-)

### Local documentation

- [Repository README](../README.md)
- [Architecture Decision](ARCHITECTURE-DECISION.md)
- [Historical Origin](HISTORICAL-ORIGIN.md)
- [Roadmap](../ROADMAP.md)
- [Lab 01](../labs/01-zos-messages-syslog-fundamentals/)

---

## Engineering rule

> Diagnose the evidence that exists, bound the conclusion to what the evidence proves, and leave the correction to the repository that owns the affected technology.
