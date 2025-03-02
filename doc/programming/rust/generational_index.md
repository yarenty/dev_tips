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