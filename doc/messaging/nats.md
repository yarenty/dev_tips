---
title: nats-server
main_link: 
keywords: [nats, server, now, restart, usr]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

# nats-server

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[redpanda]] — redpanda _(score 22.9)_
- [[apache]] — apache _(score 20.9)_
- [[programming/rust/net/nats|nats]] — NATS _(score 20.2)_
- [[kafka_2]] — Kafka _(score 16.0)_
- [[kafka]] — Kafka _(score 16.0)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#nats` `#messaging` `#server` `#start` `#now` `#restart`

## TODO

- No `main_link` could be auto-detected. Add the canonical URL (project homepage / repo / paper) to the frontmatter.
- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# nats-server
To start nats-server now and restart at login:
brew services start nats-server
Or, if you don't want/need a background service you can just run:
/usr/local/opt/nats-server/bin/nats-server
