<div align="right">

[English](#modxai-studio) | [简体中文](#modxai-studio-zh)

</div>

# ModxAI Studio
<a id="modxai-studio"></a>

> Run local models, agents, chat, and LoRA training on your own machine.

**ModxAI Studio** is a desktop app for local models, agents, chat, datasets, and LoRA training.

Install the Windows package: NSIS x64 or MSIX Lite. These pages describe that desktop. Daily chat uses the bundled llama.cpp. Install the sandboxed Python environment only when a feature asks for it. You do not need Docker, a system Python, or shell scripts to start.

Packaging scripts also build an arm64 macOS DMG, plus an arm64 Linux AppImage and deb. Those builds are not a formal release and do not match the Windows behavior documented here. Intel Mac and Linux x64 are not this product path. See the [download page](https://www.modxai.net/en/download) for files currently offered.

These docs describe product behavior through **v1.1.11** (2026-10-27).

- Product site: [https://www.modxai.net/en](https://www.modxai.net/en)
- Documentation site: [https://www.modxai.net/en/docs](https://www.modxai.net/en/docs)
- Release notes: [https://www.modxai.net/en/changelog](https://www.modxai.net/en/changelog)
- Docs preview (GitHub Pages): [https://modxai.github.io](https://modxai.github.io)

---

## Key Features

- **Autonomous Agent**  
  Plans a task, calls tools, and reports progress. The built-in toolchain covers web search, a headless browser, local file read/write, and terminal commands, and it can load external skills. The runtime is tuned so small local models (about 1B–7B, 32K context) can finish ordinary multi-step work. Online models are available when you want them.

- **AI Chat (Text and Multimodal)**  
  Local chat through llama.cpp, plus Hugging Face backends when you install an environment. Supports system prompts, thinking depth, RAG over your knowledge bases, and bookmarking conversations for later training.

- **Memory**  
  Review agent memories and conversations you bookmark. Edit, archive, export to SFT or RAG, and feed them into memory fine-tuning.

- **Scheduled Tasks**  
  Run an agent prompt, organize memory, fine-tune on memory, or refresh a knowledge base on an interval, at a fixed time, or after an upstream task finishes.

- **External Access**  
  Reach the local agent from Telegram, Discord, DingTalk, or Feishu. Pairing is required before a chat can send tasks. The same local gateway exposes an HTTP API for this machine or the LAN, with curl, Python, and Node examples in the repo.

- **Speech**  
  Local TTS for Chinese and English, with voice presets, speed control, and language detection. Open it from the agent submenu, next to SD Chat. STT transcribes microphone input and audio datasets.

- **Image and Video (SD Chat)**  
  Text-to-image, image-to-image, and video generation in a conversation. Each chat keeps its own resolution, steps, CFG, seed, and sampler profile. Open it from the agent submenu.

- **Model Library**  
  Chat, multimodal, embedding, rerank, SD, TTS, and STT models. Download from Hugging Face, ModelScope, or a repository you specify, or import local GGUF and SafeTensors files. LoRA adapters, an OpenAI-compatible LAN API, and online providers (DeepSeek, OpenRouter, and other OpenAI-compatible APIs) sit in the same library.

- **Datasets (SFT / RAG / Audio to Text)**  
  Parse, clean, split, synthesize, and vectorize data into SFT, pretrain, DPO, or RAG sets. Structured JSON/JSONL can be field-mapped. Audio transcription can resume after an interruption.

- **Training**  
  Prepare a LoRA run, watch it, evaluate checkpoints (including IFEval, GSM8K, MMLU, and HellaSwag), test interactively, and export GGUF. LoRA training can start around 8GB of VRAM.

- **One-Click Environment**  
  First launch offers an onboarding and environment guide. Install Full, Agent, or Speech capabilities into a portable sandbox, with mirror selection and resume. Basic llama.cpp chat does not need that install.

---

## Why ModxAI Studio

| Advantage | Description |
| --------- | ----------- |
| **Graphical from the start** | Download, install, and use. No Docker, system Python, or command line for the main path. |
| **Local by default** | Models, datasets, chats, and memories stay on the machine. Online APIs are optional. |
| **Small models can do tasks** | The agent keeps state and retries so 1B–7B models can work inside a 32K window. |
| **One workflow** | Data, training, inference, memory, schedules, and channels live in one app. |
| **Sandbox** | The portable environment and agent tools stay isolated from the rest of the system. |
| **Works offline after setup** | No account is required. After models and environments are in place, local features do not need a network. |

---

## Documentation

This repository is the source for the official manual. Pushing `docs/**` to `main` asks [modxai.github.io](https://github.com/ModxAI/modxai.github.io) to sync.

- English: `docs/en/`
- 简体中文: `docs/zh/`

The site index is `docs/<lang>/_index.md`. Pages are plain Markdown.

| Section | What it covers |
| ------- | -------------- |
| Getting Started | What the app is, requirements, first chat |
| Agent | Roles, tools, workspace browser, presets, auto-approval, voice input |
| AI Chat | Text, multimodal, RAG, prompt generation, inference options |
| Memory | Browse, edit, export |
| Scheduled Tasks | Agent jobs, memory organization, fine-tuning, knowledge-base refresh |
| External Access | Bots on Telegram, Discord, DingTalk, and Feishu, plus the local gateway HTTP API |
| Model Library | Import, download, load, LAN API, resource monitor |
| Data Processing | SFT, RAG, audio to text |
| Training | Prepare, monitor, evaluate, test, quantize |
| SD Chat | Image and video |
| TTS | Speech synthesis |
| Settings | Environment install, language, updates |
| FAQ | Setup and troubleshooting |

Documented minimums for the Windows desktop: **Windows 10/11 (64-bit), 16GB RAM, 20GB free disk, AVX2**. Training and image generation on that desktop work more comfortably with an NVIDIA GPU of 8GB VRAM or more. Chat can run on CPU.

On the arm64 macOS path, the environment check records a unified-memory GPU as information. Python dependencies use the CPU environment. Inference uses Metal.

---

## Acknowledgements

ModxAI Studio is built on open-source projects, including [PyTorch](https://github.com/pytorch/pytorch), [llama.cpp](https://github.com/ggml-org/llama.cpp), [Stable Diffusion.cpp](https://github.com/leejet/stable-diffusion.cpp), [Transformers](https://github.com/huggingface/transformers), [TRL](https://github.com/huggingface/trl), [PEFT](https://github.com/huggingface/peft), [faster-whisper](https://github.com/SYSTRAN/faster-whisper), [Playwright](https://github.com/microsoft/playwright), [Electron](https://github.com/electron/electron), [Vue 3](https://github.com/vuejs/core), and [FastAPI](https://github.com/tiangolo/fastapi).

---

<a id="modxai-studio-zh"></a>

# ModxAI Studio（中文）

> 在本机运行本地模型、智能体、对话和 LoRA 训练。

**ModxAI Studio** 是用于本地模型、智能体、对话、数据集和 LoRA 训练的桌面应用。

安装 Windows 包：NSIS x64 或 MSIX Lite。本文描述的是这个桌面。日常对话使用内置 llama.cpp。只有功能要求时才安装沙箱 Python 环境。开始使用不需要 Docker、系统 Python 或命令行脚本。

打包脚本还会构建 arm64 macOS DMG，以及 arm64 Linux 的 AppImage 和 deb。这些构建不是正式发布，也不按本文的 Windows 行为对照。Intel Mac 和 Linux x64 不在这条产品路径上。当前提供的文件见[下载页](https://www.modxai.net/zh/download)。

本文档描述的是 **v1.1.11**（2026-10-27）为止的产品行为。

- 产品官网：[https://www.modxai.net/zh](https://www.modxai.net/zh)
- 在线文档：[https://www.modxai.net/zh/docs](https://www.modxai.net/zh/docs)
- 更新日志：[https://www.modxai.net/zh/changelog](https://www.modxai.net/zh/changelog)
- 文档预览（GitHub Pages）：[https://modxai.github.io](https://modxai.github.io)

---

## 核心功能

- **自主智能体**  
  拆解任务、调用工具并反馈进度。内置工具覆盖网页搜索、无头浏览器、本地文件读写和终端命令，也可以挂载外部 Skill。运行时针对本地小模型优化，大约 1B–7B、32K 上下文即可完成日常多步任务。需要时也可以改用在线模型。

- **AI 对话（文本与多模态）**  
  通过 llama.cpp 进行本地对话；安装环境后也可以使用 Hugging Face 后端。支持系统提示词、思考深度、基于自有知识库的 RAG，以及把对话收藏下来供以后训练。

- **记忆**  
  查看智能体沉淀的记忆和你手动收藏的对话。可以编辑、归档，导出为 SFT 或 RAG，再用于记忆微调。

- **定时任务**  
  按间隔、固定时间，或在上游任务完成后，自动执行智能体提示词、整理记忆、按记忆微调，或更新知识库。

- **外部访问**  
  从 Telegram、Discord、钉钉或飞书访问本地智能体。聊天来源需要先配对。同一条本机网关还提供 HTTP API，仓库里有 curl、Python 和 Node 示例。

- **语音**  
  本地 TTS 支持中文和英文，可选音色、语速，并自动检测语言。入口在智能体子菜单，和 SD 聊天放在一起。STT 用于麦克风输入和音频数据集转写。

- **图像与视频（SD 聊天）**  
  以会话方式完成文生图、图生图和视频生成。每个会话保留自己的分辨率、步数、CFG、种子和采样器。入口在智能体子菜单。

- **模型库**  
  管理聊天、多模态、向量、重排、SD、TTS 和 STT 模型。可从 Hugging Face、ModelScope 或指定仓库下载，也可导入本地 GGUF 与 SafeTensors。LoRA、兼容 OpenAI 的局域网 API，以及在线服务（DeepSeek、OpenRouter 和其他 OpenAI 兼容接口）在同一处管理。

- **数据集（SFT / RAG / 语音转写）**  
  解析、清洗、切分、合成并向量化，导出 SFT、预训练、DPO 或 RAG 数据。结构化 JSON/JSONL 可以做字段映射。音频转写支持中断后续跑。

- **模型训练**  
  准备 LoRA 训练、实时监控、评估检查点（含 IFEval、GSM8K、MMLU、HellaSwag）、交互测试，并导出 GGUF。LoRA 大约 8GB 显存即可开始。

- **一键环境**  
  首次启动提供新手引导和环境安装引导。按需把「全部」「智能体」或「语音合成」装进便携沙箱，可选镜像源，安装可续传。只用 llama.cpp 对话时不必安装这套环境。

---

## 为什么选择 ModxAI Studio

| 优势 | 说明 |
| ---- | ---- |
| **从图形界面开始** | 下载安装即可使用。主路径不需要 Docker、系统 Python 或命令行。 |
| **默认在本地** | 模型、数据集、对话和记忆留在本机。在线 API 是可选项。 |
| **小模型也能做任务** | 智能体维护状态并允许重试，1B–7B 模型可以在 32K 窗口内完成日常步骤。 |
| **一条工作流** | 数据、训练、推理、记忆、定时任务和频道在同一个应用里。 |
| **沙箱隔离** | 便携环境和智能体工具与系统其余部分隔开。 |
| **准备好之后可离线** | 不需要注册账号。模型和环境就位后，本地功能不依赖网络。 |

---

## 文档

本仓库是官方手册的源文件。向 `main` 推送 `docs/**` 后，会通知 [modxai.github.io](https://github.com/ModxAI/modxai.github.io) 同步。

- 英文：`docs/en/`
- 简体中文：`docs/zh/`

站点目录是 `docs/<语言>/_index.md`。页面为普通 Markdown。

| 章节 | 内容 |
| ---- | ---- |
| 快速入门 | 产品是什么、配置要求、第一次对话 |
| 智能体 | 角色、工具、工作区浏览器、预设、自动审批、语音输入 |
| AI 对话 | 文本、多模态、RAG、提示词生成、推理选项 |
| 记忆 | 浏览、编辑、导出 |
| 定时任务 | 智能体任务、记忆整理、微调、知识库更新 |
| 外部访问 | Telegram、Discord、钉钉、飞书上的机器人，以及本机网关 HTTP API |
| 模型库 | 导入、下载、加载、局域网 API、资源监控 |
| 数据处理 | SFT、RAG、音频转文本 |
| 模型训练 | 准备、监控、评估、测试、量化 |
| SD 聊天 | 图像与视频 |
| TTS | 语音合成 |
| 设置 | 环境安装、语言、更新 |
| FAQ | 安装与排错 |

Windows 桌面的最低配置：**Windows 10/11（64 位）、16GB 内存、20GB 可用磁盘、AVX2**。在该桌面上，训练和图像生成更适合 8GB 及以上显存的 NVIDIA 显卡。对话可以使用 CPU。

arm64 macOS 路径上，环境检查把统一内存 GPU 记为信息项。Python 依赖使用 CPU 环境。推理使用 Metal。

---

## 致谢

ModxAI Studio 构建在开源项目之上，包括 [PyTorch](https://github.com/pytorch/pytorch)、[llama.cpp](https://github.com/ggml-org/llama.cpp)、[Stable Diffusion.cpp](https://github.com/leejet/stable-diffusion.cpp)、[Transformers](https://github.com/huggingface/transformers)、[TRL](https://github.com/huggingface/trl)、[PEFT](https://github.com/huggingface/peft)、[faster-whisper](https://github.com/SYSTRAN/faster-whisper)、[Playwright](https://github.com/microsoft/playwright)、[Electron](https://github.com/electron/electron)、[Vue 3](https://github.com/vuejs/core) 和 [FastAPI](https://github.com/tiangolo/fastapi)。

---

<div align="center">

**ModxAI Studio** — 让本地 AI 可以完成任务，而不只是回答一句。

</div>
