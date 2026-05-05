---
title: Garnet
main_link: https://github.com/microsoft/garnet?tab=readme-ov-file
keywords: [garnet, caching, programming, microsoft, redis, cache]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# Garnet

**Main link:** <https://github.com/microsoft/garnet?tab=readme-ov-file>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[cache]] — Cache _(score 20)_
- [[caching]] — Caching _(score 15)_
- [[redis]] — Redis _(score 15)_
- [[rtic]] — RTIC _(score 15)_
- [[map_cache_storage]] — DashMap _(score 10)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->

## Keywords

`#garnet` `#caching` `#programming` `#microsoft` `#redis` `#cache` `#research`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Garnet

https://github.com/microsoft/garnet?tab=readme-ov-file


Garnet is a new remote cache-store from Microsoft Research, that offers several unique benefits:

- Garnet adopts the popular RESP wire protocol as a starting point, which makes it possible to use Garnet from unmodified Redis clients available in most programming languages of today, such as StackExchange.Redis in C#.
- Garnet offers much better throughput and scalability with many client connections and small batches, relative to comparable open-source cache-stores, leading to cost savings for large apps and services.
- Garnet demonstrates extremely low client latencies (often less than 300 microseconds at the 99.9th percentile) using commodity cloud (Azure) VMs with accelerated TCP enabled, which is critical to real-world scenarios.
- Based on the latest .NET technology, Garnet is cross-platform, extensible, and modern. It is designed to be easy to develop for and evolve, without sacrificing performance in the common case. We leveraged the rich library ecosystem of .NET for API breadth, with open opportunities for optimization. Thanks to our careful use of .NET, Garnet achieves state-of-the-art performance on both Linux and Windows.

This repo contains the code to build and run Garnet. For more information and documentation, check out our website at https://microsoft.github.io/garnet.
