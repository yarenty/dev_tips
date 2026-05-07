---
title: "MySQL / MariaDB"
main_link: https://www.mysql.com/
keywords: [mysql, mariadb, oltp, sql, replication, aurora, root-password, brew]
status: reviewed
---

# MySQL / MariaDB

**Main link:** <https://www.mysql.com/>

Docs: <https://dev.mysql.com/doc/> · MariaDB: <https://mariadb.org/>

## Summary

MySQL is the second-most-deployed open-source RDBMS after [[postgresql]], originally from MySQL AB, now owned by Oracle. **MariaDB** is the community fork that most Linux distros now ship by default in place of MySQL, since the 2010 Oracle acquisition. Both speak essentially the same wire protocol and SQL dialect, with MariaDB diverging on storage engines, JSON, and a handful of features.

## Insight

For a greenfield project in 2025, the default choice should be Postgres. But MySQL/MariaDB is genuinely the better pick when:

- **You're in the LAMP / WordPress / cPanel / Magento / phpMyAdmin world.** That entire ecosystem assumes MySQL. Fighting it is more expensive than embracing it.
- **You're on AWS Aurora MySQL.** Aurora is a fundamentally different storage layer underneath the MySQL surface and has very good economics + replication characteristics. Aurora Postgres exists but Aurora MySQL was the original and is more mature.
- **You want a specific replication topology** that MySQL does well: classical async primary→replicas, semi-sync, group replication, ProxySQL routing, Vitess sharding. Postgres replication is fine but the toolchain ecosystem is smaller.
- **You're on shared hosting.** Almost all shared hosting offers MySQL, almost none offers Postgres.

Where Postgres beats it: stricter types and constraints, real transactional DDL, much better JSON, a dramatically richer extension ecosystem, saner default behaviour around encoding and silent data truncation. If none of the MySQL-specific reasons above apply, pick Postgres.

MariaDB-specific notes: not a drop-in replacement at the storage-engine level (e.g. Aria, ColumnStore vs InnoDB-only on MySQL); the JSON column is implemented as `LONGTEXT` for compatibility; password / auth plugin defaults differ.

## Operational notes (Homebrew, macOS)

```sh
# Initial install (no root password by default — secure it!)
mysql_secure_installation

# Connect locally
mysql -uroot

# Restart
brew services restart mysql

# Foreground
/usr/local/opt/mysql/bin/mysqld_safe --datadir=/usr/local/var/mysql
```

## Reset root password (MariaDB on Ubuntu)

```sh
sudo systemctl stop mysql
sudo systemctl set-environment MYSQLD_OPTS="--skip-grant-tables --skip-networking"
sudo systemctl start mysql
sudo mysql -u root
```

Then in the MariaDB shell:

```sql
SELECT User FROM mysql.user;
-- +-------------+
-- | User        |
-- +-------------+
-- | admin       |
-- | mariadb.sys |
-- | root        |
-- +-------------+

ALTER USER 'root'@'localhost' IDENTIFIED VIA mysql_native_password;
SET PASSWORD FOR 'root'@'localhost' = PASSWORD('pass');
FLUSH PRIVILEGES;
EXIT;
```

Reset the systemd environment and restart:

```sh
sudo systemctl set-environment MYSQLD_OPTS=""
sudo systemctl stop mysql
sudo systemctl start mysql
sudo mysql -u root -p
```

## Add a user / database

```sql
CREATE USER 'todo'@'localhost' IDENTIFIED BY 'pass2';
GRANT ALL PRIVILEGES ON todo.* TO 'todo'@'localhost';
FLUSH PRIVILEGES;
```

## Similar / related topics

- [[postgresql]] — the canonical alternative; usually the better default for new work.
- [MariaDB](https://mariadb.org/) — the community fork most Linux distros now ship.
- [[singlestore]] — wire-compatible-ish with MySQL, distributed and HTAP.
- [[limbo]] / SQLite — when you don't actually need a server.
- [Vitess](https://vitess.io/) — horizontal sharding layer over MySQL (powers YouTube, Slack).

## Internal links

- [[postgresql]]
- [[singlestore]]
- [[limbo]]
- [[barman]]

## Keywords

`#mysql` `#mariadb` `#relational` `#oltp` `#sql` `#aurora` `#replication` `#db`
