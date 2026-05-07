---
title: difftastic — syntax-aware structural diff
main_link: https://github.com/Wilfred/difftastic
keywords: [difftastic, rust, diff, structural, tree-sitter, git, syntax]
status: reviewed
---

# difftastic — syntax-aware structural diff

**Main link:** <https://github.com/Wilfred/difftastic>

## Summary

`difftastic` (binary `difft`) is a structural diff tool by Wilfred Hughes that compares files based on their *syntax tree* rather than their lines. It uses [tree-sitter](https://tree-sitter.github.io/) parsers for ~50 languages, walks the resulting trees, and shows you what *expressions* changed instead of what lines changed. The classic example: a multi-line refactor that adds an `if` wrapper around a block reads as "added an if-statement", not "every line of the block changed".

## Insight

Reach for `difft` when reading a diff that line-based tools mangle: refactors that re-indent code, JSON/YAML re-orderings, multi-line strings, big merges, format-on-save churn. Wire it into git globally:

```sh
git config --global diff.external difft
git config --global diff.tool difftastic
git config --global difftool.difftastic.cmd 'difft "$LOCAL" "$REMOTE"'

# then:
GIT_EXTERNAL_DIFF=difft git diff
git difftool          # interactive
```

**Gotchas**: it is *slower* than `git diff` on huge diffs (parsing trees costs more than splitting lines); not ideal in CI where you want fast machine-readable output (use `git diff` for CI, `difft` for humans). It produces *coloured side-by-side* output, not a patch — you can't `git apply` its output. For unsupported languages it falls back to text diff.

Compared to siblings: **`diff(1)`** / **`git diff`** are the line-based originals; **[`delta`](https://github.com/dandavison/delta)** is a Rust *pretty-printer* for line-based diffs (different niche — pair with bat for syntax highlighting but still line-based); **[`riff`](https://github.com/walles/riff)** does intra-line refinement on top of line diffs. `difft` is the only one in this group that's actually syntax-aware. The original `tree-sitter-graph` and Semantic (GitHub's old Haskell tool) attempted similar but neither is in everyday use.

## Similar / related topics

- `diff(1)` / `git diff` — POSIX line-based originals.
- [`delta`](https://github.com/dandavison/delta) — pretty-printer for line diffs.
- [`riff`](https://github.com/walles/riff) — intra-line refinement on line diffs.
- [GitHub Semantic](https://github.com/github/semantic) — abandoned Haskell tool with the same goal.
- [tree-sitter](https://tree-sitter.github.io/) — the parser library difft is built on.

## Internal links

- [[README]] — tooling section landing.
- [[bat]] — `cat` clone with syntax highlighting (related-niche Rust tool).
- [[../../../git/git_delta|git_delta]] — the `delta` pager article (line-diff pretty-printer).

## Keywords

`#difftastic` `#difft` `#rust` `#diff` `#structural` `#tree-sitter` `#syntax` `#git`

## References / raw notes

- Repo: <https://github.com/Wilfred/difftastic>
- Crate: <https://crates.io/crates/difftastic>
- Docs: <https://difftastic.wilfred.me.uk/>
- Install: `cargo install --locked difftastic` or `brew install difftastic`.
