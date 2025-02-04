<div align="center">
  <img src="docs/en/_static/image/lmdeploy-logo.svg" width="450"/>

[![docs](https://img.shields.io/badge/docs-latest-blue)](https://lmdeploy.readthedocs.io/en/latest/)
[![badge](https://github.com/InternLM/lmdeploy/workflows/lint/badge.svg)](https://github.com/InternLM/lmdeploy/actions)
[![PyPI](https://img.shields.io/pypi/v/lmdeploy)](https://pypi.org/project/lmdeploy)
[![license](https://img.shields.io/github/license/InternLM/lmdeploy.svg)](https://github.com/InternLM/lmdeploy/tree/main/LICENSE)
[![issue resolution](https://img.shields.io/github/issues-closed-raw/InternLM/lmdeploy)](https://github.com/InternLM/lmdeploy/issues)
[![open issues](https://img.shields.io/github/issues-raw/InternLM/lmdeploy)](https://github.com/InternLM/lmdeploy/issues)

English | [简体中文](README_zh-CN.md)

</div>

<p align="center">
    👋 join us on <a href="https://twitter.com/intern_lm" target="_blank">Twitter</a>, <a href="https://discord.gg/xa29JuW87d" target="_blank">Discord</a> and <a href="https://r.vansin.top/?r=internwx" target="_blank">WeChat</a>
</p>

______________________________________________________________________

## Latest News 🎉

- \[2023/12\] Turbomind supports multimodal input. [Gradio Demo](./examples/vl/README.md)
- \[2023/11\] Turbomind supports loading hf model directly. Click [here](docs/en/inference/load_hf.md) for details.
- \[2023/11\] TurboMind major upgrades, including: Paged Attention, faster attention kernels without sequence length limitation, 2x faster KV8 kernels, Split-K decoding (Flash Decoding), and W4A16 inference for sm_75
- \[2023/09\] TurboMind supports Qwen-14B
- \[2023/09\] TurboMind supports InternLM-20B
- \[2023/09\] TurboMind supports all features of Code Llama: code completion, infilling, chat / instruct, and python specialist. Click [here](./docs/en/supported_models/codellama.md) for deployment guide
- \[2023/09\] TurboMind supports Baichuan2-7B
- \[2023/08\] TurboMind supports flash-attention2.
- \[2023/08\] TurboMind supports Qwen-7B, dynamic NTK-RoPE scaling and dynamic logN scaling
- \[2023/08\] TurboMind supports Windows (tp=1)
- \[2023/08\] TurboMind supports 4-bit inference, 2.4x faster than FP16, the fastest open-source implementation🚀. Check [this](docs/en/quantization/w4a16.md) guide for detailed info
- \[2023/08\] LMDeploy has launched on the [HuggingFace Hub](https://huggingface.co/lmdeploy), providing ready-to-use 4-bit models.
- \[2023/08\] LMDeploy supports 4-bit quantization using the [AWQ](https://arxiv.org/abs/2306.00978) algorithm.
- \[2023/07\] TurboMind supports Llama-2 70B with GQA.
- \[2023/07\] TurboMind supports Llama-2 7B/13B.
- \[2023/07\] TurboMind supports tensor-parallel inference of InternLM.

______________________________________________________________________

# Introduction

LMDeploy is a toolkit for compressing, deploying, and serving LLM, developed by the [MMRazor](https://github.com/open-mmlab/mmrazor) and [MMDeploy](https://github.com/open-mmlab/mmdeploy) teams. It has the following core features:

- **Efficient Inference Engine (TurboMind)**: It develops key features like persistent batch(a.k.a. continuous batching), blocked KV cache, dynamic split&fuse, tensor parallelism, high-performance CUDA kernels and so on, ensuring the high throughput and low latency during LLMs inference.

- **Interactive Inference Mode**: By caching the k/v of attention during multi-round dialogue processes, the engine remembers dialogue history, thus avoiding repetitive processing of historical sessions.

- **Quantization**: LMDeploy supports various quantization methods and efficient inference of quantized models. The reliability of quantization has been verified on models of different scales.

# Performance

The TurboMind engine achieves up to 1.36 ~ 1.85 times higher request throughput compared to vLLM across models of various size. In terms of static inference capabilities, the token throughput (`out token/s`) of TurboMind's 4bit model inference significantly outperforms FP16/BF16 inference, with an improvement of up to 2.4 times.

![v0 1 0-benchmark](https://github.com/InternLM/lmdeploy/assets/4560679/8e455cf1-a792-4fa8-91a2-75df96a2a5ba)

For inference benchmarks in more devices and more settings, please refer to the following link:

- [A100](./docs/en/benchmark/a100_fp16.md)
- 4090
- 3090
- 2080

# Supported Models

`LMDeploy` has developed two inference engines - `Pytorch` and `TurboMind`, each with a different focus. The former strives for ultimate optimization of inference performance, while the latter, developed purely in Python, aims to decrease the barriers for developers.

As shown in the next tables, the inference engines differ in the types of supported models and the inference data type. Users can choose the one that best fits their actual needs.

## TurboMind

|       Model        |   Size   | FP16/BF16 | KV INT8 | W4A16 |
| :----------------: | :------: | :-------: | :-----: | :---: |
|       Llama        | 7B - 65B |    Yes    |   Yes   |  Yes  |
|       Llama2       | 7B - 70B |    Yes    |   Yes   |  Yes  |
|      InternLM      | 7B - 20B |    Yes    |   Yes   |  Yes  |
| InternLM-XComposer |    7B    |    Yes    |   Yes   |  Yes  |
|        QWen        | 7B - 72B |    Yes    |   Yes   |  Yes  |
|      QWen-VL       |    7B    |    Yes    |   Yes   |  Yes  |
|      Baichuan      |    7B    |    Yes    |   Yes   |  Yes  |
|     Baichuan2      |    7B    |    Yes    |   Yes   |  Yes  |
|     Code Llama     | 7B - 34B |    Yes    |   No    |  No   |

## Pytorch

|   Model   |   Size    | FP16/BF16 | KV INT8 | W8A8 |
| :-------: | :-------: | :-------: | :-----: | :--: |
|   Llama   | 7B - 65B  |    Yes    |   No    | Yes  |
|  Llama2   | 7B - 70B  |    Yes    |   No    | Yes  |
| InternLM  | 7B - 20B  |    Yes    |   No    | Yes  |
| Baichuan2 | 7B - 13B  |    Yes    |   No    | Yes  |
| ChatGLM2  |    6B     |    Yes    |   No    |  No  |
|  Falcon   | 7B - 180B |    Yes    |   No    |  No  |

# Getting Started

Please overview [getting_started](./docs/en/get_started.md) section for the basic usage of LMDeploy.

For detailed user guides and advanced guides, please refer to our [tutorials](https://lmdeploy.readthedocs.io/en/latest/):

- User Guide
  - [Inference pipeline](./docs/en/inference/pipeline.md)
  - [Inference Engine - TurboMind](docs/en/inference/turbomind.md)
  - Inference Engine - PyTorch
  - [Serving](docs/en/serving/restful_api.md)
  - [Quantization](docs/en/quantization)
- Advance Guide
  - Add chat template
  - Add a new model
  - gemm tuning
  - Long context inference

## Contributing

We appreciate all contributions to LMDeploy. Please refer to [CONTRIBUTING.md](.github/CONTRIBUTING.md) for the contributing guideline.

## Acknowledgement

- [FasterTransformer](https://github.com/NVIDIA/FasterTransformer)
- [llm-awq](https://github.com/mit-han-lab/llm-awq)
- [vLLM](https://github.com/vllm-project/vllm)
- [DeepSpeed-MII](https://github.com/microsoft/DeepSpeed-MII)

## License

This project is released under the [Apache 2.0 license](LICENSE).
