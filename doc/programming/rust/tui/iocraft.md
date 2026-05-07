---
title: iocraft
main_link: https://github.com/ccbrown/iocraft
keywords: [iocraft, rust, tui, declarative, react, jsx, ccbrown]
status: reviewed
review_date: 2026/05/03
---

# iocraft

**Main link:** <https://github.com/ccbrown/iocraft>

## Summary

`iocraft` (by `ccbrown`) is a Rust library for crafting terminal output and interactive TUIs using a *declarative*, React-style component tree. It uses a custom procedural macro (`element! { ... }`) that mimics JSX so you describe a tree of components — `View`, `Text`, `Box`, layout primitives — and let the library handle layout (Flexbox-style via `taffy`), focus, and re-render. It's the Rust take on the same idea as JavaScript's [[ink]] — and that lineage is explicit, even down to the name.

## Insight

Reach for iocraft if you want a TUI that reads like a React app rather than an imperative draw loop. The model is genuinely different from Ratatui:

- **Ratatui**: you own the main loop; every tick, you redraw the whole frame from current state.
- **iocraft**: you describe a component tree once; iocraft diffs and re-renders on state change, and handles layout for you.

This is great for forms, dashboards with predictable structure, and CLI tools that mix static output with a small interactive flourish (the `print!`-style `element!` rendering can also produce non-fullscreen terminal output that just exits — useful for nicely-laid-out CLI prints, similar to how ink is often used).

Trade-offs:

- Smaller ecosystem than Ratatui (newer, fewer real-world adopters in 2025).
- The macro magic adds compile time and can produce confusing errors when components don't fit the expected shape.
- If you need pixel-precise full-screen control (e.g. an editor, a game), you'll fight the abstraction; reach for Ratatui or [[termion]].

## Similar / related topics

- [[ink]] — the JavaScript/React library that inspired iocraft.
- [[../tui/README|Ratatui]] — the imperative, mainstream alternative.
- [[cursive]] — retained-mode widget toolkit; another "give me high-level abstractions" answer, but callback-based rather than declarative.
- [`ratatui-eventful`](https://docs.rs/ratatui-eventful/latest/) / `tui-realm` — declarative-ish wrappers *on top of* Ratatui for users who want the best of both.

## Internal links
- [[../tui/README|Rust TUI]]
- [[ink]]
- [[cursive]]
- [[termion]]

## Keywords

`#rust` `#tui` `#iocraft` `#declarative` `#react` `#jsx`

## References / raw notes

- Repo: <https://github.com/ccbrown/iocraft>
- Crate: <https://crates.io/crates/iocraft>

> iocraft is a library for crafting beautiful text output and interfaces for the terminal or logs. It allows you to easily build complex layouts and interactive elements using a declarative API.
