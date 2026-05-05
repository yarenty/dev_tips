---
title: WASM
main_link: https://github.com/yewstack/yew
keywords: [webassembly, web, rust, programming, yew, developers, javascript, gloo]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# WASM

**Main link:** <https://github.com/yewstack/yew>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- TODO: at least 2 [[wikilinks]] to related articles in this vault. -->

## Keywords

`#webassembly` `#web` `#rust` `#programming` `#yew` `#developers` `#javascript` `#gloo`

## TODO

- This file contains **6 top-level `#` headings** — it likely covers multiple distinct topics. Per plan.md §8 step 3, **split this file** into one article per topic.
- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# WASM

https://github.com/WebAssembly
(general) proposals, status, meetings, 



https://github.com/bytecodealliance
(rust) wasmtime, tools, runtime, bindgen,



Update 202211:

![Status](../../../assets/img/wasm/WASM_202211_status.png)

This is still work in progress - but good direction. 
Soon there willbe async support and hopefully cloud.

Some good thoughts and intro: https://www.youtube.com/watch?v=DkpNqcfuPOM



# wasmtime

https://crates.io/crates/wasmtime


# Dioxus [CHECK!](gui/gui.mdi.md)

# YEW

https://yew.rs/

https://github.com/yewstack/yew



Yew is a modern Rust framework for creating multi-threaded front-end web apps using WebAssembly.

It features a component-based framework which makes it easy to create interactive UIs. Developers who have experience with frameworks like React and Elm should feel quite at home when using Yew.
It achieves great performance by minimizing DOM API calls and by helping developers to easily offload processing to background threads using web workers.
It supports JavaScript interoperability, allowing developers to leverage NPM packages and integrate with existing JavaScript applications.



# Gloo
A toolkit for building fast, reliable Web applications and libraries with Rust and Wasm.


Gloo is a collection of libraries, and those libraries provide ergonomic Rust wrappers for browser APIs. web-sys/js-sys are very difficult/inconvenient to use directly so provides wrappers around the raw bindngs which makes it easier to consume those APIs. This is why it is called a "toolkit", instead of "library" or "framework".


# obsetrvability on wasm

https://github.com/dylibso/observe-sdk

https://github.com/dylibso/observe-sdk/tree/main/rust
