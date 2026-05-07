---
title: "Garnet — Microsoft's RESP-compatible cache server"
main_link: https://github.com/microsoft/garnet
keywords: [garnet, redis, cache, microsoft, dotnet, csharp, resp, kv-store]
status: reviewed
review_date: 2026/05/03
---

# Garnet — Microsoft's RESP-compatible cache server

**Main link:** <https://github.com/microsoft/garnet>

Docs: <https://microsoft.github.io/garnet/>

## Summary

Garnet is a remote cache-store from Microsoft Research, written in C# / .NET, that speaks the Redis RESP wire protocol. That means existing Redis client libraries (StackExchange.Redis, redis-py, ioredis, jedis, redis-rs, ...) can talk to it unchanged. Microsoft's pitch is materially-better throughput and scalability with many concurrent client connections and small batches, especially on multi-core hardware, with sub-millisecond p99.9 latencies on commodity cloud VMs. Cross-platform on Linux and Windows. MIT-licensed.

## Insight

Garnet's interesting position in the cache-server landscape:

- **vs. Redis**: same wire protocol, dramatically better multi-core utilization (Redis is single-threaded on the data path; Garnet scales across cores). The cost is feature-completeness — Garnet covers the popular subset of Redis commands but is not a 100% drop-in for every edge case (Lua scripting, modules, certain stream behaviors). Cluster mode exists but is younger and less battle-tested than Redis Cluster.
- **vs. Dragonfly**: similar "Redis-but-multi-threaded" pitch. Dragonfly is C++ and uses a shared-nothing architecture with io_uring; Garnet leans on .NET's async runtime and the [Tsavorite](https://github.com/microsoft/Tsavorite) (formerly FASTER) storage engine. Roughly comparable on single-host throughput; pick on operational fit.
- **vs. KeyDB**: KeyDB is a multi-threaded fork of Redis itself; Snap acquired it and the project's pace has slowed. Garnet is the more actively-developed alternative.
- **vs. Valkey**: Valkey is the Linux-Foundation BSD fork of Redis after the 2024 SSPL relicense. If you want "real Redis but actually open source", Valkey is the conservative choice; Garnet is the bet on a redesigned engine.

When to reach for Garnet specifically:

- You're already a .NET shop and want a cache server you can extend in C#.
- You need single-host throughput well past what one Redis instance gives you, but you don't want the operational complexity of a Redis Cluster yet.
- You want first-class TLS, ACLs, and snapshot/AOF persistence, all wired in.

When to *not* reach for it:

- You depend on Redis modules (RedisJSON, RediSearch, RedisGraph, RedisBloom). Not supported.
- You need extensive Lua scripting. Limited / not at parity.
- You need a battle-tested cluster mode at large scale today.

## Similar / related topics

- [[redis]] — the incumbent that defines the wire protocol.
- [Valkey](https://valkey.io/) — Linux Foundation BSD fork of Redis post-relicense.
- [Dragonfly](https://www.dragonflydb.io/) — C++ multi-threaded RESP-compatible server.
- [KeyDB](https://docs.keydb.dev/) — older multi-threaded Redis fork.
- [Tsavorite (FASTER)](https://github.com/microsoft/Tsavorite) — the underlying log-structured store.

## Internal links

- [[redis]] — the protocol Garnet implements; benchmarks here are against Redis.
- [[programming/caching/README|caching]] — parent section.
- [[postgresql]] — Garnet is often used as a hot cache *in front of* a relational store.

## Keywords

`#garnet` `#redis` `#cache` `#caching` `#microsoft` `#dotnet` `#csharp` `#resp` `#kv-store` `#programming`

## References / raw notes

From the upstream README:

> Garnet is a new remote cache-store from Microsoft Research, that offers several unique benefits:
>
> - Garnet adopts the popular RESP wire protocol as a starting point, which makes it possible to use Garnet from unmodified Redis clients available in most programming languages of today, such as StackExchange.Redis in C#.
> - Garnet offers much better throughput and scalability with many client connections and small batches, relative to comparable open-source cache-stores, leading to cost savings for large apps and services.
> - Garnet demonstrates extremely low client latencies (often less than 300 microseconds at the 99.9th percentile) using commodity cloud (Azure) VMs with accelerated TCP enabled, which is critical to real-world scenarios.
> - Based on the latest .NET technology, Garnet is cross-platform, extensible, and modern. It is designed to be easy to develop for and evolve, without sacrificing performance in the common case. We leveraged the rich library ecosystem of .NET for API breadth, with open opportunities for optimization. Thanks to our careful use of .NET, Garnet achieves state-of-the-art performance on both Linux and Windows.
