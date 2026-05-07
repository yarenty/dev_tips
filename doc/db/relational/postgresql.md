---
title: "PostgreSQL"
main_link: https://www.postgresql.org/
keywords: [postgresql, postgres, oltp, sql, work-mem, explain, ctes, triggers, pigsty, pgvector]
status: reviewed
review_date: 2026/05/03
---

# PostgreSQL

**Main link:** <https://www.postgresql.org/>

Docs: <https://www.postgresql.org/docs/> · Pigsty (opinionated PG distribution): <https://pigsty.io/blog/pg/pg-eat-db-world/>

## Summary

PostgreSQL is the world's most boringly-good open-source relational database: ACID, MVCC, strong types, SQL standard compliance, and a sprawling extension ecosystem (pgvector, PostGIS, TimescaleDB, Citus, hstore, …). It's been actively developed since the mid-1990s, is BSD-style licensed, and powers everything from a Raspberry Pi side-project to multi-petabyte fleets.

For greenfield work in 2025, "use Postgres" is almost always the correct first answer.

## Insight

When to pick Postgres over the alternatives:

- **vs [[mysql]] / MariaDB** — Postgres has stricter semantics (real check constraints, real `NULL` handling, real transactional DDL), better JSON, better extension story, and a saner geometry / range / array story. MySQL still wins in the WordPress/cPanel ecosystem, on AWS Aurora MySQL (which is a different beast under the hood), and where you specifically want the MySQL replication topology / ProxySQL ecosystem.
- **vs SQLite / [[limbo]]** — pick SQLite when the workload fits in one process; pick Postgres the moment you need concurrent writers, network access, or proper roles.
- **vs the "NewSQL" set ([[fauna]], [[edgedb]], [[surrealdb]], [[singlestore]])** — Postgres is older, more boring, more documented, has more extensions, and won't be sunset by a startup runway event. Reach for the others only when you have a specific reason.
- **vs going to NoSQL** — `jsonb` + GIN indexes covers most "I want a document store" needs; `pg_partman` + `BRIN` covers most "I want a time-series store" needs; `pgvector` covers most "I need a vector DB" needs.

The Postgres ecosystem worth knowing:

- **Pigsty** — opinionated PG-as-a-distribution with ~150 packaged extensions, monitoring, HA, PITR. The "Postgres eats the database world" thesis (link above) is half-marketing, half-correct.
- **pgvector** — vector similarity search as a Postgres extension. Often replaces a dedicated vector DB. See [[chroma]] and [[qdrant]] for when you actually do want a dedicated one.
- **Citus** — sharded Postgres for horizontal scale.
- **TimescaleDB** — time-series extension; hypertables, continuous aggregates.
- **[[barman]]**, pgBackRest, WAL-G — the real backup tools (do not rely on `pg_dump` for production).

## 9 production gotchas

Adapted notes from "Previously on Extreme Learning… all the ways I've broken production with Postgres". Most of these don't bite while your DB is small; they bite later, when changing them is expensive.

### 1. Don't keep the default `work_mem`

`work_mem` governs how much memory each query operation can use before spilling to temp files on disk. The default is tiny. Local dev runs fine; prod runs fine at first; then your data and concurrency grow and you get latency spikes (and full outages) as hashes/sorts spill.

A reasonable starting heuristic from the greybeards:

```text
work_mem = ($INSTANCE_MEMORY * 0.8 - shared_buffers) / $ACTIVE_CONNECTION_COUNT
```

Run logs through `pgbadger` or use a tool like pganalyze to detect when you're under-sized.

### 2. Don't push all your application logic into Postgres functions

Stored procedures and functions are not zero-cost — they consume the same CPU/memory budget as queries. Simple `IMMUTABLE` / `STABLE` functions are fine. Nested or recursive functions chew through your scaling headroom and can manifest as unexplained latency or replication lag. Application nodes are also far cheaper to scale than the DB; postpone DB scaling for as long as you can.

### 3. Don't sprinkle triggers everywhere

Two reasons:

- They're often less efficient than the alternative (generated columns, materialized views).
- They encourage event-shaped thinking, which works against batching. Application code naturally batches inserts; trigger graphs hide opportunities to do so.

A useful discipline: at most one `BEFORE` and one `AFTER` trigger per table, named generically (`before_foo`, `after_foo`), with all logic inline and dispatched on `TG_OP`. Resist the urge to refactor into "clean" smaller trigger functions.

### 4. Don't lean on `NOTIFY` as a message queue

`LISTEN`/`NOTIFY` is convenient but not free, especially when listeners read more rows to handle each event — every notification then implies extra reads. For anything high-volume, write events to a FIFO table and consume them in batches.

A serviceable schema:

```sql
CREATE TABLE event_queue (
  id          uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id     uuid NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  type        text NOT NULL,
  data        jsonb NOT NULL,
  created_at  timestamptz NOT NULL DEFAULT now(),
  occurred_at timestamptz NOT NULL,
  acquired_at timestamptz,
  failed_at   timestamptz
);
```

Acquire batches safely with `FOR UPDATE SKIP LOCKED`:

```sql
UPDATE event_queue
SET    acquired_at = now()
WHERE  id IN (
  SELECT id
  FROM   event_queue
  WHERE  acquired_at IS NULL
  ORDER  BY occurred_at
  FOR    UPDATE SKIP LOCKED
  LIMIT  1000
)
RETURNING *;
```

Delete acquired events in batches afterwards. For long-term event history of unbounded size, use something other than Postgres ([[kafka]], [[redpanda]], object storage).

### 5. Don't run `EXPLAIN ANALYZE` against fake data

`EXPLAIN ANALYZE` actually runs the query, so it's accurate — but only useful if the data you're running it against is realistic. A few seed rows in your local DB tell you nothing.

The pattern that works: keep a sandbox instance alongside production, restored regularly from a recent prod backup, sized smaller than prod (so it's more constrained). Run `EXPLAIN ANALYZE` there before shipping new queries.

### 6. CTEs vs subqueries — measure both

Old advice: "prefer subqueries over CTEs because CTEs are an optimisation fence". As of Postgres 12 the optimiser is much smarter and will often inline non-recursive CTEs as if they were subqueries. Still, if a CTE-heavy query is slow, rewriting as a subquery can occasionally help — `EXPLAIN` both. Read the "CTE Materialization" section of the docs for the current rules.

### 7. Recursive CTEs are elegant but slow at scale

If your model is a graph and you reach for `WITH RECURSIVE`, fine — until the graph grows. Reads typically vastly outnumber writes, so consider denormalising to a materialized view or table where each subgraph is a single row with all the columns the query needs. Reads become a `SELECT`, writes get more expensive. Often a good trade.

### 8. Postgres does not auto-index foreign keys

Unlike MySQL, Postgres does **not** create an index on a column you mark as a `FOREIGN KEY`. Two consequences:

- Joins on the FK can be slow (visible in `EXPLAIN`).
- More insidiously, `ON DELETE` / `ON UPDATE CASCADE` performance can collapse on big tables. Adding an index on the FK column often delivers huge wins here.

### 9. `IS NOT DISTINCT FROM` bypasses indexes

`= NULL` returns `NULL`, not `false` — so people reach for `IS DISTINCT FROM` / `IS NOT DISTINCT FROM`, which treat `NULL` as a regular value. The catch: those operators don't use the index that `=` would, so you get a Seq Scan.

If you have:

```sql
SELECT * FROM foo
WHERE bar IS NOT DISTINCT FROM baz;
```

Force index use by handling the null case explicitly:

```sql
SELECT * FROM foo
WHERE (bar IS NULL AND baz IS NULL)
   OR bar = baz;
```

## Operational notes

### Homebrew install / restart

```sh
# Homebrew default cluster initialisation
initdb --locale=C -E UTF-8 /usr/local/var/postgres
# See: https://www.postgresql.org/docs/14/app-initdb.html

# Restart after upgrade
brew services restart postgresql

# Or run in foreground
/usr/local/opt/postgresql/bin/postgres -D /usr/local/var/postgres
```

For a major version bump:

```sh
brew postgresql-upgrade-database
```

### Versioned formula (e.g. `postgresql@14`)

```sh
initdb --locale=C -E UTF-8 /usr/local/var/postgresql@14
brew services restart postgresql@14
# Or:
/usr/local/opt/postgresql@14/bin/postgres -D /usr/local/var/postgresql@14
```

## Similar / related topics

- [[mysql]] — the other big open-source RDBMS; honest comparison above.
- [[edgedb]] — Postgres-backed but with a typed object/graph query language (now Gel).
- [[surrealdb]] — multi-model challenger; very different model.
- [[barman]] — production-grade Postgres backup tool.
- [Pigsty](https://pigsty.io/) — packaged Postgres distribution with extensions, monitoring, HA.

## Internal links

- [[mysql]]
- [[barman]]
- [[edgedb]]
- [[limbo]]
- [[surrealdb]]

## Keywords

`#postgresql` `#postgres` `#relational` `#oltp` `#sql` `#explain` `#triggers` `#ctes` `#pgvector` `#db`
