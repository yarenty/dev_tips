---
title: Generational indices & the slotmap crate
main_link: https://crates.io/crates/slotmap
keywords: [generational-index, slotmap, ecs, arena, ownership, rust, kyren]
status: reviewed
---

# Generational indices & the slotmap crate

**Main link:** <https://crates.io/crates/slotmap>

## Summary

A **generational index** is a (slot, generation) pair used as a stable, copyable handle into an arena-backed collection — the trick that lets game engines and ECSes have many entities holding references to each other without fighting Rust's borrow checker. When a slot is freed and reused, the generation counter is bumped, so an old handle can detect it now points at a different value (`is_live` returns `false`) instead of silently aliasing. The canonical Rust crate is [`slotmap`](https://crates.io/crates/slotmap) by [@orlp](https://github.com/orlp); other widely used implementations include [`thunderdome`](https://crates.io/crates/thunderdome) and the arena types inside `bevy_ecs` and `specs`.

## Insight

Reach for a generational arena whenever you'd otherwise be tempted by `Rc<RefCell<T>>` graphs, doubly-linked structures, or `Arc<Mutex<HashMap<Id, T>>>` lookup tables. The pattern shows up in:

- **Game engines / ECS** — entities, components, scene-graph nodes ([Bevy](https://bevyengine.org/), [Amethyst], `specs`, `hecs`).
- **Compilers** — AST nodes, type-check states, IR values referencing each other (rustc's `Idx<T>`, `salsa`).
- **GUI** — widget trees with parent ↔ child pointers (`druid`, `iced` internals).
- **Graph algorithms** — `petgraph::stable_graph::StableGraph` uses a similar idea.

Mental model: "pointers" become small `Copy` structs (e.g. `(u32 index, u32 generation)`); the arena owns the values; you trade pointer chasing for an extra cache-friendly indirection through a `Vec`. The classic blog post is Catherine West's *"Using Rust for game development"* RustConf 2018 keynote, which introduced many Rust programmers to the pattern.

Gotchas:

- **Generation overflow** — `u64` is fine forever; `u32` is fine for most workloads but can be exhausted by tight allocate/deallocate loops over months.
- **Iteration order is insertion-order**, not key-order — fine for ECS systems, surprising if you want sorted output.
- **Don't store handles across processes / serialise them naïvely** — they're only meaningful inside their arena.
- **`SlotMap` vs `HopSlotMap` vs `DenseSlotMap`** — `slotmap` ships three variants with different trade-offs (sparse vs dense storage, fast iteration vs fast removal); pick from the [`slotmap` docs](https://docs.rs/slotmap/) when it matters.

## Similar / related topics

- [slotmap](https://crates.io/crates/slotmap) — the canonical generational-index crate (3 variants).
- [thunderdome](https://crates.io/crates/thunderdome) — minimalist alternative; smaller API surface.
- [generational-arena](https://crates.io/crates/generational-arena) — the original Fitzgen crate that popularised the pattern.
- [hecs](https://crates.io/crates/hecs) / [bevy_ecs](https://crates.io/crates/bevy_ecs) — ECS libraries built on this idea.
- Catherine West, [*Using Rust For Game Development*](https://kyren.github.io/2018/09/14/rustconf-talk.html) — the canonical introduction.

## Internal links
- [[README]]
- [[../learning/_must_have|_must_have]]

## Keywords

`#rust` `#generational-index` `#slotmap` `#arena` `#ecs` `#game-dev` `#data-structures`

## References / raw notes

The pattern in its simplest shape — a stable `Copy` handle with a generation counter, an allocator that recycles freed slots, and a typed array indexed by handles:

```rust
#[derive(Eq, PartialEq, Copy, Clone)]
pub struct GenerationalIndex {
    index: usize,
    generation: u64,
}

impl GenerationalIndex {
    pub fn index(&self) -> usize {
        self.index
    }
}
```

Allocator that recycles freed slots and bumps the generation:

```rust
struct AllocatorEntry {
    is_live: bool,
    generation: u64,
}

pub struct GenerationalIndexAllocator {
    entries: Vec<AllocatorEntry>,
    free: Vec<usize>,
}

impl GenerationalIndexAllocator {
    pub fn allocate(&mut self) -> GenerationalIndex { todo!() }
    pub fn deallocate(&mut self, index: GenerationalIndex) -> bool { todo!() }
    pub fn is_live(&self, index: GenerationalIndex) -> bool { todo!() }
}
```

Typed associative array from a handle to a value:

```rust
struct ArrayEntry<T> {
    value: T,
    generation: u64,
}

/// An associative array from `GenerationalIndex` to some value `T`.
pub struct GenerationalIndexArray<T>(Vec<Option<ArrayEntry<T>>>);

impl<T> GenerationalIndexArray<T> {
    pub fn set(&mut self, index: GenerationalIndex, value: T) { todo!() }
    pub fn get(&self, index: GenerationalIndex) -> Option<&T> { todo!() }
    pub fn get_mut(&mut self, index: GenerationalIndex) -> Option<&mut T> { todo!() }
}
```

ECS-style usage — every entity is a generational index, components live in parallel typed arrays:

```rust
type Entity = GenerationalIndex;
type EntityMap<T> = GenerationalIndexArray<T>;

struct Physics { /* ... */ }

struct State {
    entity_allocator: GenerationalIndexAllocator,
    physics: EntityMap<Physics>,
    // ...
    players: Vec<Entity>,
    // ...
}
```

For real code use [`slotmap`](https://crates.io/crates/slotmap) — it handles overflow, dense vs sparse storage, and the ergonomic `secondary_map` for "components keyed by entity handles".
