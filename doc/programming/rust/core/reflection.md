---
title: reflect
main_link: https://crates.io/crates/reflect
keywords: [reflection, core, rust, programming, macros, api, author, type]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# reflect

**Main link:** <https://crates.io/crates/reflect>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- TODO: at least 2 [[wikilinks]] to related articles in this vault. -->

## Keywords

`#reflection` `#core` `#rust` `#programming` `#macro` `#api` `#author` `#type`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes
<!-- auto-split by article_split.py -->
> Auto-split: 1 additional top-level heading(s) extracted into sibling files:
> - [Reflection step by step - Article](reflection_step_by_step_article.md)


<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# reflect

https://crates.io/crates/reflect

I thought Rust doesn't have reflection...?
This crate explores what it could look like to tackle the 80% use case of custom derive macros through a programming model that resembles compile-time reflection.

## Motivation
My existing syn and quote libraries approach the problem space of procedural macros in a super general way and are a good fit for maybe 95% of use cases. However, the generality comes with the cost of operating at a relatively low level of abstraction. The macro author is responsible for the placement of every single angle bracket, lifetime, type parameter, trait bound, and phantom data. There is a large amount of domain knowledge involved and very few people can reliably produce robust macros with this approach.

The design explored here focuses on what it would take to make all the edge cases disappear -- such that if your macro works for the most basic case, then it also works in every tricky case under the sun.

## Programming model
The idea is that we expose what looks like a boring straightforward runtime reflection API such as you might recognize if you have used reflection in Java or reflection in Go.

The macro author expresses the logic of their macro in terms of this API, using types like reflect::Value to retrieve function arguments and access fields of data structures and invoke functions and so forth. Importantly, there is no such thing as a generic type or phantom data in this model. Everything is just a reflect::Value with a type that is conceptually its monomorphized type at runtime.

Meanwhile the library is tracking the control flow and function invocations to build up a fully general and robust procedural implementation of the author's macro. The resulting code will have all the angle brackets and lifetimes and bounds and phantom types in the right places without the macro author thinking about any of that.

The reflection API is just a means for defining a procedural macro. The library boils it all away and emits clean Rust source code free of any actual runtime reflection. Note that this is not a statement about compiler optimizations -- we are not relying on the Rust compiler to do heroic optimizations on shitty generated code. Literally the source code authored through the reflection API will be what a seasoned macro author would have produced simply using syn and quote.

From the perspective of the person that ends up calling the macro, everything about how it is called is the same as if the macro were written the old fashioned way without reflection, and their code compiles exactly as fast and performs exactly as fast. The advantage is to the macro author for whom developing and maintaining a robust macro is greatly simplified.
