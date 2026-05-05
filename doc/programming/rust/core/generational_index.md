---
title: slotmap crate
main_link: 
keywords: [generational-index, rust, special, king]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

# slotmap crate

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[reflection_step_by_step_article]] — Reflection step by step - Article _(score 24.7)_
- [[reflection]] — reflect _(score 24.7)_
- [[error]] — eyre _(score 24.7)_
- [[pin_project]] — pin-project _(score 24.7)_
- [[eyre]] — eyre _(score 14.7)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#generational-index` `#core` `#rust` `#programming` `#special` `#king` `#super` `#index`

## TODO

- No `main_link` could be auto-detected. Add the canonical URL (project homepage / repo / paper) to the frontmatter.
- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

Special king of super index

# slotmap crate


```rust
#[derive(Eq, PartialEq, ..)]
pub struct GenerationalIndex {
    index: usize,
    generation: u64,

}


impl GenerationalIndex {
    pub fn index(&self) -> usize {
        todo!()
    }
}
```

allocator for those indexes

```rust
struct AllocatorEntry {
    is_live: bool,
    generations: u64,
}


pub struct GenerationalIndexAllocator {
    entries: Vec<AllocatorEntry>,
    free: Vec<usize>,
}


impl GenerationalIndexAllocator {
    pub fn allocate(&mut self) -> GenerationalIndex { todo!() }
    pub fn deallocate(&mut self) -> bool { todo!() }
    pub fn is_live(&mut self) -> bool { todo!() }
}

```

```rust
struct ArrayEntry<T> {
    value: T,
    generation: u64,
}

// an associative array from GenerationalIndex to some value T.
pub struct GenerationalIndexArray<t>(Vec<Option<ArrayEntry<T>>>);


impl<T> GenerationalIndexArray<T> {
    pub fn set(&mut self, index: GenerationalIndex, value: T) { todo!() }
    pub fn get(&self, index: GenerationalIndex) -> Option<T> { todo!() }
    pub fn get_mut(&mut self, index: GenerationalIndex) -> Option<&mut T> { todo!() }
}


```


```rust
struct Physics {}
//...

type Entity = GenerationalIndex;
type EntityMap<T> = GenerationalIndexArray<T>;


struct State {
    entity_allocator: GenerationalIndexAllocator,
    
    physics: EntityMap<Physics>,
    //...
    
    
    players: Vec<Entity>,
    //...
}

```
