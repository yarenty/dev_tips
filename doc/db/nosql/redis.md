---
title: Redis
main_link: https://github.com/redis/redis
keywords: [redis, nosql, structures, cache, distributed, programming]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

# Redis

**Main link:** <https://github.com/redis/redis>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[firebase]] — firebase _(score 23.4)_
- [[hbase]] — hbase _(score 23.4)_
- [[cache]] — Cache _(score 18.3)_
- [[garnet]] — Garnet _(score 15.2)_
- [[loco_rust_on_rails]] — Loco _(score 11.4)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#redis` `#nosql` `#db` `#data` `#structures` `#server` `#commands`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Redis


https://github.com/redis/redis


Redis is often referred to as a data structures server. What this means is that Redis provides access to mutable data structures via a set of commands, which are sent using a server-client model with TCP sockets and a simple protocol. So different processes can query and modify the same data structures in a shared way.

Data structures implemented into Redis have a few special properties:

Redis cares to store them on disk, even if they are always served and modified into the server memory. This means that Redis is fast, but that it is also non-volatile.
The implementation of data structures emphasizes memory efficiency, so data structures inside Redis will likely use less memory compared to the same data structure modelled using a high-level programming language.
Redis offers a number of features that are natural to find in a database, like replication, tunable levels of durability, clustering, and high availability.
Another good example is to think of Redis as a more complex version of memcached, where the operations are not just SETs and GETs, but operations that work with complex data types like Lists, Sets, ordered data structures, and so forth.

If you want to know more, this is a list of selected starting points:

- Introduction to Redis data types. https://redis.io/topics/data-types-intro
- Try Redis directly inside your browser. https://try.redis.io
- The full list of Redis commands. https://redis.io/commands
- There is much more inside the official Redis documentation. https://redis.io/documentation



https://redis.com/blog/the-future-of-redis/



https://redis.com/blog/redis-adopts-dual-source-available-licensing/


Checking obsidian..
this could be connected to 
#cache #java #distributed #cross-platform
