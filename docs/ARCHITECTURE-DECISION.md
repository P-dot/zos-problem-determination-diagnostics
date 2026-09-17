# Architecture Decision — Repository Bootstrap

## Decision

Bootstrap `zos-problem-determination-diagnostics` as the future home for new Problem Determination and Diagnostics work.

## Why now

Architecture V2 already classified the domain as a strong specialization candidate and assigned the historical Core problem-determination lab a `MIGRATE-FUTURE` destination of `zos-problem-determination-diagnostics`.

The new Lab 01 adds domain-native depth that was previously identified as missing:

- message anatomy and component identification;
- message-to-context workflow;
- failure classification by surrounding evidence;
- timeline correlation;
- explicit correlation-versus-causation reasoning;
- incident-style evidence collection;
- post-condition validation through a second operational observation.

## History policy

Do not copy or move historical Core Lab 06.

```text
Historical validation in Core
        |
        v
Referenced as origin
        |
        v
New domain-native Lab 01 here
        |
        v
Future diagnostics roadmap
```

This preserves existing Git history and prevents duplicate ownership.

## Boundary

This repository owns the **diagnostic process**.

Technology repositories continue to own their domains:

- RACF -> Security
- TCP/IP / Communications Server -> Networking
- CICS -> Transaction processing
- Db2 -> Database
- USS -> UNIX System Services
- JCL/JES usage -> Batch/JCL

Messages from those components may be used here as diagnostic evidence without recreating their full curricula.
