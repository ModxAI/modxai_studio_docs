# Audio to Text Data Processing

> v1.0.0
> 2025-12

This article details how to use the Audio to Text function of ModxAI Studio to batch transcribe audio files into text data.

---

## Table of Contents

- [Feature Introduction](#feature-introduction)
- [Model Preparation](#model-preparation)
- [Processing Flow](#processing-flow)
- [Step-by-Step Details](#step-by-step-details)
- [Output Formats](#output-formats)
- [Notes](#notes)
- [FAQ](#faq)

---

## Feature Introduction

The Audio to Text (Audio2Text) function is based on **faster-whisper**, capable of batch transcribing audio files into text. It supports multiple audio formats and provides various output format choices.

**Applicable Scenarios**:
- Meeting recording transcription
- Video subtitle generation
- Podcast content organization
- Speech dataset construction

**Supported Audio Formats**: `.mp3`, `.wav`, `.m4a`, `.wma`

**Core Features**:
- Supports automatic language detection
- Supports GPU acceleration (CUDA)
- VAD (Voice Activity Detection) filters silent segments
- Real-time progress display and log output
- Multiple output formats (JSON, SRT, HTML, Markdown)

---

## Model Preparation

### Model Requirements

Audio to Text uses **faster-whisper** format models. The model directory must contain the following core files:

| File | Description | Required |
| :--- | :--- | :--- |
| `model.bin` | Model weight file | ✓ |
| `vocabulary.json` | Vocabulary file | ✓ |
| `config.json` | Configuration file | - |
| `tokenizer.json` | Tokenizer file | - |

### Get Model

#### Method 1: Download via Downloader (Recommended)

1. Enter "Download" page.
2. Select "Audio to Text" in category.
3. Select appropriate model version to download:
   - **faster-whisper-base**: Approx 150MB, fast, suitable for general scenarios.
   - **faster-whisper-large-v3**: Approx 3.3GB, highest accuracy, suitable for professional scenarios.

**Available Model List**:

| Model Name | Size | Accuracy | Speed | Applicable Scenario |
| :--- | :--- | :--- | :--- | :--- |
| faster-whisper-base | ~150MB | Medium | Fastest | Daily use, fast transcription |
| faster-whisper-small | ~500MB | Higher | Fast | Balance accuracy and speed |
| faster-whisper-medium | ~1.5GB | High | Medium | Multi-language mixed scenarios |
| faster-whisper-large-v3 | ~3.3GB | Highest | Slower | Professional transcription, low-quality audio |

#### Method 2: Manual Download

Download model files from HuggingFace or ModelScope and unzip to local directory.

- HuggingFace: `https://huggingface.co/Systran/faster-whisper-*`
- ModelScope: `https://www.modelscope.cn/models/Systran/faster-whisper-*`

> ⚠️ **Important Note**:
> 
> Models downloaded via downloader **will not be automatically imported into the Model Library**. You need to **manually select the local model directory** in the processing interface when using.
> 
> Please ensure the selected directory contains `model.bin` file, otherwise the model cannot be loaded.

---

## Processing Flow

Audio to Text processing includes 3 steps:

| Step | Name | Description | Configurable |
| :--- | :--- | :--- | :--- |
| 0 | Load Model | Load faster-whisper model | ✓ |
| 1 | Transcribe | Convert audio to text | ✓ |
| 2 | Output | Generate output file in specified format | ✓ |

**Processing Logic**:
1. Load specified faster-whisper model into memory/VRAM.
2. Scan input directory in reverse order of file modification time.
3. Transcribe audio files one by one, outputting real-time progress.
4. Generate multi-format output files.
5. Automatically unload model to release resources after processing completes.

---

## Step-by-Step Details

### Step 0: Load Model

**Function**: Load faster-whisper model into memory or VRAM.

#### Configurable Parameters

| Parameter | Description | Default | Options/Range |
| :--- | :--- | :--- | :--- |
| **Model Path** | Local model directory path | - | Must contain model.bin |
| **Compute Precision** | Model inference precision | float16 | `float16` / `int8` / `int8_float16` |
| **Compute Device** | Running device selection | auto | `auto` / `cuda` / `cpu` |

**Advanced Parameters** (Collapsible Area):

| Parameter | Description | Default | Range |
| :--- | :--- | :--- | :--- |
| **CPU Threads** | Number of threads in CPU mode | 4 | 1-32 |
| **Worker Processes** | Number of parallel processing processes | 1 | 1-8 |
| **Device Index** | Specify which card to use for multi-GPU | 0 | 0-7 |

**Compute Precision Explanation**:

| Precision Type | Description | Applicable Scenario |
| :--- | :--- | :--- |
| `float16` | Half-precision floating point | GPU recommended, fast speed, high accuracy |
| `int8` | 8-bit integer quantization | CPU recommended, small memory usage |
| `int8_float16` | Mixed precision | Balance speed and accuracy on GPU |

---

### Step 1: Transcribe

**Function**: Convert audio content to text, identify speech and generate text segments with timestamps.

#### Configurable Parameters

| Parameter | Description | Default | Options/Range |
| :--- | :--- | :--- | :--- |
| **Beam Size** | Beam search width during decoding | 5 | 1-10 |
| **Audio Language** | Specify audio language | auto | auto/zh/en/ja/ko/es/fr/de/ru |
| **VAD** | Filter non-speech segments | Enabled | On/Off |

**Advanced Parameters** (Collapsible Area):

| Parameter | Description | Default | Range |
| :--- | :--- | :--- | :--- |
| **Repeat Penalty** | Strength to suppress repetitive generation | 1.1 | 1.0-1.5 |
| **Word Timestamps** | Return timestamp for each word | Disabled | On/Off |
| **Min Silence Duration** | Minimum silence duration recognized by VAD (ms) | 2000 | 100-5000 |

**Parameter Explanation**:

**Beam Size**:
- Larger value means more accurate transcription results, but slower speed.
- Recommended value: 5 for general scenarios, 8-10 for high accuracy requirements.

**Voice Activity Detection (VAD)**:
- When enabled, automatically filters silence and non-speech segments.
- Recommended to enable for audio with poor recording quality or loud background noise.
- If too few segments are identified after enabling, the system will automatically fallback and retry.

**Audio Language**:
- `auto`: Automatically detect language, suitable for single-language audio.
- Specify language: For audio with known language, specifying language can improve accuracy.
- Supported languages: Chinese (zh), English (en), Japanese (ja), Korean (ko), Spanish (es), French (fr), German (de), Russian (ru).

---

### Step 2: Output

**Function**: Save transcription results as files in specified format.

#### Configurable Parameters

| Parameter | Description | Default | Options |
| :--- | :--- | :--- | :--- |
| **Output Format** | Output file format (Multi-select) | json | json/srt/html/md |

---

## Output Formats

Each audio file will generate corresponding output files based on selected formats:

### JSON Format (Default)

Retains complete transcription information, including timestamps and metadata:

```json
{
  "file": "meeting_record.mp3",
  "segments": [
    {
      "index": 0,
      "start": 0.0,
      "end": 3.52,
      "text": "Hello everyone, today we discuss project progress."
    },
    {
      "index": 1,
      "start": 3.52,
      "end": 7.84,
      "text": "First, let Xiao Wang report on development status."
    }
  ],
  "info": {
    "language": "en",
    "language_probability": 0.98
  }
}
```

**Field Explanation**:
- `segments`: Transcription segment array
  - `index`: Segment sequence number
  - `start`/`end`: Start/End time (seconds)
  - `text`: Recognized text content
  - `words`: (Optional) Word-level timestamps, only when enabled
- `info`: Audio information
  - `language`: Detected language
  - `language_probability`: Language detection confidence

### SRT Format

Standard subtitle format, can be used directly in video players:

```srt
1
00:00:00,000 --> 00:00:03,520
Hello everyone, today we discuss project progress.

2
00:00:03,520 --> 00:00:07,840
First, let Xiao Wang report on development status.
```

### HTML Format

Web page format with styles, convenient for previewing in browser:

```html
<!DOCTYPE html>
<html>
<head>
  <title>meeting_record</title>
  <style>
    .seg { margin: 8px 0; }
    .ts { color: #888; margin-right: 8px; }
  </style>
</head>
<body>
  <div class="seg">
    <span class="ts">[0.00 - 3.52]</span>
    Hello everyone, today we discuss project progress.
  </div>
  ...
</body>
</html>
```

### Markdown Format

Concise list format, suitable for document editing:

```markdown
# meeting_record

- [0.00s - 3.52s] Hello everyone, today we discuss project progress.
- [3.52s - 7.84s] First, let Xiao Wang report on development status.
```

---

## Notes

### Model Loading

1. **Model Directory Verification**: Automatically checks if `model.bin` and `vocabulary.json` are included when selecting directory.
2. **VRAM Usage**: large-v3 model requires approx 4GB VRAM, please ensure GPU has sufficient space.
3. **First Load**: First time loading model may take a long time, please wait patiently.

### Transcription Quality

1. **Audio Quality**: Clear audio yields better transcription results.
2. **Background Noise**: Suggest enabling VAD filtering when noise is loud.
3. **Multi-language Mixed**: For multi-language mixed audio, suggest using large model and setting language to auto.

### Resource Management

1. **Auto Unload**: Model automatically unloads after processing completes, releasing memory/VRAM.
2. **Interrupt Processing**: If stopped during processing, model will not unload immediately (to avoid crash), will clean up on next operation.
3. **Temporary Files**: `.transcribing.json` temporary file generated during processing, automatically deleted upon completion.

### Performance Optimization

1. **GPU Acceleration**: Prioritize using CUDA when NVIDIA GPU is available, speed improvement is significant. Suggest installing [cudnn](https://developer.nvidia.com/cudnn).
2. **Precision Selection**:
   - GPU: Recommend `float16`
   - CPU: Recommend `int8` (Faster speed, smaller memory)
3. **Batch Processing**: System processes in reverse order of file modification time, latest files first.

---

## FAQ

### Q: Model load failed, prompting model.bin not found?

A: Please check the following:
1. Confirm correct model directory is selected (the layer containing model.bin).
2. If unzipped model, ensure no extra nested directories.
3. Whether model file download is complete (compare file size).

### Q: Transcription result is empty or very few segments?

A: VAD filtering might be too aggressive. System will automatically try to disable VAD and re-transcribe. If problem persists:
1. Check if audio file is normal (can play with player).
2. Try manually disabling VAD and process again.
3. For audio with lots of background music, recognition might be affected.

### Q: Why doesn't the downloaded model appear in Model Library?

A: This is by design. faster-whisper models are specialized formats, different from llama.cpp models, and are not automatically imported into the unified Model Library. You need to manually select the model directory path in the processing interface when using.

### Q: What if GPU VRAM is insufficient?

A: Try the following solutions:
1. Use smaller model (e.g., base or small).
2. Change compute precision to `int8_float16`.
3. Switch to CPU mode (speed will slow down).
4. Close other programs occupying VRAM.

### Q: Transcription speed is very slow?

A: Methods to improve speed:
1. Ensure using GPU instead of CPU.
2. Use smaller model (base model is fastest).
3. Lower beam_size to 3-4.
4. Use `float16` precision.

### Q: Is real-time transcription supported?

A: Current version is batch processing mode, does not support real-time streaming transcription. Audio files need to be fully uploaded before processing starts.

### Q: How to choose appropriate model?

A: Choose according to your needs:

| Need | Recommended Model |
| :--- | :--- |
| Fast transcription, low accuracy requirement | faster-whisper-base |
| Daily meeting recording | faster-whisper-small |
| Multi-language mixed scenario | faster-whisper-medium |
| Professional grade transcription, noisy environment | faster-whisper-large-v3 |

---

*For other questions, please refer to project documentation or ask in the community.*

