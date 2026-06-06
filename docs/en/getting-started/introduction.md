# Introduction

> v1.1.0
> 2026-5

## What is ModxAI Studio?

ModxAI Studio is a **one-stop desktop application** designed for **local AI deployment**. It integrates local model deployment, autonomous agents, image generation, knowledge base retrieval, data processing, and model training into a single application, allowing users to leverage intelligent agents for everyday tasks with **zero cost**, **high efficiency**, **unrestricted freedom**, and without concern for data leaks.

**Core Positioning**: Unlike cloud-based AI agent applications on the market, ModxAI Studio's agent framework is deeply optimized for **local small models**. Through refined state management and fault tolerance, even small models with 1B-3B parameters can complete daily tasks within a 32K context window. If used in conjunction with online models, it can also significantly reduce token costs.

---

## Core Features

### 🧠 Autonomous Agent

AI goes beyond chat to become a **task-executing** personal assistant:

- **Task Planning & Execution**: AI automatically decomposes complex tasks, executes step by step, and provides progress feedback.
- **Built-in Toolchain & External Skill Loading**: Supports 70+ built-in efficient tools and external Skill integration for web browsing, local file read/write/edit, terminal command execution, and other end-to-end task scenarios.
- **Memory & Preference Learning**: Interactive memory management and exclusive automated memory training, creating a model clone that understands you better with each use.

### 🤖 AI Chat

Intelligent conversation with local large language models:

- **Text Chat**: Daily Q&A, code generation, creative writing, etc.
- **Multimodal Chat**: Supports image-text understanding; upload images for AI analysis.
- **RAG Enhancement**: Answer questions based on your knowledge base; AI understands your domain better.
- **Data Collection**: Collect high-quality conversations for generating training or RAG data.

### 🖼️ Image & Video Generation

Creative generation using Stable Diffusion models:

- **Text-to-Image**: Generate exquisite images based on text descriptions.
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
- **Online Model Service**: Currently supports DeepSeek and OpenRouter online model services.

### 📊 Data Processing

Convert raw data into trainable datasets:

- **SFT Data Generation**: Generate supervised fine-tuning data from documents, structured data, etc.
- **RAG Knowledge Base**: Build vectorized knowledge bases supporting intelligent retrieval.
- **Audio to Text**: Transcribe speech into multiple structured text formats.
- **AI Synthesis**: Perform on-demand data analysis through local models.

### 🎯 Model Training

- **LoRA Efficient Fine-Tuning**: Start training with as little as 8GB VRAM.
- **Real-Time Monitoring**: Visualize training progress and loss curves.
- **Checkpoint Evaluation**: Automatically evaluate multiple checkpoints and recommend the best result.
- **One-Click Export**: Export training results to GGUF format for direct inference use.
- **Memory Training**: Automatically iterates and optimizes personalized models based on agent-accumulated or user-bookmarked conversation memories.

### ⚙️ Zero-Threshold Environment Configuration

Say goodbye to complex environment configurations:

- **One-Click Installation**: Automatically detects hardware and recommends the best configuration.
- **On-Demand Selection**: Supports Full / Agent / Voice Synthesis three installation capabilities, install as needed.
- **Sandbox Isolation**: Independent environment, does not affect other system software.
- **On-Demand Loading**: Dependencies are not loaded when features are not in use.

---

## Why Choose ModxAI Studio?

| Advantage | Description |
|-----------|-------------|
| **Truly Local** | All data and inference run on your own computer — no internet required, no fees. |
| **Small Models, Big Results** | Agent framework optimized for 1B-7B local models; 32K context handles daily tasks. |
| **All-in-One Experience** | Data processing -> Model training -> Inference -> Memory accumulation -> Iterative training, one complete solution. |
| **High Freedom** | No external restrictions, always available, flexible parameter control, full ownership of your AI. |
| **Privacy & Security** | Data stored and encrypted locally; agent operations run in a sandbox. |
| **Simple & Easy to Use** | Graphical interface, no command line operations required. |
| **Continuous Evolution** | Local memory management and training — your AI gets smarter with use. |

---

## System Requirements

### Minimum Configuration

| Item | Requirement |
|------|-------------|
| OS | Windows 10/11 (64-bit) |
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

- [Quick Start](./quick-start.md) - Complete your first AI chat in 5 minutes.
- [Model Library](../features/models/overview.md) - Learn how to manage and load models.
- [AI Chat](../features/chat/overview.md) - Dive deeper into chat features.
