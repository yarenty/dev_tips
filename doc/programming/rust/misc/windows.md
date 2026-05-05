---
title: Windows RS
main_link: https://github.com/microsoft/windows-rs
keywords: [windows, misc, rust, programming, winrt, api, crates]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# Windows RS

**Main link:** <https://github.com/microsoft/windows-rs>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- TODO: at least 2 [[wikilinks]] to related articles in this vault. -->

## Keywords

`#windows` `#misc` `#rust` `#programming` `#apis` `#winrt` `#api` `#crates`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Windows RS

https://github.com/microsoft/windows-rs

## Rust for Windows
The windows and windows-sys crates let you call any Windows API past, present, and future using code generated on the fly directly from the metadata describing the API and right into your Rust package where you can call them as if they were just another Rust module. The Rust language projection follows in the tradition established by C++/WinRT of building language projections for Windows using standard languages and compilers, providing a natural and idiomatic way for Rust developers to call Windows APIs.

This repo is the home of the following crates (and other supporting crates):

- windows - Safer bindings including C-style APIs as well as COM and WinRT APIs.

- windows-bindgen - Windows metadata compiler library.

- windows-core - Type support for the windows crate.

- windows-implement - The implement macro for the windows crate, for implementing COM interfaces.

- windows-interface - The interface macro for the windows crate, for declaring COM interfaces.

- windows-registry - Windows registry.

- windows-result - Windows error handling.

- windows-strings - Windows string types.

- windows-sys - Raw bindings for C-style Windows APIs.

- windows-targets - Import libs for Windows.

- windows-version - Windows version information.

- cppwinrt - Bundles the C++/WinRT compiler for use in Rust.
