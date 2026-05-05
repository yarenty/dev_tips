---
title: Derivative
main_link: https://mcarton.github.io/rust-derivative/latest/index.html
keywords: [derivative, rust, debuging]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

# Derivative

**Main link:** <https://mcarton.github.io/rust-derivative/latest/index.html>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[reflection]] — reflect _(score 17.1)_
- [[reflection_step_by_step_article]] — Reflection step by step - Article _(score 17.1)_
- [[error]] — eyre _(score 17.1)_
- [[rtic]] — RTIC _(score 13.1)_
- [[debug]] — Debug _(score 13.1)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#derivative` `#core` `#rust` `#programming` `#default` `#debuging` `#hash` `#mcarton`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->


# Derivative

https://mcarton.github.io/rust-derivative/latest/index.html


derivative = "2.2.0"



## Debuging

There is standard debuging, but here you can creae deuging even when fileds do not implement debug itself:
 - ignoring them
 - adding extra formatter // to be tested 



```rust
use derivative::Derivative;


#[derive(Derivative)]
#[derivative(Debug)]
struct SQLTableSource {
    #[derivative(Debug="ignore")]
    provider: Arc<SQLFederationProvider>,
    table_name: String,
    schema: SchemaRef,
}

// TODO: test this one
// #[derivative(Debug(format_with="path::to::my_fmt_fn"))]
//fn fmt(&T, &mut std::fmt::Formatter) -> Result<(), std::fmt::Error>;

```

## Default

```rust

#![allow(unused)]
fn main() {
extern crate derivative;
use derivative::Derivative;
#[derive(Debug, Derivative)]
#[derivative(Default(new="true"))]
struct Foo {
    foo: u8,
    bar: u8,
}

println!("{:?}", Foo::new()); // Foo { foo: 0, bar: 0 }
}


```

## Hash & Default
