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
