---
title: Cursive
main_link: https://github.com/mimblewimble/grin
keywords: [cursive, rust, programming, view, crates, views, provides]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

> Auto-split from `doc/programming/rust/tui.md` by `article_split.py`. Heading: **Cursive**.

# Cursive

**Main link:** <https://github.com/mimblewimble/grin>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- TODO: at least 2 [[wikilinks]] to related articles in this vault. -->

## Keywords

`#cursive` `#rust` `#programming` `#view` `#crates` `#views` `#provides`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Cursive

A TUI (Text User Interface) library focused on ease-of-use.

https://crates.io/crates/cursive

Cursive is a TUI (Text User Interface) library for rust. It uses ncurses by default, but other backends are available.

It allows you to build rich user interfaces for terminal applications.



## Grin
Quite good tui - check how is implemented!
https://github.com/mimblewimble/grin


## Cursive modules


## Table view
A basic table view implementation for cursive.

https://github.com/BonsaiDen/cursive_table_view

https://crates.io/crates/cursive_table_view


## Tree view

A basic tree view implementation for cursive.

https://github.com/BonsaiDen/cursive_tree_view
https://crates.io/crates/cursive_tree_view




## Multiple tabs
This project provides a wrapper view to be able to easily handle multiple tabs that can be switched to at any time without having to change the order of the views for gyscos/cursive views.


https://github.com/deinstapel/cursive-tabs


## Spinner view

spinning world , etc...

See in examples folder To see in action run cargo run --example spinner

https://github.com/otov4its/cursive-spinner-view/

https://crates.io/crates/cursive-spinner-view


## Logger view
Auto updates from file

This project provides a new debug view for gyscos/cursive using the emabee/flexi_logger crate. This enables the FlexiLoggerView to respect the RUST_LOG environment variable as well as the flexi_logger configuration file. Have a look at the demo below to see how it looks.

https://github.com/deinstapel/cursive-flexi-logger-view

https://crates.io/crates/cursive-flexi-logger-view


## Multiplex (tmux)

This project provides a tiling window manager for gyscos/cursive similar to Tmux. You can place any other cursive view inside of a Mux view to display these views in complex layouts side by side. Watch the demo below to see how it looks.

https://github.com/deinstapel/cursive-multiplex

https://crates.io/crates/cursive-multiplex


## Async veiw -- progress bar

This project provides a wrapper view with a loading screen for gyscos/cursive views. The loading screen will disappear once the wrapped view is fully loaded. This is useful for displaying views which may take long to construct or depend on e.g. the network.


https://github.com/deinstapel/cursive-async-view



## Align view
right-top , middle-top.... left-bottom,

This project provides an AlignedView for gyscos/cursive views which makes it possible to align the child view (center, left, right, top, bottom). The AlignedView uses the required_size reported by the child view and fills the rest of the available space with the views background color.


https://github.com/deinstapel/cursive-aligned-view


## Hexview

A simple and basic hexviewer which can be used with cursive.

https://github.com/hellow554/cursive_hexview


## Markup

The cursive-markup crate provides a markup view for cursive that can render HTML.

https://crates.io/crates/cursive-markup

https://git.sr.ht/~ireas/cursive-markup-rs
