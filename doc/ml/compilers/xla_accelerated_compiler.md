# XLA

https://github.com/openxla/xla

**XLA (Accelerated Linear Algebra)** is an open-source machine learning (ML) compiler for GPUs, CPUs, and ML accelerators.

The XLA compiler takes models from popular ML frameworks such as PyTorch, TensorFlow, and JAX, and optimizes them for high-performance execution across different hardware platforms including GPUs, CPUs, and ML accelerators.

## Get started
If you want to use XLA to compile your ML project, refer to the corresponding documentation for your ML framework:

- PyTorch
- TensorFlow
- JAX



Clone this repository:
```shell
git clone https://github.com/openxla/xla && cd xla
```

We recommend using a suitable docker container to build/test XLA, such as TensorFlow's docker container:

```shell
docker run --name xla -w /xla -it -d --rm -v $PWD:/xla tensorflow/build:latest-python3.9 bash
```

Run an end to end test using an example StableHLO module:

```shell
docker exec xla ./configure

docker exec xla bazel test xla/examples/axpy:stablehlo_compile_test --nocheck_visibility --test_output=all
```

This will take quite a while your first time because it must build the entire stack, including MLIR, StableHLO, XLA, and more.
