---
title: "NoSQL databases"
keywords: [nosql, kv, cache, redis, document, firebase, wide-column, hbase]
status: reviewed
---

# NoSQL databases

Landing page for non-relational data stores. The "NoSQL" label is overloaded — it covers caches, document stores, wide-column stores, KV stores, and BaaS platforms. This subsection collects the few that have come up in practice.

## How to pick one

- **You need a fast in-memory cache or pub/sub broker.** [[redis]] (or its 2024 community fork Valkey).
- **You want a hosted backend with a JSON-document API and auth/storage/realtime baked in.** [[firebase]] (Firestore primarily). Compare with [Supabase](https://supabase.com/) (Postgres-backed), [PocketBase](https://pocketbase.io/), [Appwrite](https://appwrite.io/).
- **You're maintaining a Hadoop estate that already includes a wide-column store.** [[hbase]] is the historical pick. For new work, look at Cassandra / ScyllaDB / DynamoDB instead.
- **You think you want a document DB.** Try [[postgresql]] with `jsonb` first. The threshold to actually need MongoDB / DynamoDB is higher than people think.

## Articles

- [Redis](redis.md) — KV / data-structure server, cache, queue, pub/sub. The Swiss army knife.
- [firebase](firebase.md) — Google's BaaS platform; Firestore is the document DB at the centre.
- [hbase](hbase.md) — Apache HBase, the Hadoop-era wide-column store.

## See also

- `db/relational/` — for "I think I want a document DB" cases (`jsonb` on Postgres usually wins).
- `db/streaming/` and `messaging/` — when the actual need is a queue/log, not a database.
- `db/timeseries/` — when the workload is metrics / events over time.
- `db/vector/` — when the workload is similarity search.

## Internal links

<!-- reviewed -->

- [[redis]]
- [[firebase]]
- [[hbase]]
- [[postgresql]]

## Keywords

`#nosql` `#kv` `#cache` `#document-db` `#wide-column` `#baas` `#db`
