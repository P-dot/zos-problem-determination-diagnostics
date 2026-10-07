# Lab 01 — z/OS Messages and SYSLOG Fundamentals

## Course alignment

IBM training unit:

**z/OS Introduction and Workshop — Components, Messages, and SYSLOG**

The course unit introduces:

- z/OS as a collection of components;
- base elements and optional features;
- z/OS message identifiers and component identifiers;
- message-body structure;
- SYSLOG record format;
- IBM message documentation / LookAt workflow.

This lab converts those concepts into an evidence-driven exercise on the local ADCD system.

## Objective

Use SDSF SYSLOG to identify message families, associate messages with owning components and execution contexts, build short event timelines, avoid false causal conclusions, diagnose an observed initialization condition, validate final subsystem state, and correlate a controlled MVS operator command with the resulting SYSLOG response.

## Environment

- Platform: ADCD z/OS laboratory system
- Access: 3270 / TSO / ISPF / SDSF
- Primary panel: SDSF `LOG`
- Lab date: 2026-09-17
- Mode: read-only observation plus non-destructive `DISPLAY A,L`

## Method

```text
IBM course concept
       |
       v
Observe real SYSLOG
       |
       v
Identify message family
       |
       v
Correlate timestamp + task + context
       |
       v
Diagnose observed behavior
       |
       v
Validate final state
       |
       v
Document evidence
```

## 1. Entering SYSLOG

SDSF was opened from ISPF and the `LOG` panel was selected.

```text
SDSF
LOG
```

The initial SYSLOG view contained messages from multiple components in one chronological stream.

Evidence:

- `evidence/screenshots/01-sdsf-log-navigation.png`
- `evidence/screenshots/02-syslog-overview-tso-logon.png`

## 2. TSO logon correlation

A TSO session was visible through a short sequence of messages associated with the same TSO user address space.

Observed sequence:

```text
11:09:34.47  TSU07305  $HASP100  IBMUSER ON TSO
11:09:35.05  TSU07305  $HASP373  IBMUSER STARTED
11:09:35.11  TSU07305  IEF125I   IBMUSER - LOGGED ON
```

This demonstrates that one user action can generate messages from more than one z/OS/JES processing area.

The diagnostic lesson is not to memorize one message in isolation, but to reconstruct the event using message family, timestamp, and execution context.

## 3. Message families and owning components

The SYSLOG sample exposed several message families:

| Prefix | Observed technical area |
|---|---|
| `$HASP` | JES2 |
| `IEF` / `IEE` / `IEA` | MVS system, allocation, operator and diagnostic processing |
| `DSNL` | Db2 distributed communications / DDF area |
| `DFH` | CICS Transaction Server |
| `GFSA` | z/OS Network File System |
| `IEC` | data management / I/O related messages |

The prefix is an entry point into diagnosis, not a complete diagnosis by itself.

## 4. CICS and Db2 timeline correlation

CICS messages showed its Db2 attachment progressing through initialization:

```text
11:07:24.82  STC07286  DFHSI8440I  initiating connection to DB2
11:07:24.95  STC07286  DFHDB2023I  CICS-DB2 attachment connected
11:07:24.97  STC07286  DFHSI8441I  connection successfully completed
```

Later, Db2 DDF reported TCP/IP services available:

```text
11:08:14.02  STC07283  DSNL519I  TCP/IP SERVICES AVAILABLE
```

Network endpoint information visible in the original session evidence was redacted from the published screenshot.

### Finding

The events are related to the same Db2 subsystem environment but represent different initialization paths. The CICS local Db2 attachment completed before the DDF TCP/IP availability message.

### Diagnostic rule

> Temporal proximity does not prove causation.

A useful timeline must be combined with knowledge of the owning components and their roles.

Evidence:

- `evidence/screenshots/03-component-message-families-redacted.png`
- `evidence/screenshots/04-cics-db2-timeline.png`

## 5. NFS / Kerberos diagnostic case

The NFS started task produced an initialization message showing that a Kerberos-related routine did not complete successfully:

```text
11:07:27.24  STC07290  GFSA737I
                         NETWORK FILE SYSTEM SERVER COULD NOT GET KERBEROS TICKET
                         ROUTINE: krb5_get_default_realm()
                         KERBEROS RETURN CODE: 96C73ADF
```

The lab does not treat the return code alone as proof that the NFS server failed.

The following message was observed approximately 1.54 seconds later from the same started task:

```text
11:07:28.78  STC07290  GFSA348I
                         z/OS Network File System Server started
```

### Finding

The Kerberos-related initialization condition did **not** prevent the NFS server address space from reaching its observed started state.

### Why this matters

A diagnostic-looking or failure-looking message must be interpreted together with:

- the owning component;
- the issuing address space;
- surrounding messages;
- release-compatible IBM documentation;
- documented system action;
- observed final state.

Evidence:

- `evidence/screenshots/05-nfs-kerberos-diagnostic-redacted.png`
- `evidence/screenshots/06-nfs-same-stc-validation.png`

## 6. Controlled operator event

A non-destructive MVS display command was issued from SDSF:

```text
/D A,L
```

The command displayed active address spaces and system work. The immediate response included:

```text
IEE114I 12.55.59 2026.260 ACTIVITY 249
```

The display showed active system components and address spaces such as JES2, RACF, TSO, TCP/IP, SDSF, TN3270, Db2-related address spaces, NFS, PORTMAP, FTPD, and the active TSO user.

Evidence:

- `evidence/screenshots/07-display-active-address-spaces.png`

## 7. SYSLOG correlation of the controlled command

The command was then located in SYSLOG with the corresponding response:

```text
12:55:59.52  TSU07305  IEA630I  operator context
12:55:59.53  IBMUSER   D A,L
12:55:59.56  IBMUSER   IEE114I 12.55.59 2026.260 ACTIVITY 249
```

The system response was recorded roughly 0.03 seconds after the command record.

This creates a complete controlled-event chain:

```text
Known action
   -> command record
   -> exact timestamp
   -> system response
   -> runtime state
```

Evidence:

- `evidence/screenshots/08-controlled-command-syslog-correlation.png`

## 8. Message-ID false-positive lesson

During validation, an earlier `IEE114I` from the previous day was initially found. The identifier matched, but the date and time did not.

This produced an important diagnostic rule:

> Message identification alone is insufficient for reliable problem determination. Correlation must include time, date, execution context, origin, and surrounding records.

## Commands used

See [ops/COMMANDS.md](ops/COMMANDS.md).

## Evidence index

See [evidence/README.md](evidence/README.md).

## Security review

The published evidence was curated from the working session. Network endpoint details visible in Db2 DDF output were redacted before publication. No private IPv4 address or MAC address is intentionally included in the evidence set.

See [docs/SECURITY-REVIEW.md](docs/SECURITY-REVIEW.md).

## Result

**PASS — Lab objectives completed.**

The exercise successfully moved from IBM course theory to real-system observation and demonstrated the full workflow:

```text
Observe -> Identify -> Correlate -> Diagnose -> Validate -> Document
```

No system-changing action was required.

## Next capability

The next diagnostics lab should formalize the **message-to-documentation workflow** using release-compatible IBM message documentation and a structured evidence template for message meaning, system action, operator response, and observed validation.


---
### Continue learning

**Previous:** Course introduction  
**Course:** [Course home](../../README.md)  
**Next:** [02-ibm-mq-csq7-bsds-active-log-recovery](../02-ibm-mq-csq7-bsds-active-log-recovery/)  
**Academy:** [z/OS Engineering Academy](https://github.com/P-dot/P-dot/blob/main/docs/ACADEMY.md) · [Curriculum](https://github.com/P-dot/P-dot/blob/main/docs/CURRICULUM.md)
