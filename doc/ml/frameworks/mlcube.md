---
title: MLCube
main_link: https://github.com/mlcommons/mlcube_examples
keywords: [mlcube, frameworks, ml, models, machine, learning, software]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# MLCube

**Main link:** <https://github.com/mlcommons/mlcube_examples>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[mindsdb]] — MindsDB _(score 28)_
- [[dspy]] — DSPy _(score 23)_
- [[mlgo_llvm]] — MLGO _(score 15)_
- [[ml/bigquery/links|links]] — Links _(score 15)_
- [[articles]] — Articles _(score 15)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->

## Keywords

`#mlcube` `#frameworks` `#ml` `#models` `#machine` `#learning` `#software`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# MLCube

MLCube brings the concept of interchangeable parts to the world of machine learning models.  It is the shipping container that enables researchers and developers to easily share the software that powers machine learning.

MLCube is a set of common conventions for creating ML software that can just "plug-and-play" on many different systems. MLCube makes it easier for researchers to share innovative ML models, for a developer to experiment with many different models, and for software companies to create infrastructure for models. It creates opportunities by putting ML in the hands of more people.

MLCube isn’t a new framework or service; MLCube is a consistent interface to machine learning models in containers like Docker.  Models published with the MLCube interface can be run on local machines, on a variety of major clouds, or in Kubernetes clusters - all using the same code. MLCommons provides open source “runners” for each of these environments that make training a model in an MLCube a single command.

*Note: This project is still in the very early stages and under active development, some parts may have unexpected/inconsistent behaviours.*

## Installing MLCube

Install from PyPI:
```sh
pip install mlcube
```

To uninstall:

```sh
pip uninstall mlcube
```


## Usage Examples

Check out the [examples](https://github.com/mlcommons/mlcube_examples) for detailed examples and [MLCube wiki](https://mlcommons.github.io/mlcube).
