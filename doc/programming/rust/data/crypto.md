---
title: RustCrypto: POLYVAL
main_link: https://crates.io/crates/polyval
keywords: [crypto, rust, polyval, aes]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

# RustCrypto: POLYVAL

**Main link:** <https://crates.io/crates/polyval>

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
- [[barrel]] — barrel _(score 21.8)_
- [[lance_data_format]] — Lance _(score 21.8)_
- [[adbc]] — ADBC ! _(score 21.8)_
- [[articles]] — Articles _(score 15.2)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#crypto` `#data` `#rust` `#programming` `#polyval` `#aes` `#gcm` `#rustcrypto`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# RustCrypto: POLYVAL


https://crates.io/crates/polyval


POLYVAL (RFC 8452) is a universal hash function which operates over GF(2^128) and can be used for constructing a Message Authentication Code (MAC).

Its primary intended use is for implementing AES-GCM-SIV, however it is closely related to GHASH and therefore can also be used to implement AES-GCM at no cost to performance on little endian architectures.
