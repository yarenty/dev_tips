---
title: Linfa
main_link: https://github.com/rust-ml/linfa
keywords: [linfa, ml, rust, scikit-learn, classical-ml, clustering, regression]
status: reviewed
---

# Linfa

**Main link:** <https://github.com/rust-ml/linfa>

## Summary

[`linfa`](https://github.com/rust-ml/linfa) is the most mature
classical-ML toolkit for Rust — kin in spirit to Python's `scikit-learn`
and curated by the [rust-ml](https://github.com/rust-ml) working group.
It ships ~15 sub-crates covering clustering (K-Means, GMM, DBSCAN,
OPTICS), linear / logistic / elastic-net regression, decision trees,
SVMs, naive Bayes, hierarchical clustering, ICA, PCA, t-SNE, partial
least squares, follow-the-regularised-leader, plus pre-processing and
nearest-neighbour utilities. If your problem fits in scikit-learn's
"classical ML" box, linfa is the Rust answer.

## Insight

Reach for linfa when you want:

- A scikit-learn-shaped algorithm (k-means, logistic regression, PCA,
  decision trees, SVMs, naive Bayes, etc.) inside a Rust service or CLI.
- No Python runtime in your container / binary / firmware.
- Static typing, Cargo, and the Rust performance baseline (most
  algorithms are tested *and* benchmarked).

Reach for something else when:

- You need deep learning. Linfa is *classical* ML — see
  [[ml_in_rust]] for `candle`, `burn`, `tch-rs`, `dfdx`.
- You need scikit-learn's full breadth (xgboost, lightgbm, gradient
  boosting, exotic kernels, advanced model selection). Linfa's
  algorithm count is in the dozens, not hundreds. The honest fallback
  is `tch-rs` to call libtorch / a Python sidecar via [[pyo3]].
- You need GPU. Linfa is CPU-only; the GPU story in Rust ML is in
  [[cuda|cuda (Rust)]] and `candle`/`burn`.

A few practical notes:

- **Datasets**: `linfa::Dataset` wraps `ndarray::Array2` for features
  and a label vector. Most algorithms take and return `Dataset`s, which
  makes pipelines composable but requires you to learn `ndarray` first.
- **Backends**: linear algebra goes through `ndarray-linalg`, which
  needs a BLAS — `openblas-static`, `intel-mkl`, or system BLAS via
  feature flags. Pick one explicitly; a forgotten feature flag is the
  canonical first-build failure.
- **Status**: actively maintained but releases are infrequent. The
  algorithm matrix in the README is the source of truth for what's
  benchmarked vs merely tested.
- **Cross-link**: [smartcore](https://github.com/smartcorelib/smartcore)
  is a parallel project with overlapping goals and a slightly different
  algorithm set; check both if linfa is missing what you need.

## Similar / related topics

- [[ml_in_rust]] — wider Rust-ML landscape (deep learning + GPU).
- `scikit-learn` — the Python reference linfa imitates.
- `smartcore` — adjacent Rust classical-ML library.
- `candle` / `burn` — the deep-learning answer (different problem
  shape).
- `polars` + DataFusion — feature-engineering substrate that pairs well
  with linfa.

## Internal links

<!-- reviewed -->

- [[ml_in_rust]] — sibling overview.
- [[cuda|cuda (Rust)]] — sibling on the GPU side.
- [[README]] — Rust ML section landing.
- [[../../../ml/frameworks/README]] — broader ML frameworks hub.

## Keywords

`#linfa` `#ml` `#rust` `#scikit-learn` `#classical-ml` `#clustering` `#regression`

## References / raw notes

- [github.com/rust-ml/linfa](https://github.com/rust-ml/linfa)
- [Linfa website](https://rust-ml.github.io/linfa/)
- [Community chat (Zulip)](https://rust-ml.zulipchat.com/)
- [Are we learning yet?](http://www.arewelearningyet.com/) — Rust-ML
  status tracker.
- [rust-ml/book](https://github.com/rust-ml/book) — k-means tutorial
  using linfa.

### Algorithm matrix

| Crate          | Purpose                                   | Status                | Category             | Notes |
|----------------|-------------------------------------------|-----------------------|----------------------|-------|
| `clustering`   | Data clustering                           | Tested / Benchmarked  | Unsupervised         | K-Means, GMM, DBSCAN, OPTICS |
| `kernel`       | Kernel methods                            | Tested                | Pre-processing       | Maps features into higher-dim space |
| `linear`       | Linear regression                         | Tested                | Partial fit          | OLS + GLMs |
| `elasticnet`   | Elastic Net                               | Tested                | Supervised           | Linear regression + elastic-net constraints |
| `logistic`     | Logistic regression                       | Tested                | Partial fit          | Two-class |
| `reduction`    | Dimensionality reduction                  | Tested                | Pre-processing       | Diffusion mapping + PCA |
| `trees`        | Decision trees                            | Tested / Benchmarked  | Supervised           | Linear decision trees |
| `svm`          | Support Vector Machines                   | Tested                | Supervised           | Classification or regression |
| `hierarchical` | Agglomerative hierarchical clustering     | Tested                | Unsupervised         | Cluster + build cluster hierarchy |
| `bayes`        | Naive Bayes                               | Tested                | Supervised           | Gaussian Naive Bayes |
| `ica`          | Independent component analysis            | Tested                | Unsupervised         | FastICA |
| `pls`          | Partial Least Squares                     | Tested                | Supervised           | Dimensionality reduction + regression |
| `tsne`         | Dimensionality reduction                  | Tested                | Unsupervised         | Exact + Barnes-Hut t-SNE |
| `preprocessing`| Normalization & vectorization             | Tested / Benchmarked  | Pre-processing       | Whitening + count vectorization / TF-IDF |
| `nn`           | Nearest Neighbours & Distances            | Tested / Benchmarked  | Pre-processing       | Spatial indices + distance functions |
| `ftrl`         | Follow-The-Regularised-Leader (proximal)  | Tested / Benchmarked  | Partial fit          | L1 + L2 regularisation; incremental update |

(See the [linfa README](https://github.com/rust-ml/linfa#current-state)
for current status — the matrix above is a snapshot.)
