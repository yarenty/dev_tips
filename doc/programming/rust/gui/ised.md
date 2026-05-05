---
title: Iced
main_link: https://github.com/iced-rs/iced
keywords: [ised, rust, messages, counter, self]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

# Iced

**Main link:** <https://github.com/iced-rs/iced>

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
- [[egui]] — egui _(score 24.2)_
- [[cargo_toml]] — Cargo.toml _(score 14.7)_
- [[debug]] — Debug _(score 14.7)_
- [[rtic]] — RTIC _(score 14.7)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#ised` `#gui` `#rust` `#programming` `#message` `#counter` `#increment` `#decrement`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Iced


https://github.com/iced-rs/iced


A cross-platform GUI library for Rust focused on simplicity and type-safety. Inspired by Elm.

Features
Simple, easy-to-use, batteries-included API
Type-safe, reactive programming model
Cross-platform support (Windows, macOS, Linux, and the Web)
Responsive layout
Built-in widgets (including text inputs, scrollables, and more!)
Custom widget support (create your own!)
Debug overlay with performance metrics
First-class support for async actions (use futures!)
Modular ecosystem split into reusable parts:
A renderer-agnostic native runtime enabling integration with existing systems
Two built-in renderers leveraging wgpu and tiny-skia
iced_wgpu supporting Vulkan, Metal and DX12
iced_tiny_skia offering a software alternative as a fallback
A windowing shell
A web runtime leveraging the DOM
Iced is currently experimental software. Take a look at the roadmap, check out the issues, and feel free to contribute!

Installation
Add iced as a dependency in your Cargo.toml:

iced = "0.12"
If your project is using a Rust edition older than 2021, then you will need to set resolver = "2" in the [package] section as well.

Iced moves fast and the master branch can contain breaking changes! If you want to learn about a specific release, check out the release list.

Overview
Inspired by The Elm Architecture, Iced expects you to split user interfaces into four different concepts:

State — the state of your application
Messages — user interactions or meaningful events that you care about
View logic — a way to display your state as widgets that may produce messages on user interaction
Update logic — a way to react to messages and update your state
We can build something to see how this works! Let's say we want a simple counter that can be incremented and decremented using two buttons.

We start by modelling the state of our application:

#[derive(Default)]
struct Counter {
value: i32,
}
Next, we need to define the possible user interactions of our counter: the button presses. These interactions are our messages:

#[derive(Debug, Clone, Copy)]
pub enum Message {
Increment,
Decrement,
}
Now, let's show the actual counter by putting it all together in our view logic:

use iced::widget::{button, column, text, Column};

impl Counter {
pub fn view(&self) -> Column<Message> {
// We use a column: a simple vertical layout
column![
// The increment button. We tell it to produce an
// `Increment` message when pressed
button("+").on_press(Message::Increment),

            // We show the value of the counter here
            text(self.value).size(50),

            // The decrement button. We tell it to produce a
            // `Decrement` message when pressed
            button("-").on_press(Message::Decrement),
        ]
    }
}
Finally, we need to be able to react to any produced messages and change our state accordingly in our update logic:

impl Counter {
// ...

    pub fn update(&mut self, message: Message) {
        match message {
            Message::Increment => {
                self.value += 1;
            }
            Message::Decrement => {
                self.value -= 1;
            }
        }
    }
}
And that's everything! We just wrote a whole user interface. Let's run it:

fn main() -> iced::Result {
iced::run("A cool counter", Counter::update, Counter::view)
}
Iced will automatically:

Take the result of our view logic and layout its widgets.
Process events from our system and produce messages for our update logic.
Draw the resulting user interface.
Read the book, the documentation, and the examples to learn more!
