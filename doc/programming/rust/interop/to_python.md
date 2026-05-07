---
title: Rust → Python (publishing recipe)
main_link: https://pyo3.rs
keywords: [to-python, rust, python, pyo3, maturin, datafusion, pypi]
status: reviewed
review_date: 2026/05/03
---

# Rust → Python (publishing recipe)

**Main link:** <https://pyo3.rs>

## Summary

This is the recipe page for the most common Rust ↔ Python interop
question: *"I have a Rust crate; how do I make it usable from Python and
publish it to PyPI?"* The canonical answer is [[pyo3|PyO3]] for the FFI
bindings + [[maturin]] for the build/wheel/upload pipeline. Apache
[DataFusion-Python](https://github.com/apache/datafusion-python) is the
flagship real-world example to read — it's a substantial Rust SQL engine
shipped as `pip install datafusion`.

## Insight

The minimal end-to-end flow is:

1. **Cargo.toml**: set `crate-type = ["cdylib"]` and depend on `pyo3` with
   the right Python-version feature (`extension-module`, optionally
   `abi3-py38` for a single wheel across Python versions).
2. **`#[pymodule]`** function in `lib.rs` that registers your
   `#[pyfunction]`s and `#[pyclass]`es. PyO3's
   [conversion table](https://pyo3.rs/main/conversions/tables) tells you
   which Rust types map automatically and which need explicit conversion.
3. **pyproject.toml** with `[build-system] requires = ["maturin>=1.4,<2"]`
   and `build-backend = "maturin"`.
4. **`maturin develop`** for the inner loop, **`maturin build --release`**
   for a wheel, **`maturin publish`** (or CI + `pypa/gh-action-pypi-publish`)
   to push to PyPI.

Things to settle up front, before you write a line of binding code:

- **Sync vs async**: PyO3 supports `#[pyfunction] async fn` since 0.21 by
  bridging to `asyncio` via `pyo3-async-runtimes`. Decide whether your
  Python API is sync or async — the calling code looks very different.
- **GIL discipline**: long Rust computations should release the GIL with
  `py.allow_threads(|| { ... })` so other Python threads can run.
- **Error mapping**: implement `From<MyError> for PyErr` (or use
  `pyo3::exceptions::PyValueError::new_err(msg)`) — don't `unwrap()` your
  way out, that panics across the FFI boundary.
- **Distribution matrix**: cibuildwheel + maturin in GitHub Actions is the
  standard recipe. For Linux you want manylinux (or `--zig` cross-build);
  for macOS, `universal2`; for Windows, both x64 and ARM64 if you care.
- **Type stubs**: ship a `.pyi` next to the `.so` so IDEs see proper
  signatures. PyO3 doesn't auto-generate stubs;
  [pyo3-stub-gen](https://github.com/Jij-Inc/pyo3-stub-gen) does.

For the *inverse* direction (embed CPython inside Rust), see
[[programming/rust/interop/python|interop/python (PUFF)]]. For Apache
Ballista's pyo3-based Python bindings as a worked example, see
[[ballista_py]].

## Similar / related topics

- [[pyo3|PyO3]] — the FFI binding crate.
- [[maturin]] — wheel-build & publish tool.
- [[ballista_py]] — Python bindings for distributed DataFusion.
- `setuptools-rust` — older alternative to maturin for setuptools shops.
- `uniffi-rs` — Mozilla's multi-language binding generator (Python +
  Kotlin + Swift) when you also need mobile bindings.

## Internal links

- [[pyo3|PyO3]] — FFI layer.
- [[maturin]] — build & publish.
- [[programming/rust/interop/python|interop/python]] — the inverse
  direction (embed CPython in Rust).
- [[ballista_py]] — worked PyO3 example.
- [[README]] — Rust interop landing.

## Keywords

`#to-python` `#rust` `#python` `#pyo3` `#maturin` `#pypi` `#interop`

## References / raw notes

- [PyO3 user guide](https://pyo3.rs/v0.20.0/getting_started)
- [Calling Rust from Python using PyO3 (saidvandeklundert)](https://saidvandeklundert.net/learn/2021-11-18-calling-rust-from-python-using-pyo3/)
  — short blog intro.
- [PyO3 conversion tables](https://pyo3.rs/v0.20.0/conversions/tables) —
  Rust ↔ Python type mapping reference.

### Datafusion (worked example)

[apache/datafusion-python](https://github.com/apache/datafusion-python) —
DataFusion's Python bindings, built with PyO3 + maturin. A good
production-grade reference for crate layout, CI, manylinux wheels, async
patterns, and stub generation.
