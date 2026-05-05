---
title: toyDB
main_link: https://github.com/erikgrinaker/toydb/tree/main**
keywords: [toydb, sql, distributed, based, databases, rust]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

# toyDB

**Main link:** <https://github.com/erikgrinaker/toydb/tree/main**>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[edgedb]] — edgeDB _(score 31.5)_
- [[limbo]] — Limbo _(score 26.6)_
- [[xlite]] — XLite _(score 26.6)_
- [[db/relational/mysql|mysql]] — mysql _(score 26.6)_
- [[fauna]] — fauna _(score 26.6)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#toydb` `#relational` `#db` `#sql` `#distributed` `#based` `#main`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# toyDB


**https://github.com/erikgrinaker/toydb/tree/main**

Distributed SQL database in Rust, built from scratch as an educational project. Main features:

- Raft distributed consensus for linearizable state machine replication.

- ACID transactions with MVCC-based snapshot isolation.

- Pluggable storage engine with BitCask and in-memory backends.

- Iterator-based query engine with heuristic optimization and time-travel support.

- SQL interface including joins, aggregates, and transactions.

I originally wrote toyDB in 2020 to learn more about database internals. Since then, I've spent several years building real distributed SQL databases at CockroachDB and Neon. Based on this experience, I've rewritten toyDB as a simple illustration of the architecture and concepts behind distributed SQL databases.

toyDB is intended to be simple and understandable, and also functional and correct. Other aspects like performance, scalability, and availability are non-goals -- these are major sources of complexity in production-grade databases, and obscure the basic underlying concepts. Shortcuts have been taken where possible.
