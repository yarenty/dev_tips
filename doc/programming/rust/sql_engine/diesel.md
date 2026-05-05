---
title: diesel
main_link: https://github.com/diesel-rs/diesel/tree/2.0.x
keywords: [diesel, rust, started, builder, mysql, databases]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

# diesel

**Main link:** <https://github.com/diesel-rs/diesel/tree/2.0.x>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[db]] — diesel _(score 33.3)_
- [[programming/rust/tooling/tauri|tauri]] — TAURI _(score 22.0)_
- [[programming/rust/data/mysql|mysql]] — mysql _(score 19.0)_
- [[db/relational/mysql|mysql]] — mysql _(score 17.9)_
- [[postgresql]] — postgresql _(score 17.9)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#diesel` `#sql-engine` `#rust` `#programming` `#started` `#query` `#builder` `#guides`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# diesel

https://crates.io/crates/diesel

Diesel - A safe, extensible ORM and Query Builder for Rust
Diesel is the most productive way to interact with databases in Rust because of its safe and composable abstractions over queries.


https://github.com/diesel-rs/diesel/tree/2.0.x


Diesel gets rid of the boilerplate for database interaction and eliminates runtime errors without sacrificing performance. It takes full advantage of Rust's type system to create a low overhead query builder that "feels like Rust."

Supported databases:

- PostgreSQL
- MySQL
-SQLite

You can configure the database backend in Cargo.toml:

```toml
[dependencies]
diesel = { version = "<version>", features = ["<postgres|mysql|sqlite>"] }
```

## Getting Started
Find our extensive Getting Started tutorial at https://diesel.rs/guides/getting-started. Guides on more specific features are coming soon.
