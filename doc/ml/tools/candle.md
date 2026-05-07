---
title: Candle — HuggingFace's minimalist Rust ML framework
main_link: https://github.com/huggingface/candle
keywords: [candle, huggingface, rust, ml, inference, serverless, cuda, wasm, transformers]
status: reviewed
review_date: 2026/05/03
---

# Candle — HuggingFace's minimalist Rust ML framework

**Main link:** <https://github.com/huggingface/candle>

## Summary
Candle is HuggingFace's minimalist machine-learning framework for Rust, with a deliberately PyTorch-like API and a focus on **performance and lightweight serverless deployment**. It targets CPU (with optional MKL on x86 and Apple Accelerate on Mac), CUDA (single GPU and multi-GPU via NCCL), and WASM (so a single model can run server-side or in a browser tab). The companion `candle-transformers` crate ships ready-made implementations of LLaMA / Mistral / Mixtral / Phi / Gemma / Whisper / Stable Diffusion / SAM / YOLO / many more — making Candle the practical answer to "I want to deploy an HF model from Rust without Python".

## Insight
Reach for Candle when the goal is **production inference of HuggingFace models from a small, fast Rust binary**. The two stated goals — *make serverless inference possible* (small cold-start, no Python interpreter) and *remove Python from production workloads* (no GIL, simpler deploys) — drive every design choice. The cheatsheet of PyTorch-vs-Candle ops in the README is the fastest way to read the API: tensors, ops, devices, dtypes, and `safetensors` save/load all map almost line-for-line.

Vs the Rust-DL neighbours:
- **vs [[burn]]** — Burn is more general (training-first, swappable backend trait, ONNX importer, Ratatui training UI). Candle is lighter, more PyTorch-ergonomic, and inference-shaped. Notably, Candle is *also* available as a Burn backend (`burn-candle`).
- **vs [[tch]]** — `tch` is the easiest path to "I have a libtorch model and want to call it from Rust", at the cost of a 200+ MB libtorch dependency and tight ABI pinning. Candle reimplements ops in Rust + small CUDA kernels, so the binary is small and there's no libtorch to install.
- **vs [[dfdx]]** — `dfdx` checks tensor shapes at compile time via const generics; Candle uses runtime shapes for ergonomics and a much wider model zoo. Candle's main contributor (Laurent Mazare) is also the author of `tch-rs`, so the design choices are deliberate trade-offs against that experience.
- **vs [[tract]]** — `tract` is inference-only and format-driven (ONNX/NNEF in, no model code); Candle gives you the actual Rust model code, a wider op set, GPU support, and training (though training is not the primary use case).

The real-world ecosystem matters: `candle-vllm` for OpenAI-compatible local serving; `candle-lora` for LoRA fine-tuning; `kalosm` as a higher-level multi-modal meta-framework built on Candle; `candle-flash-attn` for flash-attention v2; quantised LLaMA using llama.cpp's quantisation types. Candle is also a downstream dependency in unexpected places — for example [[../../programming/rust/data/datafusion/xorq|xorq]] embeds Candle for in-process model execution.

Gotchas: WSL model loads from `/mnt/c` are pathologically slow (use `~`); MKL/Accelerate features need the right `extern crate intel_mkl_src;` / `extern crate accelerate_src;` link incantation; flash-attention compilation requires the `cutlass` git submodule and a non-gcc-11 compiler (set `NVCC_CCBIN`); Windows CUDA needs three DLLs renamed (`nvcuda.dll` → `cuda.dll`, plus cuBLAS and cuRAND); LLaMA-2 weights are gated by Meta's licence, so you need to accept terms on HuggingFace and configure an auth token.

## Similar / related topics
- [[burn]] — pure-Rust DL framework, training-friendly, more general; uses Candle as one of its backends.
- [[tch]] — Rust bindings to libtorch; heavier and tighter PyTorch parity.
- [[dfdx]] — pure-Rust DL with compile-time shape checking; smaller scope.
- [[tract]] — Sonos's pure-Rust ONNX inference engine; CPU-only, no model code.
- [PyTorch](https://pytorch.org/) — the framework Candle's API openly mirrors.
- [[../../programming/rust/data/datafusion/xorq|xorq]] — DataFusion-based engine that embeds Candle for in-process inference.

## Internal links
- [[burn]]
- [[tch]]
- [[dfdx]]
- [[tract]]
- [[../llm/models/llama|llama]]
- [[../../programming/rust/data/datafusion/xorq|xorq]]
- [[../../programming/rust/ml/ml_in_rust|programming/rust/ml/ml_in_rust]]

## Keywords
`#candle` `#huggingface` `#rust` `#ml` `#inference` `#serverless` `#cuda` `#wasm` `#transformers`

## References / raw notes

Candle is a minimalist ML framework for Rust with a focus on performance (including GPU support) and ease of use. Try the online demos: [whisper](https://huggingface.co/spaces/lmz/candle-whisper), [LLaMA2](https://huggingface.co/spaces/lmz/candle-llama2), [T5](https://huggingface.co/spaces/radames/Candle-T5-Generation-Wasm), [yolo](https://huggingface.co/spaces/lmz/candle-yolo), [Segment Anything](https://huggingface.co/spaces/radames/candle-segment-anything-wasm).

### Get started

Install [`candle-core`](https://github.com/huggingface/candle/tree/main/candle-core) per the [installation guide](https://huggingface.github.io/candle/guide/installation.html). A minimal matrix-multiplication example:

```rust
use candle_core::{Device, Tensor};

fn main() -> Result<(), Box<dyn std::error::Error>> {
    let device = Device::Cpu;
    let a = Tensor::randn(0f32, 1., (2, 3), &device)?;
    let b = Tensor::randn(0f32, 1., (3, 4), &device)?;
    let c = a.matmul(&b)?;
    println!("{c}");
    Ok(())
}
```

`cargo run` should print a tensor of shape `Tensor[[2, 4], f32]`. To switch to GPU after installing with CUDA support:

```diff
- let device = Device::Cpu;
+ let device = Device::new_cuda(0)?;
```

### Examples and model zoo

Browser demos: yolo (pose + object detection), whisper (speech-to-text), LLaMA2, T5, Phi-1.5/Phi-2, Segment Anything, BLIP image captioning.

Command-line examples cover a representative slice of modern open models:

- **LLMs**: LLaMA v1/v2/v3 (incl. SOLAR-10.7B), Falcon, Gemma 2b/7b, RecurrentGemma, Phi-1/1.5/2/3, StableLM-3B-4E1T, Mamba, Mistral 7B, Mixtral 8x7B, StarCoder/StarCoder2, Qwen1.5, RWKV v5/v6, Replit-code-v1.5, Yi-6B/Yi-34B, **Quantized LLaMA** (using llama.cpp's quantisation).
- **Image generation**: Stable Diffusion 1.5/2.1/SDXL/Turbo, Wuerstchen.
- **Vision**: yolo-v3, yolo-v8, Segment Anything (SAM), SegFormer, DINOv2, VGG, RepVGG, ConvMixer, EfficientNet, ResNet, ViT, ConvNeXT/v2, MobileOne, EfficientVit, Moondream.
- **Audio**: Whisper, EnCodec, MetaVoice.
- **Embeddings**: T5, BERT, JinaBERT.
- **Image-to-text / multimodal**: BLIP (captioning), TrOCR, CLIP, Marian-MT.

Run with e.g. `cargo run --example quantized --release`. Add `--features cuda` for GPU; `--features cudnn` for additional speedups.

WASM examples (whisper, [llama2.c](https://github.com/karpathy/llama2.c), T5, Phi-1.5/Phi-2, SAM) build with `trunk`. For LLaMA2 in browser:

```sh
cd candle-wasm-examples/llama2-c
wget https://huggingface.co/spaces/lmz/candle-llama2/resolve/main/model.bin
wget https://huggingface.co/spaces/lmz/candle-llama2/resolve/main/tokenizer.json
trunk serve --release --port 8081
```

### Useful external libraries

- [`candle-tutorial`](https://github.com/ToluClassics/candle-tutorial) — detailed PyTorch → Candle conversion walkthrough.
- [`candle-lora`](https://github.com/EricLBuehler/candle-lora) — efficient LoRA implementation with built-in support for many Candle models ([model list](https://github.com/EricLBuehler/candle-lora/tree/master/candle-lora-transformers/examples)).
- [`candle-vllm`](https://github.com/EricLBuehler/candle-vllm) — efficient LLM serving with an OpenAI-compatible API.
- [`optimisers`](https://github.com/KGrewal1/optimisers) — SGD-momentum, AdaGrad, AdaDelta, AdaMax, NAdam, RAdam, RMSprop.
- [`candle-ext`](https://github.com/mokeyish/candle-ext) — PyTorch-style ops not yet in Candle.
- [`kalosm`](https://github.com/floneum/floneum/tree/master/interfaces/kalosm) — multi-modal meta-framework built on Candle (controlled generation, custom samplers, in-memory vector DBs, audio transcription, …).
- [`candle-sampling`](https://github.com/EricLBuehler/candle-sampling) — sampling techniques.
- [`candle-coursera-ml`](https://github.com/vishpat/candle-coursera-ml) — Coursera ML Specialization in Candle.
- [`gpt-from-scratch-rs`](https://github.com/jeroenvlek/gpt-from-scratch-rs) — Karpathy's *Let's build GPT* ported to Candle.
- [`candle-einops`](https://github.com/tomsanbear/candle-einops) — pure-Rust port of [einops](https://github.com/arogozhnikov/einops).

### Features

- PyTorch-like syntax; embed user-defined ops/kernels (e.g. [flash-attention v2](https://github.com/huggingface/candle/blob/89ba005962495f2bfbda286e185e9c3c7f5300a3/candle-flash-attn/src/lib.rs#L152)).
- Backends: optimised CPU (with MKL on x86, Accelerate on Mac), CUDA (single + multi-GPU via NCCL), WASM.
- File formats: load from safetensors, npz, ggml, or PyTorch `.pt`.
- Quantisation: llama.cpp's quantised types.
- Designed for serverless (CPU), small fast deployments.

### How to use — PyTorch ↔ Candle cheatsheet

|            | PyTorch                                  | Candle                                                                        |
|------------|------------------------------------------|-------------------------------------------------------------------------------|
| Creation   | `torch.Tensor([[1, 2], [3, 4]])`         | `Tensor::new(&[[1f32, 2.], [3., 4.]], &Device::Cpu)?`                         |
| Creation   | `torch.zeros((2, 2))`                    | `Tensor::zeros((2, 2), DType::F32, &Device::Cpu)?`                            |
| Indexing   | `tensor[:, :4]`                          | `tensor.i((.., ..4))?`                                                        |
| Operations | `tensor.view((2, 2))`                    | `tensor.reshape((2, 2))?`                                                     |
| Operations | `a.matmul(b)`                            | `a.matmul(&b)?`                                                               |
| Arithmetic | `a + b`                                  | `&a + &b`                                                                     |
| Device     | `tensor.to(device="cuda")`               | `tensor.to_device(&Device::new_cuda(0)?)?`                                    |
| Dtype      | `tensor.to(dtype=torch.float16)`         | `tensor.to_dtype(&DType::F16)?`                                               |
| Saving     | `torch.save({"A": A}, "model.bin")`      | `candle::safetensors::save(&HashMap::from([("A", A)]), "model.safetensors")?` |
| Loading    | `weights = torch.load("model.bin")`      | `candle::safetensors::load("model.safetensors", &device)`                     |

### Crate structure

- `candle-core` — core ops, devices, `Tensor`.
- `candle-nn` — building blocks (Linear, Conv, attention, etc.).
- `candle-examples` — runnable examples.
- `candle-kernels` — custom CUDA kernels.
- `candle-datasets` — data loaders.
- `candle-transformers` — model implementations (LLaMA, Mistral, Whisper, …).
- `candle-flash-attn` — flash-attention v2.
- `candle-onnx` — ONNX model evaluation.

### Common errors (FAQ excerpts)

- **Missing symbols when compiling with the `mkl` feature** (or `accelerate` on macOS): add `extern crate intel_mkl_src;` (or `extern crate accelerate_src;`) at the top of your binary to force the link.
- **`status code 401` loading LLaMA examples**: you need to accept the [LLaMA-v2 model conditions](https://huggingface.co/meta-llama/Llama-2-7b-hf) on HuggingFace and configure your auth token. See [issue #350](https://github.com/huggingface/candle/issues/350).
- **`fatal error: cute/algorithm/copy.hpp: No such file or directory`** when compiling `flash-attn`: `cutlass` is provided as a git submodule. Run `git submodule update --init`.
- **flash-attention compile errors with gcc-11** (`parameter packs not expanded with '...'`): a CUDA-with-gcc-11 bug; install gcc-10 and set `NVCC_CCBIN=/usr/lib/gcc/x86_64-linux-gnu/10`.
- **Windows linker error `LNK1181: cannot open input file 'windows.0.48.5.lib'`** when running rustdoc / mdbook tests: explicitly link the relevant native libraries via `-L native=...windows_x86_64_msvc-0.48.5\lib` (and similar for older versions).
- **Extremely slow model load on WSL**: caused by loading from `/mnt/c`. Move the model into the Linux filesystem; see [this StackOverflow answer](https://stackoverflow.com/questions/68972448/why-is-wsl-extremely-slow-when-compared-with-native-windows-npm-yarn-processing).
- **Tracking down errors**: set `RUST_BACKTRACE=1`.
- **`LoadLibraryExW { source: Os { code: 126 } }` on Windows CUDA**: rename the three CUDA DLLs onto your `PATH`:
  - `c:\Windows\System32\nvcuda.dll` → `cuda.dll`
  - `c:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v12.4\bin\cublas64_12.dll` → `cublas.dll`
  - `c:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v12.4\bin\curand64_10.dll` → `curand.dll`
