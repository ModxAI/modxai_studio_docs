[English](#modxai-studio) | [简体中文](#modxai-studio-zh)

# ModxAI Studio
<a id="modxai-studio"></a>

ModxAI Studio is a desktop app for running local AI models, chatting, using agents, preparing datasets, and training models. This repository contains the user guide. Page headers identify the version each guide covers.

## Start here

1. Download and install ModxAI Studio from the [product website](https://www.modxai.net/en/download).
2. Follow [Quick Start](docs/en/getting-started/quick-start.md) to download or import a model, load it, and send your first message.
3. Install additional components in **Settings** when a feature requires them. Basic llama.cpp chat does not need an additional environment.

Check the download page for available installers. Model, training, and image-generation requirements depend on your hardware and settings; read the relevant guide before starting.

## Features and guides

| What you want to do | Guide |
| --- | --- |
| Chat with a model, ask about images, or use a knowledge base | [AI Chat](docs/en/features/chat/overview.md) |
| Ask an agent to browse the web, work with files, or run commands | [Agent Workbench](docs/en/features/agent/overview.md) |
| Connect external tools for agents | [MCP External Tools](docs/en/features/mcp/overview.md) |
| Download, import, load, or manage models | [Model Library](docs/en/features/models/overview.md) |
| Review, edit, or export saved memories | [Memory](docs/en/features/memory/overview.md) |
| Run tasks at a set time, on an interval, or after another task | [Scheduled Tasks](docs/en/features/scheduler/overview.md) |
| Connect Telegram, Discord, DingTalk, or Feishu, or use the gateway API | [External Access](docs/en/features/gateway/overview.md) |
| Prepare training data or a knowledge base, or transcribe audio | [Data Processing](docs/en/features/dataset/overview.md) |
| Test and improve a prompt or Skill against your own tasks | [RSI Self Improvement](docs/en/features/rsi/overview.md) |
| Fine-tune a model, evaluate checkpoints, or export GGUF | [Training](docs/en/features/training/overview.md) |
| Generate images or videos | [SD Chat](docs/en/features/sdchat/overview.md) |
| Turn text into speech | [Speech Synthesis](docs/en/features/tts/overview.md) |

See the [full manual](docs/en/_index.md) for all pages, or [FAQ](docs/en/faq.md) for troubleshooting.

## Before you begin

- Choose models that fit your memory and GPU resources. Check generated answers and task results.
- Local features can work offline after models and required components are installed. Online models and web tools need a network connection; check what data they send and any provider charges.
- Review an agent's workspace and tool permissions before running it. File changes and terminal commands can affect your data; use automatic approval only for tasks you trust.
- Keep local model and gateway services on trusted networks. Do not expose their ports to the public internet.

## Documentation maintenance

- English pages: `docs/en/`; Chinese pages: `docs/zh/`.
- Each language's navigation is in `docs/<lang>/_index.md`.
- Follow the [writing standard](maintenance/user-guide-style.md) and [page template](maintenance/page-template.md). Update both languages when instructions change.
- Submit changes on a separate branch for PR review. A push affecting `docs/**` on `main` triggers a sync notification to the [preview repository](https://github.com/ModxAI/modxai.github.io); do not use `main` for draft edits.

[Online manual](https://www.modxai.net/en/docs) · [Release notes](https://www.modxai.net/en/changelog) · [Documentation preview](https://modxai.github.io)

## Acknowledgements

ModxAI Studio is built on open-source projects, including [PyTorch](https://github.com/pytorch/pytorch), [llama.cpp](https://github.com/ggml-org/llama.cpp), [Stable Diffusion.cpp](https://github.com/leejet/stable-diffusion.cpp), [Transformers](https://github.com/huggingface/transformers), [TRL](https://github.com/huggingface/trl), [PEFT](https://github.com/huggingface/peft), [faster-whisper](https://github.com/SYSTRAN/faster-whisper), [Playwright](https://github.com/microsoft/playwright), [Electron](https://github.com/electron/electron), [Vue 3](https://github.com/vuejs/core), and [FastAPI](https://github.com/tiangolo/fastapi).

---

<a id="modxai-studio-zh"></a>

# ModxAI Studio（中文）

ModxAI Studio 是一款桌面应用，用于运行本地 AI 模型、对话、执行智能体任务、处理数据和训练模型。本仓库提供用户操作指南，各页开头标明适用版本。

## 开始使用

1. 从[官网](https://www.modxai.net/zh/download)下载并安装 ModxAI Studio。
2. 按[快速开始](docs/zh/getting-started/quick-start.md)下载或导入模型，加载后发送第一条消息。
3. 需要额外组件时，按提示到**设置**中安装。使用 llama.cpp 进行基础对话无需安装额外环境。

可用安装包以下载页面为准。模型运行、训练和图像生成的资源需求取决于硬件与参数，使用前请查看对应指南。

## 功能与指南

| 想做什么 | 操作指南 |
| --- | --- |
| 与模型对话、识别图片或基于知识库问答 | [AI 对话](docs/zh/features/chat/overview.md) |
| 让智能体浏览网页、处理文件或执行命令 | [智能体工作台](docs/zh/features/agent/overview.md) |
| 为智能体连接外部工具 | [MCP 外部工具](docs/zh/features/mcp/overview.md) |
| 下载、导入、加载或管理模型 | [模型库](docs/zh/features/models/overview.md) |
| 查看、编辑或导出保存的记忆 | [记忆管理](docs/zh/features/memory/overview.md) |
| 按时间、间隔或上游任务结果自动执行任务 | [定时任务](docs/zh/features/scheduler/overview.md) |
| 连接 Telegram、Discord、钉钉、飞书或调用网关 API | [外部访问](docs/zh/features/gateway/overview.md) |
| 准备训练数据、创建知识库或转录音频 | [数据处理](docs/zh/features/dataset/overview.md) |
| 用自己的任务测试并改进提示词或 Skill | [RSI 自我改进](docs/zh/features/rsi/overview.md) |
| 微调模型、评估检查点或导出 GGUF | [模型训练](docs/zh/features/training/overview.md) |
| 生成图像或视频 | [SD 聊天](docs/zh/features/sdchat/overview.md) |
| 将文字转为语音 | [语音合成](docs/zh/features/tts/overview.md) |

全部页面见[文档目录](docs/zh/_index.md)，遇到问题可查看[常见问题](docs/zh/faq.md)。

## 使用前注意

- 根据内存和显存选择模型，并检查生成的回答和任务结果。
- 模型和所需组件安装后，本地功能可以离线使用。在线模型和网页工具需要联网，请留意发送的数据及服务商费用。
- 执行智能体任务前，检查工作区和工具权限。文件修改和终端命令可能影响您的数据，仅对可信任务开启自动审批。
- 本地模型服务和网关仅用于可信网络，不要将端口暴露到公网。

## 文档维护

- 英文页面在 `docs/en/`，中文页面在 `docs/zh/`。
- 各语言的目录入口为 `docs/<语言>/_index.md`。
- 修改时遵循[编写规范](maintenance/user-guide-style.md)和[页面模板](maintenance/page-template.md)，操作说明变更须同步中英文。
- 在独立分支修改，通过 PR 审阅。向 `main` 推送涉及 `docs/**` 的修改会通知[预览仓库](https://github.com/ModxAI/modxai.github.io)同步，请勿在主分支直接编辑草稿。

[在线文档](https://www.modxai.net/zh/docs) · [更新日志](https://www.modxai.net/zh/changelog) · [文档预览](https://modxai.github.io)

## 致谢

ModxAI Studio 构建在开源项目之上，包括 [PyTorch](https://github.com/pytorch/pytorch)、[llama.cpp](https://github.com/ggml-org/llama.cpp)、[Stable Diffusion.cpp](https://github.com/leejet/stable-diffusion.cpp)、[Transformers](https://github.com/huggingface/transformers)、[TRL](https://github.com/huggingface/trl)、[PEFT](https://github.com/huggingface/peft)、[faster-whisper](https://github.com/SYSTRAN/faster-whisper)、[Playwright](https://github.com/microsoft/playwright)、[Electron](https://github.com/electron/electron)、[Vue 3](https://github.com/vuejs/core) 和 [FastAPI](https://github.com/tiangolo/fastapi)。
