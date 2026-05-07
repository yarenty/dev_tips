---
title: "Caching"
keywords: [caching, redis, garnet, in-process, cdn, eviction, cache]
status: reviewed
review_date: 2026/05/03
---

# Caching

A small section on caching layers — both in-process and out-of-process. Caching is one of the few techniques that can reliably knock orders of magnitude off latency, but it also creates entire categories of bugs (stale reads, thundering herds, dogpile, cache stampedes, invalidation races). Most of the substance lives elsewhere; this folder mostly tracks notable cache-server projects.

## When to reach for what

- **In-process** (Caffeine for the JVM, Moka for Rust, `lru` crates) — when the hot data fits in memory and you don't need cross-process coherence. Fastest possible path; loses everything on restart.
- **External KV cache** ([[redis]], Memcached, [[garnet]], Dragonfly, KeyDB) — when multiple processes/hosts share the cache, when you want persistence as a bonus, or when you need data structures (sorted sets, streams).
- **CDN / edge cache** (Cloudflare, Fastly, Bunny) — for HTTP responses, static assets, and for pushing the cache geographically close to users.
- **Read-through query cache in the database** — usually a trap; prefer explicit application-side caching.

## Articles

- [[garnet]] — Microsoft's RESP-compatible cache server in C# / .NET.

## See also

- [[redis]] — the de-facto incumbent; everything in this space is benchmarked against it.
- [[programming/rust/README|Rust]] — `moka`, `cached`, `quick-cache` for in-process caching.
- [[indexing]] — adjacent: caching and indexing trade off similar concerns.
- [Designing Data-Intensive Applications](https://dataintensive.net/) — chapter on caching is the standard reference.

## Keywords

`#caching` `#cache` `#redis` `#garnet` `#in-process-cache` `#cdn` `#programming`
