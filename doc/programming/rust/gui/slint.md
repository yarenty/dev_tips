---
title: Slint
main_link: https://slint.dev/
keywords: [slint, rust, gui, declarative, retained-mode, qml, embedded]
status: reviewed
---

# Slint

**Main link:** <https://slint.dev/>

## Summary

Slint is a declarative GUI toolkit for Rust (and C++, JS/Node) where the UI is written in a separate `.slint` markup language compiled to Rust at build time. The renderer targets desktop (Windows/macOS/Linux), mobile, and microcontrollers (no_std, ~250 KB ROM). Originally launched as **SixtyFPS** in 2020 and rebranded **Slint** in 2022, it is dual-licensed: GPLv3 for open-source projects and a commercial / royalty-free embedded licence from SixtyFPS GmbH.

## Insight

Slint is the strongest answer in the Rust ecosystem to "I want a *designer/developer split*" — `.slint` is the equivalent of Qt's QML, and the workflow is similar (designers iterate in markup + a live-preview viewer; developers wire callbacks in Rust). Reach for it when you want a **polished, retained-mode native UI** without writing platform code, especially for **embedded HMI** (its sweet spot — the marketing focuses on industrial dashboards and IoT panels with custom MCUs, not generic desktop). Compared to alternatives: [[egui]] is faster to prototype but less designer-friendly and weaker for animation; [[dioxus]] and [[programming/rust/gui/tauri|tauri]] are web-tech (and so much heavier on memory); Qt QML is the closest "feel" but pulls in C++ Qt itself. The dual licence is the main political consideration — confirm your situation before adopting commercially.

```rust
// All-in-one inline syntax
slint::slint! {
    export component HelloWorld {
        Text {
            text: "hello world";
            color: green;
        }
    }
}

fn main() {
    HelloWorld::new().unwrap().run().unwrap();
}
```

For non-trivial UIs split the markup into `.slint` files and compile them via a `build.rs`:

```toml
[package]
build = "build.rs"

[dependencies]
slint = "1.5"

[build-dependencies]
slint-build = "1.5"
```

```rust
// build.rs
fn main() {
    slint_build::compile("ui/hello.slint").unwrap();
}

// main.rs
slint::include_modules!();
fn main() {
    HelloWorld::new().unwrap().run().unwrap();
}
```

Quick scaffold:

```shell
cargo install cargo-generate
cargo generate --git https://github.com/slint-ui/slint-rust-template
```

## Similar / related topics

- Qt QML — the conceptual ancestor; same designer/dev split, C++ underneath.
- [[egui]] — immediate-mode alternative; less polish, faster to hack on.
- [[dioxus]] — declarative + reactive but web-tech-leaning; heavier.
- [[programming/rust/gui/tauri|tauri]] — webview shell; reach for it when web design tooling matters more than native fit.
- Flutter — the cross-language equivalent of "compile a UI DSL to a native renderer".

## Internal links

<!-- reviewed -->
- [[egui]]
- [[dioxus]]
- [[programming/rust/gui/tauri|tauri]]
- [[tao]]
- [[mobile]]
- [[../README]]

## Keywords

`#slint` `#rust` `#gui` `#declarative` `#retained-mode` `#qml-like` `#embedded`

## References / raw notes

- Site: <https://slint.dev/>
- Repo / template: <https://github.com/slint-ui/slint-rust-template>
- Rust API docs: <https://slint.dev/releases/1.5.1/docs/rust/slint/>
- Live editor: <https://slint.dev/releases/1.5.1/editor/>
- History: launched as SixtyFPS, rebranded Slint in 2022; commercial backer SixtyFPS GmbH.
