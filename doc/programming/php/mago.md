---
title: "Mago — a Rust-based linter, formatter, and static analyzer for PHP"
main_link: https://mago.carthage.software/
keywords: [mago, php, linter, formatter, static-analyzer, rust, carthage, psr-12]
status: reviewed
---

# Mago — a Rust-based linter, formatter, and static analyzer for PHP

**Main link:** <https://mago.carthage.software/>

Repo: <https://github.com/carthage-software/mago> · VS Code recipe: <https://mago.carthage.software/recipes/vscode>

## Summary

Mago is an all-in-one PHP toolchain written in Rust by Carthage Software. One binary covers what the historical PHP ecosystem split across at least four tools: a code formatter (replaces PHP-CS-Fixer / PHP_CodeSniffer's fixer mode), a linter for stylistic issues and code smells (replaces PHP_CodeSniffer's sniffs), and a static analyzer that finds type errors and logical bugs without execution (replaces PHPStan and Psalm). Because it's a single Rust binary the install is one command and runs are fast — the explicit pitch is "blazing fast."

## Insight

Why this matters even if you don't write much PHP:

- The PHP ecosystem has been carrying around a *very* fragmented analyzer story for years. A typical CI pipeline ran PHP_CodeSniffer + PHP-CS-Fixer + PHPStan + Psalm + Rector — five distinct tools with overlapping concerns, separate config files, separate caches, and noticeably long total runtimes on big codebases. Mago is the first serious attempt to collapse that.
- It's part of a broader trend of "rewrite the language's tooling in Rust" that ate JavaScript first ([Biome](https://biomejs.dev/), [Oxc](https://oxc.rs/), [SWC](https://swc.rs/), [Rolldown](https://rolldown.rs/)), Python next ([Ruff](https://docs.astral.sh/ruff/), [uv](https://docs.astral.sh/uv/)), and is now reaching PHP. The ergonomic argument is the same in every case: single static binary, an order-of-magnitude faster, modern config story.
- It enforces PSR-12 by default, which is the right default. If you have a custom style, that's configurable but you may want to revisit whether you actually need a custom style.

When to reach for it:

- Greenfield PHP projects — set it up from day one and never have a tooling-config sprawl problem.
- Existing projects looking to consolidate their CI: Mago can plausibly replace 3-5 existing tools.
- Teams that find PHPStan / Psalm runs are slow enough to discourage running them locally.

Honest gotchas:

- Younger than PHPStan / Psalm. The static-analysis catalog is good but not yet at full parity for every edge case (generics inference, deep template-types, custom rules ecosystem).
- The plugin / custom-rule story is still evolving.
- If you have heavy investment in custom Psalm or PHPStan rules, migration cost is real.

## Install

Quick install (curl to bash, the usual caveats apply):

```sh
curl --proto '=https' --tlsv1.2 -sSf https://carthage.software/mago.sh | bash
```

Or via Homebrew:

```sh
brew install mago
mago self-update
```

VS Code: see <https://mago.carthage.software/recipes/vscode>.

## Similar / related topics

- [PHPStan](https://phpstan.org/) — the long-standing Go-to PHP static analyzer.
- [Psalm](https://psalm.dev/) — the other long-standing analyzer; especially strong on generics.
- [PHP-CS-Fixer](https://cs.symfony.com/) — the dominant formatter Mago is competing with.
- [Rector](https://getrector.com/) — automated PHP refactoring / upgrades; complementary, not replaced by Mago.
- [Ruff](https://docs.astral.sh/ruff/) — Mago's spiritual sibling on the Python side.
- [Biome](https://biomejs.dev/) — Mago's spiritual sibling on the JavaScript side.

## Internal links

<!-- reviewed -->

- [[programming/php/README|PHP]] — parent section.
- [[programming/rust/README|Rust]] — Mago is one of many examples of Rust eating language tooling.
- [[programming/package_managers/README|package managers]] — adjacent: Composer is where you'll typically wire Mago into a project.
- [[harper]] — comparable Rust-rewrite trend in the linter/checker space (Harper for English prose).

## Keywords

`#mago` `#php` `#linter` `#formatter` `#static-analyzer` `#rust` `#carthage` `#psr-12` `#programming`

## References / raw notes

> Blazing fast linter, formatter, and static analyzer for PHP, written in Rust.
>
> Mago is a complete toolchain for PHP, written in Rust, designed from the ground up for maximum performance. It includes:
>
> - A blazing-fast formatter that automatically formats your code according to PSR-12, ending style debates forever.
> - An intelligent linter that catches stylistic issues, inconsistencies, and code smells before they become problems.
> - A powerful static analyzer that finds type errors and logical bugs in your code without you ever having to run it.
