---
title: Google scheduling papers
keywords: [scheduling, google, borg, omega, cluster-management]
status: reviewed
review_date: 2026/05/03
---

# Google scheduling papers

Notes on papers from **Google Research** and Google-adjacent venues on
cluster/job scheduling. Google's scheduling lineage runs roughly
**Borg → Omega → Kubernetes**, and later papers extend the same ideas into
multi-cluster, ML-workload, and serverless territory.

The most well-known references in this lineage:

- **Borg** (Verma et al., EuroSys 2015) — the canonical large-cluster
  manager paper.
- **Omega** (Schwarzkopf et al., EuroSys 2013) — shared-state, optimistic
  scheduler design.
- **Borg, Omega, and Kubernetes** (Burns et al., ACM Queue / CACM) — the
  retrospective.
- A series of follow-ups on workload mixing, oversubscription, and ML-job
  scheduling extends the lineage further.

## External entry points

- [Borg — large-scale cluster management at Google](https://research.google/pubs/large-scale-cluster-management-at-google-with-borg/)
- [Omega — flexible, scalable schedulers](https://research.google/pubs/omega-flexible-scalable-schedulers-for-large-compute-clusters/)
- [Borg, Omega, and Kubernetes (ACM Queue)](https://queue.acm.org/detail.cfm?id=2898444)
- [Google Research publications](https://research.google/pubs/)

## Cross-section see-also

- [[db/distributed/scheduling/microsoft/README|microsoft]] — the parallel
  MSR scheduling line of work (Apollo / Hydra).
- [[db/distributed/scheduling/vldb/README|vldb]] — VLDB-venue scheduling
  papers.
- [[db/distributed/scheduling/README|scheduling]] — parent.

## Keywords

`#scheduling` `#google` `#borg` `#omega` `#cluster-management`
