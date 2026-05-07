---
title: rust-xcode-plugin
main_link: https://github.com/BrainiumLLC/rust-xcode-plugin
keywords: [rust-xcode-plugin, rust, xcode, ios, brainium, deprecated]
status: reviewed
---

# rust-xcode-plugin

**Main link:** <https://github.com/BrainiumLLC/rust-xcode-plugin>

## Summary

Brainium's `rust-xcode-plugin` was an Xcode plugin that wired Rust source-level debugging into the Xcode IDE so that breakpoints in `.rs` files actually stopped execution when an iOS app called into a Rust staticlib. It is part of the same Brainium "Rust on iOS" stack as the original `cargo-mobile` (now superseded by [[mobile|cargo-mobile2]]) and `cargo-lipo` (see [[lipo]]).

## Insight

Treat this as a historical pointer rather than something to install into a fresh Xcode today. The Brainium toolchain has been largely superseded by the Tauri team's actively-maintained fork `cargo-mobile2` and the broader Tauri 2 mobile pipeline — which handle iOS targeting without needing a custom Xcode plugin (Xcode itself stopped officially supporting third-party plugins after Xcode 8 / SIP changes; Brainium's plugin used the same unsigned-bundle hack tools like Alcatraz did). For Rust source-level debugging on iOS today, the more common path is to attach LLDB directly via the iOS toolchain that ships in Xcode — `lldb` already understands Rust DWARF metadata. If you genuinely have an old Xcode-plugin-based workflow that still works, the linked repo is the canonical source; otherwise migrate to [[mobile]].

## Similar / related topics

- [[mobile]] — `cargo-mobile2` is the modern replacement for the Brainium toolchain.
- [[lipo]] — the third leg of the Brainium Rust-on-iOS stack.
- LLDB direct attach — works without a plugin; preferred today.
- Xcode 8+ plugin signing changes — context for why plugins fell out of fashion.

## Internal links

<!-- reviewed -->
- [[mobile]]
- [[lipo]]
- [[macos]]
- [[../README]]

## Keywords

`#rust-xcode-plugin` `#rust` `#xcode` `#ios` `#brainium` `#deprecated`

## References / raw notes

- Repo: <https://github.com/BrainiumLLC/rust-xcode-plugin>
- Modern alternative: <https://github.com/tauri-apps/cargo-mobile2>

Notes from original entry: working debug — i.e. breakpoints fire in `.rs` source.
