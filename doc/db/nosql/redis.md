---
title: "Redis — the in-memory data-structure server"
main_link: https://redis.io/
keywords: [redis, valkey, cache, kv, pubsub, data-structures, license-drama, bsl]
status: reviewed
---

# Redis — the in-memory data-structure server

**Main link:** <https://redis.io/>

Repo: <https://github.com/redis/redis> · Valkey (community fork): <https://valkey.io/> · Future of Redis (2024 license post): <https://redis.com/blog/the-future-of-redis/>

## Summary

Redis is an in-memory data-structure server: not just a key/value cache, but a server that exposes mutable data structures (strings, hashes, lists, sets, sorted sets, streams, bitmaps, HyperLogLog, geo indexes, …) over a simple TCP protocol. It optionally persists to disk (RDB snapshots, AOF append-only log), supports replication and clustering, and is the canonical choice for "I need a fast shared cache / queue / leaderboard / rate limiter" — usually all at once.

In 2024 Redis Inc. relicensed Redis 7.4+ from BSD to a dual SSPL/RSALv2 (source-available, not OSI-approved). The Linux Foundation forked the last BSD-licensed commit as **Valkey**, which is now backed by AWS, Google, Oracle, Ericsson, and others, and is the de-facto open-source continuation. As of late 2025 the two are still wire-compatible.

## Insight

When Redis is the right tool:

- **Hot cache in front of a database.** Sub-millisecond reads, TTLs, atomic operations. Pair with [[postgresql]].
- **Distributed locks, rate limiters, counters.** The atomic ops on simple types make these one-liners.
- **Pub/sub or short-lived job queues.** Streams gives you a Kafka-lite without operating Kafka.
- **Session store / shared state for stateless web tiers.**
- **Leaderboards, geo indexes, presence systems.** Sorted sets and geo commands are very nice for this.

When it's overkill or wrong:

- **In-process LRU is enough.** A single web app with one process? Just use a `lru_cache` / Caffeine / Moka. No network hop, no extra moving part.
- **You only want a key/value cache, no data structures, multi-threaded.** [Memcached](https://memcached.org/) is simpler, multi-threaded out of the box, and uses less memory per key. Redis added multi-threaded I/O (not exec) in 6.0 but the model is still single-threaded per shard.
- **You want it as your primary database.** Redis can persist, but its "the dataset must fit in RAM" property and recovery semantics make it a poor primary store. Use it as a derived cache.
- **You need real durability with strong consistency.** Redis cluster is AP under partition; check the docs and Aphyr-style analyses before assuming "it's a database".

The 2024 license drama in one paragraph: BSD-licensed Redis was being repackaged as a service by hyperscalers without contribution. Redis Inc. moved to SSPL/RSALv2 to monetise. The community forked it as Valkey under the Linux Foundation. **Valkey is the right pick** if you want to stay on a permissive open-source licence; managed providers have largely jumped ship (AWS ElastiCache for Valkey, Google Memorystore for Valkey). Redis itself remains a fine product with the **Redis Stack** ecosystem (Search, JSON, TimeSeries, Bloom) — but be aware that the licence may matter to your legal/compliance/distribution story.

## References / raw notes

> Redis is often referred to as a data structures server. What this means is that Redis provides access to mutable data structures via a set of commands, which are sent using a server-client model with TCP sockets and a simple protocol. So different processes can query and modify the same data structures in a shared way.

Properties of the data structures Redis offers:

- Stored on disk (RDB / AOF) even though they live in server memory — fast but non-volatile.
- Implemented for **memory efficiency** — a Redis hash / set / sorted-set is typically smaller than the same structure modelled in a high-level language.

Database-shaped features that come along for the ride: replication, tunable durability, clustering, high availability (Redis Sentinel + Redis Cluster).

> Another good example is to think of Redis as a more complex version of memcached, where the operations are not just SETs and GETs, but operations that work with complex data types like Lists, Sets, ordered data structures, and so forth.

Useful starting points:

- Data types intro: <https://redis.io/topics/data-types-intro>
- Try Redis in your browser: <https://try.redis.io>
- Full command reference: <https://redis.io/commands>
- Official docs: <https://redis.io/documentation>

License/fork posts:

- "The Future of Redis" (2024 announcement): <https://redis.com/blog/the-future-of-redis/>
- "Redis adopts dual source-available licensing": <https://redis.com/blog/redis-adopts-dual-source-available-licensing/>
- Valkey project: <https://valkey.io/>

Tags this connects to: `#cache` `#java` `#distributed` `#cross-platform`.

## Similar / related topics

- [Valkey](https://valkey.io/) — the Linux-Foundation-hosted BSD-licensed fork; almost certainly what you actually want today.
- [Memcached](https://memcached.org/) — simpler, pure cache, multi-threaded.
- [DragonflyDB](https://www.dragonflydb.io/) — Redis-protocol-compatible, multi-threaded rewrite in C++/Rust.
- [KeyDB](https://docs.keydb.dev/) — multi-threaded Redis fork (now under Snap Inc).
- [[surrealdb]] — multi-model DB that also wants to be a KV store.

## Internal links

<!-- reviewed -->

- [[postgresql]]
- [[surrealdb]]
- [[firebase]]

## Keywords

`#redis` `#valkey` `#cache` `#kv` `#data-structures` `#pubsub` `#license` `#bsl` `#db`
