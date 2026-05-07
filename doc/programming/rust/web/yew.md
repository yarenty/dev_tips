---
title: Yew
main_link: https://yew.rs/
keywords: [yew, rust, wasm, frontend, framework, react, components]
status: reviewed
review_date: 2026/05/03
---

# Yew

**Main link:** <https://yew.rs/>

## Summary

Yew is a component-based Rust frontend framework that compiles to WebAssembly via `wasm-bindgen`. It uses a JSX-like `html!` macro, virtual-DOM diffing, and a Hooks/functional-component API that will feel immediately familiar to anyone who's used React. Yew has been around since ~2017 and is the longest-lived Rust frontend framework, though it now competes with several younger alternatives.

## Insight

Pick Yew when you want the React mental model in pure Rust — components, props, hooks, JSX-by-macro — and you value maturity and ecosystem (`yew-router`, `yewdux`, `yew-hooks`, `gloo`-friendly) over absolute performance. The 2025 Rust frontend landscape splits roughly:

| Framework | Style | Killer feature | Trade-off |
|---|---|---|---|
| **Yew** | React + virtual DOM | Most mature; biggest ecosystem | Bigger binaries; not the fastest |
| **Leptos** | Solid.js / signals (fine-grained reactivity) | Fastest; SSR + islands; isomorphic Rust | Steeper learning curve, churn |
| **Dioxus** | React + RSX | Cross-platform (web + desktop + mobile + TUI) | Per-target quirks |
| **Sycamore** | Solid.js-like signals | Lean, no virtual DOM | Smaller community |

Things to know:

- **Binary size is real.** A trivial Yew app gzips to ~150-300 KB; budget for `wasm-opt -Oz` and lazy code-splitting.
- **SSR + hydration** added in 0.21+ — works but trickier to set up than Leptos's isomorphic story.
- **Multi-threading** uses Web Workers via `gloo-worker` or `wasm-bindgen-rayon` (needs cross-origin isolation).
- **JS interop** works seamlessly via `wasm-bindgen` — you can `npm install` something and call it from a Yew component.
- **Hot reload** doesn't really exist; iterate with `trunk serve` and live-reload, not HMR.

When *not* to pick Yew:

- For an SSR-first or SEO-critical app, Leptos has a better story.
- For "one codebase, web + desktop + mobile", Dioxus is the explicit answer.
- For a tiny widget you're embedding in a larger JS app, raw `wasm-bindgen` may be lighter.

## Similar / related topics

- **Leptos** — fine-grained reactivity, isomorphic SSR; current performance leader.
- **Dioxus** — React-style, cross-platform (desktop / mobile / web / TUI).
- **Sycamore** — Solid-style signals; lean, smaller community.
- **Seed** — Elm-flavoured architecture; mostly historical interest now.
- **Percy** — virtual-DOM library predating Yew; historical.

## Internal links
- [[webassembly]] — section overview
- [[gloo]] — browser-API toolkit Yew apps lean on
- [[reqwest]] — HTTP client (works in Yew via wasm32 target)
- [[dioxus]] — sister React-style framework with broader targets
- [[programming/rust/gui/tauri|tauri]] — pair Yew with Tauri for a desktop app

## Keywords

`#yew` `#rust` `#wasm` `#frontend` `#framework` `#react` `#components`

## References / raw notes

- Site: <https://yew.rs/>
- Repo: <https://github.com/yewstack/yew>

> Yew is a modern Rust framework for creating multi-threaded front-end web apps using WebAssembly.
>
> It features a component-based framework which makes it easy to create interactive UIs. Developers who have experience with frameworks like React and Elm should feel quite at home when using Yew. It achieves great performance by minimising DOM API calls and helping developers easily offload processing to background threads using web workers. It supports JavaScript interoperability, allowing developers to leverage NPM packages and integrate with existing JavaScript applications.
