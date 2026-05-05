---
title: Slint
main_link: https://github.com/slint-ui/slint-rust-template
keywords: [slint, gui, rust, programming, tao, cargo, crates]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# Slint

**Main link:** <https://github.com/slint-ui/slint-rust-template>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- TODO: at least 2 [[wikilinks]] to related articles in this vault. -->

## Keywords

`#slint` `#gui` `#rust` `#programming` `#tao` `#cargo` `#build` `#crate`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes
<!-- auto-split by article_split.py -->
> Auto-split: 1 additional top-level heading(s) extracted into sibling files:
> - [Tao](tao.md)


<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Slint


https://slint.rs/


Declarative GUI for Rust


```rust
slint::slint!{
    export component HelloWorld {
        Text {
            text: "hello world";
            color: green;
        }
    }
}
fn main() {
    HelloWorld::new().unwrap().run().unwrap();
}

```


```toml
[package]
...
build = "build.rs"
edition = "2021"

[dependencies]
slint = "1.5.0"
...

[build-dependencies]
slint-build = "1.5.0"

```


Use the API of the slint-build crate in the build.rs file:

fn main() {
slint_build::compile("ui/hello.slint").unwrap();
}
Finally, use the include_modules! macro in your main.rs:

ⓘ
slint::include_modules!();
fn main() {
HelloWorld::new().unwrap().run().unwrap();
}
The cargo-generate tool is a great tool to up and running quickly with a new Rust project. You can use it in combination with our Template Repository to create a skeleton file hierarchy that uses this method:

cargo install cargo-generate
cargo generate --git https://github.com/slint-ui/slint-rust-template



## Slint

https://slint.dev/releases/1.5.1/docs/rust/slint/


https://slint.dev/releases/1.5.1/editor/
