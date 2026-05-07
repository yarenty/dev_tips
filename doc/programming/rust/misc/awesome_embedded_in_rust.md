---
title: awesome-embedded-rust — embedded Rust reading list
main_link: https://github.com/rust-embedded/awesome-embedded-rust
keywords: [awesome-embedded-rust, rust, embedded, no-std, hal, working-group]
status: reviewed
---

# awesome-embedded-rust — embedded Rust reading list

**Main link:** <https://github.com/rust-embedded/awesome-embedded-rust>

## Summary

`awesome-embedded-rust` is the curated index maintained by the [Rust Embedded Working Group](https://github.com/rust-embedded) of crates, books, and tools for writing Rust on bare-metal microcontrollers and other embedded targets. It is the single best entry point: HALs by chip family, peripheral drivers, board support crates, RTOSes (RTIC, Embassy, Drone, Bern), debugging tools, and hardware vendors with first-class Rust support.

## Insight

Treat this repo as your "where do I start?" map for embedded Rust. The actual things you'll most often pull in from it are:

- **`cortex-m`, `cortex-m-rt`** — startup + runtime for ARM Cortex-M cores.
- **`embedded-hal`** — the trait abstraction every chip-vendor HAL implements (your driver crate generally targets `embedded-hal` traits, not a specific chip).
- **`defmt`** — postcard-style deferred formatting for log output over RTT; tiny binary footprint vs `log`.
- **`probe-rs`** — modern flash + debug toolchain; replaces `openocd` / `st-link` / `J-Link` plumbing for most workflows. Pairs with `cargo-flash`, `cargo-embed`.
- **One of [[embassy]] or [[rtic]]** — the two scheduling/concurrency choices.
- **The [Rust Embedded Book](https://docs.rust-embedded.org/book/)** and the [Discovery book](https://docs.rust-embedded.org/discovery/) — the canonical tutorials.

The list itself is a directory page, not a tutorial — once you've found the crate names you need, jump to their docs/repos directly.

## Similar / related topics

- [Rust Embedded Working Group](https://github.com/rust-embedded) — the umbrella org.
- [Rust Embedded Book](https://docs.rust-embedded.org/book/) — canonical "no_std on Cortex-M" walk-through.
- [Discovery book](https://docs.rust-embedded.org/discovery/) — board-driven beginner tutorial.
- [`embedded-hal`](https://github.com/rust-embedded/embedded-hal) — the foundational trait crate.
- [`probe-rs`](https://probe.rs/) — modern flashing + debugging toolchain.

## Internal links

- [[raspberry_pi]] — Rust on Raspberry Pi specifically.
- [[embassy]] — async-first embedded executor.
- [[rtic]] — interrupt-priority-based real-time scheduler.
- [[../../../iot/drogue|drogue]] — IoT-side counterpart for cloud-connected devices.
- [[../../../iot/README|iot]] — vault-wide IoT section.

## Keywords

`#awesome-embedded-rust` `#rust` `#embedded` `#no-std` `#hal` `#working-group`

## References / raw notes

- Repo: <https://github.com/rust-embedded/awesome-embedded-rust>
- Maintained by the Rust Embedded Working Group: <https://github.com/rust-embedded>
- Companion book: <https://docs.rust-embedded.org/book/>
