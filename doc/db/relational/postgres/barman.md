---
title: "Barman — PostgreSQL Backup and Recovery Manager"
main_link: https://pgbarman.org/
keywords: [barman, postgresql, backup, pitr, recovery, enterprisedb, gpl, python]
status: reviewed
review_date: 2026/05/03
---

# Barman — PostgreSQL Backup and Recovery Manager

**Main link:** <https://pgbarman.org/>

Repo: <https://github.com/EnterpriseDB/barman> · Docs: <https://docs.pgbarman.org/> · Maintainer: <https://www.enterprisedb.com/>

## Summary

Barman ("Backup And Recovery Manager") is a Python-written, Postgres-aware tool for managing physical backups, WAL archiving, and point-in-time recovery (PITR) of one or many PostgreSQL servers from a single backup host. Maintained by EnterpriseDB and released under GPLv3, it's one of the original purpose-built backup tools for Postgres and is still in heavy use, particularly in classic on-premise / VM-shaped fleets.

## Insight

Where Barman fits today (vs the alternatives):

- **vs `pg_dump` / `pg_dumpall`** — `pg_dump` produces logical backups (a SQL script). It's fine for small DBs and migrations but useless as a serious backup strategy: no PITR, no incremental, no continuous WAL stream, restore is a full replay. Barman gives you physical base backups + WAL archiving + PITR.
- **vs pgBackRest** — pgBackRest is the modern alternative, written in C with better parallelism, much better incremental/differential backup, and more aggressive verification. For a new fleet starting today, pgBackRest is usually the better default.
- **vs WAL-G** — WAL-G is the "backup straight to object storage" choice (S3, GCS, Azure Blob), single-file WAL archiving, very cloud-native. Pick WAL-G if your truth is "everything goes in S3"; pick Barman if your truth is "we have a backup server in the rack".
- **vs cloud-managed Postgres** (RDS, Aurora, Cloud SQL, Neon) — those handle backups for you. Barman is for self-managed clusters.

When Barman is still a good pick:

- You operate Postgres on dedicated hosts/VMs and want a centralised backup server that pulls from multiple primaries.
- You're already running an EnterpriseDB-flavoured stack.
- You want a tool with a long, conservative track record in regulated environments where "the same way we've done it for 10 years" is a feature.

Common operational pattern: Barman host with large disk → pulls base backups from each primary on a schedule, continuously archives WAL via `pg_receivewal` or SSH, exposes restore commands via the `barman` CLI. Cron-friendly, easy to monitor.

License caveat: GPLv3. For most users this is fine (Barman is a tool you run, not a library you link). Check before redistributing as part of a closed product.

## References / raw notes

Key facts:

- Open-source administrative tool for disaster recovery of PostgreSQL servers.
- Enables remote physical backups of multiple PG servers.
- Continuous WAL archiving + base backups → point-in-time recovery.
- Written in Python.
- Maintained by EnterpriseDB.
- Distributed under GNU GPL v3.

Useful follow-ups:

- Recommended Postgres backup strategy patterns: weekly full + daily incremental + continuous WAL.
- Compare Barman's RPO/RTO characteristics against pgBackRest and WAL-G for your specific topology.
- Test the restore. Untested backups are not backups.

## Similar / related topics

- [[postgresql]] — what you're backing up.
- [pgBackRest](https://pgbackrest.org/) — modern Postgres backup tool, usually the better default for new deployments.
- [WAL-G](https://github.com/wal-g/wal-g) — object-storage-first backup tool.
- `pg_dump` / `pg_dumpall` — logical backups, useful for migrations, not a real backup strategy by themselves.
- [Pigsty](https://pigsty.io/) — packaged Postgres distribution that bundles backup tooling.

## Internal links

- [[postgresql]]

## Keywords

`#barman` `#postgresql` `#backup` `#pitr` `#wal-archiving` `#enterprisedb` `#gpl` `#dba-tools` `#db`
