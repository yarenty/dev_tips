---
title: Webthinker
main_link: https://github.com/RUC-NLPIR/WebThinker?utm_source=tldrai
keywords: [webthinker, tools, ml, lrms, search, deep, reasoning]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# Webthinker

**Main link:** <https://github.com/RUC-NLPIR/WebThinker?utm_source=tldrai>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[deeplake]] — DeepLake _(score 29.6)_
- [[tract]] — tract _(score 21.4)_
- [[ml/bigquery/links|links]] — Links _(score 14.4)_
- [[articles]] — Articles _(score 14.4)_
- [[agentic_ai_memory]] — Task _(score 14.4)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#webthinker` `#tools` `#ml` `#lrms` `#search` `#deep` `#reasoning`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Webthinker

https://github.com/RUC-NLPIR/WebThinker?utm_source=tldrai

WebThinker is a deep research framework fully powered by large reasoning models (LRMs). WebThinker enables LRMs to autonomously search, deeply explore web pages, and draft research reports, all within their thinking process.

Unlike existing open-source deep search agents that typically employ retrieval-augmented generation (RAG) with predefined workflows, WebThinker allows the reasoning model itself to perform actions during thinking, achieving end-to-end task execution in a single generation.



![](https://github.com/RUC-NLPIR/WebThinker/raw/main/figures/framework.png)


Key Features:

- We introduce a Deep Web Explorer that empowers LRMs to search, navigate pages by clicking interactive elements (like links or buttons), and extract relevant information. Based on initial search results, the LRM can initiate follow-up searches and traverse deeper links until it collects all relevant information.
- For scientific reporting, our Autonomous Think-Search-and-Draft strategy integrates real-time knowledge seeking with report creation. We equip LRMs with three specialized tools: (1) drafting content for specific chapters, (2) checking the current report, and (3) editing the report—ensuring reports remain comprehensive, coherent, and adaptive to new insights.
- We're developing RL-based training strategies to optimize end-to-end task performance by leveraging large-scale reasoning trajectories from complex tasks. Using accuracy of reasoning, tool usage, and final outputs, we construct preference pairs for online DPO training, enabling the model to progressively improve its research capabilities.
