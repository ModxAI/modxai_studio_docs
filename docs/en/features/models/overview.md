# Model Library

> v1.0.0
> 2025-12

This article introduces how to use the **Model Library** feature in ModxAI Studio, helping you complete tasks such as model import, download, load, management, and custom export.

## Table of Contents

- [Environment Requirements](#environment-requirements)
- [Interface Layout](#interface-layout)
- [Model Categories](#model-categories)
- [Model Import](#model-import)
- [Model Download](#model-download)
- [Model Load and Unload](#model-load-and-unload)
- [Model Management](#model-management)
- [Custom Model Export](#custom-model-export)
- [System Resource Monitoring](#system-resource-monitoring)
- [Notes](#notes)

---

## Environment Requirements

###  External Environment Dependencies

The following model types require environment to be installed in **Settings** before loading:

- **Embedding Model**: Used for RAG retrieval, requires CPU or GPU environment.
- **Rerank Model**: Used for retrieval result optimization, requires CPU or GPU environment.
- **SD Model (Image Generation)**: Requires CPU or GPU environment.
- **Custom Model Export**: Requires CPU or GPU environment.

### No Environment Dependencies

The following model types **do not** require environment and can be used directly:

- **Chat Model**: Uses llama.cpp inference engine.
- **Multimodal Model**: Uses llama.cpp inference engine.

### Environment Installation

If you need to use features dependent on Python, please go to the **Settings** page to install the CPU or GPU environment.


> **GPU Acceleration libraries (Important)**
>
> - NVIDIA (recommended):
>   - If you have an NVIDIA GPU, install the CUDA acceleration libraries using the “Install GPU Acceleration Libraries” option in the Model Library ⚡ button. Chat/multimodal models run fastest on NVIDIA+CUDA; Stable Diffusion (SD, image generation) currently requires CUDA and will only run after the CUDA libraries are installed.
>   - After installing or updating the acceleration libraries, there is no need to restart the application. The changes will take effect the next time a model is loaded.
> - AMD (limitations):
>   - AMD GPUs can use the built-in Vulkan acceleration for chat/multimodal models (no extra library needed), but SD inference is not supported on AMD/Vulkan at this time.
> - No discrete GPU / integrated GPUs:
>   - If no NVIDIA discrete GPU is detected, the “Install GPU Acceleration Libraries” control will not appear. You can still use CPU or platform Vulkan (where supported) for chat models, but performance will be limited.

---

## Interface Layout

The Model Library page is divided into the following areas:

### Top Toolbar

- **Import Button**: Manually import local model files.
- **Download Button**: Open the model downloader to download models from Hugging Face or ModelScope.
- **Load Button**: Load the selected model into memory.
- **Unload Button**: Unload the currently loaded model.
- **Refresh Button**: Refresh the model list and status.
- **Create Model Button**: Open the custom model export interface.

### Model Category Tabs

- **Chat Model**: Text conversation model.
- **Embedding Model**: Used for RAG retrieval or generating embedding models required for RAG datasets.
- **Rerank Model**: Used for reordering retrieval results.
- **Multimodal Model**: Supports image-text understanding.
- **SD Model**: Stable Diffusion image generation model.
- **TTS Model**: Text-to-Speech model.

### Model List Table

Displays all models under the current category, including:

- **Name**: Model name (clickable to edit).
- **Description**: Model description (clickable to edit).
- **Type**: Model format (e.g., GGUF, SafeTensors).
- **Status**: Load status (Loaded/Unloaded).
- **Operations**: Context settings, component configuration, delete, etc.

### System Resource Monitoring

The bottom of the page displays real-time resource usage:

- **CPU**: CPU usage.
- **Memory**: Memory usage.
- **GPU**: GPU memory and temperature (if GPU is available).

---

## Model Categories

### Chat Model

**Usage**: Text conversation, code generation, AI-related nodes required when generating SFT datasets.

**Format Support**:
- `.gguf`: llama.cpp quantization format (Recommended).
- `.bin`: GGML format.

**Environment Requirements**: No environment installation required in **Settings**.

**Quota Consumption**: Free.

**Features**:
- Supports LoRA adapters.
- Supports custom context size (n_ctx).
- Supports request queue processing (up to 50 requests queued).
- SFT data generation can reuse loaded chat models without reloading.

**Model Reuse Mechanism**:

When you are generating SFT data, if a chat model is already running:
- The system automatically detects if the model configuration matches (Base Model + LoRA).
- If matched, the SFT task reuses the model, and requests enter the queue for processing.
- The model is not unloaded after the SFT task completes, and your chat session is unaffected.
- Multiple requests are processed serially through the queue mechanism without interference.

### Embedding Model

**Usage**: Supports retrieval and text similarity calculation when RAG is enabled during text chat. Also required for generating vector processing nodes when generating RAG datasets.

**Format Support**:
- `.safetensors`: HuggingFace SafeTensors format.

**Environment Requirements**: Requires CPU or GPU environment.

**Quota Consumption**: Quota deducted after successful load.

**Features**:
- Automatically detects model dimensions.
- Supports batch encoding.

**Usage Suggestions**:

⚠️ **Must be used with a Rerank Model** to improve the speed and accuracy of knowledge base retrieval. Not using a rerank model will significantly impact retrieval speed.

The embedding model is responsible for preliminary recall of relevant documents, and the rerank model is responsible for precise sorting. The combination can:
- Improve retrieval accuracy: Rerank models can more accurately assess relevance.
- Speed up retrieval: Embedding models recall quickly, rerank models sort finely.
- Optimize resource usage: Reasonable division of labor avoids overburdening a single model.

### Rerank Model

**Usage**: Optimize retrieval result sorting.

**Format Support**:
- `.safetensors`: HuggingFace SafeTensors format.

**Environment Requirements**: Requires CPU or GPU environment.

**Quota Consumption**: Quota deducted after successful load.

**Features**:
- Supports Cross-Encoder architecture.
- Improves retrieval accuracy.

**Usage Suggestions**:

⚠️ **Recommended to install GPU environment before use**, otherwise processing speed will be affected.

Rerank models use a cross-encoder architecture with high computational complexity:
- CPU Environment: Slow processing speed, may affect RAG retrieval experience.
- GPU Environment: Inference speed significantly improved, real-time retrieval is smoother.

### Multimodal Model

**Usage**: Image-text understanding, visual Q&A.

**Format Support**:
- `.gguf`: llama.cpp quantization format (Recommended).
- `.bin`: GGML format.

**Environment Requirements**: No environment installation required in **Settings**.

**Quota Consumption**: Quota deducted after successful load.

**Features**:
- Supports LoRA adapters.
- Supports custom context size (n_ctx).
- Requires multimodal projection component (mmproj).

### SD Model (Stable Diffusion)

**Usage**: Text-to-Image, Image-to-Image, Video Generation.

**Format Support**:
- `.gguf .safetensors .bin`: Main model file.
- Requires VAE, CLIP, T5XXL, etc., components.

**Environment Requirements**: Requires CPU or GPU environment.

**Quota Consumption**: Quota deducted after successful load.

**Supported Template Types**:
- **sd_official**: Stable Diffusion official architecture (requires VAE + CLIP-L + CLIP-G, etc.).
- **flux**: FLUX architecture (requires VAE + CLIP-L + T5XXL).
- **video**: Video generation Wan series (requires VAE).

**Inference Capabilities**:
- Text to Image
- Image to Image
- Video Generation

**Features**:
- Supports LoRA adapters (all lora files in the directory, multiple can be used simultaneously).
- Supports separate VAE, CLIP, T5XXL components.
- Supports capability tag management.

### TTS Model (Speech Synthesis)

**Usage**: Text-to-Speech, supports multiple voice synthesis.

**Format Support**:
- `.pth`: Kokoro series model weight file.
- Requires `config.json` configuration file and `.pt` voice files.

**Environment Requirements**: Requires CPU or GPU environment.

**Quota Consumption**: Quota deducted per generation.

**Supported Languages**:
- **American English (a)**: American English
- **British English (b)**: British English
- **Mandarin (z)**: Mandarin Chinese

**Features**:
- Supports automatic language detection.
- Supports multiple preset voices (af_heart, am_adam, etc.).
- Supports speed adjustment (0.5x - 2.0x).
- Supports CUDA acceleration and CPU inference.

> ⚠️ **Usage Limitations**:
> - Current version only supports Chinese and English
> - Some AMD CPUs may have compatibility issues
> - NVIDIA GPU environment recommended for best performance

---

## Model Import

### Import Process

1. **Click Import Button**: Click "Import" under the corresponding model category tab.
2. **Select File**:
   - **Chat/Multimodal**: Select `.gguf` or `.bin` file.
   - **Embedding/Rerank**: Select `.safetensors` file or directory containing the file.
   - **SD Model**: Must use SD import dialog separately.
3. **Auto Verification**: System automatically detects file format and model type.
4. **Fill Information**:
   - **Name**: Model display name.
   - **Category**: System automatically infers, can be manually adjusted.
   - **Description**: Model description (Optional).
5. **Confirm Import**: Click "Import" button to complete.

### Smart Detection Rules

The system automatically infers model type based on the following information:

- **Filename**: Contains keywords like `vision`, `mmproj`, `multimodal`.
- **Directory Name**: Parent directory contains model type related keywords.
- **Format**:
  - `.gguf/.bin` → Chat or Multimodal.
  - `.safetensors` + Directory → Embedding or Rerank.
  - `.gguf/.bin/.safetensors` + Single File → SD Model.

### Multimodal Projection Component (mmproj)

Multimodal models need to be used with projection components:

- **Naming Convention**: Filename contains `mmproj`.
- **Import Method**:
  - Select the main model in the Multimodal Tab. If the mmproj component already exists in the directory, it will be automatically imported, or manually import the mmproj component separately in components.
  - When downloading a multimodal model via the downloader, the mmproj component will be downloaded together and automatically imported.

### SD Model Import

SD model import is more complex and requires step-by-step configuration:

1. **Select Template Type**: sd_official, flux, or video.
2. **Select Main Model File**: Unet/Diffusion model file (.gguf/.bin/.safetensors).
3. **Configure Components**:
   - **VAE**: Required for all templates.
   - **T5XXL**: Required for all templates.
   - **CLIP-L**: Required for sd_official and flux.
   - **CLIP-G**: Required for sd_official model.
4. **Set Inference Capabilities**: Select supported generation types (Text-to-Image/Image-to-Image/Video).
5. **Fill Name and Description**.
6. **Confirm Import**.

### LoRA Adapter Import

Add LoRA adapter to base model:

1. **Select Base Model**: Select the base model to bind in the model table.
2. **Click "Import LoRA"**: Click "+" before the main model to expand, then click "Import LoRA".
3. **Fill Information**:
   - **Name**: LoRA adapter name.
   - **File Path**: Select LoRA file.
   - **Description**: Description (Optional).
4. **Verify Compatibility**: System automatically checks compatibility between LoRA and base model.
5. **Confirm Import**.

**Note**:
- Chat/Multimodal model LoRA format: `.gguf`.
- SD model supports directory import (containing multiple LoRA files).

---

## Model Download

### Open Downloader

Click the "Download" button on the top toolbar to open the model downloader window.

### Download Source Selection

Supports two download sources:

- **ModelScope**: Faster access for users in mainland China.
- **Hugging Face**: Rich global model resources.

### Download Process

1. **Select Category**: Chat, Embedding, Rerank, Multimodal, Audio-to-Text, Text-to-Image, Video Generation.
2. **Search Model**: Search using keywords.
3. **Select Model**: Click model card to view details.
4. **Select File**:
   - Can select the entire model (all files).
   - Or select specific quantization versions.
5. **Set Save Path**: Select model save directory.
6. **Start Download**: Click "Start Download" button.

### Download Status

- **Progress Display**: Real-time display of download percentage, speed, remaining time.
- **File Count**: Downloaded files / Total files.
- **Pause/Resume**: Can cancel and resume download at any time.
- **Minimize**: Can minimize downloader to button, continue downloading in background.

### Post-Download Processing

After download is complete, the model will automatically appear in the corresponding category's model list, no manual import required.

---

## Model Load and Unload

### Load Model

1. **Select Model**: Check the model to load in the model table.
2. **Optional LoRA**: If the model supports LoRA, expand and select LoRA adapter.
3. **Click Load**: Click the "Load" button at the top.
4. **Environment Check**:
   - Embedding/Rerank/SD models automatically check environment installation status.
   - If not installed, a popup will prompt to go to the settings page to install.
5. **Wait for Load**: Button shows "Loading...".
6. **Load Complete**: Model status becomes "Loaded", button becomes "Loaded Model".

### Unload Model

1. **Click Unload Button**: Click the "Unload" button at the top for the loaded model under the corresponding category.
2. **Confirm Operation**: System prompts unload operation and the name of the model to be unloaded.
3. **Wait for Unload**: Button shows "Unloading...".
4. **Unload Complete**: Model status returns to "Unloaded".

### Load Rules

- **Single Model Load**: Only one base model can be loaded per category.
- **LoRA Combination**:
  - Chat/Multimodal: Max 1 LoRA loaded.
  - SD Model: Multiple LoRA files under specified directory can be loaded simultaneously.
- **Auto Unload**: Loading a new model automatically unloads the old one.
- **Quota Deduction**: Free quota deducted after successful load of Embedding/Rerank/Multimodal/SD models.

### Context Size Setting

Chat and Multimodal models support custom context size (n_ctx):

1. **Open Settings**: Click "Context Settings" in the operation column.
2. **Enter Value**: Manually enter or select preset value.
3. **Special Value**:
   - **0**: Use model default value (Recommended).
4. **Save Settings**: Click "Confirm".
5. **Reload**: Must reload model for changes to take effect.

**Note**:
- Larger context consumes more memory.
- Recommended to choose appropriate size based on task requirements.
- Excessively large context may cause slow inference speed.

---

## Model Management

### Edit Model Information

**Name and Description can be edited directly**:

1. **Click Field**: Click directly on the name or description column in the table.
2. **Enter Edit**: Field becomes input box.
3. **Modify Content**: Enter new content.
4. **Save**: Auto save on blur or Enter key.

### Delete Model

1. **Select Model**: Confirm the model to delete.
2. **Check Status**: Loaded models cannot be deleted, must unload first.
3. **Click Delete**: Click "Delete" button in operation column.
4. **Confirm Operation**: Popup confirms deletion.
5. **File Handling**:
   - **Delete Record Only**: Does not delete actual files.
   - **File Retention**: Model files remain in original location.

**Warning**:
- Delete operation cannot be undone.
- After base model is deleted, associated LoRA adapters will also be deleted.

### SD Capability Management

SD models support secondary editing of inference capability tags:

1. **Select SD Model**: Select model in SD Model Tab.
2. **Open Component Settings**: Click "Components" in operation column.
3. **Modify Tags**: Check or uncheck supported generation types.
4. **Modify Components**: Add or replace VAE, CLIP, T5XXL components.
5. **Save Settings**: Click "Confirm".

### LoRA Management

**Select LoRA**:
- Expand base model to show bound LoRA list.
- Check LoRA to enable (Single selection).

**Delete LoRA**:
- Click "Delete" button in LoRA row.
- Confirm delete operation.
- LoRA record deleted, file retained.

---

## Custom Model Export

### Feature Description

Create and export custom language models from scratch based on configuration parameters, for experiments or specific needs.

**Environment Requirements**: Requires CPU or GPU environment.

### Usage Flow

1. **Open Creation Drawer**: Click "Create Model" button in top right corner.
2. **Select Preset** (Optional):
   - ModxAI-Custom-Small (Lightweight)
   - ModxAI-Custom-Medium (Medium Scale)
   - ModxAI-Custom-Large (Large Scale)
   - ModxAI-Completely-Free (Fully Custom)
3. **Configure Basic Parameters**:
   - **Output Directory**: Model save path (Required).
   - **Vocab Size**: 10,000 - 100,000.
   - **Hidden Size**: 128 - 8192.
4. **Tokenizer Configuration**:
   - **Default Tokenizer**: Use pretrained tokenizer.
   - **Custom Tokenizer**: Train from corpus file (Select .txt file).
5. **Advanced Parameters** (Collapsible Panel):
   - **Layers**: Transformer layers.
   - **Attention Heads**: Multi-head attention mechanism.
   - **FFN Intermediate**: Feed-forward network dimension.
   - **KV Heads**: Key-Value attention heads.
6. **Parameter Preview**:
   - Real-time display of **Total Parameters** and **Estimated VRAM Usage**.
   - Validation error prompts (e.g., unreasonable configuration).
7. **Start Export**: Click "Create Model" button.
8. **Wait for Completion**: Export process may take a few minutes.

**Reminder**:
- This model is not recommended for direct inference use, only for scenarios like experiments and pre-training.

### Preset Explanation

| Preset | Parameters | VRAM Usage | Applicable Scenario |
| :--- | :--- | :--- | :--- |
| Small | ~100M | ~2GB | Lightweight testing, learning. |
| Medium | ~500M | ~4GB | General conversation, experiments. |
| Large | ~1B+ | ~8GB+ | Complex tasks, high quality. |
| Completely-Free | Dynamic | Dynamic | Experimental. |

### Parameter Validation Rules

System automatically checks the following constraints:

- Attention heads must be divisible by hidden size.
- KV heads must be divisible by attention heads.
- Estimated VRAM does not exceed system available memory.
- Parameters are within reasonable range.

### Custom Tokenizer

If you need to train a tokenizer from specific domain corpus:

1. **Prepare Corpus**: Plain text file (.txt), one sentence per line.
2. **Set Vocab Size**: 8,000 - 50,000 (Recommended 16,000 - 32,000).
3. **Select Type**:
   - **SentencePiece** (Recommended): Suitable for Chinese, Japanese, etc.
   - **BPE**: Suitable for English and other Latin languages.
4. **Start Training**: System automatically trains and saves tokenizer.
5. **Auto Apply**: Automatically used for model export after training completion.

---

## System Resource Monitoring

The bottom of the page displays real-time system resource usage:

### CPU Monitoring

- **Usage**: Ring progress bar shows CPU usage percentage.
- **Status**: Red warning when exceeding 90%.

### Memory Monitoring

- **Usage**: Ring progress bar shows memory usage percentage.
- **Specific Value**: Used / Total Capacity (GB).
- **Status**: Red warning when exceeding 90%, triggers memory shortage prompt, and unloads models if necessary to ensure system stability.

### GPU Monitoring

Displayed only when GPU is detected:

- **VRAM Usage**: Used / Total Capacity (MB).
- **Temperature**: Real-time temperature (°C).
- **Vendor Info**: GPU model and manufacturer.

**Note**:
- NVIDIA GPU can monitor VRAM and temperature in real-time.
- AMD/Intel GPU only displays static info (Total VRAM, Vendor).
- This card is not displayed if no GPU.

---

## Notes

### File Format Restrictions

- **Chat/Multimodal**: Only supports `.gguf` and `.bin`.
- **Embedding/Rerank**: Only supports `.safetensors` (Optional directory import).
- **SD Model**: Only supports `.gguf/.safetensors/.bin`.
- Unsupported formats will be rejected during validation phase.

### Environment Dependency Check

Environment is automatically checked when loading the following models:

- **Embedding Model**: Popup guide if not installed.
- **Rerank Model**: Popup guide if not installed.
- **SD Model**: Popup guide if not installed.
- **Custom Export**: Popup guide if not installed.

### Quota Consumption Rules

- **Chat Model**: Free, unlimited loading.
- **Multimodal Model**: Quota deducted after successful load.
- **Embedding Model**: Quota deducted after successful load.
- **Rerank Model**: Quota deducted after successful load.
- **SD Model**: Quota deducted after successful load.

**Deduction Timing**: When model status becomes "Loaded".

### Model Load Restrictions

- **Single Model Limit**: Only one base model can be loaded per category.
- **LoRA Limit**:
  - Chat/Multimodal: Max 1 LoRA.
  - SD Model: Multiple LoRA (Unlimited quantity, but limited by VRAM).
- **Concurrency Limit**: Chat and Multimodal cannot be loaded simultaneously. Chat, Embedding, Rerank, SD can be loaded simultaneously.

### Deletion Protection

- **Loaded Model**: Cannot be deleted, must unload first.
- **Base Model**: Associated LoRA deleted after base model deletion.
- **File Retention**: Delete operation only deletes database record, not actual file.

### Performance Suggestions

- **Context Size**: Choose based on task, not too large.
- **VRAM Usage**: Check system resource monitoring before loading.
- **Concurrent Use**: Avoid loading too many models simultaneously.

### FAQ

**Q: Model not showing after import?**
A: Please click the refresh button, or check if the file format is correct.

**Q: Load failed?**
A: Check if environment is installed, VRAM is sufficient, file is complete.

**Q: LoRA incompatible?**
A: LoRA must match base model architecture, recommended to use specially trained LoRA.

**Q: Download speed slow?**
A: Try switching download source.

**Q: Custom model export failed?**
A: Check if parameter configuration is reasonable, output directory has write permission, environment is correctly installed.

---

After completing this chapter, you can:
- Go to **Chat** feature, use loaded chat model for conversation.
- Go to **Knowledge Base** feature, configure embedding and rerank models for RAG retrieval.
- Go to **Data Processing** feature, use model to generate SFT or RAG datasets.
- Go to **Settings** page, manage environment and system configuration.
