# Publication Security Review

## Scope

This review applies to the curated screenshots and Markdown documentation published for Lab 01.

## Review actions

- Selected only screenshots required to prove the lab findings.
- Redacted the visible Db2 DDF host/domain and port from published screenshots.
- Did not publish raw working DOCX files or the downloaded course video.
- Did not intentionally include private IPv4 addresses.
- Did not intentionally include MAC addresses.
- Kept generic ADCD lab identifiers and IBM-supplied subsystem/user names where they are necessary to understand the z/OS evidence.

## Text scan before commit

Run from the repository root:

```bash
grep -RInE '([0-9]{1,3}\.){3}[0-9]{1,3}' . --exclude-dir=.git || true
grep -RInE '([0-9A-Fa-f]{2}[:-]){5}[0-9A-Fa-f]{2}' . --exclude-dir=.git || true
grep -RInE 'adcd\.dfw\.ibm\.com|PORT[[:space:]]+[0-9]+' . --exclude-dir=.git || true
```

Expected result for the first two scans: no private network-address evidence introduced by the lab package.

The third scan is a targeted check to ensure the session endpoint seen in the raw screenshot is not reproduced in Markdown or unredacted publication evidence.
