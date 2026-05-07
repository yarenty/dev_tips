---
title: "Parsers in Rust (pest, nom, chumsky, winnow, combine)"
main_link: https://github.com/pest-parser/awesome-pest
keywords: [rust, parsers, pest, nom, chumsky, winnow, combine, peg, combinators]
status: reviewed
---

# Parsers in Rust (pest, nom, chumsky, winnow, combine)

**Main link:** <https://github.com/pest-parser/awesome-pest>

## Summary

Rust has a healthy parser ecosystem split along two axes: **grammar-driven (PEG / EBNF)** vs. **parser combinators**, and **batch input** vs. **streaming input**. The four main libraries you'll meet are [`pest`](https://pest.rs/) (PEG via a `.pest` grammar file + derive), [`nom`](https://github.com/rust-bakery/nom) (the workhorse byte-level combinator), [`winnow`](https://github.com/winnow-rs/winnow) (the modern nom fork from the clap maintainers, now nom's recommended successor for many uses), and [`chumsky`](https://github.com/zesterer/chumsky) (high-level combinators with first-class error recovery, aimed at language tooling).

## Insight

Pick by what you're parsing:

- **`pest`** — write a PEG grammar in a separate `.pest` file, derive `Parser`, walk the resulting `Pairs<Rule>` tree. Best for **DSLs and config formats** where the grammar is the spec and you want non-Rust people to read it. Cost: error messages are mediocre, and the runtime walk has overhead. Used by Pest itself's docs, [Bevy's `.bevy` shader bits](https://github.com/bevyengine/bevy), Handlebars-Rust.
- **`nom`** — byte-level combinators. Veteran, fast, used everywhere (rustc-demangle, Nix lockfile parsing, half the binary-format parsers on crates.io). Reach for it when you're parsing **wire formats / binary protocols / network packets** and ergonomics-of-strings matter less than low overhead. The 7.x rewrite moved to a function-style API; older `do_parse!` macro tutorials are misleading.
- **`winnow`** — fork of nom 7 by Ed Page (clap, cargo). API is sister to nom but cleaner, with better error type story. The Rust `Cargo.toml` parser uses it. The recommended modern choice unless you're already deep in nom.
- **`chumsky`** — combinator with **error recovery, lookahead, and pretty diagnostics** built in. The right pick when you're writing a **compiler / linter / language server** for a real programming language and care about reporting "unexpected `;`, expected `}`" with carets. Used by [Tao](https://github.com/zesterer/tao) and several toy languages. Slower than nom/winnow at raw throughput.
- **`combine`** — older Haskell-`parsec`-style combinators. Still maintained, niche today vs. winnow / chumsky.
- **`logos`** — not a parser, a *lexer* generator (derive macro, fast). Pair with one of the above when you want hand-rolled tokenisation.
- **`lalrpop`** — LALR(1) generator (yacc-shaped). Used by Solana, MoonBit, others. Heavier to learn; produces fast tables.
- **`tree-sitter`** ([Rust bindings](https://crates.io/crates/tree-sitter)) — incremental parser generator from GitHub. Different shape entirely: best for editor / language-server tooling that needs to re-parse on every keystroke.

Decision shortcut:

| Job | Pick |
|-----|------|
| DSL / config / template language | `pest` |
| Binary wire format, byte-level | `nom` or `winnow` |
| New parser today | `winnow` |
| Compiler with great errors | `chumsky` (+ `logos`) |
| LALR/EBNF you already have | `lalrpop` |
| Editor / re-parse every keystroke | `tree-sitter` |
| Just need a lexer | `logos` |

Gotchas:

1. **`pest`'s rules are PEG**, not CFG — they're greedy and never backtrack across choice. Easy to write a rule that "looks right" but always matches the first alternative.
2. **`nom` 5/6/7 incompatibility.** Most online tutorials are pre-7. Use the current docs.
3. **Combinator stack depth.** Deeply recursive grammars in `nom`/`winnow`/`chumsky` can blow the stack on large inputs; use iteration (`many0` / `repeat`) or boxed combinators for unbounded recursion.

## Similar / related topics

- [`logos`](https://github.com/maciejhirsz/logos) — derive-based lexer; pair with any combinator parser.
- [`lalrpop`](https://github.com/lalrpop/lalrpop) — LALR(1) parser generator; heavier syntax, fast tables.
- [`tree-sitter`](https://tree-sitter.github.io/) — incremental parser generator with Rust bindings; the editor / LSP standard.
- [`syn`](https://crates.io/crates/syn) — *the* Rust-syntax parser; if you're writing a proc-macro you want syn, not a general parser.
- [`pom`](https://crates.io/crates/pom) — small PEG combinator library; minimal deps, good for tiny grammars.

## Internal links

<!-- reviewed -->

- [[programming/rust/io/README|Rust I/O]] — section landing page.
- [[programming/rust/io/json|json]] — JSON has its own dedicated stack; don't reach for a general parser for it.
- [[programming/rust/sql_engine/sqlparser|sqlparser]] — SQL-specific hand-rolled parser, the canonical "parse a real language" example in the vault.

## Keywords

`#rust` `#parsers` `#pest` `#nom` `#chumsky` `#winnow` `#peg` `#combinators`

## References / raw notes

### pest — the elegant parser

> pest is a general purpose parser written in Rust with a focus on accessibility, correctness, and performance. It uses parsing expression grammars (or PEG) as input, which are similar in spirit to regular expressions, but which offer the enhanced expressivity needed to parse complex languages.

- Site / book: <https://pest.rs/>
- `pest_meta` (the grammar of the grammar): <https://crates.io/crates/pest_meta>
- Awesome list of projects using pest: <https://github.com/pest-parser/awesome-pest>

### Other libraries in the same space

- [`nom`](https://github.com/rust-bakery/nom) — byte-level combinators; mature, fast, ubiquitous in binary-format land.
- [`winnow`](https://github.com/winnow-rs/winnow) — nom 7 fork by Ed Page (clap/cargo); modern default for new combinator parsers.
- [`chumsky`](https://github.com/zesterer/chumsky) — combinators with error recovery; aimed at language tooling.
- [`combine`](https://github.com/Marwes/combine) — parsec-style combinators in Rust.
- [`logos`](https://github.com/maciejhirsz/logos) — derive-based lexer generator; pair with the parser of your choice.
- [`lalrpop`](https://github.com/lalrpop/lalrpop) — LALR(1) generator.
- [tree-sitter Rust bindings](https://crates.io/crates/tree-sitter) — incremental parsing, the LSP standard.
