# Webthinker

https://github.com/RUC-NLPIR/WebThinker?utm_source=tldrai

WebThinker is a deep research framework fully powered by large reasoning models (LRMs). WebThinker enables LRMs to autonomously search, deeply explore web pages, and draft research reports, all within their thinking process.

Unlike existing open-source deep search agents that typically employ retrieval-augmented generation (RAG) with predefined workflows, WebThinker allows the reasoning model itself to perform actions during thinking, achieving end-to-end task execution in a single generation.



![](https://github.com/RUC-NLPIR/WebThinker/raw/main/figures/framework.png)


Key Features:

- We introduce a Deep Web Explorer that empowers LRMs to search, navigate pages by clicking interactive elements (like links or buttons), and extract relevant information. Based on initial search results, the LRM can initiate follow-up searches and traverse deeper links until it collects all relevant information.
- For scientific reporting, our Autonomous Think-Search-and-Draft strategy integrates real-time knowledge seeking with report creation. We equip LRMs with three specialized tools: (1) drafting content for specific chapters, (2) checking the current report, and (3) editing the report—ensuring reports remain comprehensive, coherent, and adaptive to new insights.
- We're developing RL-based training strategies to optimize end-to-end task performance by leveraging large-scale reasoning trajectories from complex tasks. Using accuracy of reasoning, tool usage, and final outputs, we construct preference pairs for online DPO training, enabling the model to progressively improve its research capabilities.
