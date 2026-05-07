---
title: Sample data for tests
main_link: https://scikit-learn.org/stable/datasets/toy_dataset.html
keywords: [test-data, fixtures, sample-data, parquet, orc, avro, csv, scikit-learn]
status: reviewed
review_date: 2026/05/03
---

# Sample data for tests

**Main link:** <https://scikit-learn.org/stable/datasets/toy_dataset.html>

## Summary

A pointer to the small canonical datasets you reach for when writing unit tests, CI smoke checks, or "does my pipeline read Parquet?" sanity probes — the scikit-learn toy datasets (Iris, Digits, Wine, Breast Cancer, Diabetes), the standard tabular fixtures (Titanic, MNIST), and small-but-real columnar samples in Avro / CSV / ORC / Parquet from the Teradata Kylo project. Useful precisely because they're tiny, well-known, free of license drama, and small enough to commit into a test-fixtures directory if necessary.

## Insight

The cardinal rule for test data: **keep it small enough that the test runs in milliseconds and small enough to read without a network call**. scikit-learn's `load_iris()` / `load_digits()` ship inside the wheel, so they are perfect for `pytest`-style fixtures with no I/O. For format-coverage tests (does my reader correctly parse ORC `SNAPPY`-compressed columns?), the Kylo `samples/sample-data/` tree is the canonical reference because it ships the *same* data in CSV / TSV / JSON / Avro / ORC / Parquet — letting you write one assertion per format. Don't reach for these for benchmarking (they're too small to be representative) and don't ship Titanic/MNIST snapshots inside your repo when `sklearn.datasets.fetch_openml('mnist_784')` is one line away.

Modern alternative: Hugging Face `datasets` ships small dev splits (`split="train[:100]"`) of huge corpora; pair with `pytest`'s `tmp_path_factory` for cache control. For columnar formats specifically, `pyarrow.feather` / `pyarrow.parquet` ship round-trip helpers that obviate needing pre-built test files at all.

## Similar / related topics

- **scikit-learn toy datasets** — Iris/Digits/Wine/etc., shipped inside the wheel, ideal for unit tests.
- **Hugging Face Datasets dev splits** — `split="train[:100]"` for tiny slice of a real corpus.
- **OpenML / UCI ML Repository** — slightly bigger "classical" tabular sets when the toys are too trivial.
- **Faker / Mimesis / `randomuser.me`** — synthetic data when you need something *shaped* like real data without privacy concerns.
- **`pyarrow.parquet.write_table` / `polars.write_parquet`** — generate format-coverage fixtures from in-memory data instead of hunting for sample files.

## Internal links

- [[apis]] — public no-auth APIs (a different kind of "free data for demos")
- [[public]] — bigger curated public-dataset list when toys won't cut it
- [[../../db/formats/README|db/formats]] — for the file-format side (Parquet, ORC, Arrow, Avro)

## Keywords

`#test-data` `#fixtures` `#sample-data` `#parquet` `#orc` `#avro` `#csv` `#scikit-learn`

## References / raw notes

### Multi-format columnar samples (Kylo)

Avro, CSV, ORC, Parquet — the same data shipped in each, useful for format-reader coverage tests:

<https://github.com/Teradata/kylo/blob/master/samples/sample-data/orc/README.txt>

### scikit-learn toy datasets (built-in, no download)

```python
from sklearn.datasets import load_iris, load_digits, load_wine, load_breast_cancer, load_diabetes
X, y = load_iris(return_X_y=True)   # 150 × 4, 3 classes
```

Reference: <https://scikit-learn.org/stable/datasets/toy_dataset.html>

### Slightly bigger but still small

- Titanic — `seaborn.load_dataset('titanic')` (built into seaborn).
- MNIST — `from sklearn.datasets import fetch_openml; fetch_openml('mnist_784')`.
- 20-newsgroups — `from sklearn.datasets import fetch_20newsgroups`.
