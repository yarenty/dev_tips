---
title: Summary Of Best Practices
main_link: 
keywords: [summary-of-best-practices, rag, ml, query, chunking, retrieval, documents]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# Summary Of Best Practices

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- TODO: at least 2 [[wikilinks]] to related articles in this vault. -->

## Keywords

`#summary-of-best-practices` `#rag` `#ml` `#query` `#chunking` `#retrieval` `#documents`

## TODO

- No `main_link` could be auto-detected. Add the canonical URL (project homepage / repo / paper) to the frontmatter.
- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->


First, some of the basic techniques of RAG workflow  👇

📌 Query Classification :  Classify queries to determine the necessity of retrieval. Queries requiring retrieval proceed through the RAG modules; others are handled directly by LLMs.

📌 Chunking : methods: Token-level Chunking , Semantic-level Chunking , Sentence-level Chunking

Chunk Size : Larger chunks provide more context, enhancing comprehension but increasing process time. Smaller chunks improve retrieval recall and reduce time but may lack sufficient context. Typical sizes are 2048, 1024, 512, 256 and 128.

📌 Chunking Techniques : like small-to-big and sliding windows improve retrieval quality by organizing chunk block relationships.

Embedding Model : is crucial for effective semantic matching of queries and chunk blocks.

Metadata Addition : Enhancing chunk blocks with metadata like titles, keywords, and hypothetical questions can improve retrieval during hybrid search.

Vector Databases : Vector DB is usually selected based on four key criteria: multiple index types, billion-scale vector support, hybrid search, and cloud-native capabilities

Retrieval Methods : Given a user query, the retrieval module selects the top-k relevant documents from a pre-built corpus based on the similarity between the query and the documents . Following 3 query retrieval method is used
• Query Rewriting: refines queries to better match relevant documents. Inspired by the
• Query Decomposition: retrieving documents based on sub-questions derived from the original query.
• Pseudo-documents Generation: generates a hypothetical document based on the user query and uses the embedding of hypothetical answers to retrieve similar documents e.g. HyDE

Reranking Methods : enhance the relevance of the retrieved documents, ensuring that the most pertinent information appears at the top of the list.

Two approaches are widely used
• DLM Reranking: leverages deep language models (DLMs) for reranking
• TILDE Reranking: calculates the likelihood of each query term independently by predicting token probabilities across the model’s vocabulary.
