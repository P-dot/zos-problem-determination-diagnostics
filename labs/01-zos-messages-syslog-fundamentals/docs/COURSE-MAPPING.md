# IBM Course Mapping

Source unit: **z/OS Introduction and Workshop — Components, Messages, and SYSLOG**.

## Course objective -> lab implementation

| Course concept | Lab implementation |
|---|---|
| z/OS is a collection of components | Observe messages from JES2, MVS, CICS, Db2 and NFS in one SYSLOG stream |
| Base elements and optional features | Preserve the component-oriented model when classifying messages |
| Message identifier / component identifier | Classify `$HASP`, `IEF/IEE/IEA`, `DSNL`, `DFH`, `GFSA`, `IEC` message families |
| Message body format | Read identifiers and text separately and use horizontal SDSF navigation when needed |
| SYSLOG format | Correlate record type, system, Julian date, time, task/address space and message text |
| Message documentation | Establish documentation lookup as the next formal diagnostics capability |

## Lab extensions beyond the course demonstration

The practical lab adds:

- TSO logon event reconstruction;
- cross-component timeline analysis;
- explicit warning that correlation is not causation;
- NFS condition analysis using surrounding messages and final-state validation;
- controlled generation of an operator display event;
- correlation of the operator command with its `IEE114I` response;
- publication-oriented evidence and security review.
