---
title: Awesome Rust — must-have crates and resources
main_link: https://github.com/rust-unofficial/awesome-rust
keywords: [must-have, rust, awesome, libraries, curated-list, crates]
status: reviewed
---

# Awesome Rust — must-have crates and resources

**Main link:** <https://github.com/rust-unofficial/awesome-rust>

## Summary

This page is a curated index pointer: the canonical "everything anyone has bothered to publish in Rust" list lives at [rust-unofficial/awesome-rust](https://github.com/rust-unofficial/awesome-rust), and a more opinionated personal pick lives at Cedric Chee's [must-have crates gist](https://gist.github.com/cedrickchee/f729e848b52eab8fbc88a3910072198c). For the foundational handful you'll reach for in nearly every project — error handling, async runtime, serde, intro material — see the dedicated articles split out from this index (listed below).

## Insight

The Rust standard library is intentionally minimal — by design, common-but-not-universal functionality lives in crates. Working knowledge of the awesome-rust taxonomy is one of the cheapest investments to make: you stop re-inventing things and stop reaching for the wrong (often abandoned) crate. The two lists referenced here serve different roles: **awesome-rust** is the encyclopaedic alphabetical-by-category dump (find anything, but you must filter for activity yourself), while **Cedric Chee's gist** and similar personal lists are opinionated picks (fewer choices, but the maintainer has done the filtering for you). The articles split out below this one cover the tightest "always reach for these" subset; everything else is one click away on awesome-rust.

## Similar / related topics

- [lib.rs](https://lib.rs/) — alternative front-end for crates.io with hand-curated category trees.
- [crates.io categories](https://crates.io/categories) — official taxonomic browse.
- [blessed.rs](https://blessed.rs/) — opinionated "an unofficial guide to the Rust ecosystem" picks.
- [[github_map]] — visual neighbourhood-discovery view of the same space.
- [Awesome Rust Cryptography](https://github.com/The-EWG/awesome-rust-cryptography), [Awesome Embedded Rust](https://github.com/rust-embedded/awesome-embedded-rust) — vertical sub-lists.

## Internal links
<!-- reviewed -->

The "must-have" list this page used to inline has been split into focused articles:

- [[simple_short_intro_to_rust]] — fasterthanli.me's half-hour intro.
- [[cheats]] — cheats.rs single-page reference.
- [[cargo_toml]] — `Cargo.toml` snippets, profiles, optional features.
- [[anyhow]] — application-side error handling.
- [[thiserror]] — library-side error enums.
- [[eyre]] / `color-eyre` — anyhow with custom report renderers.

Companion pages in this folder:

- [[_to_learn]] — the books reading list.
- [[scope]] — what topics to study, in what order.
- [[tutorials]] — long-form tutorials and courses.
- [[from_easy_to_advanced]] — 15-project practice ladder.
- [[7_more_ideas]] — flatter alternative project list.
- [[_tricks]] — small performance/idiom tricks.
- [[github_map]], [[xc_in_rust]] — discovery and tooling.
- [[README]] — folder landing page.

## Keywords

`#must-have` `#awesome-rust` `#rust` `#curated-list` `#crates`

## References / raw notes

- **Awesome Rust** — <https://github.com/rust-unofficial/awesome-rust>
  - Mirror via crates.io for the awesome-rust crate: <https://crates.io/crates/awesome-rust>
- **Cedric Chee — Rust must-haves gist** — <https://gist.github.com/cedrickchee/f729e848b52eab8fbc88a3910072198c>
- **blessed.rs** — <https://blessed.rs/>  (opinionated picks, organised by problem)
- **lib.rs** — <https://lib.rs/>  (curated category trees over crates.io)
