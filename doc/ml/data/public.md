---
title: Public datasets
main_link: https://github.com/awesomedata/awesome-public-datasets
keywords: [datasets, public-data, awesome-list, ml-data, kaggle, hugging-face, openml]
status: reviewed
review_date: 2026/05/03
---

# Public datasets

**Main link:** <https://github.com/awesomedata/awesome-public-datasets>

## Summary

A pointer to the canonical curated indexes of publicly-available datasets — `awesomedata/awesome-public-datasets` (the long-running domain-organised list: agriculture, biology, climate, finance, GIS, healthcare, NLP, time series, etc.) plus the Pudding's data archive (rebuildable data behind their visual-essay journalism). Use this as the entry point when you don't yet know what dataset you want; reach for Hugging Face / Kaggle / OpenML when you know which kind.

## Insight

The "awesome-X" GitHub list pattern works very well for datasets because the search problem is *discovery* — you usually don't know the name of the dataset you need; you know the *domain* (e.g. "I need labelled images of plant disease", "I need US grid-electricity time series"). Domain-organised lists win there. Once you're past discovery, the actual programmatic download experience is much better via Hugging Face Datasets (`load_dataset(...)`), Kaggle CLI (`kaggle datasets download`), or OpenML's API — those handle versioning, caching, splits, and license metadata in a way that GitHub-list links don't.

Trade-offs to be aware of:

- Awesome-lists rot. Many entries on `awesome-public-datasets` link to government portals that have moved or to Kaggle pages that are now gated. **Always sample before relying.**
- License ambiguity is the silent killer. "Public" ≠ "redistributable" ≠ "usable for ML training" ≠ "usable for commercial ML training". Check the source's terms before training on anything you intend to ship.
- For LLM-grade corpora, the modern reference points are different: Common Crawl + RefinedWeb / FineWeb / The Pile / RedPajama / SlimPajama / Dolma — all on Hugging Face, all multi-TB, all with their own licensing nuances.

## Similar / related topics

- **Hugging Face Datasets** — programmatic hub, ~200k datasets, the de-facto modern entry point for ML.
- **Kaggle Datasets** — competition-flavoured, often well-cleaned, CLI-driven download.
- **OpenML / UCI ML Repository** — classical tabular ML benchmark sets with proper metadata.
- **Papers With Code Datasets** — search by SOTA paper / benchmark.
- **Common Crawl + LLM corpora** (RefinedWeb / FineWeb / The Pile / RedPajama) — for LLM pre-training scale.
- **AWS Open Data Registry / Google Dataset Search** — cross-cutting indexes.

## Internal links

- [[apis]] — for "I want a JSON endpoint, not a download" data
- [[for_tests]] — when you only need tiny fixtures
- [[../llm/README|ml/llm]] — for LLM-pretraining-corpus-scale dataset discussion
- [[../rag/README|ml/rag]] — for retrieval-corpus selection (Wikipedia, Common Crawl subsets, etc.)

## Keywords

`#datasets` `#public-data` `#awesome-list` `#ml-data` `#kaggle` `#hugging-face` `#openml`

## References / raw notes

### Awesome Public Datasets (the canonical curated index)

<https://github.com/awesomedata/awesome-public-datasets>

Domain-organised — pick a category and browse:

Agriculture · Biology · Climate+Weather · ComplexNetworks · ComputerNetworks · CyberSecurity · DataChallenges · EarthScience · Economics · Education · Energy · Entertainment · Finance · GIS · Government · Healthcare · ImageProcessing · MachineLearning · Museums · NaturalLanguage · Neuroscience · Physics · ProstateCancer · Psychology+Cognition · PublicDomains · SearchEngines · SocialNetworks · SocialSciences · Software · Sports · TimeSeries · Transportation · eSports · Complementary Collections

### Personal mirror / fork

<https://github.com/yarenty/awesome-public-datasets>

### The Pudding

Data behind their data-journalism essays — well-curated, often quirky:

<https://github.com/the-pudding/data>

### Data workshops (R-flavoured)

Some R data outputs, may inspect later:

<https://github.com/kaybenleroll/data_workshops>

### Bigger / programmatic hubs

- Hugging Face Datasets — <https://huggingface.co/datasets>
- Kaggle Datasets — <https://www.kaggle.com/datasets>
- OpenML — <https://www.openml.org/>
- UCI ML Repository — <https://archive.ics.uci.edu/>
- Papers With Code Datasets — <https://paperswithcode.com/datasets>
- Google Dataset Search — <https://datasetsearch.research.google.com/>
- AWS Open Data Registry — <https://registry.opendata.aws/>

### Common LLM-pretraining corpora (discovery only — read each licence carefully)

- Common Crawl — <https://commoncrawl.org/>
- RefinedWeb (Falcon) — <https://huggingface.co/datasets/tiiuae/falcon-refinedweb>
- FineWeb (HF) — <https://huggingface.co/datasets/HuggingFaceFW/fineweb>
- The Pile (EleutherAI) — <https://pile.eleuther.ai/>
- RedPajama / SlimPajama (Together) — <https://huggingface.co/datasets/togethercomputer/RedPajama-Data-V2>
- Dolma (AI2) — <https://huggingface.co/datasets/allenai/dolma>
