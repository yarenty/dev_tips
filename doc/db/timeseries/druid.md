---
title: Druid
main_link: https://druid.apache.org/docs/latest/tutorials/index.html
keywords: [druid, time-series, db, start, apache, time, install]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# Druid

**Main link:** <https://druid.apache.org/docs/latest/tutorials/index.html>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- TODO: at least 2 [[wikilinks]] to related articles in this vault. -->

## Keywords

`#druid` `#timeseries` `#db` `#start` `#apache` `#time` `#install`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Druid

Apache Druid is a real-time database to power modern analytics applications.

## Install

Download:
https://druid.apache.org/docs/latest/tutorials/index.html


## Start 

```bash
./bin/start-micro-quickstart
```


http://localhost:8888/unified-console.html



### cleaning

Remember that after stopping Druid services, you can start clean next time by deleting the var directory from the Druid root directory and running the bin/start-micro-quickstart script again. You will likely want to do this before taking other data ingestion tutorials, since in them you will create the same wikipedia datasource.
