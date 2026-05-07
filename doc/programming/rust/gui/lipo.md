---
title: cargo-lipo
main_link: https://github.com/TimNN/cargo-lipo
keywords: [lipo, cargo-lipo, rust, ios, universal-binary, fat-binary]
status: reviewed
review_date: 2026/05/03
---

# cargo-lipo

**Main link:** <https://github.com/TimNN/cargo-lipo>

## Summary

`cargo-lipo` is a small Cargo subcommand that builds a Rust staticlib for multiple iOS architectures and then runs Apple's `lipo` tool to glue them into a single *universal (fat) binary* — the form you have to ship to be loadable from an Xcode iOS project. The name comes from `lipo(1)` itself: a long-standing Apple binutil for creating, inspecting, and splitting multi-architecture Mach-O files.

## Insight

This is filed under `gui/` because mobile-Rust workflows historically lean on it, but the concept is generic: any time you build a Rust library that an iOS (or macOS) app links statically, you need either `lipo` directly or a wrapper like `cargo-lipo` / `cargo-xcode` / `cargo-mobile2` to fuse `aarch64-apple-ios`, `x86_64-apple-ios` (simulator on Intel Macs), and `aarch64-apple-ios-sim` slices into one `.a`. The modern alternative is to skip the staticlib entirely and ship an **XCFramework** (Apple's preferred multi-arch container since Xcode 11) which sidesteps `lipo` because each slice lives in its own subdirectory — see `cargo-xcframework` or the build steps in `swift-bridge`/`uniffi` projects. cargo-lipo itself hasn't seen recent activity, so check the `cargo-mobile2` / Tauri-mobile path before adopting it for new work.

```shell
cargo install cargo-lipo
# build a release universal lib for iOS device + simulator
cargo lipo --release
```

## Similar / related topics

- `lipo(1)` — the underlying Apple tool (`man lipo`) for fat Mach-O surgery.
- `xcframework` — modern multi-arch container; preferred over fat staticlibs since Xcode 11.
- [[mobile]] — `cargo-mobile` / `cargo-mobile2` parent workflow that may invoke lipo for you.
- `cargo-xcframework`, `swift-bridge`, `uniffi` — toolchains that handle the packaging.
- [[macos]] — the parent macOS / iOS Rust target setup.

## Internal links

- [[macos]]
- [[mobile]]
- [[cacao]]
- [[../README]]

## Keywords

`#cargo-lipo` `#rust` `#ios` `#universal-binary` `#fat-binary` `#xcode`

## References / raw notes

- Repo: <https://github.com/TimNN/cargo-lipo>
- Crate: <https://crates.io/crates/cargo-lipo>

```shell
cargo install cargo-lipo
```
