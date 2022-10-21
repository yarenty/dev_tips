# Flume

A blazingly fast multi-producer, multi-consumer channel.

Cargo Documentation License actions-badge

```rust
use std::thread;

fn main() {
println!("Hello, world!");

    let (tx, rx) = flume::unbounded();

    thread::spawn(move || {
        (0..10).for_each(|i| {
            tx.send(i).unwrap();
        })
    });

    let received: u32 = rx.iter().sum();

    assert_eq!((0..10).sum::<u32>(), received);
}
```


**Why Flume?**

- Featureful: Unbounded, bounded and rendezvous queues
- Fast: Always faster than std::sync::mpsc and sometimes crossbeam-channel
- Safe: No unsafe code anywhere in the codebase!
- Flexible: Sender and Receiver both implement Send + Sync + Clone
- Familiar: **Drop-in replacement for std::sync::mpsc**
- Capable: Additional features like MPMC support and send timeouts/deadlines
- Simple: Few dependencies, minimal codebase, fast to compile
- Asynchronous: async support, including mix 'n match with sync code
- Ergonomic: Powerful select-like interface