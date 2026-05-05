---
title: hbase
main_link: 
keywords: [hbase, nosql, db, opts, usr, root]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# hbase

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[redis]] — Redis _(score 13)_
- [[firebase]] — firebase _(score 13)_
- [[db/relational/mysql|mysql]] — mysql _(score 10)_
- [[indexing]] — Indexing _(score 5)_
- [[sqlflow]] — SQLFlow _(score 5)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->

## Keywords

`#hbase` `#nosql` `#db` `#opts` `#usr` `#local` `#root`

## TODO

- No `main_link` could be auto-detected. Add the canonical URL (project homepage / repo / paper) to the frontmatter.
- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# hbase
To start hbase now and restart at login:
brew services start hbase
Or, if you don't want/need a background service you can just run:
HBASE_HOME="/usr/local/opt/hbase/libexec" HBASE_IDENT_STRING="root" HBASE_LOG_DIR="/usr/local/var/hbase" HBASE_LOG_PREFIX="hbase-root-master" HBASE_LOGFILE="hbase-root-master.log" HBASE_MASTER_OPTS=" -XX:PermSize=128m -XX:MaxPermSize=128m" HBASE_NICENESS="0" HBASE_OPTS="-XX:+UseConcMarkSweepGC" HBASE_PID_DIR="/usr/local/var/run/hbase" HBASE_REGIONSERVER_OPTS=" -XX:PermSize=128m -XX:MaxPermSize=128m" HBASE_ROOT_LOGGER="INFO,RFA" HBASE_SECURITY_LOGGER="INFO,RFAS" /usr/local/opt/hbase/bin/hbase --config /usr/local/opt/hbase/libexec/conf master start
