# Introduction

> v1.1.10
> 2026-10

## What is ModxAI Studio?

ModxAI Studio runs local models, agents, image generation, knowledge bases, data processing, and LoRA training in one desktop app. Models and chats stay on the machine by default.

This guide covers version 1.1.10 for Windows. Agent results depend on model capabilities, task complexity, and available resources. See [Model Requirements](../features/agent/overview.md#model-requirements). Online models follow the provider’s pricing and terms.

---

## Core Features

### 🧠 Autonomous Agent

The agent splits a task, calls tools, and reports progress.

- **Execution**: Breaks the task into steps, runs them, and shows progress in the UI
- **Tools and skills**: Web browsing, local file read/write, and terminal commands, plus external skills. Subtasks can be assigned to other configured roles
- **Memory**: Review, edit, and export memories, then use them for a later fine-tune

### 🤖 AI Chat

Intelligent conversation with local large language models:

- **Text Chat**: Daily Q&A, code generation, creative writing, etc.
- **Multimodal Chat**: Supports image-text understanding; upload images for AI analysis.
- **RAG Enhancement**: Retrieve material from your knowledge base for the model to use when answering.
- **Data Collection**: Collect high-quality conversations for generating training or RAG data.

### 🖼️ Image & Video Generation

Creative generation using Stable Diffusion models:

- **Text-to-Image**: Generate images from text descriptions.
- **Image-to-Image**: Perform style transfer or content modification based on existing images.
- **Video Generation**: Supports video generation models like the Wan series.

### 🎤 TTS Speech Synthesis

Text-to-speech using local TTS models:

- **Bilingual Support**: Supports speech synthesis for Chinese and English text.
- **Multiple Voice Selection**: Provides multiple preset voices to choose from.
- **Speed Adjustment**: Can adjust speech playback speed.
- **Automatic Language Detection**: Automatically identifies language based on input text.

### 📚 Model Library Management

Unified management of various AI models:

- **Multi-Model Support**: Chat models, Multimodal models, HF Standard, Embedding models, Rerank models, SD models, TTS models, STT models.
- **One-Click Download**: Download popular models directly from Hugging Face or ModelScope.
- **Local Import**: Supports multiple formats like GGUF, SafeTensors.
- **LoRA Adaptation**: Load exclusive fine-tuning adapters for models.
- **LAN Service**: Automatically exposes an OpenAI-compatible API after loading, available for other applications on the local network.
- **Online Model Service**: DeepSeek, OpenRouter, and OpenAI-compatible Chat Completions endpoints. Advanced settings also include prompt-cache options for OpenAI and Claude.

### 📊 Data Processing

Convert raw data into trainable datasets:

- **SFT Data Generation**: Generate supervised fine-tuning data from documents, structured data, etc.
- **RAG Knowledge Base**: Build vectorized knowledge bases supporting intelligent retrieval.
- **Audio to Text**: Transcribe speech into multiple structured text formats.
- **AI Synthesis**: Perform on-demand data analysis through local models.

### 🎯 Model Training

- **LoRA Efficient Fine-Tuning**: Fine-tune models with LoRA. VRAM requirements depend on the model and training settings.
- **Real-Time Monitoring**: Visualize training progress and loss curves.
- **Checkpoint Evaluation**: Automatically evaluate multiple checkpoints and recommend the best result.
- **One-Click Export**: Export training results to GGUF format for direct inference use.
- **Memory Training**: Use memories saved by the agent or bookmarked conversations for fine-tuning.

### 🗂️ Memory, Schedules, and Channels

- **Memory Management**: Review, edit, and archive agent memories and bookmarks, then export them for training or a knowledge base.
- **Scheduled Tasks**: Run agent tasks, memory organization, memory fine-tuning, and knowledge-base updates on an interval, at a fixed time, or after an upstream task finishes.
- **External Access**: Reach the local agent from Telegram, Discord, DingTalk, or Feishu, or call the gateway HTTP API from this machine or the LAN. Unpaired chats cannot send tasks.

<a id="⚙️-zero-threshold-environment-configuration"></a>

### Environment Configuration

Install the components required by a feature in Settings:

- **First-Run Guide**: The first launch walks through getting started and installing an environment.
- **One-Click Installation**: Detects hardware and provides installation options.
- **On-Demand Selection**: Supports Full / Agent / Voice Synthesis three installation capabilities, install as needed.
- **Environment Management**: Install and manage the required runtime components in Settings.
- **On-Demand Loading**: Dependencies are not loaded when features are not in use.

---

## Why Choose ModxAI Studio?

| Advantage | Description |
|-----------|-------------|
| **Local inference** | Run models locally. When using online models or network tools, consider the data sent and any provider charges |
| **Small models can run tasks** | Choose a local model for your task and check its results with a simple task first |
| **One workflow** | Data processing, training, inference, memory, and schedules are in the same app |
| **Adjustable parameters** | Roles, tools, context length, and inference settings are changed in the UI |
| **Environment setup** | Install required components in Settings without manually configuring system Python |
| **Graphical path** | The main path does not need Docker, a system Python, or a shell |
| **Memory can be trained again** | Export memories and use them for a later memory fine-tune |

---

## System Requirements

### Minimum Configuration

| Item | Requirement |
|------|-------------|
| OS | Windows 10/11 (64-bit). Formal packages are NSIS x64 and MSIX Lite |
| Memory | 16GB RAM |
| Storage | 20GB available space |
| Processor | Supports AVX2 instruction set |

### Recommended Configuration

| Item | Requirement |
|------|-------------|
| Memory | 32GB+ RAM |
| Graphics Card | NVIDIA RTX Series (8GB+ VRAM) |
| Storage | SSD Solid State Drive |

> 💡 You can also run in CPU mode without a GPU, suitable for introductory experience.

---

## Next Steps

- [Quick Start](./quick-start.md) - Finish the first local chat.
- [Agent](../features/agent/overview.md) - Let a model carry out multi-step tasks.
- [Model Library](../features/models/overview.md) - Learn how to manage and load models.
- [AI Chat](../features/chat/overview.md) - Dive deeper into chat features.
