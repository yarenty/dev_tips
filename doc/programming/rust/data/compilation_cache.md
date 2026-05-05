---
title: sccache - Shared Compilation Cache
main_link: https://github.com/mozilla/sccache/
keywords: [compilation-cache, data, rust, programming, sccache, compilation, wrapper, rustc]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# sccache - Shared Compilation Cache

**Main link:** <https://github.com/mozilla/sccache/>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- TODO: at least 2 [[wikilinks]] to related articles in this vault. -->

## Keywords

`#compilation-cache` `#data` `#rust` `#programming` `#sccache` `#compilation` `#wrapper` `#rustc`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# sccache - Shared Compilation Cache

https://github.com/mozilla/sccache/

sccache is a ccache-like compiler caching tool. It is used as a compiler wrapper and avoids compilation when possible, storing cached results either on local disk or in one of several cloud storage backends.

sccache includes support for caching the compilation of C/C++ code, Rust, as well as NVIDIA's CUDA using nvcc.



## Usage

Running sccache is like running ccache: prefix your compilation commands with it, like so:

```sh
sccache gcc -o foo.o -c foo.c
```

If you want to use sccache for caching Rust builds you can define build.rustc-wrapper in the cargo configuration file. For example, you can set it globally in $HOME/.cargo/config.toml by adding:

```yaml
[build]
rustc-wrapper = "/path/to/sccache"
```

Note that you need to use cargo 1.40 or newer for this to work.

Alternatively you can use the environment variable RUSTC_WRAPPER:

```shell
export RUSTC_WRAPPER=/path/to/sccache
cargo build
```

sccache supports gcc, clang, MSVC, rustc, NVCC, and Wind River's diab compiler.

If you don't specify otherwise, sccache will use a local disk cache.
