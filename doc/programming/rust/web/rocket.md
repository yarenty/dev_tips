---
title: Rocket
main_link: https://rocket.rs/
keywords: [rocket, rust, hello, john]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

> Auto-split from `doc/programming/rust/web/http.md` by `article_split.py`. Heading: **Rocket**.

# Rocket

**Main link:** <https://rocket.rs/>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[http]] — Hyper _(score 33.0)_
- [[gloo]] — Gloo _(score 24.1)_
- [[webassembly]] — WASM _(score 24.1)_
- [[rtic]] — RTIC _(score 14.7)_
- [[assembly]] — Assembly _(score 13.4)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#rocket` `#web` `#rust` `#programming` `#hello` `#john` `#simple` `#type`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Rocket

https://rocket.rs/

Rocket is a web framework for Rust that makes it simple to write fast, secure web applications without sacrificing flexibility, usability, or type safety.




* Type Safe
From request to response Rocket ensures that your types mean something.

* Boilerplate Free
Spend your time writing code that really matters, and let Rocket generate the rest.

* Easy To Use
Simple, intuitive APIs make Rocket approachable, no matter your background.

* Extensible
Create your own first-class primitives that any Rocket application can use.


```rust
#[macro_use] extern crate rocket;

#[get("/hello/<name>/<age>")]
fn hello(name: &str, age: u8) -> String {
    format!("Hello, {} year old named {}!", age, name)
}

#[launch]
fn rocket() -> () {
    rocket::build().mount("/", routes![hello])
}
```


This is a complete Rocket application. It does exactly what you would expect. If you were to visit /hello/John/58, you’d see:

Hello, 58 year old named John!

If someone visits a path with an <age> that isn’t a u8, Rocket doesn’t blindly call hello. Instead, it tries other matching routes or returns a 404.
