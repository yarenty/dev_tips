---
title: AQP papers (reading list)
main_link: https://link.springer.com/article/10.1007/s41019-018-0074-4
keywords: [aqp, approximate-query-processing, papers, sketches, sampling, streaming, survey]
status: reviewed
---

# AQP papers (reading list)

**Main link:** <https://link.springer.com/article/10.1007/s41019-018-0074-4>

## Summary

A small curated reading list for **Approximate Query Processing** (AQP) over
data streams: one foundational paper on continuous tracking over distributed
streams (Cormode & Garofalakis), a 2006 PhD thesis on one-pass algorithms
(Das), an encyclopedia reference entry (Liu), and a 2018 survey (Li & Li)
that pulls everything together.

If you only read one of these, read the **Li & Li 2018 survey** — it's the
shortest path from "what is AQP" to "what techniques exist" with up-to-date
context.

## Insight

AQP papers cluster into three loose categories:

1. **What summary structure?** — sketches (Count-Min, HyperLogLog, AMS,
   Theta), uniform/stratified sampling, wavelet decompositions, histograms.
   Each picks a different point on the (error, memory, query class) trade.
2. **One-pass vs distributed.** Single-machine streams (Das) ask "how do I
   summarise in one pass?" Distributed streams (Cormode & Garofalakis) add
   "and how do I keep the *answer* fresh without flooding the network?"
3. **System integration.** Most production AQP today (Druid, BlinkDB, Snowflake's
   sample tables, ClickHouse `quantilesTDigest`) bundles a small subset of
   these techniques behind SQL. The papers explain *why* the production
   choices look the way they do.

The big practical takeaway: **bounded-error answers are usually fine, and
they are dramatically cheaper than exact ones.** Most BI dashboards and
monitoring queries don't need the last decimal place; AQP gives you the
99%-confidence ±1% answer in a fraction of the cost.

## Similar / related topics

- [[approximate_continuous_querying_over_distributed_streams]] — deeper notes
  on the Cormode & Garofalakis paper specifically.
- [[druid]] — production OLAP engine that ships HLL / Theta / Quantiles
  sketches as first-class aggregates.
- Apache DataSketches — production-quality library implementing most of the
  sketch families mentioned in these papers.

## Reading list

### 1. Cormode & Garofalakis (2008) — Approximate Continuous Querying over Distributed Streams

- **Authors:** Graham Cormode, Minos Garofalakis
- **Venue:** ACM Transactions on Database Systems, 33(2), Article 9, June 2008
- **Link:** <https://dl.acm.org/doi/10.1145/1366102.1366106>
- **DOI:** [10.1145/1366102.1366106](https://doi.org/10.1145/1366102.1366106)
- **Takeaway:** continuously track aggregate queries (joins, multi-joins,
  wavelet representations) over physically distributed streams using
  randomized sketch summaries plus per-site prediction models, with provable
  error guarantees and dramatically reduced communication cost.
- **Deeper notes:** [[approximate_continuous_querying_over_distributed_streams]]

### 2. Das (2006) — Approximate Query Answering over Data Streams

- **Author:** Abhinandan Das (advisor: Johannes Gehrke), Cornell University
- **Venue:** PhD dissertation
- **Link:** <https://dl.acm.org/doi/book/10.5555/1124067>
- **Takeaway:** single-pass approximation algorithms for stream queries,
  including load shedding under bursty input rates, online summarization, and
  set-expression tracking in a distributed setting — all with provable error
  bounds.

### 3. Liu — Approximate Query Processing (encyclopedia entry)

- **Author:** Qing Liu
- **Venue:** Encyclopedia of Database Systems (Springer)
- **Link:** <https://link.springer.com/referenceworkentry/10.1007/978-0-387-39940-9_534>
- **DOI:** [10.1007/978-0-387-39940-9_534](https://doi.org/10.1007/978-0-387-39940-9_534)
- **Takeaway:** short, definitional reference entry — useful as a glossary /
  cite when introducing AQP in a paper or talk.

### 4. Li & Li (2018) — Approximate Query Processing: What is New and Where to Go?

- **Authors:** Kaiyu Li, Guoliang Li
- **Venue:** Data Science and Engineering, 3, 379–397 (Springer, 2018)
- **Link:** <https://link.springer.com/article/10.1007/s41019-018-0074-4>
- **PDF:** <https://link.springer.com/content/pdf/10.1007/s41019-018-0074-4.pdf>
- **Takeaway:** modern survey covering offline-sample, online-sample, and
  ML-based AQP approaches, with the most useful "where to go next" framing
  of the four. Best starting point if you're new to the area.

## Internal links

- [[approximate_continuous_querying_over_distributed_streams]] — full notes
  on the first paper above.
- [[db/streaming/streaming_query_approximation/README|streaming_query_approximation]] —
  parent folder.
- [[db/streaming/README|streaming]] — wider section.
- [[druid]] — production OLAP engine with built-in sketch aggregates.

## Keywords

`#aqp` `#approximate-query-processing` `#papers` `#streaming` `#sketches` `#sampling` `#survey`

## References / raw notes

Approximate continuous querying over distributed streams

https://dl.acm.org/doi/10.1145/1366102.1366106

Authors:
Graham Cormode

,
Minos Garofalakis

Authors Info & Claims
ACM Transactions on Database SystemsVolume 33Issue 2June 2008 Article No.: 9pp 1–39https://doi.org/10.1145/1366102.1366106

Abstract
While traditional database systems optimize for performance on one-shot query processing, emerging large-scale monitoring applications require continuous tracking of complex data-analysis queries over collections of physically distributed streams. Thus, effective solutions have to be simultaneously space/time efficient (at each remote monitor site), communication efficient (across the underlying communication network), and provide continuous, guaranteed-quality approximate query answers. In this paper, we propose novel algorithmic solutions for the problem of continuously tracking a broad class of complex aggregate queries in such a distributed-streams setting. Our tracking schemes maintain approximate query answers with provable error guarantees, while simultaneously optimizing the storage space and processing time at each remote site, and the communication cost across the network. In a nutshell, our algorithms rely on tracking general-purpose randomized sketch summaries of local streams at remote sites along with concise prediction models of local site behavior in order to produce highly communication- and space/time-efficient solutions. The end result is a powerful approximate query tracking framework that readily incorporates several complex analysis queries (including distributed join and multi-join aggregates, and approximate wavelet representations), thus giving the first known low-overhead tracking solution for such queries in the distributed-streams model. Experiments with real data validate our approach, revealing significant savings over naive solutions as well as our analytical worst-case guarantees.

References
Alon, N., Gibbons, P. B., Matias, Y., and Szegedy, M. 1999. Tracking join and self-join sizes in limited storage. In Proceedings of the 18th ACM SIGACT-SIGMOD-SIGART Symposium on Principles of Database Systems. Philadeplphia, PA. Google ScholarDigital Library
Alon, N., Matias, Y., and Szegedy, M. 1996. The space complexity of approximating the frequency moments. In Proceedings of the 28th Annual ACM Symposium on the Theory of Computing. Philadelphia, PA, 20--29. Google ScholarDigital Library
Babcock, B. and Olston, C. 2003. Distributed top-K monitoring. In Proceedings of the ACM SIGMOD International Conference on Management of Data. San Diego, CA. Google ScholarDigital Library

---

Approximate query answering over data streams

https://dl.acm.org/doi/book/10.5555/1124067

Author:Abhinandan Das,Adviser:Johannes Gehrke
Publisher:
Cornell UniversityPO Box 250, 124 Roberts Place Ithaca, NYUnited States
ISBN:978-0-542-35073-3
Order Number:AAI3192121

Abstract
The emergence of sensor networks and a new class of high-speed, data-intensive streaming applications such as IP network management, financial analysis, and telephone fraud detection, have begun to change the traditional view of databases as a random-access store of data amenable to multiple pass algorithms. The need to query, analyze and manage vast amounts of continuous stream data in an online fashion, potentially in real time, makes it imperative to design one pass algorithms which use limited resources to process queries over these data streams. In all these applications, for all but the simplest queries, computing exact answers online in one pass with limited storage is usually not possible. In addition, stream data is often lost, becomes stale, or is intentionally dropped for various reasons (e.g. in load shedding). All these factors necessitate approximate answers to queries. However, in many online applications, approximate answers usually suffice. Moreover, for applications with real time requirements, approximate but timely answers are often preferable to exact answers returned after a relatively long delay. Thus, approximation algorithms for stream query processing are not only necessary, but often desirable and constitute an integral part of any practical data stream processing system.

This dissertation focuses on approximate query answering over data streams, and proposes several novel approximation techniques for online processing of streaming data. The two principal reasons for using approximations in data stream processing are to enable quick real time responses and/or to deal with resource constraints. The approximation techniques in this dissertation make contributions in both of these areas. These include techniques for load shedding in case system resources are insufficient to handle bursty/variable rate streams, one pass algorithms for online summarization of streaming data, and approximation schemes for tracking set expressions in a distributed streaming environment.

All approximation techniques presented in this dissertation are complemented with provable (deterministic or probabilistic) guarantees on the quality of the approximation, many of them being the first known techniques to provide approximation guarantees for their respective problems. We also experimentally demonstrate the efficacy of all the proposed techniques on synthetic as well as real life data. While most of this research is presented in the context of stream systems, many of the proposed techniques also equally apply to large databases for which making multiple passes over data may not be practical.

---

https://link.springer.com/referenceworkentry/10.1007/978-0-387-39940-9_534

Approximate Query Processing
Authors
Authors and affiliations
Qing Liu
1.
Reference work entry
DOI: https://doi.org/10.1007/978-0-387-39940-9_534
1
Citations
756
Downloads
How to cite
Synonyms
Approximate query answering

Definition
Query processing in a database context is the process that deduces information that is available in the database. Due to the huge amount of data available, one of the main issues of query processing is how to process queries efficiently. In many cases, it is impossible or too expensive for users to get exact answers in the short query response time. Approximate query processing (AQP) is an alternative way that returns approximate answer using information which is similar to the one from which the query would be answered. It is designed primarily for aggregate queries such as count, sum and avg, etc. Given a SQL aggregate query Q, the accurate answer is y while the approximate answer is y′. The relative error of query Q can be quantified as:
𝐸𝑟𝑟𝑜𝑟(𝑄)=|𝑦−𝑦′𝑦|.

---
Approximate Query Processing: What is New and Where to Go?
A Survey on Approximate Query Processing

Kaiyu Li & Guoliang Li

https://link.springer.com/article/10.1007/s41019-018-0074-4

https://link.springer.com/content/pdf/10.1007/s41019-018-0074-4.pdf
