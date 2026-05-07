---
title: When you don't need deep learning
main_link: https://arxiv.org/abs/2110.01889
keywords: [tabular, gbdt, xgboost, lightgbm, catboost, classical-ml, interpretability]
status: reviewed
review_date: 2026/05/03
---

# When you don't need deep learning

**Main link:** <https://arxiv.org/abs/2110.01889>

## Summary

A pointer page for the genuine, well-documented niche where **classical ML beats deep learning**:
small-to-medium tabular datasets, problems that demand interpretability, edge devices, real-time
low-latency inference, and any setting where the dataset is in the thousands-to-low-millions of rows
rather than billions. Gradient-Boosted Decision Trees (XGBoost, LightGBM, CatBoost) remain
state-of-the-art for tabular Kaggle competitions, and Borisov et al.'s 2021 survey *Deep Neural
Networks and Tabular Data: A Survey* provides the canonical literature review confirming this.
A 2024 counter-trend — "tabular foundation models" like TabPFN and TabM — is starting to nibble at
this consensus on small datasets, but the GBDT default is still the right starting point.

## Insight

The "DL on everything" reflex burns time and money on problems where a 200-line scikit-learn pipeline
would have hit higher accuracy in an hour. Three concrete heuristics worth applying before reaching
for a neural net:

1. **Tabular and < ~1M rows?** Start with LightGBM or XGBoost. If a well-tuned GBDT can't reach the
   target metric, a Transformer almost certainly won't either, but it will cost 100× more to train
   and serve.
2. **Need to explain a single prediction to a human (regulator, doctor, judge)?** Linear models,
   logistic regression, and shallow trees give you actual coefficients/splits. SHAP/LIME on a deep
   model is a post-hoc rationalisation, not a true explanation.
3. **Edge / battery / sub-millisecond latency?** Small trees, kNN with vector quantisation, or even
   linear models will run on a microcontroller. A 100M-parameter Transformer will not.

The places deep learning genuinely wins (and the GBDT-vs-DL line should not be treated as religion):
unstructured data (images, audio, text, video), problems where you have millions of examples and care
about the last 1% of accuracy, transfer learning from a pretrained foundation model, and any task
where representation learning *is* the point.

## Similar / related topics

- **XGBoost / LightGBM / CatBoost** — the three big GBDT libraries; CatBoost handles categorical features natively, LightGBM is fastest, XGBoost is the most-tuned default.
- **scikit-learn** — the canonical Python library for "classical" ML; covers SVM, RF, GBDT, k-NN, linear models, clustering, dim-reduction.
- **TabPFN / TabM** — the 2024 "tabular foundation models" trying to challenge GBDT on small data.
- **AutoML** (auto-sklearn, AutoGluon, FLAML) — frameworks that automate the GBDT-vs-MLP-vs-linear bake-off.
- **Symbolic regression** — when you want a *formula*, not a model.

## Internal links
- [[../tools/README|ml/tools]] — the tooling that wraps these classical libraries.
- [[../frameworks/README|frameworks]] — DL frameworks (the alternatives this article argues against in many cases).
- [[classification]] — sibling on the niche of ordinal classification.
- [[kmeans]] — sibling on classical clustering.
- [[ml_auto_indexing_dbs]] — sibling: ML-for-systems work that mostly uses small models, not deep nets.

## Keywords

`#tabular` `#gbdt` `#xgboost` `#lightgbm` `#catboost` `#classical-ml` `#interpretability`

## References / raw notes

- Survey: [Borisov et al. 2021, *Deep Neural Networks and Tabular Data: A Survey*](https://arxiv.org/abs/2110.01889).
- Follow-up: [Grinsztajn et al. 2022, *Why do tree-based models still outperform deep learning on tabular data?*](https://arxiv.org/abs/2207.08815) (NeurIPS'22).
- The big GBDT libraries:
  - [XGBoost](https://xgboost.readthedocs.io/)
  - [LightGBM](https://lightgbm.readthedocs.io/)
  - [CatBoost](https://catboost.ai/)
- The 2024 counter-trend: [TabPFN](https://github.com/PriorLabs/TabPFN), [TabM](https://github.com/yandex-research/tabm).
- Practical checklist: scikit-learn's [*Choosing the right estimator* flowchart](https://scikit-learn.org/stable/machine_learning_map.html).
