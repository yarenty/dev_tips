---
title: dfdx — pure-Rust deep learning with compile-time tensor shapes
main_link: https://github.com/coreylowman/dfdx
keywords: [dfdx, rust, deep-learning, const-generics, shape-checking, autodiff, cuda]
status: reviewed
---

# dfdx — pure-Rust deep learning with compile-time tensor shapes

**Main link:** <https://github.com/coreylowman/dfdx>

## Summary
`dfdx` (by Corey Lowman) is a pure-Rust deep-learning library whose **killer differentiator is compile-time tensor shape checking**: tensor dimensions live in the type, so `Tensor<Rank2<5, 10>>` and `Tensor<(usize, Const<10>)>` are distinguishable types and a shape mismatch refuses to compile. Optimisers, neural-network building blocks (`Linear`, `Conv2D`, `Transformer`, …), CPU and CUDA backends, and a small but well-shaped `nn`/optimiser API are all built around that core trick.

## Insight
Reach for `dfdx` when you want to **lean hard on Rust's type system to make whole categories of ML bugs unrepresentable** — wrong batch dim, transposed matmul, off-by-one in a reshape — and you don't need a giant model zoo. The "you cannot call `.backward()` without a `traced()` tensor" pattern is a beautiful application of move semantics: the gradient tape can only live on intermediate results, never on parameters, never behind `Rc<RefCell<…>>`. Tuples-as-feedforward (`type Mlp = ((Linear<10,32>, ReLU), (Linear<32,32>, ReLU), (Linear<32,2>, Tanh));`) is the most idiomatic-Rust way of expressing a model in any of the Rust DL frameworks.

Compared to the neighbours:
- **vs [[burn]]** — Burn picks runtime shapes for ergonomics and ships a much wider operator surface, ONNX/PyTorch importers, training UI, swappable backends, more active maintenance. dfdx is smaller, more research-y, but enforces shape correctness at compile time (which Burn does not).
- **vs [[candle]]** — Candle has the bigger model zoo (`candle-transformers`) and PyTorch-like ergonomics; dfdx is more type-system-forward but smaller in scope.
- **vs [[tch]]** — `tch` wraps libtorch; you get the entire PyTorch op set at the cost of a C++ dependency. dfdx is pure-Rust, no FFI, but a much smaller op set.

Honest caveats: **less actively maintained than Burn or Candle as of 2024-2025**. Corey Lowman moved on to contribute to Burn (taking some of the design lessons with him), so dfdx is best read today as an "interesting research artefact + a useful exemplar of typed-tensor design" rather than the recommended choice for new production projects. Some features still required nightly Rust (per Candle's own framing in their FAQ); the const-generics-heavy API can be daunting for non-Rust-experts; the project itself is described in its README as "still in pre-alpha state. The next few releases are planned to be breaking releases."

## Similar / related topics
- [[burn]] — pure-Rust DL, runtime shapes, swappable backends; where Corey Lowman now contributes.
- [[candle]] — HuggingFace's pure-Rust DL; PyTorch-like API, much bigger model zoo.
- [[tch]] — Rust bindings to libtorch; full PyTorch op set, FFI cost, no compile-time shapes.
- [[../fundamentals/no_deep_learning_needed|no_deep_learning_needed]] — the GBDT-first counter-trend; reach there before any DL framework for tabular data.
- [JAX](https://github.com/google/jax) — Python's spiritual cousin for typed (via `jaxtyping` + tools like `beartype`) functional autodiff.

## Internal links
<!-- reviewed -->
- [[burn]]
- [[candle]]
- [[tch]]
- [[tract]]
- [[../../programming/rust/ml/ml_in_rust|programming/rust/ml/ml_in_rust]]

## Keywords
`#dfdx` `#rust` `#deep-learning` `#const-generics` `#shape-checking` `#autodiff` `#cuda`

## References / raw notes

Ergonomics- and safety-focused deep learning in Rust.

> **Status note**: still in pre-alpha; next few releases planned as breaking. Maintenance is reduced as of 2024 — the author moved on to contribute to [[burn]].

### Features at a glance

1. 🔥 GPU-accelerated tensor library, shapes up to 6D.
2. Shapes mix compile-time and runtime sized dimensions: e.g. `Tensor<(usize, Const<10>)>` and `Tensor<Rank2<5, 10>>`.
3. Big tensor-op library (`matmul`, `conv2d`, …), all shape- and type-checked at compile time.
4. Ergonomic neural-network building blocks: `Linear`, `Conv2D`, `Transformer`, …
5. Standard DL optimisers: `Sgd`, `Adam`, `AdamW`, `RMSprop`, …

`dfdx` is on [crates.io](https://crates.io/crates/dfdx). Add to `Cargo.toml`:

```toml
dfdx = "0.13.0"
```

Docs: [docs.rs/dfdx](https://docs.rs/dfdx).

### Design goals

1. Ergonomics top to bottom (frontend interface and internals).
2. Check as much as possible at compile time — if it's wrong, it shouldn't compile.
3. Maximise performance.
4. Minimise `unsafe` (currently the only `unsafe` calls are matmul).
5. Minimise `Rc<RefCell<T>>` in internal code (only `Arc`-on-tensor-data, used over `Box` to make tensor clones cheap).

### GPU acceleration

Enable the `cuda` feature for the `Cuda` device. Requires the NVIDIA CUDA toolkit. See [feature-flag docs](https://docs.rs/dfdx/latest/dfdx/feature_flags/index.html).

### API preview

Simple shape-checked neural network:

```rust
type Mlp = (
    (Linear<10, 32>, ReLU),
    (Linear<32, 32>, ReLU),
    (Linear<32, 2>, Tanh),
);

fn main() {
    let dev: Cuda = Default::default(); // or `Cpu`
    let mlp = dev.build_module::<Mlp, f32>();
    let x: Tensor<Rank1<10>, f32, Cpu> = dev.zeros();
    let y: Tensor<Rank1<2>, f32, Cpu> = mlp.forward(x);
    mlp.save("checkpoint.npz")?;
}
```

Optimiser:

```rust
type Model = ...;
let mut model = dev.build_module::<Model, f32>();
let mut grads = model.alloc_grads();
let mut sgd = Sgd::new(&model, SgdConfig {
    lr: 1e-2,
    momentum: Some(Momentum::Nesterov(0.9))
});

let loss = ...;
grads = loss.backward();
sgd.update(&mut model, &grads);
```

Const tensors round-trip with normal Rust arrays:

```rust
let t0: Tensor<Rank0, f32, _> = dev.tensor(0.0);
assert_eq!(t0.array(), &0.0);

let t1 /*: Tensor<Rank1<3>, f32, _>*/ = dev.tensor([1.0, 2.0, 3.0]);
assert_eq!(t1.array(), [1.0, 2.0, 3.0]);

let t2: Tensor<Rank2<2, 3>, f32, _> = dev.sample_normal();
assert_ne!(t2.array(), [[0.0; 3]; 2]);
```

### Notable implementation details

#### `Module` trait

```rust
pub trait Module<Input> {
    type Output;
    fn forward(&self, input: Input) -> Self::Output;
}
```

This single trait gives you single + batched inputs (multiple impls), multiple inputs/outputs (multi-headed modules, RNNs), and tape-presence-dependent behaviour (which is *not* the `.train()`/`.eval()` flag from other libraries — it's encoded in the type).

#### Tuples as feedforward sequences

Implementing traits for tuples gives you a lovely sequential-composition syntax:

```rust
type Model = (Linear<10, 5>, Tanh);
let model = dev.build_module::<Model, f32>();
```

Implementing `Module` for a 2-tuple:

```rust
impl<Input, A, B> Module<Input> for (A, B)
where
    Input: Tensor,
    A: Module<Input>,
    B: Module<A::Output>,
{
    type Output = B::Output;
    fn forward(&self, x: Input) -> Self::Output {
        let x = self.0.forward(x);
        let x = self.1.forward(x);
        x
    }
}
```

Implemented for tuples up to 6 elements; nest arbitrarily.

#### No `Rc<RefCell<T>>` — gradient tape isn't behind a cell

Other DL frameworks store a reference to the gradient tape on tensors, requiring mutation or `Rc<RefCell<…>>` everywhere. dfdx avoids both: since every operation produces exactly one child tensor, the tape can be moved to the child of the last operation. Model parameters never own the tape (they're never the *result* of an op). The tape is always on intermediate values, never on long-lived state.

For tensors that need to be reused multiple times in a graph: clone the tensor and manually move the tape.

#### Type-checked backward

If you forget `.trace()` / `.traced()`, the program won't compile:

```diff
-let pred = module.forward(x);
+let pred = module.forward(x.traced(grads));
let loss = (y - pred).square().mean();
let gradients = loss.backward();
```

Because we know exactly which tensor owns the tape, `.backward()` requires the tensor to *own* the tape and to be moved in — destructing the tape and producing the gradients. **All of this is checked at compile time.**

#### Validated against PyTorch

All functions and operations are tested against equivalent PyTorch behaviour.
