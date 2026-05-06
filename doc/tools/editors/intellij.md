---
title: "IntelliJ IDEA — productivity tips"
main_link: https://www.youtube.com/watch?v=cK19rE2V9UY
keywords: [intellij, jetbrains, ide, java, kotlin, productivity, refactoring, video]
status: reviewed
---

# IntelliJ IDEA — productivity tips

**Main link:** <https://www.youtube.com/watch?v=cK19rE2V9UY> — _"Secrets of the fastest Java developers"_

Docs: <https://www.jetbrains.com/idea/> · Keymap PDFs: <https://www.jetbrains.com/idea/docs/IntelliJIDEA_ReferenceCard.pdf>

## Summary

IntelliJ IDEA is JetBrains' flagship IDE. It comes in a free **Community Edition** (Java, Kotlin, Scala, Groovy, Android with the Android Studio fork) and a paid **Ultimate Edition** that adds first-class support for web frameworks, Spring, JavaScript/TypeScript, databases, profilers, and remote development. The product line shares a kernel with PyCharm, GoLand, RustRover, WebStorm, CLion, RubyMine, PhpStorm, and DataGrip — learn the muscle memory once and it transfers across every JetBrains tool.

## Insight

If you write Java, Kotlin, or anything JVM-based for a living, IntelliJ is still the gold standard — VS Code's Java extension pack and Eclipse don't come close on refactoring depth, dataflow analysis, or the Spring/JPA tooling. For polyglot work the calculus is murkier: VS Code and Zed start instantly and have a bigger AI-extension ecosystem, while IntelliJ takes 10–30 seconds to index and eats RAM.

Things I keep coming back to:

- **Double-shift "Search Everywhere"** — the only shortcut you really need; finds files, classes, symbols, settings, and Git actions.
- **`Alt+Enter`** — context-aware "fix this" — imports, intention actions, refactors, quick-fixes.
- **`Ctrl/Cmd+Shift+A`** — "Find Action" — discoverability for any menu item by name.
- **`Ctrl+Alt+L` (format)**, **`Ctrl+Alt+O` (optimise imports)**, **`Shift+F6` (rename)** — non-negotiable.
- **Structural Search & Replace** — regex on the AST, not on text. Saves hours when migrating APIs.
- **Database tool window** — full DataGrip inside the IDE in Ultimate; live SQL with autocompletion against the actual schema.

Gotchas: licensing changes (per-user subscription, free non-commercial tier introduced in 2024 for individual hobby use), invalidating caches is the universal "have you tried turning it off and on again", and the new "AI Assistant" / Junie are paid add-ons separate from the IDE subscription.

## Similar / related topics

- [[void]] — open-source Cursor-style fork of VS Code, AI-first.
- [VS Code](https://code.visualstudio.com/) — the polyglot default; weaker refactoring, stronger extension ecosystem.
- [Zed](https://zed.dev/) — Rust-native, very fast, multiplayer; getting AI features fast.
- [Eclipse](https://www.eclipse.org/) — still alive in enterprise Java; the historical alternative.
- [[helix]] — terminal modal editor; complementary, not competitive.

## Internal links

<!-- reviewed -->

- [[void]] — open-source AI editor as the lighter alternative.
- [[markitdown]] — convert proprietary docs to Markdown to feed an IDE-side LLM.
- [[mdfried]] — read project Markdown in a nicer terminal preview.
- [[helix]] — when you want a terminal-only modal flow.
- [[lazygit]] — Git TUI that complements any IDE's built-in VCS panel.

## Keywords

`#intellij` `#jetbrains` `#ide` `#java` `#kotlin` `#productivity` `#refactoring` `#editors`

## References / raw notes

Source video — _Secrets of the fastest Java developers_: <https://www.youtube.com/watch?v=cK19rE2V9UY>
