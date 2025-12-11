# FAQ

> v1.0.0
> 2025-12

This document summarizes common questions and solutions when using ModxAI Studio.

---

## Quick Start

### What should I do after the first launch?

1. Go to the **Model Library**, download or import a chat model.
2. After loading the model, go to the **Chat** page to start using it.
3. If you need to use advanced features (Training, Data Processing, etc.), please install environment in **Settings** first.

### Which features require environment installation in Settings?

| Feature | Environment Requirement |
|---------|-------------------------|
| Text Chat | **No** (Uses llama.cpp) |
| Multimodal Chat | **No** (Uses llama.cpp) |
| Embedding/Rerank Models | **Yes** (CPU or GPU environment) |
| SD Image Generation | **Yes** (CPU or GPU environment) |
| TTS Speech Synthesis | **Yes** (CPU or GPU environment, GPU recommended) |
| Model Training | **Yes** (GPU environment recommended) |
| Audio to Text | **Yes** (GPU environment recommended) |

---

## Model Management

### Why is model download very slow?

- **Switch Download Source**: Users in Chinese mainland are recommended to use ModelScope, while overseas users should use Hugging Face.
- **Check Network**: Ensure your network connection is stable.
- **Download in Parts**: For large models, you can choose to download specific quantized versions first.

### Why doesn't the model show up after importing?

- Click the **Refresh Button** to update the model list.
- Check if the file format is correct:
  - Chat/Multimodal Models: `.gguf` or `.bin`
  - Embedding/Rerank Models: `.safetensors` (requires importing the directory)

### Why did model loading fail?

Common causes and solutions:

| Cause | Solution |
|-------|----------|
| Insufficient Memory | Close other applications, or choose a smaller quantized version (Q2_K, Q3_K_S). |
| Environment Not Installed | Embedding/Rerank/SD models require environment installation in **Settings** first. |
| File Corrupted | Re-download the model file. |
| Unsupported Format | Confirm the file format meets requirements. |

### What if LoRA is incompatible?

LoRA must match the base model architecture. Please ensure:
- The LoRA was trained for that specific base model.
- The LoRA format is correct (Chat models use `.gguf` format LoRA).

### How many models can be loaded at the same time?

- **Same Category**: Only one base model can be loaded per category.
- **Different Categories**: Chat, Embedding, Rerank, and SD models can be loaded simultaneously.
- **Special Restriction**: Chat and Multimodal models cannot be loaded at the same time.

---

## AI Chat

### Why is there no response in chat?

1. Confirm the model has been successfully loaded (Status shows "Loaded").
2. Check the system resource monitor at the bottom of the page (Is Memory/CPU overloaded?).
3. Try creating a new chat and retry.

### Why is the AI reply empty or showing "Model returned no valid reply"?

Suggestions:
- Rephrase your question to make the description clearer.
- Check if inference parameters are reasonable (Temperature should not be too high).
- Confirm the model file is intact.

### Why is the attachment button not showing in Multimodal Chat?

You must load a **Multimodal Model** to use the image-text chat feature. Ordinary chat models do not support image input.

### Why can't I use the RAG switch?

Before using the RAG feature, you need to:
1. Create a RAG Knowledge Base in the **Data Processing** module first.
2. Select the Knowledge Base in the inference parameters.
3. It is recommended to load an Embedding Model (and optionally a Rerank Model), but be mindful of resource usage.

### What are the image format restrictions?

- **Supported**: JPG, PNG
- **Not Supported**: GIF, BMP, WebP, etc.

---

## Data Processing

### Can I switch pages during processing?

Yes. Processing tasks run in the background, and switching pages does not affect progress. You can check the status in "Progress Monitor" when you return.

### Why is AI Node processing very slow?

AI Nodes need to call models for inference, and speed depends on hardware configuration:
- Use smaller models for processing.
- Ensure GPU is available to accelerate inference.
- Test with a small amount of data first.

### How to save quota consumption?

- Set data splitting parameters reasonably to avoid generating too many fragmented data pieces.
- Turn off unnecessary AI steps (such as AI Filtering).
- Test the flow with a small dataset first.

### How to resume from a breakpoint?

1. Uncheck the steps that have been completed.
2. Keep the steps that need to be re-executed checked.
3. Ensure the output data from the previous steps exists.

---

## Model Training

### What configuration is needed for training?

| Model Scale | VRAM Requirement (LoRA) | VRAM Requirement (Full) |
|-------------|-------------------------|-------------------------|
| Under 1B | 4GB | 16GB |
| 1-3B | 6GB | 24GB |
| 3-7B | 8GB | 40GB |
| 7-14B | 12GB | 80GB |

### What to fill in "Training Model Directory"?

This parameter requires specifying a **HuggingFace format model directory** (containing `.safetensors` and `config.json`), **NOT** a GGUF inference model.

### Does saving checkpoints consume quota?

Yes, saving each checkpoint consumes 1 training quota. Suggestions:
- Set a reasonable save interval (e.g., every 100 steps).
- Set a maximum number of saved checkpoints.

### How to resume after training interruption?

In the Training Preparation page, set "Resume Training Checkpoint Path" to the directory of the last saved checkpoint.

---

## SD Image Generation

### Why is there no reaction when clicking the Generate button?

Check:
1. Is an SD model loaded?
2. Is the prompt empty?
3. Is there an ongoing generation task?

### Why is the generation quality not ideal?

Try the following adjustments:
- Increase sampling steps (Recommended 30-50).
- Adjust CFG scale (Recommended 7-12).
- Use more detailed prompts.
- Add negative prompts to exclude unwanted elements.

### Why is the Image-to-Image effect too different from the original image?

Lower the **Denoising Strength** parameter, recommended 0.3-0.6.

### Why does video generation fail or get stuck?

- Reduce video frames (Recommended 20-40 frames).
- Reduce resolution (Recommended 512x512).
- Check if VRAM is sufficient.

### Resource Usage Suggestions

| VRAM | Recommended Settings |
|------|----------------------|
| 4GB | 512x512, Steps 20-30 |
| 8GB | 768x768, Steps 30-50 |
| 12GB+ | 1024x1024, Steps 50+ |

---

## TTS Speech Synthesis

### TTS feature unavailable?

Check the following:
1. Has environment been installed in Settings
2. Has status been refreshed after environment installation
3. Is TTS capability shown as available
4. Has TTS model been loaded

### TTS dependency installation is slow?

- misaki library is installed from PyPI official source, may be slow on domestic networks
- Keep network stable during installation
- If timeout fails, retry by reinstalling environment

### TTS capability shows as unavailable?

- spaCy models that TTS depends on are downloaded asynchronously, may be delayed by tens of seconds to several minutes
- After main environment installation completes, please wait and refresh environment status
- If still unavailable, try restarting application or repair installation

### Generated speech sounds unnatural?

- Check if input text has grammatical errors
- Try different voice presets
- Adjust speed parameter
- Ensure language setting matches text content

### TTS only supports Chinese and English?

Yes, current version only supports Chinese and English. Other languages like Japanese require additional dependencies, not built-in yet.

### TTS inference failed?

- Some AMD CPUs may have compatibility issues
- Recommend using NVIDIA GPU environment
- Check if model is correctly loaded

---

## Environment Configuration

### Why did environment installation fail?

Common solutions:
- **Switch Mirror Source**: Users in Chinese mainland should choose Aliyun or Tencent Cloud.
- **Check Disk Space**: Environment installation requires about 5-10GB of space.
- **Check Network**: Ensure access to the download source.
- **Retry Installation**: Click the "Reinstall" button.

### Which environment to choose: CPU or GPU?

- **With NVIDIA Graphics Card**: Choose GPU environment for significantly faster inference and training.
- **No Dedicated Graphics Card**: Choose CPU environment; most features will still work normally.

### Do I need to restart after environment installation?

No. Related features are available immediately after installation completes.

---

## License & Quota

### What are the limitations of the Free Version?

The Free Version has monthly quota limits for advanced features, including:
- Loading Embedding/Rerank/Multimodal/SD models.
- Number of data generation items.
- Saving training checkpoints.

Basic chat features (Chat Models) are free and unlimited.

### How to check quota usage?

Quota information is displayed in the application's top toolbar.

### Why did registration code activation fail?

- Check if the registration code was entered correctly.
- Confirm network connection is normal.
- Contact support to confirm the status of the registration code.

---

## Other Questions

### Where is data stored?

- **Model Files**: Stored in the directory you specified.
- **Chat History**: Stored in the application's local database.
- **Generated Images**: Default is the system temporary directory; you can specify a save location in parameters.

> Uninstalling the application will not delete the local database and model files.

### How to completely clear data?

You need to manually delete:
- Model file directory.
- Application data directory (`C:\Users\<Username>\AppData\Local\ModxAI` on Windows, or corresponding path on macOS).

### How to report issues?

- **In-App Feedback**: Settings -> Help & Feedback -> Submit Feedback.
- **Email**: support@modyu.tech

---

## Contact Support

If the above content does not solve your problem:

- **Email**: support@modyu.tech
- **Documentation**: This Help Center.
- **In-App**: Settings -> Submit Feedback.
