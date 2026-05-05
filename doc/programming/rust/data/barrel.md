---
title: barrel
main_link: https://crates.io/crates/barrel
keywords: [barrel, data, rust, programming, sql, crates, database, builder]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

> Auto-split from `doc/programming/rust/data/db.md` by `article_split.py`. Heading: **barrel**.

# barrel

**Main link:** <https://crates.io/crates/barrel>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- TODO: at least 2 [[wikilinks]] to related articles in this vault. -->

## Keywords

`#barrel` `#data` `#rust` `#programming` `#sql` `#crates` `#database` `#builder`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# barrel

https://crates.io/crates/barrel

A powerful database schema builder, that lets you write your SQL migrations in Rust!

barrel offers callback-style builder functions for SQL migrations and is designed to be flexible, portable and fun to use. It provides you with a common interface over SQL, with additional database-specific builders.

```rust
use barrel::{types, Migration};
use barrel::backend::Pg;

fn main() {
    let mut m = Migration::new();

    m.create_table("users", |t| {
        t.add_column("name", types::varchar(255));
        t.add_column("age", types::integer());
        t.add_column("owns_plushy_sharks", types::boolean());
    });

    println!("{}", m.make::<Pg>());
}
```
