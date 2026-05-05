---
title: println! speedup 500%
main_link: 
keywords: [tricks, learning, rust, programming, println, display, speedup, tczajka]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# println! speedup 500%

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- TODO: at least 2 [[wikilinks]] to related articles in this vault. -->

## Keywords

`#tricks` `#learning` `#rust` `#programming` `#println` `#display` `#speedup` `#tczajka`

## TODO

- No `main_link` could be auto-detected. Add the canonical URL (project homepage / repo / paper) to the frontmatter.
- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# println! speedup 500%
tczajka

Here is a way to make the code 5x faster -- replace this line:

println!("{} {} {} {} {}", x1, x2, x3, x4, x5);

with:

println!("{} {} {} {} {}", {x1}, {x2}, {x3}, {x4}, {x5});

The compiler is worried that the variables may get changed through immutable references passed to Display implementations, making the f calls not invariant across loop iterations. That is unfortunate, given that immutability is such a core language feature.
