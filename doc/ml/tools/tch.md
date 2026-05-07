---
title: tch — Rust bindings for PyTorch's C++ API (libtorch)
main_link: https://github.com/LaurentMazare/tch-rs
keywords: [tch, tch-rs, libtorch, pytorch, rust, ffi, ml, deep-learning]
status: reviewed
---

# tch — Rust bindings for PyTorch's C++ API (libtorch)

**Main link:** <https://github.com/LaurentMazare/tch-rs>

## Summary
`tch` (also known as `tch-rs`) is Laurent Mazare's Rust crate that wraps PyTorch's C++ runtime (libtorch) and exposes a Rust tensor + autograd + `nn` API that stays close to the C++ shape. It is the most-mature Rust path to "I have a PyTorch model and want to run (or train) it from Rust without rewriting it", because the underlying kernels, CUDA/Metal acceleration, TorchScript JIT loader, and nearly the whole `torch.*` op set come along for free. The code-generation that turns the C API into Rust bindings is shared with Mazare's earlier [`ocaml-torch`](https://github.com/LaurentMazare/ocaml-torch) project.

## Insight
Reach for `tch` when **interop with the existing PyTorch ecosystem matters more than a pure-Rust dependency footprint** — loading a `.pt` / TorchScript model trained in Python, reusing PyTorch's optimised CUDA kernels, or running a model on Apple Metal via libtorch. The cost is the libtorch C++ dependency: you have to point the build at a matching libtorch (system install, downloaded prebuilt, or a Python `torch` install via `LIBTORCH_USE_PYTORCH=1`), pin the libtorch ABI to the version `tch` targets, and — on Linux/macOS — set `LD_LIBRARY_PATH` / `DYLD_LIBRARY_PATH` so dynamically-linked binaries find `libtorch_cpu.so` at runtime. On Windows, the MSVC toolchain is strongly recommended over MinGW.

Compared to the other Rust DL frameworks: `tch` is the **heavyweight, batteries-included** option. [[candle]] is pure-Rust and serverless-friendly but reimplements ops; [[burn]] is pure-Rust and backend-agnostic (and can use libtorch *via* `tch` as one of its backends); [[dfdx]] gives you compile-time shape checking but a much smaller op surface. `tch` is what you pick when you have a real PyTorch model in production and want a Rust binary as the host process.

A few practical gotchas: incremental rebuilds of `torch-sys` can be slow if rust-analyzer doesn't see `LIBTORCH` / `LD_LIBRARY_PATH`; the M-series Mac story works but needs the right libtorch (see issue #488); Windows debug and release libtorch builds are not ABI-compatible; and you can call back into Python via [PyO3](../../programming/rust/python/pyo3) using `tch-ext` if you ever need to expose Rust/`tch` code as a Python extension.

## Similar / related topics
- [[candle]] — HuggingFace's pure-Rust ML framework; lighter, no libtorch dependency, smaller op set.
- [[burn]] — backend-agnostic pure-Rust DL framework; can wrap `tch` as the LibTorch backend.
- [[dfdx]] — pure-Rust DL with compile-time tensor shapes; much smaller scope than `tch`.
- [[tract]] — Sonos's pure-Rust ONNX/TF inference engine; CPU-only, no training, no libtorch.
- [PyTorch](https://pytorch.org/) — the upstream Python framework whose libtorch this binds to.

## Internal links
- [[candle]]
- [[burn]]
- [[dfdx]]
- [[tract]]
- [[../../programming/rust/ml/ml_in_rust|programming/rust/ml/ml_in_rust]]

## Keywords
`#tch` `#libtorch` `#pytorch` `#rust` `#ffi` `#ml` `#deep-learning`

## References / raw notes

Rust bindings for the C++ API of PyTorch. The goal of the `tch` crate is to provide thin wrappers around the C++ PyTorch API (a.k.a. libtorch). It aims at staying as close as possible to the original C++ API. More idiomatic Rust bindings could then be developed on top of this. The [documentation](https://docs.rs/tch/) is on docs.rs.

The code generation part for the C API on top of libtorch comes from [ocaml-torch](https://github.com/LaurentMazare/ocaml-torch).

### Getting started

This crate requires the C++ PyTorch library (libtorch) — a specific version (e.g. v2.3.0) must be available on your system. You can either:

- Use the system-wide libtorch installation (default).
- Install libtorch manually and point the build script at it via the `LIBTORCH` environment variable.
- Use a Python PyTorch install: set `LIBTORCH_USE_PYTORCH=1` and the active Python interpreter is queried for the `torch` package location.
- Have the build script download a prebuilt binary by enabling the `download-libtorch` feature. Defaults to CPU; set `TORCH_CUDA_VERSION=cu117` (or similar) for a CUDA prebuilt.

#### System-wide libtorch

On Linux, the build script looks for `/usr/lib/libtorch.so`.

#### Manual install

Get libtorch from the [PyTorch download page](https://pytorch.org/get-started/locally/) and extract it. On Linux/macOS, point the build at it:

```sh
export LIBTORCH=/path/to/libtorch
```

The header and library locations can be specified separately:

```sh
# LIBTORCH_INCLUDE must contain `include` directory.
export LIBTORCH_INCLUDE=/path/to/libtorch/
# LIBTORCH_LIB must contain `lib` directory.
export LIBTORCH_LIB=/path/to/libtorch/
```

On Windows, set `LIBTORCH=X:\path\to\libtorch` via Control Panel → Environment Variables and append `X:\path\to\libtorch\lib` to `Path`. In a PowerShell session:

```powershell
$Env:LIBTORCH = "X:\path\to\libtorch"
$Env:Path += ";X:\path\to\libtorch\lib"
```

#### Windows-specific notes

Per [the PyTorch docs](https://pytorch.org/cppdocs/installing.html), Windows debug and release builds of libtorch are not ABI-compatible — mixing them produces segfaults. Use the MSVC Rust toolchain (`stable-x86_64-pc-windows-msvc` via rustup); MinGW has compatibility problems with PyTorch.

#### Static linking

Setting `LIBTORCH_STATIC=1` switches to static linking. The prebuilt artifacts don't include `libtorch.a`, so you need to build it yourself:

```sh
git clone -b v2.3.0 --recurse-submodule https://github.com/pytorch/pytorch.git pytorch-static --depth 1
cd pytorch-static
USE_CUDA=OFF BUILD_SHARED_LIBS=OFF python setup.py build
# export LIBTORCH to point at the build directory in pytorch-static.
```

### Examples

#### Basic tensor operations

```rust
use tch::Tensor;

fn main() {
    let t = Tensor::from_slice(&[3, 1, 4, 1, 5]);
    let t = t * 2;
    t.print();
}
```

#### Training via gradient descent

PyTorch provides automatic differentiation, commonly used for gradient descent. Variables are created via `nn::VarStore` with shapes and initialisations.

```rust
use tch::nn::{Module, OptimizerConfig};
use tch::{kind, nn, Device, Tensor};

fn my_module(p: nn::Path, dim: i64) -> impl nn::Module {
    let x1 = p.zeros("x1", &[dim]);
    let x2 = p.zeros("x2", &[dim]);
    nn::func(move |xs| xs * &x1 + xs.exp() * &x2)
}

fn gradient_descent() {
    let vs = nn::VarStore::new(Device::Cpu);
    let my_module = my_module(vs.root(), 7);
    let mut opt = nn::Sgd::default().build(&vs, 1e-2).unwrap();
    for _idx in 1..50 {
        let xs = Tensor::zeros(&[7], kind::FLOAT_CPU);
        let ys = Tensor::zeros(&[7], kind::FLOAT_CPU);
        let loss = (my_module.forward(&xs) - ys).pow_tensor_scalar(2).sum(kind::Kind::Float);
        opt.backward_step(&loss);
    }
}
```

#### A simple neural network on MNIST

```rust
use anyhow::Result;
use tch::{nn, nn::Module, nn::OptimizerConfig, Device};

const IMAGE_DIM: i64 = 784;
const HIDDEN_NODES: i64 = 128;
const LABELS: i64 = 10;

fn net(vs: &nn::Path) -> impl Module {
    nn::seq()
        .add(nn::linear(vs / "layer1", IMAGE_DIM, HIDDEN_NODES, Default::default()))
        .add_fn(|xs| xs.relu())
        .add(nn::linear(vs, HIDDEN_NODES, LABELS, Default::default()))
}

pub fn run() -> Result<()> {
    let m = tch::vision::mnist::load_dir("data")?;
    let vs = nn::VarStore::new(Device::Cpu);
    let net = net(&vs.root());
    let mut opt = nn::Adam::default().build(&vs, 1e-3)?;
    for epoch in 1..200 {
        let loss = net
            .forward(&m.train_images)
            .cross_entropy_for_logits(&m.train_labels);
        opt.backward_step(&loss);
        let test_accuracy = net
            .forward(&m.test_images)
            .accuracy_for_logits(&m.test_labels);
        println!("epoch: {:4} train loss: {:8.5} test acc: {:5.2}%",
            epoch, f64::from(&loss), 100. * f64::from(&test_accuracy));
    }
    Ok(())
}
```

See the [detailed MNIST tutorial](https://github.com/LaurentMazare/tch-rs/tree/master/examples/mnist).

#### Pretrained models

The [pretrained-models example](https://github.com/LaurentMazare/tch-rs/tree/master/examples/pretrained-models/main.rs) shows how to use a pre-trained CV model on an image. Weights extracted from PyTorch implementations are downloadable, e.g. [resnet18.ot](https://github.com/LaurentMazare/tch-rs/releases/download/mw/resnet18.ot).

```rust
let image = imagenet::load_image_and_resize(image_file)?;
let vs = tch::nn::VarStore::new(tch::Device::Cpu);
let resnet18 = tch::vision::resnet::resnet18(vs.root(), imagenet::CLASS_COUNT);
vs.load(weight_file)?;
let output = resnet18.forward_t(&image.unsqueeze(0), /*train=*/ false).softmax(-1);
for (probability, class) in imagenet::top(&output, 5).iter() {
    println!("{:50} {:5.2}%", class, 100.0 * probability)
}
```

#### Importing pre-trained weights via SafeTensors

[`safetensors`](https://github.com/huggingface/safetensors) is HuggingFace's tensor format — no Python `pickle`, zero-copy loads. Export from PyTorch:

```python
import torchvision
from safetensors import torch as stt

model = torchvision.models.resnet18(pretrained=True)
stt.save_file(model.state_dict(), 'resnet18.safetensors')
```

The filename must end in `.safetensors` for `tch` to decode it. Then in Rust:

```rust
use anyhow::Result;
use tch::{Device, Kind, nn::VarStore, vision::{imagenet, resnet::resnet18}};

fn main() -> Result<()> {
    let mut vs = VarStore::new(Device::cuda_if_available());
    let model = resnet18(&vs.root(), 1000);
    vs.load("resnet18.safetensors")?;
    let image = imagenet::load_image_and_resize224("dog.jpg")?.to_device(vs.device());
    let output = image.unsqueeze(0).apply_t(&model, false).softmax(-1, Kind::Float);
    for (probability, class) in imagenet::top(&output, 5).iter() {
        println!("{:50} {:5.2}%", class, 100.0 * probability)
    }
    Ok(())
}
```

Further examples in the repo: [char-rnn](https://github.com/LaurentMazare/tch-rs/blob/master/examples/char-rnn), [neural style transfer](https://github.com/LaurentMazare/tch-rs/blob/master/examples/neural-style-transfer), [ResNet on CIFAR-10](https://github.com/LaurentMazare/tch-rs/tree/master/examples/cifar), [TorchScript JIT loading](https://github.com/LaurentMazare/tch-rs/tree/master/examples/jit), [reinforcement learning on OpenAI Gym](https://github.com/LaurentMazare/tch-rs/blob/master/examples/reinforcement-learning), [transfer learning](https://github.com/LaurentMazare/tch-rs/blob/master/examples/transfer-learning), [min-GPT](https://github.com/LaurentMazare/tch-rs/blob/master/examples/min-gpt), and [Stable Diffusion](https://github.com/LaurentMazare/diffusers-rs).

External material:
- [Vegapit tutorial](http://vegapit.com/article/how-to-use-torch-in-rust-with-tch-rs) — option pricing and greeks via Torch.
- [`tchrs-opencv-webcam-inference`](https://github.com/metobom/tchrs-opencv-webcam-inference) — `tch-rs` + OpenCV running mobilenet-v3 inference on a webcam feed.

### FAQ highlights

- **Python → Rust model translation best practices**: see [issue #549](https://github.com/LaurentMazare/tch-rs/issues/549#issuecomment-1296840898).
- **M1/M2 Mac**: see [issue #488](https://github.com/LaurentMazare/tch-rs/issues/488).
- **`torch-sys` rebuilt every cargo run**: usually rust-analyzer not seeing `LIBTORCH` / `LD_LIBRARY_PATH` — see [issue #596](https://github.com/LaurentMazare/tch-rs/issues/596).
- **Calling `tch` from Python**: possible via [PyO3](../../programming/rust/python/pyo3); [`tch-ext`](https://github.com/LaurentMazare/tch-ext) is a worked example.
- **Shared-library load errors at runtime** (e.g. `libtorch_cpu.so: cannot open shared object file`): point the dynamic loader at libtorch:

```sh
# For Linux
export LD_LIBRARY_PATH=/path/to/libtorch/lib:$LD_LIBRARY_PATH
# For macOS
export DYLD_LIBRARY_PATH=/path/to/libtorch/lib:$DYLD_LIBRARY_PATH
```

### Licence
`tch-rs` is dual-licensed under MIT and Apache-2.0.
