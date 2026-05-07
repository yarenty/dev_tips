---
title: sqlmap
main_link: https://github.com/sqlmapproject/sqlmap
keywords: [sqlmap, sql-injection, pentest, security, owasp, defensive, audit, ethics]
status: reviewed
---

# sqlmap

**Main link:** <https://github.com/sqlmapproject/sqlmap>

Homepage: <https://sqlmap.org/> · Manual: <https://github.com/sqlmapproject/sqlmap/wiki>

## Summary

`sqlmap` is the long-standing open-source penetration-testing tool for
**detecting and exploiting SQL injection** flaws — and, once a foothold is
found, automating database takeover (fingerprinting, schema dump, file-system
access, OS command execution via out-of-band channels). It supports the
major engines (MySQL, PostgreSQL, Oracle, SQL Server, SQLite, and many more)
and the major injection techniques (boolean-based blind, time-based blind,
error-based, UNION-based, stacked queries, out-of-band).

## Insight

The defensible use case is **auditing your own SQL endpoints** — running it
against your dev/staging API to confirm parameterised queries actually work,
or pointing it at a deliberately-vulnerable test app to learn what the
attack surface looks like. Pair it with the OWASP Juice Shop or DVWA for
practice.

**Ethics / legal**: never run sqlmap against systems you don't own or have
explicit written authorisation to test. SQL injection is a criminal offence
basically everywhere, and sqlmap leaves obvious fingerprints in logs.
"Curiosity" is not a defence.

What's actually useful in practice:

- **Detection over exploitation.** Use `--batch` and the lower-risk levels
  (`--level=1 --risk=1`) for "is this endpoint vulnerable at all?" runs
  against your own apps. Higher levels start sending payloads that *will*
  show up as alarming in any IDS / WAF.
- **Fingerprint mode** (`--fingerprint`, `--banner`) is a benign-ish way to
  confirm what database is actually behind an API.
- **Tamper scripts** are sqlmap's payload-mutation library; useful for
  understanding what your WAF rules actually catch.
- **Output is noisy.** Treat sqlmap reports the way you'd treat any scanner:
  triage manually, don't trust everything as a true positive.

Modern alternatives / complements (not replacements):

- **OWASP ZAP** / **Burp Suite** — broader web-app pentest frameworks; both
  have SQL injection modules but go far beyond.
- **NoSQLMap** — analogous tool for NoSQL injection (MongoDB, etc.).
- **Ghauri** — newer SQL-injection automation, similar shape.
- **CodeQL** / **Semgrep** — static analysis to find SQLi *before* deploy;
  much higher signal-to-noise than runtime fuzzing.

## Quick reference

```sh
# Clone
git clone --depth 1 https://github.com/sqlmapproject/sqlmap.git sqlmap-dev

# Basic options
python sqlmap.py -h

# All options
python sqlmap.py -hh

# Detection-only against your own test endpoint
python sqlmap.py -u 'https://test.example.com/item?id=1' --batch --level=1 --risk=1
```

Runs on Python 2.6+, 2.7, and 3.x on any platform.

## Similar / related topics

- **OWASP ZAP** — open-source web-app security scanner.
- **Burp Suite** — commercial web-app pentest framework (Community edition is
  free).
- **NoSQLMap** — SQL injection's NoSQL cousin.
- **Ghauri** — modern SQLi automation alternative.
- **CodeQL / Semgrep** — static analysis for finding injection at build time.

## Internal links

- [[db/tools/README|tools]] — section landing.
- [[db/relational/README|relational]] — engines whose query parsers sqlmap
  exercises.
- [[rustscan]] — adjacent network reconnaissance.
- [[sniffnet]] — adjacent traffic visibility.
- [[shodan]] — adjacent attack-surface discovery.

## Keywords

`#sqlmap` `#sql-injection` `#pentest` `#security` `#owasp` `#defensive` `#audit` `#ethics`

## References / raw notes

- Repo: <https://github.com/sqlmapproject/sqlmap>
- Homepage: <https://sqlmap.org/>
- User's manual: <https://github.com/sqlmapproject/sqlmap/wiki>
- FAQ: <https://github.com/sqlmapproject/sqlmap/wiki/FAQ>
- Issue tracker: <https://github.com/sqlmapproject/sqlmap/issues>
- Demo runs (asciinema): <https://asciinema.org/a/46601>
