---
title: "Timely Dataflow — low-latency cyclic dataflow in Rust"
main_link: https://github.com/TimelyDataflow/timely-dataflow
keywords: [timely, dataflow, rust, differential, naiad, materialize, mcsherry, distributed]
status: reviewed
review_date: 2026/05/03
---

# Timely Dataflow — low-latency cyclic dataflow in Rust

**Main link:** <https://github.com/TimelyDataflow/timely-dataflow>

Sister project: [Differential Dataflow](https://github.com/TimelyDataflow/differential-dataflow) · Book: <https://timelydataflow.github.io/timely-dataflow/>

## Summary

[Timely Dataflow](https://github.com/TimelyDataflow/timely-dataflow) is the Rust port and modular re-implementation of the **Naiad** "timely dataflow" model from MSR Cambridge (Murray et al., SOSP'13). Frank McSherry and collaborators rebuilt it as a Rust crate that scales the same dataflow program from one thread on a laptop to a cluster of machines, with **low-latency cyclic dataflow** (i.e. iterative computations expressed as feedback loops) as its distinguishing feature. On top of timely, [**Differential Dataflow**](https://github.com/TimelyDataflow/differential-dataflow) adds incremental update semantics — change a single input record and only the affected outputs are recomputed — which is the engine behind [Materialize](https://materialize.com/), [DDlog](https://github.com/vmware-archive/differential-datalog), and a slice of academic stream-processing work.

## Insight

Timely is the academic-grade dataflow primitive that underpins one specific commercial product (Materialize) and a long tail of research systems. It is not a "pick this off crates.io for a microservice" library — it's a substrate. Reach for it when:

- You want **iterative dataflow with feedback loops** (graph algorithms, fixed-point computations, recursive view maintenance) at scale, and the BSP / map-reduce / micro-batch model isn't expressive enough.
- You want **incremental view maintenance** over changing inputs (use **Differential Dataflow** on top); changing one input row should propagate to outputs in milliseconds without re-running the whole computation.
- You're operating in the same problem space as **Materialize** / **RisingWave** / **Flink** / **Naiad** but want to write the dataflow logic directly in Rust instead of through a SQL frontend.

What makes it different from a stream-processing library you'd normally pick:

1. **Logical timestamps with totally-ordered "frontiers".** Every record carries a timestamp; operators reason about which timestamps are still "live" via frontier progress, which is what enables *exactly-once* incremental updates and proper handling of cycles.
2. **Cyclic dataflow is first-class.** You can write loops (`scope.iterative`) in the dataflow graph itself, not as outer Rust loops, so the engine can incrementalise iteration.
3. **Static dataflow graph.** Operators and edges are declared at start-up; you can't add a new operator at runtime the way you'd add a new Tokio task. That's a deliberate trade for predictability and incremental reasoning.
4. **Distribution is built in but assumes a friendly network.** Workers communicate over TCP, the same binary runs on each node, scale-out is by command-line arguments. Production distributed deployment is more researchy than turn-key — Materialize stripped/replaced large parts to ship it.

Caveats:

- **Small community.** A few research groups and Materialize-adjacent contributors. Stack Overflow will not save you; read the book and the repo issues.
- **Heavy machinery for the simple case.** If you want "fan a producer out to N async workers" reach for [[streaming]] (`par-stream`) or `futures::stream::buffer_unordered`. If you want "memoise a derived value when one input changes" reach for [[salsa]]. Timely is the right answer when those don't compose into what you actually need.
- **Differential Dataflow is where most users actually are.** Plain timely is the substrate; the incremental story (DD) is what most projects build against.

## Similar / related topics

- [Differential Dataflow](https://github.com/TimelyDataflow/differential-dataflow) — incremental view maintenance on top of timely; the layer most users actually program against.
- [Materialize](https://materialize.com/) — streaming SQL database built on the timely / DD lineage; the production-grade commercial use.
- [DDlog](https://github.com/vmware-archive/differential-datalog) — Datalog-to-Rust compiler that targets Differential Dataflow; archived but reference-worthy.
- [Naiad paper (SOSP'13)](https://www.microsoft.com/en-us/research/publication/naiad-a-timely-dataflow-system/) — the original "timely dataflow" research; required reading.
- [Apache Flink](https://flink.apache.org/) — JVM-world incumbent for stateful stream processing; different lineage (Stratosphere / Flink-CEP) but solving overlapping problems.
- [[noria]] — MIT/PDOS streaming dataflow for materialised SQL views; same problem family, different research track.
- [[streaming]] (`par-stream`) — when you just want async parallelism, not a dataflow engine.
- [[salsa]] — demand-driven incremental computation; the pull-based dual of timely's push-based dataflow.

## Internal links

- [[noria]] — sibling streaming-dataflow research project (MIT/PDOS) for SQL materialised views.
- [[streaming]] — lighter-weight async parallelism; the right tool when you don't need full dataflow.
- [[salsa]] — demand-driven counterpart to push-based dataflow.
- [[concurrency/README|Rust concurrency]] — landing page; timely lives in the dataflow row.

## Keywords

`#rust` `#dataflow` `#timely` `#differential` `#naiad` `#materialize` `#streaming` `#distributed`

## References / raw notes

### Origins

Timely dataflow is a low-latency cyclic dataflow computational model, introduced in the paper *Naiad: a timely dataflow system* (SOSP 2013). The Rust crate is an extended and more modular re-implementation maintained by Frank McSherry and collaborators.

The system scales the same program up from a single thread on a laptop to distributed execution across a cluster. The headline goals are expressive power and high throughput / low latency.

### Add to `Cargo.toml`

```toml
[dependencies]
timely = "*"
```

### Hello, world

Available in the repo as `timely/examples/simple.rs`:

```rust
use timely::dataflow::operators::*;

fn main() {
    timely::example(|scope| {
        (0..10)
            .to_stream(scope)
            .inspect(|x| println!("seen: {x:?}"));
    });
}
```

Run from the repo root:

```shell
$ cargo run --example simple
seen: 0
seen: 1
...
seen: 9
```

### Differential Dataflow

The incremental layer on top of timely, supporting efficient updates as inputs change:

<https://github.com/TimelyDataflow/differential-dataflow>

This is what powers [Materialize](https://materialize.com/) and most academic uses.

### Useful entry points

- Repo: <https://github.com/TimelyDataflow/timely-dataflow>
- mdBook: <https://timelydataflow.github.io/timely-dataflow/>
- McSherry blog series introducing the model: <https://github.com/frankmcsherry/blog>
- Naiad paper (SOSP'13): <https://www.microsoft.com/en-us/research/publication/naiad-a-timely-dataflow-system/>
- Differential Dataflow: <https://github.com/TimelyDataflow/differential-dataflow>
- Materialize (commercial product built on this lineage): <https://materialize.com/>
