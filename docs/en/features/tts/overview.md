# TTS Speech Synthesis

> v1.0.0
> 2025-12

This article introduces the **TTS (Text-to-Speech)** feature in ModxAI Studio, helping you understand how to use local TTS models to convert text into natural speech.

## Table of Contents

- [Environment Requirements](#environment-requirements)
- [Feature Introduction](#feature-introduction)
- [Prerequisites](#prerequisites)
- [Interface Layout](#interface-layout)
- [Supported Languages](#supported-languages)
- [Basic Operations](#basic-operations)
- [Parameter Configuration](#parameter-configuration)
- [Conversation Management](#conversation-management)
- [Important Notes](#important-notes)

---

## Environment Requirements

**This feature requires installing GPU or CPU environment dependencies in Settings.**

TTS uses Kokoro series models for speech synthesis. Before using, ensure:

1. Successfully installed CPU or GPU environment in **Settings**
2. Successfully loaded a **TTS (Speech Synthesis)** type model in **Model Library**

> **GPU Acceleration (Recommended)**
>
> - **NVIDIA Graphics Cards (Recommended)**:
>   - TTS inference supports CUDA acceleration, running faster on NVIDIA GPUs
>   - Installing GPU environment is recommended for best performance
>
> - **AMD Graphics Cards (Limited)**:
>   - Windows platform AMD GPU does not support TTS inference acceleration
>   - AMD users need to use CPU mode for inference
>
> - **CPU Mode**:
>   - Pure CPU inference is supported but relatively slower
>   - Some AMD CPUs may have compatibility issues; if inference fails, try NVIDIA GPU environment

---

## Feature Introduction

TTS (Text-to-Speech) is a dedicated text-to-speech module that supports:

- **Bilingual Synthesis**: Supports speech synthesis for Chinese and English text
- **Multiple Voice Selection**: Provides multiple preset voices to choose from
- **Speed Adjustment**: Can adjust speech playback speed
- **Automatic Language Detection**: Automatically identifies language based on input text
- **Conversational Interaction**: Supports multiple generations for easy comparison of different parameter effects

---

## Prerequisites

1. **Must Install Environment First**
   - Go to the **Settings** page
   - Install CPU or GPU environment (GPU environment recommended)
   - Wait for environment installation to complete and refresh status

2. **Must Load TTS Model First**
   - Go to the **Model Library** page
   - Import or download a TTS model (currently only Kokoro series models are supported)
   - Load a model in the **TTS (Speech Synthesis)** tab
   - After successful model loading, TTS page features will be automatically unlocked

3. **Understand Environment Capabilities**
   - After environment installation, wait for TTS capability detection to complete
   - Refresh environment status to check if TTS capability is available
   - If capability is unavailable, try reinstalling or repairing the environment

---

## Interface Layout

TTS interface uses a left-right dual-column layout:

### Left Sidebar (Conversation List)
- **New Conversation** button: Located at the top
- **Conversation List**: Grouped by time (Today/Yesterday/Last 7 Days/Last 30 Days/Earlier)
  - Shows conversation title (automatically generated from first input text)

### Right Panel (Generation Area)
- **Message List**: Displays user input text and generated audio
- **Progress Bar**: Shown below message list during generation
- **Input Box**: Bottom area for entering text to convert
- **Parameters Button**: Bottom toolbar of input box, click to open parameters drawer
- **Generate Button**: Bottom right of toolbar

> **Prompt When Model Not Loaded**: Right panel shows "Go to Model Library to Load <Speech Synthesis Model>" button, click to jump to Model Library.

---

## Supported Languages

The current version of TTS supports the following languages:

| Language Code | Language | Description |
|---------------|----------|-------------|
| `a` | American English | American English |
| `b` | British English | British English |
| `z` | Mandarin | Mandarin Chinese |

> **Automatic Language Detection**:
> - When language parameter is set to "auto", system automatically detects language based on text content
> - Automatically uses Chinese mode when Chinese characters are detected
> - Automatically uses English mode when English characters are detected

> ⚠️ **Language Limitations**:
> - Current version only supports Chinese and English
> - Other languages like Japanese require additional dependencies (e.g., pyopenjtalk), not built-in yet

---

## Basic Operations

### Create Conversation

1. Click **New Conversation** button at the top of the left sidebar
2. System will create a new conversation and automatically switch to it
3. Conversation title is automatically generated from input text after first generation

### Input Text and Generate

1. Enter text to convert in the input box
2. Click **Generate** button (or press `Ctrl+Enter`)
3. System immediately shows user message and starts generation
4. After generation completes, AI message will display audio player

> **Tips**:
> - Text supports mixed Chinese and English input
> - Longer texts are automatically split by paragraphs for processing
> - Real-time progress is displayed during generation

### Play and Save

1. After generation completes, click the audio player to play
2. Use **Save As** button to save audio locally
3. Default save format is WAV

---

## Parameter Configuration

Click the **Parameters button** (slider icon) on the bottom toolbar of the input box to open the parameters drawer.

### Basic Parameters

| Parameter | Default | Range | Description |
|-----------|---------|-------|-------------|
| **Voice** | af_heart | Preset list | Select voice preset |
| **Speed** | 1.0 | 0.5-2.0 | Speech playback speed, 1.0 is normal speed |
| **Language** | Auto | auto/a/b/z | Language code, auto for automatic detection |
| **Sample Rate** | 24000 | 16000/22050/24000/44100 | Output audio sample rate |

### Voice Presets

System includes multiple voice presets:

| Voice Name | Gender | Description |
|------------|--------|-------------|
| af_heart | Female | Default female voice |
| af_bella | Female | Gentle female voice |
| af_nicole | Female | Clear female voice |
| af_sky | Female | Bright female voice |
| bf_emma | Female | Natural female voice |
| bf_isabella | Female | Soft female voice |
| am_adam | Male | Steady male voice |
| am_michael | Male | Magnetic male voice |
| bm_george | Male | Calm male voice |
| bm_lewis | Male | Clear male voice |

> **Voice Notes**:
> - `af_` / `bf_` prefix indicates female voice
> - `am_` / `bm_` prefix indicates male voice
> - Available voices depend on the loaded model

### Speed Adjustment

| Speed Value | Effect |
|-------------|--------|
| 0.5 | Slow playback |
| 1.0 | Normal speed |
| 1.5 | Fast playback |
| 2.0 | Very fast playback |

---

## Conversation Management

### Select Conversation
- Click conversation item in the left list to switch

### Rename Conversation
1. Right-click conversation item (or click `···` menu on the right of conversation)
2. Select **Rename**
3. Enter new title
4. Click **Confirm**

### Delete Conversation
1. Right-click conversation item
2. Select **Delete**
3. Confirm deletion (will delete conversation along with all messages and generated audio records)

---

## Important Notes

### Environment Dependencies

1. **TTS Dependencies**
   - TTS feature depends on misaki library for G2P (Grapheme-to-Phoneme) conversion
   - This library is installed from PyPI official source, which may be slow in mainland China
   - Installation failure does not affect main environment, can be fixed by reinstalling

2. **spaCy Models**
   - TTS feature also depends on spaCy language models (en_core_web_sm, zh_core_web_sm)
   - These models are **downloaded asynchronously** after environment installation
   - Download completion time depends on network conditions, may be delayed by tens of seconds to several minutes
   - After main environment installation completes, please **refresh environment status** to confirm if TTS capability is available

3. **Capability Status Detection**
   - TTS capability status after environment refresh is the source of truth
   - If TTS capability shows as unavailable, dependencies are not fully installed
   - Can **reinstall** or **repair environment** to download related dependencies again

### Hardware Compatibility

1. **Recommended Configuration**
   - NVIDIA GPU + CUDA environment: Fastest inference speed
   - 8GB+ VRAM works better

2. **CPU Mode Limitations**
   - Pure CPU inference is slower, long text generation may take considerable time
   - Some AMD CPUs may have compatibility issues
   - If inference fails, switching to NVIDIA GPU environment is recommended

### Generation Limitations

1. **Concurrency Limit**
   - Only one generation task at a time
   - Cannot send new messages during generation

2. **Text Length**
   - Supports automatic paragraph splitting for long texts
   - Recommended single input not exceeding 2000 characters for best experience

3. **Quota Consumption**
   - Each successful generation consumes 1 data quota

### Data Management

1. **History Records**
   - Conversations and messages are persistently stored in local database
   - Uninstalling the app will not delete local database

2. **Audio Storage**
   - Generated audio is saved to system temporary directory by default
   - Important audio should be saved to specified location promptly

---

## FAQ

**Q: Why is TTS feature unavailable?**

A: Check the following:
1. Has environment been installed in Settings
2. Has status been refreshed after environment installation
3. Is TTS capability shown as available
4. Has TTS model been loaded

**Q: Why does the generated speech sound unnatural?**

A: Try the following adjustments:
1. Check if input text has grammatical errors
2. Try different voice presets
3. Adjust speed parameter
4. Ensure language setting matches text content

**Q: TTS dependency installation is slow in mainland China, what to do?**

A:
1. misaki library is installed from PyPI official source, may be slow on domestic networks
2. Keep network stable during installation
3. If timeout fails, retry by reinstalling environment
4. spaCy models are downloaded asynchronously, please wait patiently and refresh status

**Q: What to do if TTS capability is shown as unavailable?**

A:
1. Refresh environment status, wait for capability detection to complete
2. If still unavailable, try reinstalling environment
3. Check if network connection is normal
4. Check installation logs for specific failure reasons

---

## Related Documentation

- [Model Library](../models/overview.md) - Learn how to import and load TTS models
- [Environment Configuration](../settings/environment.md) - Learn about environment installation details
