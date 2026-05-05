---
title: Rust - brain
main_link: https://github.com/brain-lang/brain
keywords: [brain, funny, brainfuck, rust, programming]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

> Auto-split from `doc/funny/brainfuck.md` by `article_split.py`. Heading: **Rust - brain**.

# Rust - brain

**Main link:** <https://github.com/brain-lang/brain>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[brainfuck]] — Brainfuck _(score 33)_
- [[rtic]] — RTIC _(score 20)_
- [[algorithms]] — Algorithms _(score 13)_
- [[_todo_ideas]] — Move blokchain from python to rust _(score 10)_
- [[programming/rust/tooling/tauri|tauri]] — TAURI _(score 10)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->

## Keywords

`#rust-brain` `#funny` `#brainfuck` `#brain` `#rust` `#programming`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Rust - brain

https://github.com/brain-lang/brain

brain is a strongly-typed, high-level programming language that compiles into brainfuck. Its syntax is based on the Rust programming language (which it is also implemented in). Though many Rust concepts will work in brain, it deviates when necessary in order to better suit the needs of brainfuck programming.

brainfuck is an esoteric programming language with only 8 single-byte instructions: +, -, >, <, ,, ., [, ]. These limited instructions make brainfuck code extremely verbose and difficult to write. It can take a long time to figure out what a brainfuck program is trying to do. brain makes it easier to create brainfuck programs by allowing you to write in a more readable and understandable language.

The type system makes it possible to detect a variety of logical errors when compiling, instead of waiting until runtime. This is an extra layer of convenience that brainfuck does not have. The compiler takes care of generating all the necessary brainfuck code to work with the raw bytes in the brainfuck turing machine.

The brain programming language compiles directly into brainfuck. The generated brainfuck code can be run by a brainfuck interpreter. brain only targets this interpreter which means that its generated programs are only guaranteed to work when run with that. The interpreter implements a brainfuck specification specially designed and written for the brain programming language project.
