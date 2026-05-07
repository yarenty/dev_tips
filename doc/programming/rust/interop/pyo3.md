---
title: PyO3
main_link: https://pyo3.rs
keywords: [pyo3, python, rust, ffi, bindings, interop]
status: reviewed
---

# PyO3

**Main link:** <https://pyo3.rs>

## Summary

[PyO3](https://pyo3.rs) is the de-facto Rust ↔ CPython binding crate,
maintained by David Hewitt and a wide community of contributors. It lets you
both *expose* Rust code to Python (the common case — building a native
extension module) and *call* CPython from a Rust process (the embedding
case). PyO3 is the foundation of the Rust-in-Python wave: Polars,
`pydantic-core`, Ruff, `cryptography`, Tokenizers, Robyn, `orjson`,
`bcrypt`, and roughly half of the modern Python performance-critical
package ecosystem all use it.

## Insight

Reach for PyO3 whenever you have a Rust crate (or want one) and need it to
be usable from `pip install`-land. The actual *building* is normally done
with [[maturin]] — PyO3 is the "what does the FFI look like" layer, maturin
is the "how do I produce a wheel" layer. They are co-developed and are
almost always used together; `setuptools-rust` is the older alternative if
you must integrate with an existing `setup.py`/`pyproject.toml` build
pipeline.

A few things worth knowing before adopting it:

- **GIL story**: until ~2024, every PyO3 call took a `Python<'py>` token
  proving you held the GIL. Modern PyO3 (0.21+) introduced the
  `Bound<'py, T>` / `Py<T>` split and is being incrementally retrofitted
  for the free-threaded ("no-GIL") CPython 3.13+ — expect breaking changes
  release-to-release until this stabilises. Pin your version.
- **Async**: `pyo3-asyncio` (now `pyo3-async-runtimes`) bridges Rust
  futures to `asyncio`. Cross-runtime panics and event-loop ownership are
  the usual footguns.
- **ABI**: builds are CPython-version-specific by default. The `abi3`
  feature (stable ABI) gives you one wheel for many Python versions at the
  cost of giving up some APIs — usually worth it for distribution.
- **Predecessors**: `rust-cpython` was the earlier crate (also
  Dropbox-originated); PyO3 forked from it in 2017 and has fully eclipsed
  it. Don't start a new project on `rust-cpython`.
- **Alternatives by direction**: from the Python side, `cffi` and `ctypes`
  call any C ABI (so you can dlopen a `cdylib` Rust crate); Cython is the
  C-extension answer; `nanobind`/`pybind11` are the C++ analogues.

## Similar / related topics

- [[maturin]] — the build tool that turns a PyO3 crate into a wheel.
- [[to_python]] — the recipe page: "I have a Rust crate, how do I publish
  it to PyPI?"
- [[programming/rust/interop/python|python (PUFF)]] — embeds CPython inside
  a Rust process (the inverse direction PyO3 also supports).
- `cffi` / `ctypes` — Python-side FFI to any C ABI, including a Rust
  `cdylib`. Looser typing, no Rust-side ergonomics.
- `nanobind` / `pybind11` — the C++ equivalents; PyO3 occupies the same
  ecological niche for Rust.

## Internal links

<!-- reviewed -->

- [[maturin]] — companion build tool.
- [[to_python]] — Rust→Python publishing recipe.
- [[programming/rust/interop/python|interop/python]] — PUFF (CPython
  embedded in Rust).
- [[ballista_py]] — example of a real-world PyO3+maturin Python binding.
- [[README]] — Rust interop landing.

## Keywords

`#pyo3` `#python` `#rust` `#ffi` `#bindings` `#interop`

## References / raw notes

- [pyo3.rs](https://pyo3.rs) — the user guide (start here).
- [GitHub — PyO3/pyo3](https://github.com/PyO3/pyo3)
- [Conversion table](https://pyo3.rs/main/conversions/tables) — Rust ↔
  Python type mapping cheatsheet.
- [Calling Rust from Python (saidvandeklundert)](https://saidvandeklundert.net/learn/2021-11-18-calling-rust-from-python-using-pyo3/)
  — short blog intro.
- Notable users: [Polars](https://github.com/pola-rs/polars),
  [pydantic-core](https://github.com/pydantic/pydantic-core),
  [Ruff](https://github.com/astral-sh/ruff),
  [cryptography](https://github.com/pyca/cryptography),
  [Tokenizers](https://github.com/huggingface/tokenizers).
