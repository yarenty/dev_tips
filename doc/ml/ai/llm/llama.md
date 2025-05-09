# LLAMA


https://github.com/meta-llama



## Llama - stack



https://github.com/meta-llama/llama-stack

Llama Stack defines and standardizes the core building blocks that simplify AI application development. It codified best practices across the Llama ecosystem. More specifically, it provides

- Unified API layer for Inference, RAG, Agents, Tools, Safety, Evals, and Telemetry.
- Plugin architecture to support the rich ecosystem of implementations of the different APIs in different environments like local development, on-premises, cloud, and mobile.
- Prepackaged verified distributions which offer a one-stop solution for developers to get started quickly and reliably in any environment
- Multiple developer interfaces like CLI and SDKs for Python, Typescript, iOS, and Android
- Standalone applications as examples for how to build production-grade AI applications with Llama Stack

![](https://private-user-images.githubusercontent.com/19390/389183148-33d9576d-95ea-468d-95e2-8fa233205a50.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3Mzg3NzM2MDAsIm5iZiI6MTczODc3MzMwMCwicGF0aCI6Ii8xOTM5MC8zODkxODMxNDgtMzNkOTU3NmQtOTVlYS00NjhkLTk1ZTItOGZhMjMzMjA1YTUwLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTAyMDUlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUwMjA1VDE2MzUwMFomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWRjZTkwZmEwMmE2NmE0MmU0Zjg2MDFjOWJhZWVlN2U5NDE0MzViZjY3OGNkMWI3NzMyZWQ4MjAwMGU4MjFiZTAmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.i7wdEJOHDzMsOCKE6QA5Uw02GNQP4F4pIjQroNckWik)



Llama Stack Benefits
- Flexible Options: Developers can choose their preferred infrastructure without changing APIs and enjoy flexible deployment choice.
- Consistent Experience: With its unified APIs Llama Stack makes it easier to build, test, and deploy AI applications with consistent application behavior.
- Robust Ecosystem: Llama Stack is already integrated with distribution partners (cloud providers, hardware vendors, and AI-focused companies) that offer tailored infrastructure, software, and services for deploying Llama models.

### Stack Apps - EXAMPLES


https://github.com/meta-llama/llama-stack-apps

This repo shows examples of applications built on top of Llama Stack. Starting Llama 3.1 you can build agentic applications capable of:

- breaking a task down and performing multi-step reasoning.
- using tools to perform some actions
- built-in: the model has built-in knowledge of tools like search or code interpreter
- zero-shot: the model can learn to call tools using previously unseen, in-context tool definitions
- providing system level safety protections using models like Llama Guard.




## Cookbook


https://github.com/meta-llama/llama-cookbook

Welcome to the official repository for helping you get started with inference, fine-tuning and end-to-end use-cases of building with the Llama Model family.

This repository covers the most popular community approaches, use-cases and the latest recipes for Llama Text and Vision models.





## Code Llama

https://github.com/meta-llama/codellama

Code Llama is a family of large language models for code based on Llama 2 providing state-of-the-art performance among open models, infilling capabilities, support for large input contexts, and zero-shot instruction following ability for programming tasks. We provide multiple flavors to cover a wide range of applications: foundation models (Code Llama), Python specializations (Code Llama - Python), and instruction-following models (Code Llama - Instruct) with 7B, 13B and 34B parameters each. All models are trained on sequences of 16k tokens and show improvements on inputs with up to 100k tokens. 7B and 13B Code Llama and Code Llama - Instruct variants support infilling based on surrounding content. Code Llama was developed by fine-tuning Llama 2 using a higher sampling of code. As with Llama 2, we applied considerable safety mitigations to the fine-tuned versions of the model. For detailed information on model training, architecture and parameters, evaluations, responsible AI and safety refer to our research paper. Output generated by code generation features of the Llama Materials, including Code Llama, may be subject to third party licenses, including, without limitation, open source licenses.

We are unlocking the power of large language models and our latest version of Code Llama is now accessible to individuals, creators, researchers and businesses of all sizes so that they can experiment, innovate and scale their ideas responsibly. This release includes model weights and starting code for pretrained and fine-tuned Llama language models — ranging from 7B to 34B parameters.

This repository is intended as a minimal example to load Code Llama models and run inference.





## Llama 3 - deprecated

https://github.com/meta-llama/llama3


## Llama 2 - deprecated

https://github.com/meta-llama/llama



## CPP


https://github.com/ggml-org/llama.cpp


Inference of Meta's LLaMA model (and others) in pure C/C++