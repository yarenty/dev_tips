---
title: Memory storage
main_link: https://github.com/jonhoo/evmap
keywords: [memory-storage, rust, reads, refresh, storage, meta]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

> Auto-split from `doc/programming/rust/data/map_cache_storage.md` by `article_split.py`. Heading: **Memory storage**.

# Memory storage

**Main link:** <https://github.com/jonhoo/evmap>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[programming/rust/data/sqlparser|sqlparser]] — sqlparser _(score 21.8)_
- [[lance_data_format]] — Lance _(score 21.8)_
- [[adbc]] — ADBC ! _(score 21.8)_
- [[cache]] — Cache _(score 21.8)_
- [[articles]] — Articles _(score 15.2)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#memory-storage` `#data` `#rust` `#programming` `#reads` `#map` `#refresh` `#evmap`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Memory storage

## EVMap

A lock-free, eventually consistent, concurrent multi-value map.

This map implementation allows reads and writes to execute entirely in parallel, with no implicit synchronization overhead. Reads never take locks on their critical path, and neither do writes assuming there is a single writer (multi-writer is possible using a Mutex), which significantly improves performance under contention. It is backed by the concurrency primitive left-right.

The trade-off exposed by this module is one of eventual consistency: writes are not visible to readers except following explicit synchronization. Specifically, readers only see the operations that preceeded the last call to WriteHandle::flush by a writer. This lets writers decide how stale they are willing to let reads get. They can refresh the map after every write to emulate a regular concurrent HashMap, or they can refresh only occasionally to reduce the synchronization overhead at the cost of stale reads.

For read-heavy workloads, the scheme used by this module is particularly useful. Writers can afford to refresh after every write, which provides up-to-date reads, and readers remain fast as they do not need to ever take locks.

The map is multi-value, meaning that every key maps to a collection of values. This introduces some memory cost by adding a layer of indirection through a Vec for each value, but enables more advanced use. This choice was made as it would not be possible to emulate such functionality on top of the semantics of this map (think about it -- what would the operational log contain?).

To facilitate more advanced use-cases, each of the two maps also carry some customizeable meta-information. The writers may update this at will, and when a refresh happens, the current meta will also be made visible to readers. This could be useful, for example, to indicate what time the refresh happened.


https://crates.io/crates/evmap


https://github.com/jonhoo/evmap
