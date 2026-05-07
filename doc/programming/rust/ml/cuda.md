---
title: CUDA in Rust
main_link: https://github.com/Rust-GPU/Rust-CUDA
keywords: [cuda, gpu, rust, cudarc, cubecl, rust-cuda, rapids, ptx]
status: reviewed
---

# CUDA in Rust

**Main link:** <https://github.com/Rust-GPU/Rust-CUDA>

## Summary

Three serious projects let you talk to NVIDIA GPUs from Rust, each
solving a different layer of the problem:

- **[`cudarc`](https://github.com/coreylowman/cudarc)** — safe, thin
  Rust wrappers over the CUDA driver API + NVRTC + cuRAND + cuBLAS.
  Use it when you have existing CUDA / PTX kernels (or hand-written
  ones) and just want to load and launch them from Rust. Active,
  used by `dfdx` and `candle`'s CUDA backend.
- **[`Rust-CUDA`](https://github.com/Rust-GPU/Rust-CUDA)** (the
  Rust-GPU working group) — *write* GPU kernels in Rust and compile
  them to PTX via a custom `rustc` codegen backend. Ambitious, more
  upheaval-prone, recently revived after a 2022-2024 quiet period.
- **[`cubecl`](https://crates.io/crates/cubecl)** (Tracel, the burn
  team) — `#[cube]`-attribute kernels that compile to CUDA *and* WGPU,
  giving you cross-vendor GPU portability from a single Rust source.

For broader (non-Rust-specific) CUDA framing see
[[../../../ml/compilers/cuda|ml/compilers/cuda]].

## Insight

Pick by what layer you actually want to write:

| You want to…                                              | Reach for                                        |
|-----------------------------------------------------------|--------------------------------------------------|
| Load and launch a `.ptx` / `.cu` kernel from Rust         | [`cudarc`](https://github.com/coreylowman/cudarc) |
| Run cuBLAS / cuRAND from Rust                             | `cudarc`                                         |
| Write GPU kernels in *Rust source*, CUDA-only             | `Rust-CUDA` (rustc_codegen_nvvm)                 |
| Write GPU kernels in Rust source, CUDA *and* WGPU         | `cubecl`                                         |
| Pre-built, idiomatic deep-learning ops                    | `candle` / `burn` (use `cudarc` underneath)      |
| Run CUDA dataframes from Rust                             | NVIDIA RAPIDS (`cudf`) — currently Python-only; Rust bindings open issue (`cudf#11742`) |

Things to know:

- **CUDA toolkit dependency**: every option needs the NVIDIA CUDA
  toolkit and a matching driver on the target machine. None of these
  crates ship CUDA itself.
- **PTX vs cubin**: `cudarc` and `Rust-CUDA` both produce PTX (forward-
  compatible across CUDA architectures); cubin is GPU-specific. Build
  with `--gpu-architecture=compute_NN` and you get a fat binary that
  JITs on first launch.
- **Cross-vendor portability**: only `cubecl` and (separately) the
  `wgpu` ecosystem give you NVIDIA + AMD + Apple Silicon from one
  source. Pure CUDA crates lock you to NVIDIA.
- **Maturity**: `cudarc` is the most boring choice and that's a
  feature. `Rust-CUDA` is exciting but you should expect to pin
  versions and read changelogs. `cubecl` is young but the burn team
  ships releases.
- **Rayon on GPU**: there is no GPU `rayon` (issue
  [`rayon#778`](https://github.com/rayon-rs/rayon/issues/778) tracks
  the wish). Use `cubecl` / `wgpu` if you want SPMD-style "Rust
  closure on a GPU".

## Similar / related topics

- [[../../../ml/compilers/cuda|ml/compilers/cuda]] — broader CUDA
  framing (non-Rust-specific).
- [[ml_in_rust]] — Rust ML libraries that *use* these crates.
- `wgpu` — cross-vendor GPU API on top of Vulkan / Metal / DX12 /
  WebGPU; `cubecl` builds on it for the non-CUDA backend.
- `rustc_codegen_nvvm` — the codegen backend behind `Rust-CUDA`;
  cousin of `rustc_codegen_clr` (see [[../interop/to_net]]) and
  `cranelift`.
- NVIDIA RAPIDS / cuDF — GPU dataframes. Python-first; Rust bindings
  remain wishlist.

## Internal links

- [[ml_in_rust]] — sibling ML overview.
- [[linfa]] — sibling on the CPU classical-ML side.
- [[README]] — Rust ML section landing.
- [[../../../ml/compilers/cuda|ml/compilers/cuda]] — broader CUDA hub.

## Keywords

`#cuda` `#gpu` `#rust` `#cudarc` `#cubecl` `#rust-cuda` `#ptx` `#rapids`

## References / raw notes

### Rust-CUDA (Rust-GPU)

[github.com/Rust-GPU/Rust-CUDA](https://github.com/Rust-GPU/Rust-CUDA) —
"An ecosystem of libraries and tools for writing and executing
extremely fast GPU code fully in Rust." Aims to make Rust a tier-1
language for CUDA: compile Rust to PTX via `rustc_codegen_nvvm`, plus
libraries for using existing CUDA libraries from Rust. Quiet
2022-2024, revived 2024+.

### cudarc

- [crates.io/crates/cudarc](https://crates.io/crates/cudarc)
- [github.com/coreylowman/cudarc](https://github.com/coreylowman/cudarc)
- [Build-workflow example](https://github.com/coreylowman/cudarc/blob/main/examples/07-build-workflow/src/main.rs)

Safe abstractions over: CUDA driver API, NVRTC, cuRAND, cuBLAS. Used
by `dfdx` and `candle`'s CUDA backend. The PTX-format handling has
sharp edges; otherwise the most production-ready option.

### CubeCL

[crates.io/crates/cubecl](https://crates.io/crates/cubecl) (Tracel /
the burn team).

> With CubeCL, you can program your GPU using Rust, taking advantage
> of zero-cost abstractions to develop maintainable, flexible, and
> efficient compute kernels. CubeCL currently fully supports
> functions, generics, and structs, with partial support for traits
> and type inference.

Annotate functions with `#[cube]` to indicate they should run on the
GPU:

```rust
use cubecl::prelude::*;

#[cube(launch_unchecked)]
fn gelu_array<F: Float>(input: &Array<F>, output: &mut Array<F>) {
    if ABSOLUTE_POS < input.len() {
        output[ABSOLUTE_POS] = gelu_scalar::<F>(input[ABSOLUTE_POS]);
    }
}

#[cube]
fn gelu_scalar<F: Float>(x: F) -> F {
    x * (F::erf(x / F::sqrt(2.0.into())) + 1.0) / 2.0
}
```

Launch the autogenerated kernel:

```rust
fn launch<R: Runtime>(device: &R::Device) {
    let client = R::client(device);
    let input = &[-1., 0., 1., 5.];
    let output_handle = client.empty(input.len() * core::mem::size_of::<f32>());
    let input_handle = client.create(f32::as_bytes(input));

    unsafe {
        gelu_array::launch_unchecked::<F32, R>(
            &client,
            CubeCount::Static(1, 1, 1),
            CubeDim::new(input.len() as u32, 1, 1),
            ArrayArg::from_raw_parts(&input_handle, input.len(), 1),
            ArrayArg::from_raw_parts(&output_handle, input.len(), 1),
        )
    };

    let bytes = client.read(output_handle.binding());
    let output = f32::from_bytes(&bytes);
    // [-0.1587, 0.0000, 0.8413, 5.0000]
    println!("Executed gelu with runtime {:?} => {output:?}", R::name());
}

fn main() {
    launch::<cubecl::cuda::CudaRuntime>(&Default::default());
    launch::<cubecl::wgpu::WgpuRuntime>(&Default::default());
}
```

Run the example:

```shell
cargo run --example gelu --features cuda  # CUDA runtime
cargo run --example gelu --features wgpu  # WGPU runtime
```

### NVIDIA RAPIDS / cuDF

- [github.com/rapidsai](https://github.com/rapidsai)
- [github.com/rapidsai/cudf](https://github.com/rapidsai/cudf) —
  GPU-accelerated DataFrame library, currently Python/C++.
- [Rust bindings tracker (cudf#11742)](https://github.com/rapidsai/cudf/issues/11742)
  — open issue, no formal Rust bindings yet.
- [docs.rapids.ai/api/cudf/stable/](https://docs.rapids.ai/api/cudf/stable/)

### Rayon support

[`rayon#778`](https://github.com/rayon-rs/rayon/issues/778) — long-
standing wishlist for GPU-targeted Rayon parallelism. Still no
implementation; use `cubecl` or `wgpu` for SPMD on GPU.
