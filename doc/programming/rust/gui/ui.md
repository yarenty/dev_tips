---
title: UIs
main_link: https://github.com/eythaann/Seelen-UI
keywords: [ui, gui, rust, programming, floem, widgets, transitions, customizable]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# UIs

**Main link:** <https://github.com/eythaann/Seelen-UI>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[swww]] — swww _(score 24.2)_
- [[dioxus]] — Dioxus _(score 24.2)_
- [[egui]] — egui _(score 24.2)_
- [[debug]] — Debug _(score 14.7)_
- [[rtic]] — RTIC _(score 14.7)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#ui` `#gui` `#rust` `#programming` `#floem` `#widgets` `#transitions` `#customizable`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes
<!-- auto-split by article_split.py -->
> Auto-split: 1 additional top-level heading(s) extracted into sibling files:
> - [swww](swww.md)


<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# UIs

7 GUI Tasks

https://eugenkiss.github.io/7guis/tasks


## See [Dioxus / Freya](gui/gui.mdi.md)


## Selen - UI


https://github.com/eythaann/Seelen-UI

Fully Customizable Desktop Environment for Windows
Available in 70+ Languages




## floem

https://github.com/lapce/floem?tab=readme-ov-file

A native Rust UI library with fine-grained reactivity


### Features

Inspired by Xilem, Leptos and rui, Floem aims to be a high performance declarative UI library with a highly ergonomic API.

- Cross-platform: Floem supports Windows, macOS and Linux with rendering using wgpu. In case a GPU is unavailable, a CPU renderer powered by tiny-skia will be used.
- Fine-grained reactivity: The entire library is built around reactive primitives inspired by leptos_reactive. The reactive "signals" allow you to keep your UI up-to-date with minimal effort, all while maintaining very high performance.
- Performance: The view tree is constructed only once, safeguarding you from accidentally creating a bottleneck in a view generation function that slows down your entire application. Floem also provides tools to help you write efficient UI code, such as a virtual list.
- Flexbox layout: Using Taffy, the library provides the Flexbox and Grid layout systems, which can be applied to any View node.
- Customizable widgets: Widgets are highly customizable. You can customize both the appearance and behavior of widgets using the styling API, which supports theming with classes. You can also install third-party themes.
- Transitions and Animations: Floem supports both transitions and animations. Transitions, like css transitions, can animate any property that can be interpolated and can be applied alongside other styles, including in classes. Floem also supports full keyframe animations that build on the ergonomics of the style system. In both transitions and animations, Floem supports easing with spring functions.
- Element inspector: Inspired by your browser's developer tools, Floem provides a diagnostic tool to debug your layout.


https://github.com/lapce/floem/blob/main/examples/editor/src/main.rs




## EWW


https://github.com/elkowar/eww?tab=readme-ov-file


Elkowars Wacky Widgets is a standalone widget system made in Rust that allows you to implement your own, custom widgets in any window manager.

Documentation and instructions on how to install can be found here.

Dharmx also wrote a nice, beginner friendly introductory guide for eww here.
