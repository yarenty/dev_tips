---
title: Iced
main_link: https://iced.rs/
keywords: [iced, rust, gui, elm-architecture, retained-mode, wgpu]
status: reviewed
---

# Iced

**Main link:** <https://iced.rs/>

## Summary

Iced is a cross-platform Rust GUI library inspired by The Elm Architecture (TEA): your app is a `State` + `Message` enum + `update`/`view` pair, and the framework wires events into messages and re-renders. Started by Héctor Ramón, it ships with built-in widgets, a `wgpu` renderer (with a `tiny-skia` software fallback) for native targets, and a separate web runtime that maps to the DOM. The 0.12+ API matches the snippet below; the project moves fast and breaks the API across minor versions, so pin and read changelogs.

> Note: this article lives at `gui/ised.md` for historical reasons (the on-disk filename is a typo of `iced`); the article is about Iced. Use `[[ised|Iced]]` when linking from outside.

## Insight

Iced is the Rust GUI library to reach for when you want a *type-safe, message-passing* mental model and you can tolerate a moving API. It's the philosophical counterpoint to immediate-mode [[egui]]: state is explicit, the view is a pure function of state, and side effects are values (`Command`s) you return rather than fire imperatively. That makes it pleasant for apps with non-trivial state machines (form wizards, settings, multi-pane editors). Gotchas: (1) layout is responsive but less expressive than Flexbox/Grid for now; (2) custom widgets are a real commitment because the renderer-agnostic native runtime is split across several crates; (3) accessibility and IME are weaker than mature retained-mode toolkits. The poster app is the [Cosmic desktop](https://github.com/pop-os/cosmic-epoch) (System76) which uses a custom Iced fork — that's also a useful reference codebase.

```rust
// Counter — state + message + update + view
#[derive(Default)]
struct Counter { value: i32 }

#[derive(Debug, Clone, Copy)]
pub enum Message { Increment, Decrement }

impl Counter {
    fn update(&mut self, message: Message) {
        match message {
            Message::Increment => self.value += 1,
            Message::Decrement => self.value -= 1,
        }
    }
    fn view(&self) -> iced::widget::Column<Message> {
        iced::widget::column![
            iced::widget::button("+").on_press(Message::Increment),
            iced::widget::text(self.value).size(50),
            iced::widget::button("-").on_press(Message::Decrement),
        ]
    }
}

fn main() -> iced::Result {
    iced::run("A cool counter", Counter::update, Counter::view)
}
```

## Similar / related topics

- [[egui]] — immediate-mode alternative; less ceremony, weaker for polished consumer apps.
- [[slint]] — declarative `.slint` markup compiled to native; sharper for designer/dev split.
- [[dioxus]] — React-shaped, also retained, web-first.
- Elm — the JavaScript original of TEA; the conceptual ancestor.
- Cosmic Desktop (System76) — the largest production Iced consumer.

## Internal links

- [[egui]]
- [[slint]]
- [[dioxus]]
- [[ui]]
- [[../README]]

## Keywords

`#iced` `#rust` `#gui` `#elm-architecture` `#retained-mode` `#wgpu`

## References / raw notes

- Site: <https://iced.rs/>
- Repo: <https://github.com/iced-rs/iced>
- Book: <https://book.iced.rs/>

A cross-platform GUI library for Rust focused on simplicity and type-safety. Inspired by Elm.

Features:

- Type-safe, reactive programming model
- Cross-platform support (Windows, macOS, Linux, and the Web)
- Responsive layout
- Built-in widgets (text inputs, scrollables, etc.)
- Custom widget support
- Debug overlay with performance metrics
- First-class support for async actions (uses futures)
- Modular ecosystem split into reusable parts:
  - A renderer-agnostic native runtime
  - Two built-in renderers: `iced_wgpu` (Vulkan / Metal / DX12) and `iced_tiny_skia` (CPU fallback)
  - A windowing shell
  - A web runtime leveraging the DOM

Iced expects you to split user interfaces into four concepts: **State**, **Messages**, **View logic**, **Update logic** — see the counter snippet in *Insight*.
