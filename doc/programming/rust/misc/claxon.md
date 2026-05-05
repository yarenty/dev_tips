---
title: Claxon
main_link: https://github.com/ruuda/claxon
keywords: [claxon, rust, flac, decoding]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

> Auto-split from `doc/programming/rust/misc/audio.md` by `article_split.py`. Heading: **Claxon**.

# Claxon

**Main link:** <https://github.com/ruuda/claxon>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[audio]] — Coreaudio _(score 41.4)_
- [[lewton]] — lewton _(score 32.6)_
- [[hound]] — Hound _(score 32.6)_
- [[daktilo]] — Daktilo _(score 23.7)_
- [[rtic]] — RTIC _(score 23.7)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#claxon` `#misc` `#rust` `#programming` `#flac` `#example` `#decoding` `#decoder`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Claxon

https://github.com/ruuda/claxon


A FLAC decoding library in Rust.


Claxon is a FLAC decoder written in pure Rust. It has been fuzzed and verified against the reference decoder for correctness. Its performance is similar to the reference decoder.

Example
The following example computes the root mean square (RMS) of a FLAC file:

```rust
let mut reader = claxon::FlacReader::open("testsamples/pop.flac").unwrap();
let mut sqr_sum = 0.0;
let mut count = 0;
for sample in reader.samples() {
let s = sample.unwrap() as f64;
sqr_sum += s * s;
count += 1;
}
println!("RMS is {}", (sqr_sum / count as f64).sqrt());

```

More examples can be found in the examples directory. For a simple example of decoding a FLAC file to wav with Claxon and Hound, see decode_simple.rs. A more efficient way of decoding requires dealing with a few details of the FLAC format. See decode.rs for an example.
