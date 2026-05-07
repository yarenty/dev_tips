---
title: Classification — ordinal targets
main_link: https://github.com/ayrna/dlordinal
keywords: [classification, ordinal, dlordinal, corn, coral, pytorch]
status: reviewed
review_date: 2026/05/03
---

# Classification — ordinal targets

**Main link:** <https://github.com/ayrna/dlordinal>

## Summary

The classification niche of **ordinal targets** — labels with a meaningful order but no fixed
distance between them (star ratings 1-5, severity scales, age brackets, education levels,
disease-stage). Treating ordinal labels as plain nominal classes throws away the order
information; treating them as continuous regression targets imposes false equal-spacing.
Ordinal classification methods (CORAL, CORN, ordinal-cross-entropy, soft labels) sit between
the two, exploiting the order while not pretending the gaps are equal. The flagship Python
library is `dlordinal` (Ayrna group, University of Córdoba) — a PyTorch-based unified
implementation of the major recent deep-ordinal methods, with ordinal-aware metrics
(MAE, QWK) baked in.

## Insight

The single most useful thing to remember when you spot an ordinal target: **report MAE / QWK,
not just accuracy**. Plain accuracy treats "predicted 1, true 5" as exactly as wrong as
"predicted 4, true 5" — that's almost never what the business actually wants. CORN
(Conditional Ordinal Regression for Neural Networks, Shi et al. 2022) is the current
recommended starting point for deep models because it has fewer rank-inconsistency edge cases
than its predecessor CORAL. For non-deep settings, a couple of practical defaults: (a) ordinal
logistic regression in `statsmodels` / `mord` is often competitive on small tabular data;
(b) GBDTs with the right loss (`xgboost`'s `multi:softprob` plus a custom QWK metric, or
LightGBM with `application=multiclass_ordinal` patches) frequently beat dedicated deep ordinal
nets when the dataset is < 100k rows. Reach for `dlordinal` when the input is unstructured
(images, text) and the label is genuinely ordinal — image-based age estimation, text-based
severity scoring, medical-imaging staging.

## Similar / related topics

- **CORAL / CORN** — the two canonical rank-consistent ordinal-classification heads for neural nets.
- **Ordinal logistic regression** (proportional-odds model) — the classical statistical baseline; available in `statsmodels`, `mord`, R's `MASS::polr`.
- **Quadratic Weighted Kappa (QWK)** — the standard ordinal-classification metric (penalises distant misclassifications more than near ones); used in many Kaggle ordinal competitions.
- **Regression with rounding** — naive ordinal baseline; surprisingly hard to beat on smooth, well-spaced labels.
- **Multi-class classification** — the wider family this is a refinement of.

## Internal links
- [[../frameworks/README|frameworks]] — PyTorch is what `dlordinal` builds on.
- [[no_deep_learning_needed]] — sibling: ordinal logistic regression / GBDTs often win on tabular data.
- [[zero_shot]] — counterpoint: when you have no labels at all.
- [[pattern_learning]] — sibling parent topic.

## Keywords

`#classification` `#ordinal` `#dlordinal` `#corn` `#coral` `#pytorch`

## References / raw notes

- Library: [`ayrna/dlordinal`](https://github.com/ayrna/dlordinal) — PyTorch deep-ordinal classification toolkit.
  Unifies many recent deep ordinal classification methodologies, includes loss functions, output
  layers, dropout techniques, soft labelling, and ordinal-aware evaluation metrics.
- CORN: [Shi, Cao & Raschka 2022, *Deep Neural Networks for Rank-Consistent Ordinal Regression Based on Conditional Probabilities*](https://arxiv.org/abs/2111.08851).
- CORAL: [Cao, Mirjalili & Raschka 2020, *Rank Consistent Ordinal Regression for Neural Networks*](https://arxiv.org/abs/1901.07884).
- Classical: [`mord`](https://pythonhosted.org/mord/) — ordinal regression in scikit-learn style.
- Survey: [Gutiérrez et al. 2016, *Ordinal Regression Methods: Survey and Experimental Study*](https://ieeexplore.ieee.org/document/7161338).
