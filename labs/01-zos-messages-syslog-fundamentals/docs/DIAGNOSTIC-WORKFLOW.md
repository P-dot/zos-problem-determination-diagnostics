# Diagnostic Workflow

## Workflow used in Lab 01

```text
1. Detect
   Identify the message or behavior that deserves investigation.

2. Identify
   Determine message family, owning component, task/address space and message context.

3. Correlate
   Compare timestamps and surrounding SYSLOG records. Build the smallest useful timeline.

4. Classify the relationship
   Decide whether nearby events are causal, dependent, parallel, or merely temporally adjacent.

5. Diagnose
   Use release-compatible IBM documentation and system knowledge to explain the observed condition.

6. Validate
   Look for independent evidence of the resulting state: later messages, active address spaces, subsystem state, or other read-only displays.

7. Document
   Preserve commands, evidence, findings, limitations, and security redactions.
```

## Minimum evidence rule

A conclusion should not rely on message ID alone when additional context is available.

Prefer:

```text
message ID
+ timestamp
+ system/task
+ surrounding records
+ owning component
+ final-state evidence
```

## Correlation is not causation

Two messages close together in SYSLOG can belong to different initialization paths. The CICS/Db2 example in this lab demonstrates why technical ownership must be understood before assigning causal relationships.
