---
title: ripgrep — fast recursive `grep` (the defining Rust replacement)
main_link: https://github.com/BurntSushi/ripgrep
keywords: [ripgrep, rg, rust, grep, ag, ack, search, regex, unix-replacement]
status: reviewed
---

# ripgrep — fast recursive `grep` (the defining Rust replacement)

**Main link:** <https://github.com/BurntSushi/ripgrep>

## Summary

`ripgrep` (binary `rg`) is Andrew Gallant's (BurntSushi) line-oriented regex search tool — recursive by default, gitignore-aware by default, multi-threaded, and built on his own `regex` and `memchr` crates. It is the canonical and defining article in the "Rust replaces a Unix tool" trend: Andrew's original blog post [*ripgrep is faster than {grep, ag, git grep, ucg, pt, sift}*](https://blog.burntsushi.net/ripgrep/) (2016) is required reading for anyone interested in either Rust performance or text-search engineering.

## Insight

Reach for `rg` whenever you'd reach for `grep -r` in a source tree. The defaults are the killer feature: it skips `.git/`, respects `.gitignore` and `.ignore`, skips binaries (set `--binary` to override), and uses parallelism without you asking. **Common patterns**:

```sh
rg 'TODO|FIXME'                      # extended regex, two patterns
rg -t rust 'unsafe'                  # only Rust files
rg -g '!*.lock' 'serde'              # exclude glob
rg -l 'pub fn'                       # filenames only
rg -A 3 -B 1 'panic!'                # context lines
rg --json 'foo' | jq                 # structured output for tooling
rg -P '(?<=\bfn\s)\w+'               # PCRE2 lookbehinds (built with --features pcre2)
```

**Gotchas**: `rg` defaults to *Rust regex* syntax (no lookarounds, no backrefs); use `-P` for PCRE2 if compiled in, or `-F` for fixed-string. Binary detection uses a heuristic — minified JS sometimes triggers it (override with `-a`/`--text`). Editor integrations (VS Code, Helix, Neovim's telescope/`fzf-lua`) all default to `rg` for project search.

Compared to siblings: **`grep`** is universal and POSIX; **`ag`** (the silver searcher, C) was the first "fast" recursive `grep` but is no longer faster than `rg`; **`ack`** (Perl) is the readable but slow ancestor; **`ugrep`** (C++) is the closest competitor on benchmarks. For *replace* (not just search), see [`sd`](https://github.com/chmln/sd) (Rust, sed-shaped).

## Similar / related topics

- `grep`(1) / `egrep` / `fgrep` — the POSIX originals.
- `ag` (the silver searcher) / `ack` — the previous "fast grep" generation.
- [`ugrep`](https://github.com/Genivia/ugrep) — C++ competitor, close on speed.
- [`sd`](https://github.com/chmln/sd) — Rust `sed` replacement (the natural pairing).
- [`fd`](https://github.com/sharkdp/fd) — Rust `find` replacement (also by sharkdp).

## Internal links

- [[README]] — tooling section landing.
- [[bat]] / [[exa]] / [[dust]] / [[bottom]] — siblings in the "Rust replaces a Unix tool" family.
- [[../../../tools/shell/must_have|must_have]] — fresh-box CLI bundle.

## Keywords

`#ripgrep` `#rg` `#rust` `#grep` `#search` `#regex` `#unix-replacement` `#cli`

## References / raw notes

- Repo: <https://github.com/BurntSushi/ripgrep>
- Crate: <https://crates.io/crates/ripgrep>
- Andrew Gallant's design post: <https://blog.burntsushi.net/ripgrep/> — canonical reading.
- Install: `cargo install ripgrep` or `brew install ripgrep` / `apt install ripgrep`.
