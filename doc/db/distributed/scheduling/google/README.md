---
title: Google scheduling papers
keywords: [scheduling, google, borg, omega, cluster-management, papers]
status: reviewed
---

# Google scheduling papers

A folder of paper PDFs from **Google Research** and Google-adjacent venues
on cluster/job scheduling. Google's scheduling lineage runs roughly
**Borg → Omega → Kubernetes**, and later papers extend the same ideas into
multi-cluster, ML-workload, and serverless territory.

The PDFs are named by their ACM Digital Library citation IDs (`<conf>.<paperid>`),
not by title; they're left as-is so they round-trip with citation tooling.
The most well-known references in this lineage:

- **Borg** (Verma et al., EuroSys 2015) — the canonical large-cluster
  manager paper.
- **Omega** (Schwarzkopf et al., EuroSys 2013) — shared-state, optimistic
  scheduler design.
- **Borg, Omega, and Kubernetes** (Burns et al., ACM Queue / CACM) — the
  retrospective.
- A series of follow-ups on workload mixing, oversubscription, and ML-job
  scheduling that the rest of the PDFs cover.

## What's in this folder

PDF papers (filenames are ACM DL identifiers — open in a reader for the
real titles):

- `3295500.3356164.pdf`
- `3314872.3314896.pdf`
- `3323165.3323179.pdf`
- `3356467.pdf`
- `3381089.3381203.pdf`
- `3395363.3397371.pdf`
- `3409964.3461790.pdf`
- `3477132.3483542.pdf`
- `3485447.3512060.pdf`
- `3493287.3493288.pdf`
- `3552326.3587445.pdf`
- `3595879.pdf`

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

`#scheduling` `#google` `#borg` `#omega` `#cluster-management` `#papers`
