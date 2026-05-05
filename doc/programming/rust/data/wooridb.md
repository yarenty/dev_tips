---
title: WooriDB
main_link: https://github.com/naomijub/wooridb
keywords: [wooridb, rust, entity, databases, prometheus]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

> Auto-split from `doc/programming/rust/data/db.md` by `article_split.py`. Heading: **WooriDB**.

# WooriDB

**Main link:** <https://github.com/naomijub/wooridb>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[r2d2]] — r2d2 _(score 22.5)_
- [[db]] — diesel _(score 22.5)_
- [[prometheus]] — prometheus _(score 18.9)_
- [[diesel]] — diesel _(score 18.5)_
- [[programming/rust/data/greptimedb|greptimedb]] — GreptimeDB _(score 17.1)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#wooridb` `#data` `#rust` `#programming` `#entity` `#database` `#time` `#datetime`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# WooriDB

https://github.com/naomijub/wooridb

WooriDB is a general purpose (EXPERIMENTAL) time serial database, which means it contains all entities registries indexed by DateTime. It is schemaless, key-value storage and uses its own query syntax that is similar to SparQL and Crux's Datalog.

Some other features are:

- Hashing keys content with ENCRYPT keyword.
- Hashed values are filtered out and can only be checked with CHECK keyword.
- Ron schemas for input and output.
- JSON is supported via feature.
- Entities are indexed by entity_name (Entity Tree), DateTime (Time Serial) and Uuid (Entity ID). Entity format is a HashMap where keys are strings and values are supported Types.
- Stores persistent data locally.
- Able to handle very large numbers when using the P suffix.  Ex: 98347883122138743294728345738925783257325789353593473247832493483478935673.9347324783249348347893567393473247832493483478935673P.
- Configuration is done via environment variables.
- Authentication and Authorization via session token
- Conditional Update
- Some Relation Algebra
- Entity history

Woori means our and although I developed this DB initially alone, it is in my culture to call everything that is done for our community and by our community ours.

This project is hugely inspired by:

- Crux;
- Datomic;
- Prometheus
- SparQL.
- Database Internals
- Database System Concept
- Designing Data Intensive Application
- Professor Andy Pavlo Database classes.
- Zero Trust in Time Series Data?
