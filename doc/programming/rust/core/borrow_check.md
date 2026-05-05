---
title: RustOWL
main_link: https://github.com/cordx56/rustowl
keywords: [borrow-check, core, rust, programming, visualize, ownership, lifetimes, variable]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# RustOWL

**Main link:** <https://github.com/cordx56/rustowl>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- TODO: at least 2 [[wikilinks]] to related articles in this vault. -->

## Keywords

`#borrow-check` `#core` `#rust` `#programming` `#visualize` `#ownership` `#lifetimes` `#variable`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# RustOWL

Visualize ownership and lifetimes in Rust for debugging and optimization. 


https://github.com/cordx56/rustowl


![](https://github.com/cordx56/rustowl/raw/main/docs/readme-screenshot.png)


RustOwl visualizes ownership movement and lifetimes of variables. When you save Rust source code, it is analyzed, and the ownership and lifetimes of variables are visualized when you hover over a variable or function call.

RustOwl visualizes those by using underlines:

- 🟩 green: variable's actual lifetime
- 🟦 blue: immutable borrowing
- 🟪 purple: mutable borrowing
- 🟧 orange: value moved / function call
- 🟥 red: lifetime error - diff of lifetime between actual and expected


Currently, we offer VSCode extension, Neovim plugin and Emacs package. For these editors, move the text cursor over the variable or function call you want to inspect and wait for 2 seconds to visualize the information. We implemented LSP server cargo owlsp with an extended protocol. So, RustOwl can be used easily from other editor.
