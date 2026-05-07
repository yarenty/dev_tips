---
title: RustOWL — borrow checker visualizer
main_link: https://github.com/cordx56/rustowl
keywords: [rustowl, borrow-checker, ownership, lifetimes, lsp, vscode, neovim, emacs]
status: reviewed
---

# RustOWL — borrow checker visualizer

**Main link:** <https://github.com/cordx56/rustowl>

## Summary

RustOwl is an editor tool by [@cordx56](https://github.com/cordx56) that paints **ownership and lifetime information directly into your source code** as coloured underlines while you write. It ships an LSP server (`cargo owlsp`) plus official VSCode, Neovim, and Emacs front-ends, so on save the code is analysed and hovering a variable or function call surfaces its actual lifetime, who's borrowing it, and where a move happened. The intent is to make the borrow checker's reasoning *visible* instead of forcing you to reconstruct it from compiler error messages.

## Insight

Reach for RustOwl when **the compiler is yelling about lifetimes and you don't yet have the mental model** to see why — students learning Rust, programmers context-switching back into Rust after months in Go/Python, or anyone wading through a tangled `&mut self` callsite. The colour scheme is the API: green = the variable's actual lifetime, blue = immutable borrow, purple = mutable borrow, orange = value moved / function call, red = lifetime mismatch (actual vs expected). It runs as an LSP, so the *real* effort is one-time editor setup; after that it's a hover-and-wait loop.

Gotchas: the analysis runs on save and adds a second or two of latency on big crates; it works on what compiles, so deeply broken code may underline nothing useful; and it's a teaching aid, not a replacement for `rustc`'s actual error messages — for diagnoses-as-you-type the standard `rust-analyzer` + Clippy combo still does the heavy lifting. Pair RustOwl with [Polonius](https://rust-lang.github.io/polonius/) reading and the Rustonomicon for the underlying theory.

## Similar / related topics

- [rust-analyzer](https://rust-analyzer.github.io/) — the canonical Rust LSP; inline type hints and diagnostics, but no lifetime visualization.
- [Clippy](https://doc.rust-lang.org/clippy/) — official lint set, including a few borrow-related smells.
- [Polonius](https://rust-lang.github.io/polonius/) — the next-generation borrow checker analysis RustOwl conceptually visualises.
- [Aquascope](https://cognitive-engineering-lab.github.io/aquascope/) — another borrow/ownership visualiser, from the *Rust Book Experiment* team; complementary, browser-embedded.

## Internal links
<!-- reviewed -->
- [[README]]
- [[../learning/README|learning]]

## Keywords

`#rust` `#rustowl` `#borrow-checker` `#ownership` `#lifetimes` `#lsp` `#tooling`

## References / raw notes

- Repo: <https://github.com/cordx56/rustowl>
- Screenshot: <https://github.com/cordx56/rustowl/raw/main/docs/readme-screenshot.png>

RustOwl visualises ownership movement and lifetimes by overlaying coloured underlines on Rust source. On save, the code is analysed; hover a variable or function call for ~2 seconds to see the visualisation.

Underline legend:

- 🟩 green — variable's actual lifetime
- 🟦 blue — immutable borrow
- 🟪 purple — mutable borrow
- 🟧 orange — value moved / function call
- 🟥 red — lifetime error (actual ≠ expected)

Editor support: official VSCode extension, Neovim plugin, Emacs package. Other editors can use the LSP server `cargo owlsp` directly via its extended LSP protocol.
