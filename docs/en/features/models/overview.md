# Model Library

> v1.1.11
> 2026-11

Use the Model Library to download, import, load, and manage models.

Open **Model Center** in the main menu. To download a model, choose **Download** in its submenu.

## Table of Contents

- [Environment Requirements](#environment-requirements)
- [Interface Layout](#interface-layout)
- [Model Categories](#model-categories)
- [Model Import](#model-import)
- [Model Download](#model-download)
- [Model Load and Unload](#model-load-and-unload)
- [Model Management](#model-management)
- [LAN Inference Service](#lan-inference-service)
- [System Resource Monitoring](#system-resource-monitoring)
- [Usage Statistics](#usage-statistics)
- [Notes](#notes)

---

## Environment Requirements

###  External Environment Dependencies

The following model types require environment to be installed in **Settings** before loading:

- **Embedding Model**: Used for RAG retrieval, requires CPU or GPU environment.
- **Rerank Model**: Used for retrieval result optimization, requires CPU or GPU environment.
- **SD Model (Image Generation)**: Requires CPU or GPU environment.

### No Environment Dependencies

The following model types **do not** require environment and can be used directly:

- **Chat Model**: Uses llama.cpp inference engine.
- **Multimodal Model**: Uses llama.cpp inference engine.

### Environment Installation

If you need to use features dependent on Python, please go to the **Settings** page to install the CPU or GPU environment.


> **GPU Acceleration libraries (Important)**
>
> - NVIDIA (recommended):
>   - If you have an NVIDIA GPU, install the CUDA acceleration libraries under **Settings → System → CUDA Inference Library Installation**. Chat/multimodal models run fastest on NVIDIA+CUDA; Stable Diffusion (SD, image generation) currently requires CUDA and will only run after the CUDA libraries are installed.
>   - After installing or updating the acceleration libraries, there is no need to restart the application. The changes take effect the next time a model is loaded. Inference libraries can also update themselves.
>   - In the model library, turn on speculative decoding, choose type `draft-dflash`, then choose a local draft model (GGUF).
> - AMD (limitations):
>   - AMD GPUs can use the built-in Vulkan acceleration for chat/multimodal models (no extra library needed), but SD inference is not supported on AMD/Vulkan at this time.
> - No discrete GPU / integrated GPUs:
>   - If no NVIDIA discrete GPU is detected, the **CUDA Inference Library Installation** section will not appear. You can still use CPU or platform Vulkan (where supported) for chat models, but performance will be limited.

---

## Interface Layout

The Model Library page is divided into the following areas:

### Top Toolbar

- **Import Button**: Manually import local model files.
- **Load Button**: Load the selected model into memory.
- **Unload Button**: Unload the currently loaded model.
- **Refresh Button**: Refresh the model list and status.

### Model Category Tabs

- **Chat Model**: Text conversation model.
- **Embedding Model**: Used for RAG retrieval or generating embedding models required for RAG datasets.
- **Rerank Model**: Used for reordering retrieval results.
- **Multimodal Model**: Supports image-text understanding.
- **SD Model**: Stable Diffusion image generation model.
- **TTS Model**: Text-to-Speech model.
- **STT Model**: Speech-to-Text model (based on faster-whisper).

### Model List Table

Displays all models under the current category, including:

- **Name**: Model name (clickable to edit).
- **Description**: Model description (clickable to edit).
- **Type**: Model format (e.g., GGUF, SafeTensors).
- **Status**: Load status (Loaded/Unloaded).
- **Operations**: Context settings, component configuration, delete, etc.

### System Resource Monitoring

The resource strip at the top right shows CPU, memory, and VRAM usage percentages.

---

## Model Categories

### Chat Model

**Usage**: Text conversation, code generation, AI-related nodes required when generating SFT datasets.

**Inference Backend**:
- **llama.cpp** (GGUF format): No environment installation needed, ready to use out of the box. Recommended for most users.
- **HF Transformers** (HuggingFace format): Requires CPU or GPU environment installation. Supports more flexible precision control and device selection.

**Format Support**:
- `.gguf`: llama.cpp quantization format (Recommended).
- `.bin`: GGML format.
- `.safetensors`: HF Transformers format.

**Environment Requirements**: llama.cpp backend requires no environment installation; HF backend requires environment installation in **Settings**.

**Features**:
- Supports LoRA adapters.
- Supports custom context size (n_ctx).
- Supports request queue processing (up to 50 requests queued).
- SFT data generation can reuse loaded chat models without reloading.

**HF Model Specific Settings**:

After importing an HF Transformers format chat model, the following configurations are available in the model operations column:

| Parameter | Description | Default | Options |
|-----------|-------------|---------|---------|
| Inference Device | Select device for inference | auto | auto / cpu / cuda |
| Weight Precision | Numerical precision for model loading | Empty (auto) | float16 / float32 / bfloat16 |

> HF models currently only support text inference; multimodal (image input) is not supported.

**Model Reuse Mechanism**:

Local AI nodes in data processing can reuse a llama.cpp chat model when its configuration matches. Matching includes the base model, LoRA, context size, and runtime parameters, not only the model name.

Finishing a processing step does not itself unload the shared model, but data processing and chat compete for compute resources, so chat may wait or respond more slowly. Switching configurations may also be blocked while other tasks use the model. See [Model Reuse in Data Processing](../dataset/overview.md#model-reuse-mechanism).

### Embedding Model

**Usage**: Supports retrieval and text similarity calculation when RAG is enabled during text chat. Also required for generating vector processing nodes when generating RAG datasets.

**Format Support**:
- `.safetensors`: HuggingFace SafeTensors format.

**Environment Requirements**: Requires CPU or GPU environment.

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

**Features**:
- Supports LoRA adapters.
- Supports custom context size (n_ctx).
- Requires multimodal projection component (mmproj).

**Special Reminder**:

⚠️ With Vulkan, setting context to Unlimited makes a multimodal model fail to load. CUDA does not. On an NVIDIA GPU, install the CUDA acceleration library.

⚠️ **Multimodal models do not support knowledge-base Q&A (RAG).** RAG is unavailable while a multimodal conversation is active. Switch to a text chat model to use a knowledge base.

### SD Model (Stable Diffusion)

**Usage**: Text-to-Image, Image-to-Image, Video Generation.

**Format Support**:
- `.gguf .safetensors .bin`: Main model file.
- Requires VAE, CLIP, T5XXL, etc., components.

**Environment Requirements**: Requires CPU or GPU environment.

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

### STT Model (Speech to Text)

**Usage**: Transcribe audio files or real-time recordings to text.

**Format Support**:
- Based on faster-whisper engine, model files are obtained via the downloader.

**Environment Requirements**: Requires CPU or GPU environment (GPU recommended).

**Features**:
- Supports multiple computation precisions (float32 / float16 / int8).
- Supports automatic language detection and language specification.
- Supports VAD (Voice Activity Detection) to filter silence.
- Supports CUDA acceleration and CPU inference.

**Configuration Parameters**:

| Parameter | Description | Default |
|-----------|-------------|---------|
| Computation Precision | Inference computation precision | float16 |
| Inference Device | Device used for inference | auto |
| CPU Threads | Parallel threads for CPU inference | 4 |
| Worker Processes | Number of parallel worker processes | 1 |

---

## Model Import

### Import Process

1. **Click Import Button**: Click "Import" under the corresponding model category tab.
2. **Select File**:
   - **Chat/Multimodal**: Select a `.gguf` or `.bin` file. For HF chat models, select the directory containing `.safetensors` files.
   - **Embedding/Rerank**: Select `.safetensors` file or directory containing the file.
   - **SD Model**: Use the separate SD import dialog.
   - **TTS Model**: Select the `.pth` weights file.
   - **STT Model**: Use the downloader; it manages the model files.
3. **Auto Verification**: System automatically detects file format and model type.
4. **Fill Information**:
   - **Name**: Model display name.
   - **Category**: System automatically infers, can be manually adjusted.
   - **Description**: Model description (Optional).
5. **Confirm Import**: Click "Import" button to complete.

Importing a `.safetensors` file reports the file size.

### Smart Detection Rules

Check the detected category before confirming the import. If it is incorrect, select the category that matches the model’s purpose.

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

Open **Model Center → Download** in the main menu. Browse the catalog, specify a repository, or view download management.

### Download Source Selection

Supports two download sources:

- **ModelScope**: Faster access for users in Chinese mainland.
- **Hugging Face**: Rich global model resources.
- **A chosen repository**: The download station can download a model from a repository you select, not only by browsing the built-in list. The list includes the Qwopus series.

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
- **Switch pages**: Use other pages while downloads continue. Return to download management to check progress.
- **The window stays usable**: A fast download should not freeze the interface. If it still stalls, pause the download before doing other work.
- **Refresh progress**: The current download progress can be read again.
- **Delete local files**: A download entry can remove the files already written to disk.
- **Open directory**: That button opens the folder that holds the files.

### Post-Download Processing

After download is complete, the model will automatically appear in the corresponding category's model list, no manual import required.

---

## LAN Inference Service

After loading a model, ModxAI Studio automatically exposes an OpenAI-compatible HTTP API for other applications or devices on the local network to call.

### Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/v1/chat/completions` | POST | OpenAI-compatible chat inference endpoint |
| `/v1/models` | GET | List currently available models |

### Use Cases

- **LAN Collaboration**: Other devices on the same network can directly call the local model inference service.
- **Third-Party Integration**: Supports any OpenAI API-compatible client (e.g., other agent applications, automation scripts) for direct integration.
- **Development & Debugging**: Use curl, Postman, or similar tools to test the inference interface directly.

> This API does not require an API key. View the service address and port in the app. Use it only on a trusted network; do not expose it through port forwarding or a public tunnel. Other devices that can reach the service may call your model.

---

## Model Load and Unload

### Load Model

1. **Select Model**: Check the model to load in the model table.
2. **Optional LoRA**: If the model supports LoRA, expand and select LoRA adapter.
3. **Click Load**: Click the "Load" button at the top.
4. **Environment Check**:
   - Embedding/Rerank/SD models automatically check environment installation status.
   - If not installed, a popup will prompt to go to the settings page to install.
5. **Wait for Load**: Button shows "Loading...". Status text is more specific before and after the load.
6. **You can stop**: A load in progress can be stopped manually. You do not have to wait for it to finish.
7. **Load Complete**: Model status becomes "Loaded", button becomes "Loaded Model".

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
- **Concurrency Limit**: The agent cannot keep a chat model and a multimodal model loaded at the same time. Chat, embedding, rerank, SD, TTS, and STT can be loaded together.

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
   - **Delete the record only**: The files stay on disk.
   - **Also delete local files**: A model that came from the download station can have its local files removed safely together with the library record. Unload a loaded model first.

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

## System Resource Monitoring

The top-right resource strip shows CPU, memory, and VRAM usage percentages. A bar shows a warning state above 90%. VRAM shows “—” when its usage data is unavailable.

If usage stays high, stop unnecessary tasks or unload unused models before retrying.

---

## Usage Statistics

Open **More → Toolbox → Local Model Usage** to view input tokens, output tokens, and request count. Choose day, week, month, or year, then click **Refresh** to read the latest data.

---

## Notes

### File Format Restrictions

- **Chat**: `.gguf` or `.bin` files, or an HF model directory containing `.safetensors` files.
- **Multimodal**: `.gguf` or `.bin` files.
- **Embedding/Rerank**: Only supports `.safetensors` (Optional directory import).
- **SD Model**: Only supports `.gguf/.safetensors/.bin`.
- Unsupported formats will be rejected during validation phase.

### Environment Dependency Check

Environment is automatically checked when loading the following models:

- **Embedding Model**: Popup guide if not installed.
- **Rerank Model**: Popup guide if not installed.
- **SD Model**: Popup guide if not installed.
- **Custom Export**: Popup guide if not installed.

### Model Load Restrictions

- **Single Model Limit**: Only one base model can be loaded per category.
- **LoRA Limit**:
  - Chat/Multimodal: Max 1 LoRA.
  - SD Model: Multiple LoRA (Unlimited quantity, but limited by VRAM).
- **Concurrency Limit**: Chat and Multimodal cannot be loaded simultaneously. Chat, Embedding, Rerank, SD, TTS, STT can be loaded simultaneously.

### Deletion Protection

- **Loaded Model**: Cannot be deleted, must unload first.
- **Base Model**: Associated LoRA deleted after base model deletion.
- **Local files**: Deleting only the record leaves the files on disk. A downloaded model can also have those local files removed safely.

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

---

After completing this chapter, you can:
- Go to **Chat** feature, use loaded chat model for conversation.
- Go to **Knowledge Base** feature, configure embedding and rerank models for RAG retrieval.
- Go to **Data Processing** feature, use model to generate SFT or RAG datasets.
- Go to **Settings** page, manage environment and system configuration.
