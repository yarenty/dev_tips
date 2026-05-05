---
title: Dioxus
main_link: https://github.com/DioxusLabs/dioxus
keywords: [dioxus, rust, count, cli, linux]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

# Dioxus

**Main link:** <https://github.com/DioxusLabs/dioxus>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[matrix]] — Matrix - rust bindings to ios version _(score 24.2)_
- [[mobile]] — Mobile _(score 24.2)_
- [[egui]] — egui _(score 24.2)_
- [[http]] — Hyper _(score 20.1)_
- [[hyperfs]] — HyperFS _(score 20.1)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#dioxus` `#gui` `#rust` `#programming` `#server` `#count` `#web` `#integrated`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Dioxus


https://github.com/DioxusLabs/dioxus

Build for web, desktop, and mobile, and more with a single codebase. Zero-config setup, integrated hot-reloading, and signals-based state management. Add backend functionality with Server Functions and bundle with our CLI.

fn app() -> Element {
let mut count = use_signal(|| 0);

    rsx! {
        h1 { "High-Five counter: {count}" }
        button { onclick: move |_| count += 1, "Up high!" }
        button { onclick: move |_| count -= 1, "Down low!" }
    }
}
⭐️ Unique features:
Cross-platform apps in three lines of code (web, desktop, mobile, server, and more)
Ergonomic state management combines the best of React, Solid, and Svelte
Type-safe Routing and server functions to leverage Rust's powerful compile-time guarantees
Integrated bundler for deploying to the web, macOS, Linux, and Windows
And more! Take a tour of Dioxus.
