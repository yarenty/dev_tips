---
title: Cluster & query scheduling (papers)
keywords: [scheduling, cluster-management, query-scheduling, borg, omega, apollo]
status: reviewed
---

# Cluster & query scheduling (papers)

Curated paper folders on **cluster scheduling** and **query / job scheduling**
research, broken down by the institution producing the bulk of the work.
Most assets are paper PDFs from ACM/USENIX venues, plus a handful of
overview decks and screenshots from earlier survey work.

The big themes the papers cover:

- **Cluster managers** — Borg, Omega, Mesos, Kubernetes, YARN.
- **Two-level vs monolithic vs shared-state schedulers.**
- **Workload-specific scheduling** — ML training (Hydra, Pollux), big-data
  jobs (Apollo, Sparrow), latency-critical microservices.
- **Multi-resource / multi-tenant fairness** — DRF and successors.

## What's in this folder

### Subfolders

- [[db/distributed/scheduling/google/README|google]] — Google Research
  papers (Borg / Omega / multi-cluster lineage).
- [[db/distributed/scheduling/microsoft/README|microsoft]] — Microsoft
  Research papers (Apollo / Hydra / generic cluster-scheduling).
- [[db/distributed/scheduling/vldb/README|vldb]] — scheduling-related VLDB
  papers across institutions.

### Top-level assets

- `Insight_Scheduler.pptx` — internal summary deck synthesizing the papers
  below.
- `ericsson_patent.pptx` — companion deck for the Ericsson scheduler patent.
- `US20210173712A1.pdf` — Ericsson scheduler patent (US patent application).
- `s10462-018-9667-6.pdf` — Springer survey paper on scheduling techniques.
- `Screenshot 2023-05-26 at 11.57.26.png`, `Screenshot 2023-09-05 at 17.36.33.png` —
  whiteboard / diagram captures from working sessions.

## External entry points

- [Borg paper (Verma et al., EuroSys'15)](https://research.google/pubs/large-scale-cluster-management-at-google-with-borg/)
- [Omega paper (Schwarzkopf et al., EuroSys'13)](https://research.google/pubs/omega-flexible-scalable-schedulers-for-large-compute-clusters/)
- [Apache Mesos](https://mesos.apache.org/)
- [Kubernetes scheduler](https://kubernetes.io/docs/concepts/scheduling-eviction/)

## Cross-section see-also

- [[db/distributed/long_running_query/README|long_running_query]] — the
  workload-side requirements that drive scheduler design.
- [[db/distributed/ballista_distributed/README|ballista_distributed]] —
  Ballista has its own scheduler component for query stages.
- [[db/distributed/README|distributed]] — parent section.

## Keywords

`#scheduling` `#cluster-management` `#query-scheduling` `#borg` `#omega` `#apollo`
