# References

## Primary training source

IBM training unit:

**z/OS Introduction and Workshop — Components, Messages, and SYSLOG**

The unit explicitly covers component-oriented z/OS structure, message format, component identifiers, SYSLOG format, MVS System Messages, and IBM LookAt.

## IBM documentation workflow

For production-quality message analysis, use the IBM documentation level compatible with the z/OS/product release being investigated and search by the exact message ID.

Relevant IBM documentation families include:

- z/OS MVS System Messages;
- JES2 messages;
- CICS Transaction Server messages (`DFH...`);
- Db2 for z/OS messages (`DSN...` / `DSNL...`);
- z/OS Network File System messages (`GFSA...`).

## Historical P-dot lab

The earlier foundation remains in:

```text
P-dot/zos-adcd-hercules-engineering-lab
labs/06-system-services-structure-problem-determination-logrec-dumps
```

It is referenced rather than copied so validated history remains intact.
