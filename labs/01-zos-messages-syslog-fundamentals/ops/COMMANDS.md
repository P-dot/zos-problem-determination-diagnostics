# Lab 01 Commands and Navigation

This lab intentionally uses only SDSF navigation, searches, and one non-destructive MVS `DISPLAY` command.

## Open SDSF SYSLOG

```text
SDSF
LOG
```

`SDSF` opens the System Display and Search Facility. `LOG` selects the system log view used for chronological message analysis.

## Search for the TSO user event

```text
FIND IBMUSER PREV
```

Searches backward from the current position for the previous occurrence of `IBMUSER`.

## Search for Db2 DDF evidence

```text
FIND DSNL519I
```

Locates a Db2 distributed communications message in SYSLOG.

## Horizontal navigation

```text
PF10   -> LEFT
PF11   -> RIGHT
```

Used to switch between timestamp/address-space columns and the extended message body.

## Search for CICS-to-Db2 initialization evidence

```text
FIND DFHSI8440I
```

Locates the CICS initialization message used to build the CICS/Db2 timeline.

## Search for the NFS diagnostic condition

```text
FIND GFSA737I
```

Locates the observed NFS/Kerberos initialization condition.

## Validate NFS final state

```text
FIND GFSA348I
```

Locates the later NFS message confirming the observed started state and allows correlation with the same started task.

## Controlled operator display

```text
/D A,L
```

The leading `/` tells SDSF to route the text as an MVS operator command rather than interpret it as an SDSF panel command.

`D A,L` is the abbreviated form of `DISPLAY A,L` and displays active address spaces/system activity. It is observational and does not change system state.

## Locate the controlled-event response

```text
BOTTOM
FIND IEE114I PREV
```

`BOTTOM` moves to the end of the displayed SYSLOG data. `FIND IEE114I PREV` then locates the most recent matching response while preserving the temporal context needed to verify that it belongs to the controlled command.

## Safety

No `START`, `STOP`, `CANCEL`, `FORCE`, `VARY`, dump capture, trace change, SLIP creation, or configuration-changing command was used.
