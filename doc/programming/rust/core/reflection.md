---
title: Reflection in Rust — the `reflect` crate & friends
main_link: https://crates.io/crates/reflect
keywords: [reflection, reflect, dtolnay, syn, quote, derive-macros, any, downcast, oso, rust]
status: reviewed
---

# Reflection in Rust — the `reflect` crate & friends

**Main link:** <https://crates.io/crates/reflect>

## Summary

Rust deliberately doesn't ship runtime reflection in the Java/C#/Go sense — there is no `obj.GetType().GetFields()` — but the *demand* for reflection-shaped APIs comes up constantly, especially when writing custom derive macros or implementing dynamic frameworks (ORMs, RPC, policy engines). Two distinct pieces of the puzzle are covered here:

1. **[`reflect`](https://crates.io/crates/reflect)** by [@dtolnay](https://github.com/dtolnay) — a *compile-time* reflection API that looks like runtime reflection but compiles down to plain `syn`/`quote` macro output. Aimed at making custom-derive macros dramatically easier to write.
2. **`std::any::Any`** + downcasting — the closest thing Rust has to genuine runtime reflection in the standard library. Used for Python-getattr-style dynamic attribute lookup (the [Oso blog series](https://www.osohq.com/post/rust-reflection-pt-1) is the canonical walkthrough).

## Insight

The two approaches solve different problems and you almost never combine them:

| Need                                              | Reach for                                |
|---------------------------------------------------|------------------------------------------|
| Write a custom `#[derive(MyTrait)]` for any struct | `reflect` (compile-time) or hand-rolled `syn` + `quote`. |
| Look up "what fields does this value have?" at runtime | `Any` + downcast, or a generated registry; see the Oso article series for one design. |
| Serialise/deserialise generically                | Don't roll your own — use [`serde`](https://serde.rs/), which solves this for ~everyone. |
| Plugin / scripting integration                   | Use a richer system: `bevy_reflect` (game-engine grade), [`mlua`](https://crates.io/crates/mlua), [`rhai`](https://rhai.rs/), or a wasm host. |
| Policy / authorisation rules over arbitrary types | [Oso](https://www.osohq.com/) (the company behind the article series) sells exactly this. |

**`reflect`'s pitch**: `syn` and `quote` are very general (and that's why they're the foundation of nearly every macro), but they make the macro author responsible for every angle bracket, lifetime, type parameter, trait bound, and `PhantomData`. dtolnay's `reflect` exposes what *looks* like a Java-style reflection API (`reflect::Value`, fields, function calls) — the macro author writes logic against this API, and the library tracks control flow and emits the `syn`/`quote` code that a seasoned macro author would have written by hand. The end user calls the macro identically to a hand-rolled one; compile time and runtime are the same. **Status check**: the crate is more design exploration than production tool — last released 2019 — but the *idea* keeps influencing newer crates (`bevy_reflect`, `frunk`, etc.).

**`Any`-based runtime reflection**: Rust's `std::any::Any` trait gives you `type_id()` and `downcast_ref::<T>()`, which is enough to build a small registry of known types and dispatch dynamically. The Oso "Rust reflection step by step" series walks through building this up to the point of replicating Python's `getattr` magic method. Gotchas: `Any` requires `'static`, downcasting is `O(1)` but ungainly, and you typically end up registering types in a `HashMap<TypeId, ...>` — at which point you're really building a typed plugin registry, not reflection.

## Similar / related topics

- [bevy_reflect](https://crates.io/crates/bevy_reflect) — Bevy's production-grade runtime reflection (used for editor integration, scripting, scene serialisation).
- [serde](https://serde.rs/) — solves the most common "I want reflection" need (serialisation) without actual reflection.
- [syn](https://crates.io/crates/syn) / [quote](https://crates.io/crates/quote) — what you actually use to write derive macros today.
- [frunk](https://crates.io/crates/frunk) — generic programming with HLists; another "reflection-shaped" approach.
- Oso blog: [*Rust Reflection, part 1*](https://www.osohq.com/post/rust-reflection-pt-1) — `Any`-based runtime reflection walkthrough.
- [[derivative]] — derive helpers for the standard traits (different problem, same neighbourhood).

## Internal links
- [[README]]
- [[derivative]]

## Keywords

`#rust` `#reflection` `#reflect` `#dtolnay` `#derive-macros` `#any` `#downcast`

## References / raw notes

### `reflect` (dtolnay) — compile-time reflection for derive macros

- Crate: <https://crates.io/crates/reflect>
- Repo: <https://github.com/dtolnay/reflect>

> I thought Rust doesn't have reflection...?
> This crate explores what it could look like to tackle the 80% use case of custom derive macros through a programming model that resembles compile-time reflection.

**Motivation.** `syn` and `quote` approach procedural macros in a super-general way and are a good fit for maybe 95% of use cases. However, that generality comes with the cost of operating at a relatively low level of abstraction — the macro author is responsible for the placement of every single angle bracket, lifetime, type parameter, trait bound, and `PhantomData`. There is a large amount of domain knowledge involved and very few people can reliably produce robust macros with this approach.

The design explored in `reflect` focuses on what it would take to make the edge cases disappear: if your macro works for the basic case, it should also work in every tricky case.

**Programming model.** The library exposes what looks like a boring straightforward runtime-reflection API — recognisable to anyone who has used reflection in Java or Go. The macro author expresses the logic of their macro in terms of this API, using types like `reflect::Value` to retrieve function arguments, access fields of data structures, invoke functions, and so on. There is no such thing as a generic type or `PhantomData` in this model — everything is just a `reflect::Value` with a type that is conceptually its monomorphised type at runtime.

Meanwhile the library tracks control flow and function invocations to build up a fully general procedural implementation of the macro. The resulting code has all the angle brackets, lifetimes, bounds, and `PhantomData` in the right places without the macro author thinking about any of it.

The reflection API is just a means for *defining* a procedural macro. The library boils it all away and emits clean Rust source code free of any actual runtime reflection. From the perspective of someone calling the macro, everything is the same as if it were hand-written with `syn` and `quote` — same compile speed, same runtime speed. The advantage is to the macro author.

### Runtime reflection with `Any` (Oso article series)

- Article: [*Rust Reflection, part 1*](https://www.osohq.com/post/rust-reflection-pt-1) (and follow-ups) by Oso.

Builds runtime reflection on top of `std::any::Any`:

> The foundation of our runtime reflection system through classes and instances, and we've shown some simple dynamic type checking using the built in `Any` trait.

Subsequent posts attempt to replicate Python's `getattr` magic — looking up attributes on Rust structs dynamically at runtime. The pattern: register types in a `HashMap<TypeId, ClassMetadata>` keyed by `TypeId::of::<T>()`, store per-field accessor closures, dispatch through `Any::downcast_ref`. The article series ultimately motivates the design of Oso's policy engine.
