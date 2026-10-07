# Evidence index

| Evidence | Result |
|---|---|
| JOB08314 | Pre-repair dual BSDS map; checkpoint/RBA inconsistency |
| JOB08320–08322 | Safety copies and validation |
| JOB08334 | CRESTART at ENDRBA 351000, CC 0000 |
| STC08337 / JOB08339–08341 | First successful recovery start and validation |
| JOB08344 / 08352 / 08356 / 08362 | Archive/offload recovery; OFFLRBA advanced to 355FFF |
| JOB08365 / 08367 | DISPLAY LOG and persistence validation |
| JOB08373 | LDS inventory and original geometry |
| JOB08374 / 08376 / 08377 | Oversized sample allocation discovered and measured |
| JOB08378 | DS01R recreated/formatted with 1080 records |
| JOB08380 | Fresh post-recovery BSDS backup; both steps CC 0000 |
| JOB08381 | Both CSQJU003 NEWLOG operations successful |
| JOB08382 | Both BSDS mapped successfully after NEWLOG |
| JOB08383 / STC08384 | Normal restart; CSQ7 active |
| JOB08386 | Final post-start dual BSDS map |
| RACF final verification | Temporary OPERPARM removed: NO OPERPARM INFORMATION |

## Scope boundaries

The job numbers are evidence identifiers from this ADCD laboratory, not portable configuration values. No network configuration was changed. The damaged historical COPY1.DS01 was not reconstructed or physically deleted. Production-grade failure-domain distribution remains future work.
