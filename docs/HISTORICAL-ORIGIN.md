# Historical Origin

The P-dot Architecture V2 model classified Problem Determination and Diagnostics as a future specialized domain.

The historical seed remains in the Core repository:

```text
P-dot/zos-adcd-hercules-engineering-lab
└── labs/06-system-services-structure-problem-determination-logrec-dumps
```

That lab established read-only visibility into SYSLOG, LOGREC evidence, dump configuration, SLIP, trace state, pending messages, console state, and active address spaces.

Architecture V2 assigned future ownership of this capability family to:

```text
zos-problem-determination-diagnostics
```

with a `MIGRATE-FUTURE` strategy: preserve the validated historical lab in Core and place new diagnostics capability here.

Lab 01 in this repository therefore does **not** replace Core Lab 06. It extends the domain with detailed message anatomy, component recognition, timeline correlation, diagnostic reasoning, and controlled-event validation.
