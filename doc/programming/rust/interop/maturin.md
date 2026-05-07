---
title: maturin
main_link: https://github.com/PyO3/maturin
keywords: [maturin, pyo3, python, rust, build, wheels, packaging]
status: reviewed
---

# maturin

**Main link:** <https://github.com/PyO3/maturin>

## Summary

[maturin](https://github.com/PyO3/maturin) is the build tool for Python
packages whose extension modules are written in Rust. It produces
PEP-427/660-compliant wheels and source distributions from a Rust crate
that uses [[pyo3|PyO3]], `cffi`, `uniffi`, or `cbindgen`, supports Python
3.8+ on Windows / Linux / macOS / FreeBSD, and has basic PyPy and GraalPy
coverage. Think of it as "Cargo for PyO3 packages": one binary, no
`setup.py`.

## Insight

If you're authoring a PyO3-based Python package, maturin is the default
answer for *building* it. The two commands you'll actually use are:

- **`maturin develop`** — builds the Rust crate and installs it into the
  active virtualenv as an editable extension. This is the inner-loop
  command; it's much faster than `pip install -e .` because it skips wheel
  packaging.
- **`maturin build --release`** — produces a wheel under `target/wheels/`.
  Add `--strip` to drop debug symbols.
- **`maturin publish`** — `build` + upload to PyPI in one step. In CI you
  almost always want `build` followed by a separate `twine upload` or
  `gh-action-pypi-publish` step instead, for token hygiene.

Things that bite people:

- **Manylinux**: PyPI wheels for Linux must be built against an old glibc.
  Maturin ships a Docker image
  (`ghcr.io/pyo3/maturin`) and a Zig-based mode
  (`maturin build --zig`) that cross-compiles against a manylinux baseline
  without spinning up a container — much faster in CI.
- **Universal2 macOS**: `--target universal2-apple-darwin` produces an
  `x86_64+arm64` fat wheel. Apple Silicon CI runners are now the easier
  route.
- **abi3**: combine the PyO3 `abi3-py38` feature with maturin's
  `--features` and you get one wheel that works on every CPython 3.8+ —
  drastically fewer wheels to publish.
- **Layout**: the canonical project layout is a `pyproject.toml` with
  `[build-system] requires = ["maturin>=1.4,<2.0"]`, a `Cargo.toml` next
  to it, and an optional `python/` subdirectory of pure-Python code that
  re-exports the Rust extension.

The main alternative is `setuptools-rust` (still useful when you must drop
into an existing setuptools build chain — e.g. `cryptography`); maturin is
otherwise the de-facto standard.

## Similar / related topics

- [[pyo3|PyO3]] — the FFI layer maturin almost always builds.
- [[to_python]] — the end-to-end "publish a Rust crate to PyPI" recipe.
- `setuptools-rust` — the older Setuptools-integrated alternative.
- `cibuildwheel` — orthogonal: drives matrix builds across Python
  versions/platforms in CI; works fine on top of maturin.
- `uniffi-rs` — Mozilla's multi-language binding generator (Python,
  Kotlin, Swift, Ruby) that maturin can also build for.

## Internal links

<!-- reviewed -->

- [[pyo3|PyO3]] — the FFI binding layer.
- [[to_python]] — Rust → PyPI recipe.
- [[ballista_py]] — example PyO3+maturin Python package.
- [[README]] — Rust interop landing.

## Keywords

`#maturin` `#pyo3` `#python` `#rust` `#packaging` `#wheels`

## References / raw notes

- [GitHub — PyO3/maturin](https://github.com/PyO3/maturin)
- [maturin user guide](https://www.maturin.rs/)
- Original blurb: "Build and publish crates with pyo3, cffi and uniffi
  bindings as well as rust binaries as python packages with minimal
  configuration. It supports building wheels for python 3.8+ on Windows,
  Linux, macOS and FreeBSD, can upload them to pypi and has basic PyPy
  and GraalPy support."
- [`maturin build --zig`](https://www.maturin.rs/distribution#cross-compile-to-linux-aarch64-using-zig)
  — cross-compile manylinux wheels without Docker.
