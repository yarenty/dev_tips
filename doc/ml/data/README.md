---
title: ML data — sources, fixtures, and ML-side vector search
keywords: [data, datasets, ml-data, vector-search, fixtures, public-data]
status: reviewed
review_date: 2026/05/03
---

# ML data — sources, fixtures, and ML-side vector search

Where to get data **for** ML — and how to look it up once you've embedded it. Curated entry-points to public-dataset hubs, no-auth public APIs, tiny test fixtures, and the ML-application angle on Qdrant for embedding search.

## Articles

- [API access to data](apis.md) — curated no-auth JSON endpoints for demos / hobby ML / dashboards.
- [Sample data for tests](for_tests.md) — scikit-learn toys + Kylo multi-format columnar fixtures.
- [Public datasets](public.md) — `awesome-public-datasets` index + Hugging Face / Kaggle / OpenML / LLM-corpora pointers.
- [Qdrant for ML](qdrant_vector_search.md) — embeddings, RAG retrieval, semantic & image similarity (operator-side at [[../../db/vector/qdrant|db/vector/qdrant]]).

## Decision shortcuts

| You want… | Reach for |
|-----------|-----------|
| A JSON endpoint, no signup, for a demo or CI | [[apis]] (mixedanalytics list) |
| A 150-row dataset to unit-test a classifier | [[for_tests]] (`sklearn.datasets.load_iris`) |
| A small Parquet/ORC/Avro file to test a reader | [[for_tests]] (Kylo samples) |
| A real dataset for training a model | [[public]] → Hugging Face / Kaggle / OpenML |
| A multi-TB pretraining corpus for an LLM | [[public]] → Common Crawl / FineWeb / RedPajama / Dolma |
| To store embeddings for RAG / semantic search | [[qdrant_vector_search]] (ML angle) + [[../../db/vector/qdrant|db/vector/qdrant]] (ops) |
| To pick a vector DB at all | [[../../db/vector/README|db/vector]] decision matrix |

## See also

- [[../README|ml]] — the broader Machine Learning section
- [[../rag/README|ml/rag]] — retrieval pipelines that consume what's indexed here
- [[../llm/README|ml/llm]] — embeddings come out of LLMs / encoder models
- [[../../db/vector/README|db/vector]] — vector-DB landscape (the operator side)
- [[../../db/formats/README|db/formats]] — Parquet / ORC / Arrow / Avro
- [[../tools/README|ml/tools]] — surrounding ML tooling

## Keywords

`#data` `#datasets` `#ml-data` `#vector-search` `#fixtures` `#public-data`
