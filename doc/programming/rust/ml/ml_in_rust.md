---
title: ML libraries in Rust
main_link: http://www.arewelearningyet.com/
keywords: [ml, rust, linfa, candle, burn, dfdx, tch, smartcore, big-brain]
status: reviewed
---

# ML libraries in Rust

**Main link:** <http://www.arewelearningyet.com/>

## Summary

Overview of the Rust ML landscape, viewed from a Rust perspective. The
canonical "are we there yet?" tracker is
[arewelearningyet.com](http://www.arewelearningyet.com/). The ecosystem
splits into three families: **classical ML** (scikit-learn-shaped — see
[[linfa]]), **deep learning** (PyTorch-shaped — `candle`, `burn`,
`tch-rs`, `dfdx`), and **decision-making / utility AI** for games and
agents (`big-brain`, `ai_kit`). For the broader ML/AI body of knowledge
that is not language-specific, see [[../../../ml/README]] and
[[../../../ml/frameworks/README]].

## Insight

Rust is a poor fit for *training* large deep nets in 2025 — the
ecosystem, tooling (notebooks, debuggers, autograd), and pre-trained
weights all live in PyTorch/JAX. Where Rust ML actually shines is:

- **Inference at the edge / in production** — `candle` (HuggingFace) and
  `burn` (Tracel) load HF/PyTorch checkpoints and run them with no
  Python runtime, smaller binaries, simpler containers.
- **Classical ML in production services** — [[linfa]] gives you logistic
  regression, k-means, PCA, etc. as a normal Cargo dependency with no
  Python install.
- **GPU compute as a Rust-first concern** — see [[cuda|cuda (Rust)]] for
  `cudarc` / `cubecl` / `Rust-CUDA`.
- **Game / agent decision systems** — `big-brain` (Bevy ECS-native
  utility AI) is the standout.

Picker:

| Need                                                | Reach for           |
|-----------------------------------------------------|---------------------|
| sklearn-equivalent (linear/logistic/PCA/k-means)    | [[linfa]]           |
| Inference of HuggingFace / PyTorch models in Rust   | `candle`            |
| Train a smallish neural net entirely in Rust        | `burn` or `dfdx`    |
| PyTorch C++ from Rust (libtorch FFI)                | `tch-rs`            |
| Utility AI for a Bevy game                          | `big-brain`         |
| GPU compute kernels in Rust                         | [[cuda|cubecl/cudarc]] |
| Distributed map-reduce-shaped data analysis         | DataFusion / Polars (not really "ML") |

Things to know:

- **`candle` vs `burn`**: `candle` is the more mature inference path
  (HF backing, lots of ports of LLaMA / Mistral / Phi / Whisper);
  `burn` has the more elegant API, multiple backends (wgpu, CUDA,
  Metal, Candle), and is positioning as the "PyTorch of Rust". Pick
  candle if you want models running this week, burn if you're building
  a longer-lived in-house framework.
- **`tch-rs`** is the pragmatic escape hatch — full libtorch behind a
  Rust API. Heavy native dependency, but you get every PyTorch op and
  CUDA kernel.
- **`linfa`** is the only serious classical-ML toolkit in pure Rust;
  see its dedicated page for the full algorithm matrix.

## Similar / related topics

- [[linfa]] — the scikit-learn-equivalent toolkit; classical ML.
- [[cuda|cuda (Rust)]] — GPU compute crates for Rust.
- [[../../../ml/frameworks/README]] — broader framework hub (Python,
  C++, Rust together).
- [arewelearningyet.com](http://www.arewelearningyet.com/) — community
  Rust-ML status tracker.
- [rust-ml/book](https://github.com/rust-ml/book) — community-written
  intro that covers k-means and friends with linfa.

## Internal links

<!-- reviewed -->

- [[linfa]] — sibling article on the classical-ML toolkit.
- [[cuda|cuda (Rust)]] — sibling article on Rust GPU compute.
- [[README]] — Rust ML section landing.
- [[../../../ml/README]] — broader ML hub.
- [[../../../ml/frameworks/README]] — ML frameworks across languages.

## Keywords

`#ml` `#rust` `#linfa` `#candle` `#burn` `#dfdx` `#tch` `#big-brain`

## References / raw notes

### Are we learning yet?

[arewelearningyet.com](http://www.arewelearningyet.com/) — the canonical
status site. Tracks crates by category (linear algebra, NLP, CV,
reinforcement learning, etc.) with maintenance signals.

### Linfa

[github.com/rust-ml/linfa](https://github.com/rust-ml/linfa) — the most
mature classical-ML toolkit in Rust; covered in detail in [[linfa]].

### Deep-learning frameworks (not detailed here; track upstream)

- [`candle`](https://github.com/huggingface/candle) — HuggingFace's
  minimal-dependency inference framework. Many model ports.
- [`burn`](https://github.com/tracel-ai/burn) — modern PyTorch-shaped
  framework with multiple backends (wgpu/CUDA/Metal/Candle).
- [`tch-rs`](https://github.com/LaurentMazare/tch-rs) — Rust bindings
  for libtorch (PyTorch C++ API).
- [`dfdx`](https://github.com/coreylowman/dfdx) — pure-Rust deep
  learning with type-level shapes (compile-time tensor shape checking).

### Big Brain (utility AI for Bevy)

[lib.rs/crates/big-brain](https://lib.rs/crates/big-brain) — utility-AI
library for the Bevy game engine. Define `Scorer`s (perceive the world,
emit a score) and `Action`s (do things), wire them with `Thinker`s.
ECS-native, highly parallel, no boilerplate beyond your domain types.

### AI Kit

[lib.rs/crates/ai_kit](https://lib.rs/crates/ai_kit) — single-dependency
collection of classical AI algorithms (unification, planning, etc.).
Trait-based core (`BindingsValue`, `Unify`, `Operation`); ships optional
data types (`Datum`, `Rule`) so it's usable out of the box.

### Honourable mentions / older crates

- [`liquid-ml`](https://crates.io/crates/liquid-ml) — distributed
  data-analysis MVP (decision tree, random forest demos). Mostly of
  historical interest now; reach for DataFusion / Polars + linfa
  instead.
- [`easy-ml`](https://github.com/Skeletonxf/easy-ml) — pedagogical
  pure-Rust library (linear regression, k-means, naive bayes, NN,
  autodiff). Good for learning, not for production.
- [`smartcore`](https://github.com/smartcorelib/smartcore) — a
  linfa-adjacent classical-ML library; partial overlap with linfa,
  worth a look if linfa is missing what you need.
- `simple_ml` / `util_ml` (`radialHuman/rust`) — single-file mini
  libraries; useful as code-reading for a specific algorithm, not as
  dependencies.
