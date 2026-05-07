---
title: rust_in_jupyter — `evcxr_jupyter` kernel for Rust notebooks
main_link: https://github.com/evcxr/evcxr/blob/main/evcxr_jupyter/README.md
keywords: [rust-in-jupyter, evcxr, jupyter, notebook, polars, plotters, data-science]
status: reviewed
review_date: 2026/05/03
---

# rust_in_jupyter — `evcxr_jupyter` kernel for Rust notebooks

**Main link:** <https://github.com/evcxr/evcxr/blob/main/evcxr_jupyter/README.md>

## Summary

`evcxr_jupyter` is a Jupyter kernel that lets you run Rust code in a notebook cell, install crates with `:dep`, and render charts and tables inline. It's built on the same [[repl|`evcxr`]] evaluation engine as the terminal Rust REPL. Install with `cargo install --locked evcxr_jupyter && evcxr_jupyter --install`; thereafter "Rust" appears in the Jupyter / JupyterLab / VS Code notebook kernel picker.

## Insight

The killer use case is **data exploration in Rust** — pair `evcxr_jupyter` with:

- **[`polars`](https://github.com/pola-rs/polars)** — fast Rust DataFrame library; the same library you'd use in production (Rust crate or Python `polars` package). Notebooks let you iterate on the query *and* keep the production-ready Rust types.
- **[`plotters`](https://github.com/plotters-rs/plotters)** — Rust drawing library with a `evcxr` integration; renders SVG/PNG inline.
- **[`ndarray`](https://github.com/rust-ndarray/ndarray)** — for numerics; pairs with `linfa` for ML.
- **[`tract`](https://github.com/sonos/tract)** / **[`burn`](https://github.com/tracel-ai/burn)** — for inference / model development; iterating in a notebook keeps you fast.

```sh
cargo install --locked evcxr_jupyter
evcxr_jupyter --install
jupyter notebook       # or jupyter lab / VS Code
```

In a notebook cell:

```rust
:dep polars = { version = "0.40", features = ["lazy", "csv"] }
:dep plotters = { version = "0.3", default-features = false, features = ["evcxr", "all_series"] }

use polars::prelude::*;
let df = LazyCsvReader::new("sales.csv").finish()?
    .group_by([col("region")])
    .agg([col("amount").sum()])
    .collect()?;
df
```

**When it shines**: prototyping a data pipeline you'll later extract into a binary; teaching Rust with cells you can iterate on; demos at meetups; reproducible analysis where you want the same crate versions in notebook and prod.

**When it doesn't**: tight async / GUI / native-deps work — the REPL workspace doesn't always cope cleanly with crates that need build scripts. Heavy crates (`tokio` with `full`, `polars` with all features) take 30–60s to compile on first `:dep`. State is per-kernel; restart kernel ⇒ rebuild dependencies.

**Compared to siblings**:

- [Python's Jupyter] — vastly more ecosystem; the natural choice if you don't specifically need Rust types.
- **[Pluto.jl](https://plutojl.org/)** for Julia — reactive notebooks; different paradigm.
- **[`marimo`](https://marimo.io/)** for Python — reactive Python notebooks; closer to Pluto's model.
- **[`Quarto`](https://quarto.org/)** with the Rust engine — closer to literate-programming output.

## Similar / related topics

- [[repl]] — the underlying `evcxr` engine; terminal REPL.
- [`polars`](https://github.com/pola-rs/polars) — DataFrame library; the natural pairing.
- [`plotters`](https://github.com/plotters-rs/plotters) — drawing library with evcxr integration. See also [[../../../visualization/plotters|visualization/plotters]].
- [Jupyter](https://jupyter.org/) — the notebook host.

## Internal links

- [[README]] — tooling section landing.
- [[repl]] — sibling `evcxr` REPL article.
- [[../../../visualization/plotters|visualization/plotters]] — Rust drawing library used inline in evcxr notebooks.

## Keywords

`#rust-in-jupyter` `#evcxr` `#jupyter` `#notebook` `#polars` `#plotters` `#data-science`

## References / raw notes

- evcxr repo: <https://github.com/evcxr/evcxr>
- Jupyter kernel install: `cargo install --locked evcxr_jupyter && evcxr_jupyter --install`.
- After install, launch with `jupyter notebook` (or lab) and pick the "Rust" kernel.
