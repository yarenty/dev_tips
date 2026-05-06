---
title: "SurrealDB — multi-model document/graph/relational/KV in one Rust binary"
main_link: https://surrealdb.com/
keywords: [surrealdb, multi-model, graph, document, surrealql, rust, newsql, edge]
status: reviewed
---

# SurrealDB — multi-model document/graph/relational/KV in one Rust binary

**Main link:** <https://surrealdb.com/>

Docs: <https://surrealdb.com/docs/> · Repo: <https://github.com/surrealdb/surrealdb> · Features: <https://surrealdb.com/features>

## Summary

SurrealDB is a multi-model database written in Rust that aims to combine document, graph, relational, key-value, and time-series capabilities behind a single SQL-flavoured query language called **SurrealQL**. It can run as a single in-memory node, a single on-disk node, or a distributed cluster on top of TiKV. The pitch is "one database instead of three" — replacing the typical web-app stack of Postgres + Neo4j + Redis with a single binary.

## Insight

Where SurrealDB is genuinely tempting:

- **Greenfield apps that need graph traversals next to relational queries** — e.g. social, recommendations, knowledge graphs — without operating two databases.
- **Edge / embedded scenarios.** Single-node in-memory mode is very fast to start; on-disk mode handles datasets bigger than RAM. Tauri-style desktop apps can embed it.
- **You like the [[edgedb]] / [[fauna]] philosophy** ("the database does more for me") but want something open-source and Rust-shaped.

Honest gotchas:

- **It's young.** 1.0 shipped late 2023; even now (late 2025) production deployments are still relatively rare and the changelog moves quickly. Watch for breaking SurrealQL semantics changes.
- **Multi-model is a double-edged sword.** Doing N things means each individual axis (raw OLTP, raw graph, raw analytics) is competing against specialists ([[postgresql]], Neo4j, ClickHouse) that have decades of optimisation work.
- **The "distributed" story currently leans on TiKV.** Native multi-node Raft is on the roadmap. This means the operational story for a real distributed deployment is more complex than the marketing implies.
- **The license model has wobbled** — the core moved from Apache 2.0 to BSL (Business Source Licence) for the Surreal Cloud era. Check the current state before betting a company on it.

For a stable equivalent of "Postgres + a graph extension", `pgvector` + Apache AGE on Postgres is the boring-but-works alternative.

## Highlights from the feature matrix

What's complete (per the project's own status page at the time of these notes):

- Single-node in-memory and on-disk runtimes; distributed multi-node via TiKV.
- ACID multi-row, multi-table transactions.
- Schemafull or schemaless tables (mix in the same DB).
- Field-level definitions with types, `ASSERT` constraints, and `VALUE` defaults.
- Table events (triggers) with `$before` / `$after`.
- Indexes, including unique and composite.
- Pre-defined aggregate / materialized views.
- REST API, SurrealQL over WebSocket, JSON-RPC over WebSocket.
- Rich data types: arrays, objects, durations, datetimes, GeoJSON geometries, futures, casts.
- `SELECT` / `CREATE` / `UPDATE` / `DELETE` / `INSERT` / `RELATE` (graph edges).

## SurrealQL examples

Define a schemafull table with a constraint and an event:

```sql
DEFINE FIELD email ON TABLE user TYPE string ASSERT is::email($value);
DEFINE INDEX email ON TABLE user COLUMNS email UNIQUE;

DEFINE EVENT email ON TABLE user
  WHEN $before.email != $after.email
  THEN (
    CREATE event SET
      user   = $this,
      time   = time::now(),
      value  = $after.email,
      action = 'email_changed'
  );
```

Constraints and defaults via `ASSERT` / `VALUE`:

```sql
DEFINE FIELD countrycode ON user TYPE string
  ASSERT $value != NONE AND $value = /[A-Z]{3}/
  VALUE  $value OR 'GBR';
```

Pre-defined aggregate view (a materialized rollup):

```sql
DEFINE TABLE reading DROP;

DEFINE TABLE temperatures_by_month AS
  SELECT count() AS total,
         time::month(recorded_at) AS month,
         math::mean(temperature) AS average_temp
  FROM reading
  GROUP BY city;

CREATE reading SET
  temperature = 27.4,
  recorded_at = time::now(),
  city        = 'London',
  location    = (-0.118092, 51.509865);
```

CRUD basics:

```sql
CREATE article:surreal SET name = "SurrealDB: The next generation database";
UPDATE article:surreal SET time.created = time::now();
SELECT * FROM article, post WHERE name CONTAINS 'SurrealDB';
DELETE article:surreal;
```

Graph edges with `RELATE`:

```sql
RELATE user:tobie->write->article:surreal SET time.written = time::now();

LET $from = (SELECT users FROM company:surrealdb);
LET $devs = (SELECT * FROM user WHERE tags CONTAINS 'developer');
RELATE $from->like->$devs UNIQUE SET time.connected = time::now();
```

Insert with upsert semantics:

```sql
INSERT INTO company {
  name:     'SurrealDB',
  founded:  "2021-09-10",
  founders: [person:tobie, person:jaime],
  tags:     ['big data', 'database']
};

INSERT IGNORE INTO company (name, founded)
  VALUES ('SurrealDB', '2021-09-10')
  ON DUPLICATE KEY UPDATE tags += 'developer tools';
```

Geometries (GeoJSON-native):

```sql
UPDATE city:london SET
  centre   = (-0.118092, 51.509865),
  boundary = {
    type: "Polygon",
    coordinates: [[
      [-0.38314819, 51.37692386], [0.1785278, 51.37692386],
      [0.1785278, 51.61460570], [-0.38314819, 51.61460570],
      [-0.38314819, 51.37692386]
    ]]
  };
```

Per-row table permissions:

```sql
DEFINE TABLE post SCHEMALESS PERMISSIONS
  FOR select
    WHERE published = true OR user = $auth.id
  FOR create, update
    WHERE user = $auth.id
  FOR delete
    WHERE user = $auth.id OR $auth.admin = true;
```

## Connectivity

- **REST**:
  - `POST /sql` — execute SurrealQL.
  - `GET|POST|DELETE /key/{table}` — bulk table ops.
  - `GET|PUT|POST|PATCH|DELETE /key/{table}/{id}` — per-record ops.
- **WebSocket**: SurrealQL or JSON-RPC, bi-directional with live updates.
- **GraphQL**: planned (queries / mutations / subscriptions).

## CLI

```sh
surreal --help
# Subcommands:
#   start   — start the database server
#   backup  — backup data to/from an existing DB
#   import  — import a SurrealQL script
#   export  — export the DB as SurrealQL
#   sql     — interactive SQL REPL (with pipe support)
#   version — print version
```

Run a quick instance via Docker:

```sh
docker run --rm -p 8000:8000 surrealdb/surrealdb:latest start
```

## Client libraries

Available (or close to it): JavaScript, Deno, Golang, Rust (async), WebAssembly, Node.js native, Ember.js, React.js. On the roadmap / future: Python, C, PHP, Swift, Java, .NET, Erlang, Vue.js, Angular, Apollo GraphQL, Dart/Flutter.

## Similar / related topics

- [[postgresql]] — the boring stack: Postgres + AGE (graph) + pgvector covers a lot of the same ground.
- [[edgedb]] — narrower scope (no graph DB ambitions) but a similar "richer query language" pitch.
- [[fauna]] — was the closest commercial analogue; now sunset.
- [Neo4j](https://neo4j.com/) — the dedicated graph DB SurrealDB partly aims to displace.
- [[redis]] — what SurrealDB also wants to displace on the KV / cache axis.

## Internal links

<!-- reviewed -->

- [[postgresql]]
- [[edgedb]]
- [[fauna]]
- [[redis]]

## Keywords

`#surrealdb` `#multi-model` `#graph-database` `#document-database` `#surrealql` `#rust` `#newsql` `#db`
