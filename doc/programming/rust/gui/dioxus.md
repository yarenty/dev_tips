---
title: Dioxus
main_link: https://dioxuslabs.com/
keywords: [dioxus, rust, gui, react, signals, cross-platform, wasm]
status: reviewed
---

# Dioxus

**Main link:** <https://dioxuslabs.com/>

## Summary

Dioxus is a React-shaped Rust UI framework that targets web (WASM), desktop (webview / native), mobile (iOS / Android), TUI, LiveView, and SSR from a single component tree. The component model uses an `rsx!` macro and a *signals* state system (closer to Solid / Leptos than to React's hooks-and-rerender model), with the Dioxus CLI handling hot-reload, bundling, and platform deployment. Maintained by DioxusLabs (Jonathan Kelley et al.); Dioxus 0.5 introduced signals, and 0.6+ has matured the mobile and server-functions story.

## Insight

Dioxus is the right pick when you want one Rust codebase to ship to web *and* desktop *and* mobile and you are happy paying with a webview for desktop/mobile (i.e. you don't strictly need native widgets). Its closest neighbours in the Rust web-frontend space are [[yew]] (mature, hooks-based, web-only), Leptos (fine-grained reactivity, very fast, web-only), and Sycamore (also fine-grained); Dioxus distinguishes itself by going *cross-platform first*. The signals system means you can mutate state without `clone`-then-`set` ceremony, but it's a different mental model from React — expect a learning bump if you arrived from JS hooks. Like all Rust UI frameworks the live ecosystem (component libraries, dev tools, Stack Overflow answers) is much thinner than React's; treat that as the cost of admission.

```rust
fn app() -> Element {
    let mut count = use_signal(|| 0);
    rsx! {
        h1 { "High-Five counter: {count}" }
        button { onclick: move |_| count += 1, "Up high!" }
        button { onclick: move |_| count -= 1, "Down low!" }
    }
}
```

## Similar / related topics

- [[yew]] — older Rust+WASM React-alike; web only, hooks-based.
- Leptos — fine-grained reactive Rust web framework; web-focused, very fast.
- Sycamore — another fine-grained reactive Rust web framework.
- [[programming/rust/gui/tauri|tauri]] — webview shell you bring your own JS/TS UI to.
- [[slint]] / [[egui]] — non-web UI alternatives if "no DOM" is a requirement.

## Internal links

<!-- reviewed -->
- [[programming/rust/gui/tauri|tauri]]
- [[slint]]
- [[egui]]
- [[mobile]]
- [[../web/yew|yew]]
- [[../README]]

## Keywords

`#dioxus` `#rust` `#gui` `#react-like` `#signals` `#cross-platform` `#wasm`

## References / raw notes

- Site: <https://dioxuslabs.com/>
- Repo: <https://github.com/DioxusLabs/dioxus>

Cross-platform apps in three lines of code (web, desktop, mobile, server, and more). Ergonomic state management combines the best of React, Solid, and Svelte. Type-safe routing and server functions to leverage Rust's compile-time guarantees. Integrated bundler for deploying to the web, macOS, Linux, and Windows.
