<div align="right">

[English](#modxai-studio) | [简体中文](#modxai-studio-zh)

</div>

# ModxAI Studio
<a id="modxai-studio"></a>

> Zero-setup local AI workstation — GUI desktop app with one-click environment install for everyone.

**ModxAI Studio** is a desktop application that turns a single machine into a local AI workstation. It bundles model management, AI chat, TTS, data processing, training, and Stable Diffusion–based image/video generation into one unified, GUI-driven tool — **without requiring users to configure Python environments, Docker containers, or command-line scripts**.

Official Site: [https://www.modxai.net/en](https://www.modxai.net/en) · 中文官网: [https://www.modxai.net/zh](https://www.modxai.net/zh)

---

# Documentation Preview:
https://modxai.github.io

---

## Key Features

- **AI Chat (Text & Multimodal)**  
  Local LLM chat powered by `llama.cpp`, with support for text Q&A, code generation, multimodal (image+text) queries, system prompts, RAG retrieval, and high-quality conversation collection for later training.

- **TTS Speech Synthesis**  
  Local text-to-speech with Chinese/English bilingual support, multiple voice presets, speed adjustment, and automatic language detection.

- **Stable Diffusion Chat (SD Chat)**  
  Conversational interface for text-to-image, image-to-image, and video generation. Per-conversation parameter profiles (resolution, steps, CFG, seeds, samplers), negative-prompt templates, and parameter recommendations.

- **Model Library**  
  Unified management for chat, multimodal, embedding, rerank and SD models. Supports one-click download from Hugging Face / ModelScope, local import of GGUF/SafeTensors, and LoRA adapters.

- **Datasets & Pipelines (SFT / RAG / Audio2Text)**  
  Pipeline-style data processing: convert, clean, split, analyze, AI-synthesize, vectorize and output standardized SFT or RAG datasets. Includes audio-to-text transcription with resumable flows.

- **Model Training (LoRA & Fine-tuning)**  
  End-to-end training workflow: preparation, real-time monitor, evaluation, interactive test, and GGUF packaging. LoRA keeps VRAM requirements low (as little as 8GB).

- **One-Click Environment Setup**  
  Built-in environment manager with sandboxed portable Python, CPU/GPU variants, mirror selection, and resumable installs. **No Docker, no manual Python setup, no command line required.**

---

## Why ModxAI Studio

| Advantage | Description |
| --------- | ----------- |
| **Zero Technical Barrier** | GUI-first design. No Docker, no Python config, no command line. Just download, install, and use. |
| **One-Click Environment** | Sandboxed, portable Python with on-demand dynamic dependencies. Does not pollute your system. |
| **Local-First & Privacy** | Models, datasets and chat history stay on your machine. No cloud required. |
| **All-in-One Workflow** | Chat, TTS, SD image/video, RAG, training, model management — unified in one app. |
| **Offline Ready** | No account needed. Fully functional offline after initial setup. |
| **Resource Efficient** | Quantized models run on regular PCs. LoRA training with 8GB VRAM. |

---

## Documentation

This repository hosts the official documentation for ModxAI Studio.

- Documentation (English): `docs/en/`
- Documentation (简体中文): `docs/zh/`

Key sections include:

- **Getting Started**: Introduction and 5-minute quick start
- **Features · Chat**: AI chat, prompt optimization, personalized training data
- **Features · TTS**: Text-to-speech synthesis
- **Features · SD Chat**: Stable Diffusion image and video generation
- **Features · Dataset**: SFT data, RAG knowledge bases, audio-to-text
- **Features · Models**: Model library, downloads, LoRA, SD models
- **Features · Training**: Prepare, monitor, evaluate, test, package
- **Features · Settings**: Environment setup, license, global options
- **FAQ**: Common problems and troubleshooting

---

## Acknowledgements

ModxAI Studio is built on top of a rich open-source ecosystem. Special thanks to: [PyTorch](https://github.com/pytorch/pytorch), [llama.cpp](https://github.com/ggml-org/llama.cpp), [Stable Diffusion.cpp](https://github.com/leejet/stable-diffusion.cpp), [Transformers](https://github.com/huggingface/transformers), [TRL](https://github.com/huggingface/trl), [PEFT](https://github.com/huggingface/peft), [faster-whisper](https://github.com/SYSTRAN/faster-whisper), [Electron](https://github.com/electron/electron), [Vue 3](https://github.com/vuejs/core), [FastAPI](https://github.com/tiangolo/fastapi), and many more.

---

<a id="modxai-studio-zh"></a>

# ModxAI Studio（中文）

> 零门槛本地 AI 工作站 — 一键环境安装，GUI 桌面应用，面向所有人。

**ModxAI Studio** 是一款面向普通用户的本地 AI 一站式解决方案。它将大语言模型对话、TTS 语音合成、Stable Diffusion 图像/视频生成、数据处理和模型训练整合到一个简洁易用的桌面应用中 — **无需配置 Python 环境、Docker 容器或命令行操作**。

官网（英文）：[https://www.modxai.net/en](https://www.modxai.net/en) · 官网（中文）：[https://www.modxai.net/zh](https://www.modxai.net/zh)

---

# 文档预览:
https://modxai.github.io

---

## 核心功能

- **AI 对话（文本 & 多模态）**  
  基于 `llama.cpp` 的本地大模型对话，支持文本问答、代码生成、多模态图文理解、系统提示词、RAG 检索增强，以及高质量对话收藏用于后续训练。

- **TTS 语音合成**  
  本地文本转语音，支持中英双语、多音色选择、语速调节、自动语言检测。

- **SD 图像/视频生成**  
  以会话方式完成文生图、图生图和视频生成；每个会话拥有独立的参数配置，支持负面提示词模板。

- **模型库管理**  
  统一管理聊天、多模态、Embedding、Rerank 与 SD 模型；支持从 Hugging Face / ModelScope 一键下载，本地导入 GGUF / SafeTensors，挂载 LoRA 适配器。

- **数据处理流水线（SFT / RAG / 语音转写）**  
  流水线式数据处理：解析、清洗、切分、AI 合成、向量化，导出规范的 SFT 或 RAG 数据集；支持音频转文本、断点续跑。

- **模型训练（LoRA 微调）**  
  覆盖从任务配置、训练监控、Checkpoint 评估、交互测试到 GGUF 打包的完整流程。LoRA 训练仅需 8GB 显存即可开始。

- **一键环境安装**  
  内置环境管理器，提供沙盒化的便携 Python 环境，区分 CPU / GPU 方案，支持镜像源选择和断点续装。**无需 Docker、无需手动配置 Python、无需命令行。**

---

## 为什么选择 ModxAI Studio

| 优势 | 说明 |
| ---- | ---- |
| **零技术门槛** | 图形界面优先。无需 Docker、无需配置 Python、无需命令行，下载安装即用。 |
| **一键环境配置** | 沙盒隔离的便携 Python，按需动态加载依赖，不污染系统环境。 |
| **本地优先 & 隐私安全** | 模型、数据集与对话记录保存在本机，无需云端。 |
| **功能全面一站式** | 对话、TTS、SD 图像/视频、RAG、训练、模型管理统一在一个应用内。 |
| **离线可用** | 无需账号注册，下载完成后可完全离线使用。 |
| **资源高效** | 支持量化模型，普通电脑可运行；LoRA 训练仅需 8GB 显存。 |

---

## 文档

本仓库托管 ModxAI Studio 的官方文档，支持中英文双语：

- 英文文档：`docs/en/`
- 中文文档：`docs/zh/`

主要内容包括：

- **快速入门**：产品简介与 5 分钟上手
- **功能说明**：对话、TTS、SD 生成、模型库、数据处理、训练、设置
- **FAQ**：常见问题与故障排查

---

## 致谢

ModxAI Studio 构建在丰富的开源生态之上，特别感谢：[PyTorch](https://github.com/pytorch/pytorch)、[llama.cpp](https://github.com/ggml-org/llama.cpp)、[Stable Diffusion.cpp](https://github.com/leejet/stable-diffusion.cpp)、[Transformers](https://github.com/huggingface/transformers)、[TRL](https://github.com/huggingface/trl)、[PEFT](https://github.com/huggingface/peft)、[faster-whisper](https://github.com/SYSTRAN/faster-whisper)、[Electron](https://github.com/electron/electron)、[Vue 3](https://github.com/vuejs/core)、[FastAPI](https://github.com/tiangolo/fastapi) 等开源项目与社区。

---

<div align="center">

**ModxAI Studio** — Making local AI accessible to everyone.

</div>
