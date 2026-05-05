---
title: Trippy
main_link: https://github.com/fujiapple852/trippy
keywords: [trace, linux, see, cli, cargo]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

# Trippy

**Main link:** <https://github.com/fujiapple852/trippy>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[tools/linux/tmux|tmux]] — TMUX _(score 20.3)_
- [[tools/security/pake|pake]] — Pake _(score 16.3)_
- [[web_to_app_pake]] — Pake _(score 16.3)_
- [[programming/rust/tooling/tools|tools]] — coreutils _(score 16.3)_
- [[dioxus]] — Dioxus _(score 9.9)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#trace` `#linux` `#tools` `#see` `#install` `#bsd` `#windows`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Trippy

https://github.com/fujiapple852/trippy


Trippy combines the functionality of traceroute and ping and is designed to assist with the analysis of networking issues.

![](https://raw.githubusercontent.com/fujiapple852/trippy/master/assets/0.12.0/demo.gif)

## Install
Trippy runs on Linux, BSD, macOS, and Windows. It can be installed from most package managers, precompiled binaries, or source.

For example, to install Trippy from cargo:
```shell

cargo install trippy --locked
```

All package managers
See the installation guide for details of how to install Trippy on your system.


```shell
brew install trippy
```

Run
To run a basic trace to example.com with default settings, use the following command:
```shell
sudo trip example.com
```
See the usage examples and CLI reference for details of how to use Trippy. To use Trippy without elevated privileges, see the privileges guide.
