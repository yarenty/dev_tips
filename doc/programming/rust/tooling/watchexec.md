---
title: watchexec
main_link: https://github.com/watchexec/watchexec
keywords: [watchexec, tooling, rust, programming, crates, software, boring]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

> Auto-split from `doc/programming/rust/tooling/tools.md` by `article_split.py`. Heading: **watchexec**.

# watchexec

**Main link:** <https://github.com/watchexec/watchexec>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[rtic]] — RTIC _(score 25)_
- [[starship]] — starship _(score 23)_
- [[debug]] — Debug _(score 23)_
- [[tux]] — tux _(score 23)_
- [[programming/rust/tooling/bottom|bottom]] — bottom _(score 23)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->

## Keywords

`#watchexec` `#tooling` `#rust` `#programming` `#simple` `#crates` `#software` `#boring`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# watchexec

https://crates.io/crates/watchexec

https://github.com/watchexec/watchexec



Software development often involves running the same commands over and over. Boring!

watchexec is a simple, standalone tool that watches a path and runs a command whenever it detects modifications.

Example use cases:

Automatically run unit tests
Run linters/syntax checkers
Features
Simple invocation and use, does not require a cryptic command line involving xargs
Runs on OS X, Linux, and Windows
Monitors current directory and all subdirectories for changes
Coalesces multiple filesystem events into one, for editors that use swap/backup files during saving
Loads .gitignore and .ignore files
Uses process groups to keep hold of forking programs
Provides the paths that changed in environment variables
Does not require a language runtime, not tied to any particular language or ecosystem
And more!
