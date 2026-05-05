---
title: Mac targets
main_link: https://github.com/BrainiumLLC/rust-xcode-plugin
keywords: [macos, gui, rust, programming, matrix, mac, bindings, lipo]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# Mac targets

**Main link:** <https://github.com/BrainiumLLC/rust-xcode-plugin>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- TODO: at least 2 [[wikilinks]] to related articles in this vault. -->

## Keywords

`#macos` `#gui` `#rust` `#programming` `#matrix` `#mac` `#bindings` `#lipo`

## TODO

- This file contains **6 top-level `#` headings** — it likely covers multiple distinct topics. Per plan.md §8 step 3, **split this file** into one article per topic.
- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Mac targets
```shell
rustup target add aarch64-apple-ios aarch64-apple-ios-sim x86_64-apple-ios aarch64-apple-darwin x86_64-apple-darwin
```



# Cacao

This library provides safe Rust bindings for AppKit on macOS (beta quality, fairly usable) and UIKit on iOS/tvOS (alpha quality, see repo). It tries to do so in a way that, if you've done programming for the framework before (in Swift or Objective-C), will feel familiar. This is tricky in Rust due to the ownership model, but some creative coding and assumptions can get us pretty far.



# Lipo


https://crates.io/crates/cargo-lipo


```shell
cargo install cargo-lipo
```



# Rust xcode plugin

https://github.com/BrainiumLLC/rust-xcode-plugin

- working debug


# Matrix - rust bindings to ios version

https://github.com/matrix-org/matrix-rust-sdk/tree/main/bindings/apple




# mac-notification-sys

A simple wrapper to deliver or schedule macOS Notifications in Rust.


Example
```rust
use mac_notification_sys::*;

fn main() {
let bundle = get_bundle_identifier_or_default("firefox");
set_application(&bundle).unwrap();

    send_notification(
        "Danger",
        Some("Will Robinson"),
        "Run away as fast as you can",
        None,
    )
    .unwrap();

    send_notification(
        "NOW",
        None,
        "Without subtitle",
        Some(Notification::new().sound("Blow")),
    )
    .unwrap();
}
```
