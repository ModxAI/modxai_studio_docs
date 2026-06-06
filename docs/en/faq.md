# Frequently Asked Questions

> v1.1.0
> 2026-05

This page summarizes common issues and solutions encountered while using ModxAI Studio.

---

## Quick Start

### What should I do after first launch?

1. Go to the **Model Library**, download or import a chat model.
2. Load the model, then navigate to the **Chat** page to start using it.
3. To use advanced features (training, data processing, etc.), install the environment in **Settings** first.

### Which features require environment installation in Settings?

| Feature | Environment Requirement |
|---------|------------------------|
| Text Chat (llama.cpp) | **Not required** |
| Text Chat (HF Transformers) | **Requires** CPU or GPU environment |
| Multimodal Chat | **Not required** (uses llama.cpp) |
| Embedding / Rerank Models | **Requires** CPU or GPU environment |
| SD Image Generation | **Requires** CPU or GPU environment |
| TTS Speech Synthesis | **Requires** CPU or GPU environment (GPU recommended) |
| STT Speech to Text | **Requires** CPU or GPU environment (GPU recommended) |
| Model Training | **Requires** GPU environment (recommended) |
| Agent (Browser/Web Tools) | **Requires** "Full" or "Agent" capability installation |
| Audio to Text | **Requires** GPU environment (recommended) |

---

## Model Management

### Model downloads are very slow. What can I do?

- **Switch download source**: Use ModelScope for users in China, and Hugging Face for overseas users.
- **Check network**: Ensure a stable internet connection.
- **Batch download**: For large models, download quantized versions in parts first.

### Imported model is not showing up?

- Click the **Refresh button** to update the model list.
- Check the file format:
  - Chat/Multimodal models: `.gguf` or `.bin`
  - Embedding/Rerank models: `.safetensors` (import the directory)

### Model failed to load?

Common causes and solutions:

| Cause | Solution |
|-------|----------|
| Insufficient memory | Close other applications, or choose a smaller quantized version (Q2_K, Q3_K_S). |
| Environment not installed | Embedding/Rerank/SD models require environment installation in **Settings** first. |
| Corrupted file | Re-download the model file. |
| Unsupported format | Ensure the file format meets requirements. |

### LoRA is incompatible?

LoRA must match the base model architecture. Ensure:
- The LoRA was trained for that specific base model.
- The LoRA format is correct (chat models use `.gguf` format LoRA).

### How many models can be loaded at the same time?

- **Same category**: Only one base model can be loaded per category.
- **Different categories**: Chat, Embedding, Rerank, SD, TTS, and STT models can be loaded simultaneously.
- **Special restriction**: Chat and Multimodal models cannot be loaded at the same time.

### What is the difference between HF Transformers and GGUF models?

| Aspect | GGUF (llama.cpp) | HF Transformers |
|--------|-------------------|-----------------|
| Environment | No installation needed | Environment must be installed |
| Inference Speed | Fast | Depends on hardware |
| Precision Control | Fixed quantization levels | Flexible float16/32 selection |
| Multimodal | Supported | Not supported (text only) |


### How can other applications call my loaded model?

Once a model is loaded, ModxAI Studio automatically provides an OpenAI-compatible HTTP API (`/v1/chat/completions`). Other devices or applications on the same local network can call it directly. See [Model Library - LAN Inference Service](../features/models/overview.md#lan-inference-service).

### Can a multimodal model use a knowledge base (RAG)?

No. RAG retrieval is unavailable in multimodal dialogue mode. For knowledge base Q&A, switch to a text-only chat model.

---

## Agent

### What are the prerequisites for the agent?

1. Load a chat model for the main role.
2. Install "Full" or "Agent" environment capabilities in **Settings** (required for browser and web tools).
3. Switch to agent mode in the chat interface.

### What model size is suitable for the agent?

The ModxAI Studio agent framework is optimized for local small models. Models with 1B-3B parameters can handle simple multi-step tasks within a 32K context. Larger models (7B+) can handle more complex tasks. Multimodal models like Qwen3.5 are recommended.

### What can the agent do?

- Search the internet automatically for information.
- Browse and extract web page content.
- Read and write local files.
- Execute terminal commands.
- Manage task lists and track progress.
- Remember preferences.

### Is agent data secure?

Yes. All agent operations are executed in a local sandbox. File operations are restricted to authorized directories, and the browser runs in an isolated environment. Your data is never uploaded to any cloud service. However, when using online models, please assess based on the respective service provider's terms.

---

## AI Chat

### Chat is not responding?

1. Confirm the model is loaded successfully (status shows "Loaded").
2. Check the system resource monitor at the bottom of the page (excessive memory/CPU usage).
3. Try starting a new conversation.

### AI reply is empty or shows "Model did not return a valid response"?

Suggestions:
- Rephrase your question for clarity.
- Check if inference parameters are reasonable (temperature should not be too high).
- Confirm the model file is intact.

### Multimodal chat attachment button is not showing?

You must load a **multimodal model** to use image-text chat. Standard chat models do not support image input.

### RAG inference prompt says it's unavailable?

Before using RAG:
1. First create a RAG knowledge base in the **Data Processing** module.
2. Select the knowledge base in inference parameters.
3. It is recommended to load both embedding and reranking models, but be mindful of resource usage.

### Image format limitations?

- **Supported**: JPG, PNG
- **Not supported**: GIF, BMP, WebP, etc.

---

## Data Processing

### Can I switch pages during processing?

Yes. Processing tasks run in the background; switching pages does not affect progress. Check status under "Progress Monitor" when you return.

### AI node processing is very slow. What can I do?

AI nodes require model inference; speed depends on hardware:
- Use a smaller model for processing.
- Ensure GPU is available for acceleration.
- Test with a small amount of data first.

### How do I resume from a checkpoint?

1. Uncheck completed steps.
2. Keep the steps that need re-execution checked.
3. Ensure the output data of preceding steps exists.

### What is structured data input?

In SFT or RAG data processing, selecting "JSON" as the file type enters structured data mode. Ideal for JSON/JSONL data with fixed fields (e.g., news, business records), allowing direct field mapping to standard formats without going through the unstructured cleaning pipeline.

### What is the difference between sft_chatml / pretrain / dpo export formats?

| Format | Output | Use Case |
|--------|--------|----------|
| sft_chatml | system/user/assistant dialogue format | SFT fine-tuning |
| pretrain | Plain text corpus | Pre-training / Continued pre-training |
| dpo | prompt/chosen/rejected preference pairs | DPO preference alignment training |

Switch the export format parameter in the AI synthesis step. Pre-training no longer has a separate step.

### What is Chain of Thought (CoT) mode?

When enabled, the system automatically injects reasoning prompts to guide the model to output its thought process. Suitable for data that requires reasoning steps to be preserved.

---

## Model Training

### What configuration is needed for training?

| Model Size | VRAM Requirement (LoRA) | VRAM Requirement (Full) |
|------------|------------------------|-------------------------|
| < 1B | 4GB | 16GB |
| 1-3B | 6GB | 24GB |
| 3-7B | 8GB | 40GB |
| 7-14B | 12GB | 80GB |

### What should I fill in "Training Model Directory"?

This parameter requires a **HuggingFace format model directory** (containing `.safetensors` and `config.json`), **not** a GGUF inference model.

### How to resume training after interruption?

On the training preparation page, set "Resume Training Checkpoint Path" to the last saved checkpoint directory.

---

## SD Image Generation

### The generate button does nothing?

Check:
1. Whether an SD model is loaded.
2. Whether the prompt is empty.
3. Whether there is an ongoing generation task.

### Unsatisfactory generation quality?

Try the following adjustments:
- Increase sampling steps (recommended 30-50).
- Adjust CFG scale (recommended 7-12).
- Use more detailed prompts.
- Add negative prompts to exclude unwanted elements.

### Image-to-image result differs too much from the original?

Lower the **Denoising Strength** parameter, recommended range 0.3-0.6.

### Video generation fails or freezes?

- Reduce the number of frames (recommended 20-40 frames).
- Lower the resolution (recommended 512x512).
- Check available VRAM.

### Resource Usage Recommendations

| VRAM | Recommended Settings |
|------|---------------------|
| 4GB | 512x512, steps 20-30 |
| 8GB | 768x768, steps 30-50 |
| 12GB+ | 1024x1024, steps 50+ |

---

## TTS Speech Synthesis

### TTS function is unavailable?

Check the following:
1. Whether the environment has been installed in Settings.
2. Whether the status was refreshed after installation.
3. Whether TTS capability shows as available.
4. Whether a TTS model is loaded.

### TTS dependency installation is very slow?

- The misaki library installs from PyPI's official source; network speeds in China may be slow.
- Keep the network stable during installation.
- If it times out, retry by reinstalling the environment.

### TTS capability shows as unavailable?

- The spaCy model required by TTS is downloaded asynchronously and may be delayed by tens of seconds to minutes.
- After the main environment is installed, wait a moment and refresh the environment status.
- If still unavailable, try restarting the application or repairing the installation.

### Generated speech sounds unnatural?

- Check the input text for grammatical errors.
- Try different voice presets.
- Adjust the speech speed parameter.
- Ensure the language setting matches the text content.

### Does TTS only support Chinese and English?

Yes. The current version only supports Chinese and English. Other languages like Japanese require additional dependency libraries that are not currently bundled.

### TTS inference fails?

- Some AMD CPUs may have compatibility issues.
- An NVIDIA GPU environment is recommended.
- Check if the model is loaded correctly.

---

## Environment Configuration

### Environment installation failed?

Common solutions:
- **Switch mirror source**: Users in China should select Alibaba Cloud or Tencent Cloud.
- **Check disk space**: Environment installation requires approximately 5-10 GB of space.
- **Check network**: Ensure access to the download source.
- **Retry installation**: Click the "Reinstall" button.

### Which environment should I choose: CPU or GPU?

- **With NVIDIA GPU**: Choose GPU environment for significantly faster inference and training.
- **Without dedicated GPU**: Choose CPU environment; most features will still work.

### What is the difference between Full / Agent / Voice Synthesis installation capabilities?

| Capability | Contents | Use Case |
|------------|----------|----------|
| Full | Training + Agent + TTS all dependencies | Users who need all features |
| Agent | Core libraries + browser environment | Users who only need agent web and web tools |
| Voice Synthesis | Core libraries + TTS dependencies | Users who only need speech synthesis |

### Agent browser tools don't work?

"Full" or "Agent" installation capability is required. When only "Voice Synthesis" is installed, the agent's browser and web tools are unavailable.

### TTS still unavailable after installation?

In "Full" mode, TTS dependencies are downloaded asynchronously. After completion, wait and refresh the environment status. If the async download fails, switch to "Voice Synthesis" mode for an independent reinstall (synchronous download).

### Do I need to restart after environment installation?

No. Features are immediately available after installation. For TTS async installation, based on network conditions, it may take a few minutes before refreshing confirms availability.

---

## Other Questions

### Where is data stored?

- **Model files**: Saved in the directory you specified.
- **Chat history**: Stored in the application's local database.
- **Generated images**: Default to the system temp directory; save location can be specified in parameters.

> Uninstalling the application does not delete the local database or model files.

### How to completely clear data?

Manual deletion is required:
- Model file directory.
- Application data directory (`C:\Users\<username>\AppData\Local\ModxAI`).

### How to provide feedback?

- **In-app feedback**: Settings -> Help & Feedback -> Submit Feedback
- **Email**: support@modyu.tech

---

## Contact Support

If the above did not resolve your issue:

- **Email**: support@modyu.tech
- **Docs**: This help center
- **In-app**: Settings -> Submit Feedback
