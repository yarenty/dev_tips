---
title: Burn — Tracel-AI's pure-Rust deep-learning framework
main_link: https://github.com/tracel-ai/burn
keywords: [burn, tracel, rust, deep-learning, wgpu, ndarray, libtorch, candle, autodiff, onnx]
status: reviewed
---

# Burn — Tracel-AI's pure-Rust deep-learning framework

**Main link:** <https://github.com/tracel-ai/burn>

## Summary
Burn (built by [Tracel-AI](https://tracel.ai/)) is the most ambitious pure-Rust deep-learning framework: a comprehensive, dynamic-graph framework that's **backend-agnostic** by design — the same model code runs on WGPU (cross-platform GPU via Vulkan/Metal/DX12/WebGPU), CUDA (in-house Cubecl backend), Metal, LibTorch (via [[tch]]), [[candle]], and the pure-CPU NdArray backend (the only one that supports `no_std` for embedded). Autodiff and kernel fusion are implemented as **backend decorators** that wrap any inner backend. Burn handles training and inference (with a Ratatui-based training TUI dashboard), imports ONNX and PyTorch model weights, and compiles to WebAssembly for in-browser inference.

## Insight
Reach for Burn when you want **pure-Rust deep learning that scales from a Raspberry Pi (NdArray + `no_std`) up to a multi-GPU training rig (CUDA / Cubecl / WGPU)** without rewriting your model. The backend trait + decorator design is the killer feature: `type Backend = Autodiff<Fusion<Wgpu>>` literally composes "give me automatic differentiation + kernel fusion on top of WGPU" as a type. The same model also runs as `Autodiff<LibTorch>` for benchmarking against PyTorch, or as plain `NdArray` for embedded inference. The ONNX importer (`burn-import`) and a parallel PyTorch-weight importer cover the "trained in Python, deploy in Rust" path — turning Burn into a competitor to [[tract]] for inference *and* a competitor to PyTorch for training.

Vs the Rust-DL neighbours:
- **vs [[candle]]** — Candle (HuggingFace) is lighter, more PyTorch-ergonomic, and aimed at production inference; Burn is heavier, more general, and explicitly training-friendly. Burn even *uses* Candle as one of its backends.
- **vs [[tch]]** — `tch` is the easiest path to PyTorch parity (because it *is* PyTorch via libtorch), at the cost of a C++ dependency and PyTorch ABI pinning. Burn is pure-Rust end-to-end (with `tch` as one of many available backends).
- **vs [[dfdx]]** — `dfdx` does compile-time tensor shape checking via const generics, which Burn does not; Burn picks runtime shapes for ergonomics and a much wider operator surface. Notably, dfdx's author Corey Lowman moved on to contribute to Burn.

Gotchas worth knowing: kernel fusion currently lives in the WGPU/Cubecl backends; not all backends support every op (Candle backend in particular is "inference-mostly"); `no_std` works only on NdArray; Tensor Cores are wired in via the LibTorch and Candle backends but not yet WGPU (tracking [gpuweb#4195](https://github.com/gpuweb/gpuweb/issues/4195)); the project is still in active breaking-changes territory. The [Burn Book](https://burn.dev/book/) is the canonical learning path; the [tracel-ai/models](https://github.com/tracel-ai/models) repo curates pretrained checkpoints.

## Similar / related topics
- [[candle]] — HuggingFace's lighter pure-Rust ML framework; a Burn backend and a Burn alternative.
- [[tch]] — Rust bindings to libtorch; available as a Burn backend (`burn-tch`).
- [[dfdx]] — pure-Rust DL with compile-time shape checking; smaller op surface, more research-y.
- [[tract]] — Sonos's inference-only ONNX/NNEF/TF engine; the deploy-only counterpart to Burn's training-and-inference.
- [PyTorch](https://pytorch.org/) — the Python framework Burn is most often compared against; Burn imports its weights and ONNX exports.

## Internal links
- [[candle]]
- [[tch]]
- [[dfdx]]
- [[tract]]
- [[../../programming/rust/ml/ml_in_rust|programming/rust/ml/ml_in_rust]]

## Keywords
`#burn` `#tracel-ai` `#rust` `#deep-learning` `#wgpu` `#cubecl` `#autodiff` `#onnx` `#no_std`

## References / raw notes

Burn is a comprehensive dynamic Deep Learning Framework built using Rust with extreme flexibility, compute efficiency, and portability as its primary goals.

### Performance

Burn pushes hard on performance:

- **Automatic kernel fusion** — at runtime, custom low-level kernels are generated for your specific tensor expression. For example, a custom GELU written with the high-level tensor API:

  ```rust
  fn gelu_custom<B: Backend, const D: usize>(x: Tensor<B, D>) -> Tensor<B, D> {
      let x = x.clone() * ((x / SQRT_2).erf() + 1);
      x / 2
  }
  ```

  is fused at runtime into a custom WGSL kernel that rivals a hand-written GPU implementation. (Fusion is currently implemented for the in-house WGPU/Cubecl backends.)

- **Asynchronous execution** — in-house backends use async execution so framework overhead doesn't block model compute and vice versa. See the [async-backends blog post](https://burn.dev/blog/creating-high-performance-asynchronous-backends-with-burn-compute).

- **Thread-safe building blocks** — modules own their weights; you can send a module to another thread, compute gradients there, and aggregate on the main thread. This is structurally different from PyTorch (which mutates `.grad` on each parameter, requiring lower-level synchronisation primitives).

- **Intelligent memory management** — backend-pluggable memory pools; in-place mutation tracked through Rust's ownership system. See [tensor handling blog post](https://burn.dev/blog/burn-rusty-approach-to-tensor-handling).

- **Automatic kernel selection** — in-house backends benchmark kernel parameter combinations on first use and cache the winner. Adds warmup cost; saves a lot at steady state.

- **Hardware-specific features** — Tensor Cores supported via the LibTorch and Candle backends; tracking [gpuweb#4195](https://github.com/gpuweb/gpuweb/issues/4195) for the WGPU equivalent.

- **Custom backend extension** — you can extend any backend with custom ops (e.g. flash attention, custom kernels). See [the backend-extension chapter of the Burn Book](https://burn.dev/book/advanced/backend-extension/index.html).

### Training & inference

Built from the ground up with both in mind. The training dashboard is a Ratatui-based TUI ([Ratatui](https://github.com/ratatui-org/ratatui)) showing live train/validation metrics with arrow-key history navigation and graceful break-from-training-loop checkpointing.

- **ONNX support** via [`burn-import`](https://burn.dev/book/import/onnx-model.html) — supports a [growing subset](https://github.com/tracel-ai/burn/blob/main/crates/burn-import/SUPPORTED-ONNX-OPS.md) of ONNX operators.
- **PyTorch model import** — load PyTorch state-dict weights into Burn's native architecture. See [the PyTorch-import chapter](https://burn.dev/book/import/pytorch-model.html).
- **Browser inference via WASM** — the Candle, NdArray (CPU), and WGPU (GPU) backends all compile to WebAssembly. Examples: MNIST in browser and image classification in browser.
- **Embedded `no_std`** — `burn-core` supports [`no_std`](https://docs.rust-embedded.org/book/intro/no-std.html). Currently only the NdArray backend works in a `no_std` environment.

### Backends

Burn's defining design choice: most code is generic over the `Backend` trait, so backends are swappable and *composable* (e.g. `Autodiff<Fusion<Wgpu>>`). Backends fall into two groups: real backends and decorator backends.

- **WGPU** — the cross-platform GPU backend. Targets Vulkan, OpenGL, Metal, DX11/12, and WebGPU via [WGPU](https://wgpu.rs) and [WGSL](https://www.w3.org/TR/WGSL/). Compiles to WASM for in-browser GPU compute. The first in-house backend; full kernel fusion, the optimisation playground. Read the [cross-platform-GPU blog post](https://burn.dev/blog/cross-platform-gpu-backend).

- **Candle** — wraps [HuggingFace's Candle](https://github.com/huggingface/candle); CPU + WASM + Nvidia CUDA. Inference-mostly today.

- **LibTorch** — wraps [tch-rs](https://github.com/LaurentMazare/tch-rs), giving you libtorch C++ kernels on CPU, CUDA, and Metal. Heavyweight but the most-mature path to PyTorch parity.

- **NdArray** — pure-Rust CPU backend on the `ndarray` crate; admittedly not the fastest, but the most portable and the only `no_std` option.

- **Autodiff (decorator)** — wrapping any backend with `Autodiff` adds backpropagation transparently:

  ```rust
  use burn::backend::{Autodiff, Wgpu};
  use burn::tensor::{Distribution, Tensor};

  fn main() {
      type Backend = Autodiff<Wgpu>;
      let x: Tensor<Backend, 2> = Tensor::random([32, 32], Distribution::Default);
      let y: Tensor<Backend, 2> = Tensor::random([32, 32], Distribution::Default).require_grad();
      let tmp = x.clone() + y.clone();
      let tmp = tmp.matmul(x);
      let tmp = tmp.exp();
      let grads = tmp.backward();
      let y_grad = y.grad(&grads).unwrap();
      println!("{y_grad}");
  }
  ```

  Notably, calling `.backward()` on a non-Autodiff backend is a *compile error* — you can't accidentally backprop on an inference-only stack.

- **Fusion (decorator)** — adds kernel fusion when the inner backend supports it (currently WGPU). Composes with Autodiff: `Autodiff<Fusion<Wgpu>>`. Automatic gradient checkpointing on top is planned (see [issue #936](https://github.com/tracel-ai/burn/issues/936)).

### Getting started

Read the first sections of [The Burn Book](https://burn.dev/book/) — covers tensors, modules, optimisers, and goes all the way to writing your own GPU kernels.

A taste of the API — a position-wise feed-forward block:

```rust
use burn::nn;
use burn::module::Module;
use burn::tensor::backend::Backend;

#[derive(Module, Debug)]
pub struct PositionWiseFeedForward<B: Backend> {
    linear_inner: nn::Linear<B>,
    linear_outer: nn::Linear<B>,
    dropout: nn::Dropout,
    gelu: nn::Gelu,
}

impl<B: Backend> PositionWiseFeedForward<B> {
    pub fn forward<const D: usize>(&self, input: Tensor<B, D>) -> Tensor<B, D> {
        let x = self.linear_inner.forward(input);
        let x = self.gelu.forward(x);
        let x = self.dropout.forward(x);
        self.linear_outer.forward(x)
    }
}
```

The repo has a substantial `examples/` directory; pretrained models are tracked at [tracel-ai/models](https://github.com/tracel-ai/models).

### Why Rust for deep learning?

Deep learning needs *both* high-level abstraction and very fast execution. Rust's zero-cost abstractions and fine-grained memory control fit that pairing well; the mainstream Python-with-C/C++-bindings approach trades portability and complexity for ergonomics. Cargo also makes building, testing, and deploying easy across environments where Python is painful. Rust has a steeper learning curve up-front, but the project bets on more reliable, bug-free solutions over the long run.

### Status & licence

Active development; expect breaking changes. Distributed under MIT and Apache-2.0. Discord at <https://discord.gg/uPEBbYYDB6>; contributing guide and architecture overview in the repo.
