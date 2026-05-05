---
title: NATS
main_link: https://crates.io/crates/nats
keywords: [nats, rust, programming, jetstream, highly, generation]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

> Auto-split from `doc/programming/rust/net/mq.md` by `article_split.py`. Heading: **NATS**.

# NATS

**Main link:** <https://crates.io/crates/nats>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[mq]] — ruMQTT _(score 23)_
- [[rtic]] — RTIC _(score 20)_
- [[http]] — Hyper _(score 20)_
- [[messaging/mqtt|mqtt]] — MQTT _(score 15)_
- [[grpc]] — GRPC _(score 13)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->

## Keywords

`#nats` `#rust` `#programming` `#jetstream` `#highly` `#next` `#generation`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# NATS

https://crates.io/crates/nats


## NATS 2.2
NATS 2.2 is the largest feature release since version 2.0. The 2.2 release provides highly scalable, highly performant, secure and easy-to-use next generation streaming in the form of JetStream, allows remote access via websockets, has simplified NATS account management, native MQTT support, and further enables NATS toward our goal of securely democratizing streams and services for the hyperconnected world we live in.

## Next Generation Streaming

JetStream is the next generation streaming platform for NATS, highly resilient, highly available, and easy to use. We’ve spent a long time listening to our community, learning from our experiences, looking at the needs of today, and thinking deeply about the needs of tomorrow. We built JetStream to address these needs.
JetStream:
- is easy to deploy and manage, built into the NATS server
- simplifies and accelerates development
- supports wildcard subjects
- supports at least once delivery and exactly once within a window
- is horizontally scalable at runtime with no interruptions
- persists data via streams and delivers or replays via consumers
- supports multiple patterns to consume data on the same stream
- supports push and pull modes when consuming messages
- is account aware
- allows for detailed granularity of security, by stream, by consumer, by function
