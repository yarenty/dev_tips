---
title: KAN — Kolmogorov-Arnold Networks
main_link: https://arxiv.org/abs/2404.19756
keywords: [kan, kolmogorov-arnold, mlp-alternative, splines, interpretability, deep-learning]
status: reviewed
---

# KAN — Kolmogorov-Arnold Networks

**Main link:** <https://arxiv.org/abs/2404.19756>

## Summary

KAN (Kolmogorov-Arnold Networks), introduced by Liu et al. (MIT/CalTech, April 2024), is an alternative
neural-network architecture that swaps the standard MLP layout — fixed activations on nodes, learned
linear weights on edges — for **learnable activations on edges** (parameterised as B-splines) and simple
sums on nodes. The construction is motivated by the Kolmogorov-Arnold representation theorem
(any multivariate continuous function decomposes into sums of single-variable functions). The reference
implementation is [`KindXiaoming/pykan`](https://github.com/KindXiaoming/pykan); a successor paper
*KAN 2.0: Kolmogorov-Arnold Networks Meet Science* extended the framework with symbolic regression
hooks aimed at scientific-discovery workflows.

## Insight

KAN briefly broke ML Twitter in spring 2024 with two real selling points: **interpretability**
(spline-shaped edge functions can often be read off as named primitives like `sin`, `exp`, `x²`) and
**sample-efficiency** on small smooth-function-fitting tasks. The hype tail was less kind:
training is much slower than an MLP of comparable size (B-spline updates, grid extension, no
hardware-fused kernels), parameter count grows fast with grid size, and on real-world benchmarks at
scale (vision, NLP, tabular) KAN has **not** convincingly beaten well-tuned MLPs / GBDTs / Transformers.
The best fit today is the original niche the authors emphasised: PDE solving, function discovery,
small symbolic-regression problems, and as a scientific tool where you actually want to *read* the
fitted function rather than treat it as a black box. Follow-up work (FastKAN with Gaussian RBFs,
Wav-KAN with wavelets, EfficientKAN reparametrisations) has chipped away at the speed problem
without changing the basic conclusion: KAN is a useful new option in the toolbox, not the
post-MLP era it was initially marketed as.

## Similar / related topics

- **MLP / fully-connected layers** — the baseline KAN is pitched against; usually still the right default.
- **SIREN (sinusoidal representation networks)** — fixed-frequency sinusoid activations for implicit-neural-representation tasks.
- **Neural ODEs / continuous-depth models** — another "rethink the layer" line of work.
- **Symbolic regression** (PySR, DEAP, AI Feynman) — what KAN-as-interpretable-fitter is implicitly competing with.
- **Physics-informed neural networks (PINNs)** — adjacent application area where KAN has shown promise on PDEs.

## Internal links
<!-- reviewed -->
- [[../frameworks/README|frameworks]] — DL framework landing (PyTorch, JAX — what `pykan` builds on).
- [[no_deep_learning_needed]] — sibling on the "do you actually need a neural net?" question.
- [[pattern_learning]] — sibling meta-topic on learning structure from data.
- [[../time_series/kan|time_series/kan]] — separate sibling article on KAN for time-series forecasting.

## Keywords

`#kan` `#kolmogorov-arnold` `#mlp-alternative` `#splines` `#interpretability` `#deep-learning`

## References / raw notes

- Original paper: [Liu et al. 2024, *KAN: Kolmogorov-Arnold Networks*](https://arxiv.org/abs/2404.19756)
- Reference implementation: [`KindXiaoming/pykan`](https://github.com/KindXiaoming/pykan)
- Follow-up: [*KAN 2.0: Kolmogorov-Arnold Networks Meet Science*](https://arxiv.org/abs/2408.10205)
- Faster reparametrisations:
  - [FastKAN — Gaussian RBF substitution](https://github.com/ZiyaoLi/fast-kan)
  - [EfficientKAN](https://github.com/Blealtan/efficient-kan)
  - [Wav-KAN — wavelet basis](https://arxiv.org/abs/2405.12832)
- Awesome list: [`mintisan/awesome-kan`](https://github.com/mintisan/awesome-kan)
