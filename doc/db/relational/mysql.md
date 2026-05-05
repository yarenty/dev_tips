---
title: mysql
main_link: 
keywords: [mysql, relational, db, root, password, secure, reset]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# mysql

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[programming/rust/data/mysql|mysql]] — mysql _(score 15)_
- [[xlite]] — XLite _(score 13)_
- [[limbo]] — Limbo _(score 13)_
- [[toydb]] — toyDB _(score 13)_
- [[edgedb]] — edgeDB _(score 13)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->

## Keywords

`#mysql` `#relational` `#db` `#root` `#password` `#secure` `#reset`

## TODO

- No `main_link` could be auto-detected. Add the canonical URL (project homepage / repo / paper) to the frontmatter.
- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# mysql
We've installed your MySQL database without a root password. To secure it run:
mysql_secure_installation

MySQL is configured to only allow connections from localhost by default

To connect run:
```shell
mysql -uroot
```

To restart mysql after an upgrade:
```shell
brew services restart mysql
```
Or, if you don't want/need a background service you can just run:

```shell
/usr/local/opt/mysql/bin/mysqld_safe --datadir=/usr/local/var/mysql
```


## Reset root password


```shell

sudo systemctl stop mysql

sudo systemctl set-environment MYSQLD_OPTS="--skip-grant-tables --skip-networking"

sudo systemctl start mysql

sudo mysql -u root

Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 32
Server version: 10.6.16-MariaDB-0ubuntu0.22.04.1 Ubuntu 22.04
Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> SELECT User FROM mysql.user;
+-------------+
| User        |
+-------------+
| admin       |
| mariadb.sys |
| root        |
+-------------+
3 rows in set (0.001 sec)


MariaDB [(none)]> ALTER USER 'root'@'localhost' IDENTIFIED VIA mysql_native_password;
Query OK, 0 rows affected (0.003 sec)

MariaDB [(none)]> SET PASSWORD FOR 'root'@'localhost' = PASSWORD('pass');
Query OK, 0 rows affected (0.001 sec)

MariaDB [(none)]> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.000 sec)

MariaDB [(none)]> exit;



sudo systemctl set-environment MYSQLD_OPTS=""
sudo systemctl stop mysql
sudo systemctl start mysql
sudo mysql -u root -p

```


## Add user

```shell

MariaDB [(none)]> create user 'todo'@localhost identified by 'pass2';
Query OK, 0 rows affected (0.003 sec)

MariaDB [(none)]> grant ALL PRIVILEGES  on todo.* to 'todo'@'localhost';
Query OK, 0 rows affected (0.001 sec)

MariaDB [(none)]> flush privileges;
Query OK, 0 rows affected (0.000 sec)
```
