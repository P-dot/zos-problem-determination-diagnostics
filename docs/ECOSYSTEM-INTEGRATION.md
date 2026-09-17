# Ecosystem Integration

Problem determination is a cross-domain capability. This repository owns the diagnostic method; domain repositories continue to own their technologies.

```text
                       SYSLOG / evidence
                              |
                              v
          z/OS Problem Determination & Diagnostics
             /        |        |        \
            /         |        |         \
         RACF       CICS      Db2      Comm Server
       Security   Transactions Data      Network
```

Examples from Lab 01:

- `$HASP...` and `IEF...` messages correlate TSO/JES/MVS activity.
- `DFH...` messages expose CICS initialization behavior.
- `DSNL...` messages expose Db2 DDF communication state.
- `GFSA...` messages expose z/OS NFS behavior.

This repository does not duplicate RACF, CICS, Db2, Communications Server, USS, or JCL curricula. It uses their messages as evidence when demonstrating diagnostic techniques.

## Core relationship

`zos-adcd-hercules-engineering-lab` remains the Core Platform and Integration repository. It provides the system baseline and retains historical diagnostics labs.

## TSO/ISPF relationship

SDSF is accessed through the interactive environment, but a SYSLOG diagnostic lab belongs here because the engineering responsibility is problem determination, not ISPF productivity.
