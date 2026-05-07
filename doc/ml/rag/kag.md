---
title: KAG — Knowledge Augmented Generation (OpenSPG / Ant Group)
main_link: https://github.com/OpenSPG/KAG
keywords: [kag, openspg, ant-group, knowledge-graph, logical-reasoning, multi-hop, rag]
status: reviewed
review_date: 2026/05/03
---

# KAG — Knowledge Augmented Generation (OpenSPG / Ant Group)

**Main link:** <https://github.com/OpenSPG/KAG>

## Summary

KAG (Knowledge Augmented Generation) is Ant Group's RAG framework, released in September 2024 on top of their **OpenSPG** (Semantic-enhanced Programmable Graph) engine. It positions itself as a deliberate alternative to both vanilla RAG (whose vector similarity it calls "ambiguous") and Microsoft-style GraphRAG (whose OpenIE-extracted graphs it calls "noisy"), built around four ideas: knowledge ⇄ chunk mutual indexing, conceptual-semantic *knowledge alignment*, schema-constrained KG construction, and **logical-form-guided** hybrid reasoning that turns a natural-language question into an executable retrieval program. The target use case is vertical-domain Q&A with multi-hop factual reasoning (medical, legal, financial).

## Insight

KAG is best understood as a *strong-schema* counterpoint to GraphRAG's *open-schema* extraction: instead of letting the LLM invent the entity types, you supply an ontology and the system builds and queries the graph against it. That makes it appealing in regulated verticals (Ant's home turf is fintech), where knowledge engineers already curate schemas and noisy entities are unacceptable, and less appealing for "throw a folder of PDFs at it" exploratory use.

Reach for KAG when you (a) have or can write a domain schema, (b) face genuinely multi-hop questions where logical decomposition matters, and (c) can tolerate a heavier stack — KAG depends on OpenSPG (a graph DB + reasoning engine) and is a noticeably bigger install than `microsoft/graphrag` or `LightRAG`. Skip it for English-first generic-knowledge Q&A where the lighter GraphRAG variants or plain hybrid search will likely match it at a fraction of the operational cost.

Honest caveats: KAG is China-originated (Alibaba/Ant), so docs and community signal are split between English and Chinese (the Yuque guide is the most thorough source); benchmark claims of "significantly better than SOTA" are on author-curated evals (HotpotQA, 2WikiMultiHopQA, MuSiQue) and worth reproducing before betting a project on them.

## Similar / related topics

- **Microsoft GraphRAG** — open-schema, community-summary alternative; see [[graph_rag]].
- **LightRAG** — lighter dual-level GraphRAG (HKU, 2024).
- **HippoRAG** — Personalised-PageRank graph traversal over an extracted KG.
- **Neo4j GraphRAG** — Neo4j-as-substrate alternative; see [[neo4j_rag]].
- **Schema-first KG-augmented LLM** — broader research thread (KG-COT, ToG, RoG).

## Internal links

- [[README]] — RAG hub
- [[graph_rag]] — open-schema sibling/competitor
- [[neo4j_rag]] — alternative graph substrate
- [[summary_of_best_practices]] — chunking / reranking foundations still apply
- [[../knowledge_graph/README|knowledge_graph]] — KG fundamentals
- [[../knowledge_graph/papers|kg/papers]] — KG-augmented-LLM reading list

## Keywords

`#kag` `#openspg` `#ant-group` `#knowledge-graph` `#logical-reasoning` `#multi-hop` `#rag`

## References / raw notes

- KAG repo: <https://github.com/OpenSPG/KAG>
- OpenSPG engine repo: <https://github.com/OpenSPG/openspg>
- KAG paper (Liang et al. 2024): <https://arxiv.org/abs/2409.13731>
- OpenSPG / KAG guide (Yuque, Chinese-first): <https://openspg.yuque.com/ndx6g9/wc9oyq/wni2suux7g2tt6s2>
- "KAG — Knowledge Augmented Generation: a practical guide" (AI Plain English): <https://ai.plainenglish.io/kag-knowledge-augmented-generation-a-pratical-guide-better-than-rag-eaaa1793369c>

### Core feature highlights (from upstream README)

- Knowledge and Chunk **mutual indexing** to integrate richer contextual text.
- **Knowledge alignment** via conceptual semantic reasoning — alleviates the noise OpenIE-extracted graphs introduce.
- **Schema-constrained** KG construction so domain expert ontologies stay first-class.
- **Logical-form-guided** hybrid reasoning + retrieval for multi-hop factual Q&A.
