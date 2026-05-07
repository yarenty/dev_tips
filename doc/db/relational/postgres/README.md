---
title: "Postgres ecosystem"
keywords: [postgres, postgresql, ecosystem, backup, barman, pg]
status: reviewed
review_date: 2026/05/03
---

# Postgres ecosystem

Subfolder for PostgreSQL-adjacent tooling — backups, ops, extensions, and other "you'll want this if you run Postgres seriously" projects. The canonical Postgres article itself lives one level up at [[postgresql]] (which also covers the production-gotchas piece, when to pick Postgres over alternatives, and the Pigsty / pgvector / Citus / Timescale ecosystem).

## Articles

- [Barman: PostgreSQL Backup and Recovery Manager](barman.md) — Postgres-aware remote backups and PITR, multi-server, cron-friendly. The original "DBA-shaped" backup tool.

## See also

- [[postgresql]] — start here for the language, the gotchas, and the rest of the ecosystem.
- pgBackRest — modern alternative to Barman with better parallelism and incremental support.
- WAL-G — backup tool focused on WAL archiving to object storage (S3, GCS, etc).
- pgvector / Citus / TimescaleDB — extensions worth knowing about (covered in [[postgresql]]).

## Internal links

- [[postgresql]]
- [[barman]]

## Keywords

`#postgres` `#postgresql` `#backup` `#pitr` `#dba-tools` `#db`
