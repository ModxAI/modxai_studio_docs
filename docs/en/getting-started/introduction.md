# Introduction

> v1.1.11
> 2026-11

## What is ModxAI Studio?

ModxAI Studio brings model management, chat, agents, content generation, data processing, and LoRA training into one desktop app. You can run local models or configure online model services in supported features.

Local-model inference runs on your machine. Online models, web tools, and external channels may send task content to their respective services. Check their configuration, data scope, and charges first. Results depend on the model, tools, task complexity, and available resources.

## Core Features

- **[Agent](../features/agent/overview.md)**: Configure roles, tools, and a workspace for multi-step tasks. Review progress, to-dos, and file changes.
- **[AI Chat](../features/chat/overview.md)**: Talk to models, use images or knowledge-base material where supported, and bookmark conversations.
- **[RSI](../features/rsi/overview.md)**: Iterate prompts or Skills against evaluation tasks, inspect each round, and choose a version to save.
- **[Model Center](../features/models/overview.md)**: Download, import, and load chat, multimodal, embedding, reranking, image, and speech models. Formats and environments vary by model category.
- **[Image & Video Generation](../features/sdchat/overview.md)**: Use suitable models for text-to-image, image-to-image, or video generation.
- **[Speech Synthesis](../features/tts/overview.md)**: Generate audio with a selected speech model. Languages, voices, and reference-audio support depend on the model.
- **[Data Processing](../features/dataset/overview.md)**: Process text, code, and other material into training data or knowledge bases, or [transcribe audio](../features/dataset/audio2text.md).
- **[Model Training](../features/training/prepare.md)**: Prepare data and models, run LoRA training, monitor progress, test or evaluate results, and export as needed.
- **[Memory Management](../features/memory/overview.md)**: Organize agent memories and bookmarks, then export them for training or knowledge-base use.
- **[Scheduled Tasks](../features/scheduler/overview.md)**: Run tasks on a schedule and review their history and results.
- **[External Access](../features/gateway/overview.md)**: Connect supported messaging channels or let external applications call the agent through the gateway.

## Environment and Hardware Preparation

Check the [download page](https://www.modxai.net/en/download) and package instructions for supported platforms. Loading one model successfully does not mean that every feature will work with the same configuration.

- Local inference needs enough RAM or VRAM for model files, context, and runtime use.
- Image, video, and training tasks generally need more resources than short text inference. Validate your setup with a small task first.
- Models, datasets, checkpoints, and generated files all need storage. Check the target disk before downloading or training.
- Prepare required environments in **Settings → System**. You still need to select and load the model in the relevant feature.
- Check CPU, memory, and VRAM use at the top right of **Model Center**.

See [Environment Setup](../features/settings/environment.md) and [Loading Models](../features/models/overview.md) for feature-specific requirements.

## Where to Start

- First use: [Quick Start](./quick-start.md).
- Have the model carry out a task: [Agent Overview](../features/agent/overview.md).
- Improve results over repeated task evaluations: [RSI Overview](../features/rsi/overview.md).
- Resolve setup or runtime problems: [FAQ](../faq.md).
