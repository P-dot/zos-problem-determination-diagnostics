# Lab 02 — IBM MQ CSQ7 BSDS and Active Log Recovery

## Purpose

Recover an IBM MQ for z/OS V7 queue manager that could not complete restart because its BSDS metadata referenced an unreadable/invalid log position, then restore a supportable operational state without destroying the damaged historical evidence.

## Incident

CSQ7 failed during restart around RBA `000000355000`:

```text
CSQJ151I ... ERROR READING RBA 000000355000 ... REASON CODE=00D10327
CSQV086E ... REASON=00D96021
ABEND S6C6
```

The decisive inconsistency was between restart/checkpoint metadata and readable active-log history. The BSDS contained a checkpoint at `354E2E–355878`, while the pre-recovery highest RBA written was `353C54`.

## Safety and recovery boundary

Both BSDS copies and relevant active logs were backed up before recovery. A second rollback point was created after recovery/offload and immediately before the later BSDS inventory change:

```text
IBMUSER.MQSAFE.POSTREC.BSDS01
IBMUSER.MQSAFE.POSTREC.BSDS02
```

Both post-recovery backup steps completed CC 0000. Original damaged DS01 data sets were not physically deleted.

The prior checkpoint `34FE2E–350878` was the last usable recovery boundary before the inconsistent region. Recovery therefore used:

```text
CRESTART CREATE,ENDRBA=351000
```

The queue manager subsequently started successfully from RBA `351000`.

## Archive/offload validation

Post-recovery evidence proved:

```text
HIGHEST RBA OFFLOADED 000000355FFF
OFFLOAD YES
TWOACTV YES
TWOARCH YES
TWOBSDS YES
```

Archive generations covering the recovered ranges were registered in the BSDS.

## Active-log sanitation

The IBM-supplied `CSQ4LREC` sample established the supported `CSQJU003 NEWLOG` workflow. Its example `RECORDS(180000)` allocation was rejected after measurement because it was far larger than this ADCD configuration. The replacement geometry was corrected to `RECORDS(1080)`, matching the original DS01 allocation.

Fresh formatted LDSs were created:

```text
CSQ700.CSQ7.LOGCOPY1.DS01R
CSQ700.CSQ7.LOGCOPY2.DS01R
```

COPY1 resides on SBPRD2 and COPY2 on ZVOL00. With CSQ7 offline, both were added to the dual BSDS inventory in one CSQJU003 execution:

```text
NEWLOG DSNAME=CSQ700.CSQ7.LOGCOPY1.DS01R,COPY1
NEWLOG DSNAME=CSQ700.CSQ7.LOGCOPY2.DS01R,COPY2
```

Both NEWLOG operations completed successfully.

## Final validation

A normal `STOP QMGR` was followed by:

```text
%CSQ7 START QMGR PARM(CSQ7ZPRM)
```

No new conditional restart was created. The new address space reached:

```text
STC08384 CSQ7MSTR START2 ACTIVE
```

The final BSDS map showed `HIGHEST RBA WRITTEN 000000359078` and `HIGHEST RBA OFFLOADED 000000355FFF`. Both DS01R entries were `NEW, REUSABLE`. The original COPY1.DS01 remained `TRUNCATED, STOPPED, NOT REUSABLE` and was deliberately preserved.

This clean stop/start is the key proof that CSQ7 no longer requires another CRESTART merely to become operational.

## Security closure

Console authority was elevated only for the maintenance window. Final RACF verification after rollback returned:

```text
NO OPERPARM INFORMATION
```

No temporary OPERPARM console authority was left enabled.

## Residual risk

This lab does not claim that historical COPY1.DS01 was reconstructed. COPY1.DS01 and COPY2.DS01 did not provide equivalent intact historical ranges, so treating COPY2 as a complete reconstruction source would not be supported by the evidence.

The stopped data set remains a forensic artifact. A later phase can address its permanent retirement and wider physical separation of active logs and BSDS copies.

## Engineering lessons

The BSDS is recovery metadata, not merely a catalog of data-set names. Utility return codes alone are insufficient: RBA continuity, checkpoint boundaries, physical log readability, archive registration, dual-copy consistency and actual restart behavior must agree.

The recovery point was selected from evidence rather than from the failing RBA. Vendor samples were also treated as semantic references, not sizing templates: `CSQ4LREC` supplied the correct NEWLOG method, while its sample allocation size was measured and rejected for this environment.


---
### Continue learning

**Previous:** [01-zos-messages-syslog-fundamentals](../01-zos-messages-syslog-fundamentals/)  
**Course:** [Course home](../../README.md)  
**Next:** [Choose the next Academy course](https://github.com/P-dot/P-dot/blob/main/docs/COURSES.md)  
**Academy:** [z/OS Engineering Academy](https://github.com/P-dot/P-dot/blob/main/docs/ACADEMY.md) · [Curriculum](https://github.com/P-dot/P-dot/blob/main/docs/CURRICULUM.md)
