---
title: k-means clustering
main_link: https://scikit-learn.org/stable/modules/clustering.html#k-means
keywords: [kmeans, clustering, unsupervised, lloyd, kmeans-pp]
status: reviewed
review_date: 2026/05/03
---

# k-means clustering

**Main link:** <https://scikit-learn.org/stable/modules/clustering.html#k-means>

## Summary

k-means is the canonical unsupervised clustering algorithm: pick `k`, randomly initialise `k`
centroids, then iterate "assign each point to its nearest centroid; recompute centroids as the
mean of their assigned points" until convergence. The standard implementation is **Lloyd's
algorithm** (1957/1982); the standard initialisation upgrade is **k-means++** (Arthur & Vassilvitskii
2007), which spreads initial centroids out and dramatically reduces sensitivity to seed. Available
everywhere — scikit-learn (`KMeans`, `MiniBatchKMeans`), Spark MLlib, Faiss, Rust's
`[[programming/rust/ml/linfa|linfa]]` (and the [`rust-ml/book` k-means chapter](https://github.com/rust-ml/book/blob/master/src/3_kmeans.md) is a good walkthrough of the Rust implementation).

## Insight

k-means is the *default first thing to try* for unsupervised clustering on numeric Euclidean data —
it's fast, well-understood, scales to millions of points with `MiniBatchKMeans`, and almost every
ML library ships it. But it has four well-known sharp edges:

1. **You have to pick `k`.** The elbow method, silhouette score, and gap statistic are the standard
   diagnostics, but none gives a definitive answer. If "what's the right number of clusters?" is
   the actual research question, k-means is the wrong tool — try Gaussian mixtures with BIC, or HDBSCAN.
2. **Sensitive to feature scaling.** A feature measured in millions will dominate one measured in
   millimetres. Always `StandardScaler` (or domain-specific normalise) numeric features first.
3. **Assumes spherical, equal-variance clusters.** It will happily slice a banana-shaped cluster in
   half. For non-convex shapes use DBSCAN/HDBSCAN or spectral clustering.
4. **Sensitive to outliers.** A single far-away point pulls a centroid. k-medoids (PAM) or
   trimmed-k-means are more robust.

When to reach for what: **k-means** for fast prototyping and round numeric Euclidean clusters;
**DBSCAN/HDBSCAN** when shape is irregular or `k` is unknown; **Gaussian mixtures** when soft
assignment / probabilistic membership matters; **hierarchical (Ward, average-linkage)** when you
want the dendrogram and don't have millions of points; **spectral clustering** for graphs and
manifolds.

## Similar / related topics

- **k-means++** — the spread-out initialisation; default in scikit-learn since 0.20. Always use it.
- **MiniBatchKMeans** — stochastic mini-batch variant; necessary above a million points.
- **DBSCAN / HDBSCAN** — density-based; finds arbitrary-shaped clusters and auto-discovers `k`.
- **Gaussian Mixture Models** — soft probabilistic clustering; gives you uncertainty over assignments.
- **Hierarchical / agglomerative clustering** — produces a dendrogram; useful when you want to *see* the cluster structure.
- **Spectral clustering** — graph-Laplacian-based; handles non-convex clusters cheaper than HDBSCAN on small data.

## Internal links
- [[programming/rust/ml/linfa|linfa]] — the canonical Rust ML crate; ships a k-means implementation.
- [[../tools/README|ml/tools]] — tooling that wraps clustering pipelines.
- [[no_deep_learning_needed]] — sibling: classical-ML-still-wins page (k-means is exhibit A).
- [[classification]] — sibling supervised cousin (clustering is unsupervised).
- [[pattern_learning]] — sibling parent topic.

## Keywords

`#kmeans` `#clustering` `#unsupervised` `#lloyd` `#kmeans-pp`

## References / raw notes

- [scikit-learn — k-means user guide](https://scikit-learn.org/stable/modules/clustering.html#k-means).
- [Rust-ML book — k-means chapter](https://github.com/rust-ml/book/blob/master/src/3_kmeans.md) — clean walkthrough of `linfa`'s implementation.
- [Arthur & Vassilvitskii 2007, *k-means++: The Advantages of Careful Seeding*](http://ilpubs.stanford.edu:8090/778/) — initialisation paper.
- [Lloyd 1982 (orig. 1957), *Least squares quantization in PCM*](https://ieeexplore.ieee.org/document/1056489) — the original algorithm.
- [Faiss k-means](https://github.com/facebookresearch/faiss/wiki/Faiss-building-blocks:-clustering,-PCA,-quantization) — GPU-accelerated for large-scale.
