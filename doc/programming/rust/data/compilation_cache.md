---
title: sccache — shared compilation cache
main_link: https://github.com/mozilla/sccache/
keywords: [sccache, ccache, compilation-cache, rustc, build, cuda]
status: reviewed
---

# sccache — shared compilation cache

**Main link:** <https://github.com/mozilla/sccache/>

## Summary

`sccache` is a `ccache`-style compiler-caching tool from Mozilla. It wraps your compiler (rustc, gcc, clang, MSVC, NVCC, Wind River diab) and skips recompilation when input hashes match a previous run. The cache can live on local disk, on Redis/Memcached, or on object storage (S3, GCS, Azure Blob, R2). It is the canonical "shared build cache" for Rust + CI environments.

## Insight

Reach for sccache when you have **either** a slow Rust build (large workspace, many features), **or** multiple machines/CI workers building the same project (the shared S3 backend is the killer feature — every PR pulls warm artefacts). The simplest local install: set `RUSTC_WRAPPER` (or `build.rustc-wrapper` in `~/.cargo/config.toml`) to the sccache binary, and Cargo automatically routes per-crate compiles through it. CI pattern: `SCCACHE_BUCKET=...` + `SCCACHE_REGION=...` + `RUSTC_WRAPPER=sccache` in your job env. Gotchas: incremental compilation (`CARGO_INCREMENTAL=1`) and sccache **don't compose well** — disable incremental in CI and let sccache do the work; the cache key includes the full `RUSTFLAGS` so toggling them invalidates everything; on macOS, the launchd-managed `sccache --start-server` daemon sometimes wedges — `sccache --stop-server` + restart fixes it. For C++ the original `ccache` is still leaner; for distributed C++ builds look at `distcc` or `BuildBuddy`.

## Similar / related topics

- `ccache` — the original C/C++ compiler cache; sccache's spiritual ancestor.
- `cargo-cache` — managing the local Cargo registry/build cache (different scope).
- Bazel / Buck2 — full build systems with built-in remote caching.
- `cranky` / `cargo-zigbuild` — adjacent Rust build helpers.
- `mold` / `lld` — link-time speedups; pair well with sccache.

## Internal links
- [[../tooling/README|Rust tooling]] — broader build/dev-tools landing.
- [[cache]] — in-process caches (very different scope).

## Keywords

`#sccache` `#compilation-cache` `#rustc` `#ccache` `#build`

## References / raw notes

- Repo: <https://github.com/mozilla/sccache/>

> sccache is a ccache-like compiler caching tool. It is used as a compiler wrapper and avoids compilation when possible, storing cached results on local disk or in one of several cloud storage backends. Supports caching of C/C++ code, Rust, and NVIDIA CUDA via nvcc.

## Usage

Prefix your compilation commands with sccache like ccache:

```sh
sccache gcc -o foo.o -c foo.c
```

For Rust, set `build.rustc-wrapper` in `$HOME/.cargo/config.toml`:

```toml
[build]
rustc-wrapper = "/path/to/sccache"
```

Requires Cargo 1.40+.

Or use the environment variable:

```sh
export RUSTC_WRAPPER=/path/to/sccache
cargo build
```

Supported compilers: gcc, clang, MSVC, rustc, NVCC, Wind River diab. Default backend is local disk.
