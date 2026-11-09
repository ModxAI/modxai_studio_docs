# Data Processing

> v1.1.11
> 2026-11

Turn documents, code, or audio into training data, knowledge bases, or transcripts. Choose a data type, then configure the processing steps and output directory.

Open the **Self Improvement** submenu and choose **Dataset**.

## Table of Contents

- [Environment Requirements](#environment-requirements)
- [Feature Overview](#feature-overview)
- [Interface Layout](#interface-layout)
- [Dataset Types](#dataset-types)
- [Processing Flow](#processing-flow)
- [Output Directory and Files](#output-directory-and-files)
- [AI Node Features](#ai-node-features)
- [Resume from Breakpoint](#resume-from-breakpoint)
- [Notes](#notes)

---

## Environment Requirements

**This feature requires installing CPU or GPU environment dependencies in the settings.**
- Go to the **Settings** page of ModxAI Studio
- Under **System**, in Environment Configuration, select and install the required environment (CPU/GPU)
- After installation, return to the data processing page to use the related features

---

## Feature Overview

Choose a data type, then convert, clean, and export your files. Enable AI synthesis or quality filtering when needed. If SFT or RAG processing stops, resume from a selected step.

---

## Interface Layout

### Outer Tabs

Three main categories are arranged vertically on the left side of the page:

| Tab | Description |
| :--- | :--- |
| **RAG** | RAG knowledge base data processing, generating vectorized data for retrieval augmentation. |
| **SFT** | Supervised Fine-Tuning data processing, generating datasets for model training. |
| **MM** | Multimedia processing, currently supports audio-to-text. |

### Inner Tabs

Each outer category contains different sub-functions:

**SFT Category**:
- **Text**: Process document data (PDF, Word, HTML, etc.).
- **Code**: Process C/C++, Shader, and other code files.
- **Progress Monitor**: View current processing progress and logs.

**RAG Category**:
- **Text**: Process RAG knowledge base source files.
- **Progress Monitor**: View vector generation and storage progress.

**MM Category**:
- **Audio to Text**: Transcribe audio files to text.
- **Progress Monitor**: View transcription progress.

---

## Dataset Types

### SFT Dataset

Used to generate supervised fine-tuning training data, supporting three source data types:

**Text Dataset**:
- Supported formats: PDF, Word (.doc/.docx), PowerPoint (.ppt/.pptx), Excel (.xls/.xlsx), HTML, Text files (.txt/.md/.csv/.json/.xml), ZIP archives.
- Output format: Standardized JSONL training data.

**Code Dataset**:
- Supported formats: C/C++ (.c/.cpp/.cc/.h/.hpp), Shader (.hlsl/.glsl/.cg/.ush/.usf).
- Output format: Structured code training data.

**Structured Dataset** (file type "JSON"):
- Supported formats: JSON, JSONL
- Map fields for the training format: instruction, input, and output for SFT; text for pretraining; prompt, chosen response, and rejected response for DPO. See [SFT Data Processing](./sft.md) for fields and examples.
- Output format: Standardized JSONL. The export format can be SFT (`sft_chatml`), pretraining (`pretrain`), or DPO (`dpo`). Field mapping is described in [SFT Data Processing](./sft.md).

### RAG Dataset

Used to generate vectorized knowledge bases required for retrieval augmentation:

- Supported formats: Same as text datasets, including structured JSON/JSONL.
- Output format: Vector embedding data, stored in vector database.

### Audio to Text

Transcribe audio files into structured text:

- Supported formats: MP3, WAV, M4A, WMA.
- Output formats: JSON, SRT subtitles, HTML, Markdown.

---

## Processing Flow

### SFT Text/Code Processing Steps

This table applies to unstructured text and code. Structured JSON training data runs only Steps 0 and 10.

| Step | Name | Description | Required |
| :--- | :--- | :--- | :--- |
| 0 | Convert | Parse input files and convert to unified format. | ✓ |
| 1 | Clean | Clean and preprocess data. | ✓ |
| 2 | Split | Split long data chunks. | ✓ |
| 3 | Analyze | Quality analysis and complexity assessment. | ✓ |
| 4 | AI Synthesis | Use AI models to synthesize enhanced data. | Optional |
| 5 | AI Synthesis Parse | Parse AI-generated data. | Optional |
| 7 | Integrate | Integrate all processed data. | ✓ |
| 8 | AI Filter | Use AI for quality assessment filtering. | Optional |
| 9 | AI Filter Parse | Parse filtering results. | Optional |
| 10 | Output | Output final training data. | ✓ |

### RAG Processing Steps

| Step | Name | Description | Required |
| :--- | :--- | :--- | :--- |
| 0 | Convert | Parse input files. | ✓ |
| 1 | Clean | Clean and preprocess. | ✓ |
| 2 | Split | Split data chunks. | ✓ |
| 3 | Analyze | Quality analysis. | ✓ |
| 4 | AI Synthesis | Generate summaries for text blocks with a chat model. | Optional |
| 5 | AI Parsing | Parse the generated summaries. | Optional |
| 11 | Generate Vector | Use an embedding model to generate embeddings. | ✓ |
| 12 | Store Vector | Store the vectors in the knowledge base. | ✓ |

### Audio to Text Processing Steps

| Step | Name | Description | Required |
| :--- | :--- | :--- | :--- |
| 0 | Load Model | Load audio-to-text model. | ✓ |
| 1 | Transcribe | Transcribe audio to text. | ✓ |
| 2 | Output | Output formatted text data. | ✓ |

### Workflow Templates

The system presets multiple workflow templates for quick selection of common step combinations:

- **AI Synthesis**: Includes AI synthesis steps, suitable for scenarios needing data expansion.
- **AI Quality**: Includes AI synthesis and AI filtering steps, suitable for pursuing high-quality data.
- **Custom**: Manually select steps to execute.

> For unstructured text and code, set **Export Format** to `pretrain` in Step 4 (AI Synthesis). For structured JSON, use the page's **Training Format** selection.

---

## Output Directory and Files

### SFT Dataset Output

Text output normally uses `output directory/datasets/text_dataset/final`. C/C++ uses `datasets/code_dataset/final`, and Shader uses `datasets/shader_dataset/final`. Existing tasks may retain an earlier layout; follow progress messages and the output manifest.

Example SFT/pretraining output:

```text
final/
├── training_dataset_manifest.json
├── training_validation.jsonl     # When a validation report is available
├── training_metadata.jsonl       # When additional metadata is present
├── train/
│   ├── data.jsonl
│   └── final_dataset.jsonl
├── eval/
│   ├── data.jsonl
│   └── final_dataset.jsonl
└── test/
    ├── data.jsonl
    └── final_dataset.jsonl
```

Only nonempty splits are written. `data.jsonl` holds normalized raw samples; `final_dataset.jsonl` holds tokenized training records. Step 10 writes JSONL, without a JSON/NPY save-format selector.

For training, select `train/final_dataset.jsonl`. For evaluation during training, select `eval/final_dataset.jsonl` and enable an evaluation strategy. Pretraining `data.jsonl` cannot replace the tokenized training file.

DPO writes only `data.jsonl` per nonempty split, containing prompts, chosen responses, and rejected responses. The current Training module does not support DPO training.

### Output File Naming

Step 10 writes `training_dataset_manifest.json`, listing data format, split information, counts, validation summary, and actual artifact paths. Check this manifest before selecting a training file.

Steps can produce several intermediate files; `opt{N}_dataset.jsonl` is not a universal filename for every step. See [SFT Data Processing](./sft.md#output-formats) for fields and file purposes.

### Output Format Selection

Structured JSON uses the page's Training Format selection. Unstructured text and code use export settings in the selected processing steps. SFT, pretraining, and DPO samples can be exported; see [SFT Data Processing](./sft.md) for file purposes and current training support.

---

## AI Node Features

### Model Requirements

Using AI nodes (Step 4, 8) requires ensuring:

1. **Available Chat Model in Model Library**: AI nodes use chat models for data synthesis and filtering.
2. **Select Model in Step Parameters**: Click the settings button of the step to select the AI model to use.

### Processing Features
1. Stable and efficient processing of **large-scale data**
2. **Progress tracking and recovery**
3. **Automatic repair of abnormal data blocks**, attempting to automatically repair abnormal data blocks first after restarting
4. **Incremental processing** to avoid reprocessing completed data
5. **Model status monitoring**, automatic model restart upon detecting anomalies, and timeout retry mechanisms

### Model Reuse Mechanism

Local AI nodes can reuse a loaded llama.cpp chat model when the configuration matches.

- Matching includes the base model, LoRA, context size, and runtime parameters, not only the model name.
- Finishing a data-processing step does not itself unload the shared model.
- Data processing and chat share compute resources, so chat requests may wait or respond more slowly.
- Switching to a different configuration may be blocked while other tasks use the model. Wait for those tasks to finish first.

### RAG Embedding Model

Step 11 (Generate Vector) of RAG data processing requires:

1. **Embedding Model in Model Library**.
2. **Install Environment in Settings**: Embedding models require CPU or GPU environment support.
3. Select the embedding model in step parameters.

---

## Resume from Breakpoint

This section applies to SFT and RAG text and code workflows. Audio transcription cannot resume from its interruption point; starting again retranscribes from the beginning and may overwrite existing results with matching names. See [Audio to Text](./audio2text.md).

### Usage Scenarios

- Restart from the failed step after a step execution failure.
- Re-execute only the modified step and subsequent steps after modifying parameters.
- Continue from the last stop position after interrupting processing.

### Operation Method

1. **Uncheck Completed Steps**: In the step selection area, uncheck steps that do not need to be re-executed.
2. **Keep Steps to Execute**: Check the step to restart from and its subsequent steps.
3. **Click Start Processing**: The system will start execution from the first checked step.

### Notes

- Ensure that the output data of the preceding steps exists and is valid.
- If parameters of preceding steps are modified, it is recommended to re-execute that step.
- Data dependencies exist between steps; skipping intermediate steps may lead to incomplete data.

---

## Notes

### Environment Requirements

| Feature | Environment Requirement |
| :--- | :--- |
| SFT Text Processing | No **Settings** environment needed (Basic steps) |
| SFT Code Processing | Environment needed in **Settings** (Parsing step depends on libclang) |
| SFT AI Node | Chat model must be imported |
| RAG AI Node | Chat model must be imported |
| RAG Vector Generation | Embedding model must be imported, Environment needed in **Settings** + Embedding model |
| Audio to Text | Environment needed in **Settings** (GPU environment recommended) |

### FAQ

**Q: Can I switch to other pages during processing?**

A: Yes. Processing tasks run in the background, and switching pages does not affect processing progress. You can view the latest status when returning to the data processing page.

**Q: How to view processing logs?**

A: Switch to the "Progress Monitor" tab to view real-time processing logs. Supports filtering display by log level (INFO/WARNING/ERROR).

**Q: What if AI node processing is slow?**

A: AI nodes need to call AI models for inference, and processing speed depends on model size and hardware configuration. Suggestions:
- Use smaller models for fast processing.
- Ensure GPU is available to accelerate inference.
- Reduce the amount of data to be processed for testing.

---

## Subsequent Documentation

- [SFT Data Processing](./sft.md) - Detailed guide for SFT text/code data processing.
- [RAG Data Processing](./rag.md) - Detailed guide for RAG knowledge base generation.
- [Audio to Text](./audio2text.md) - Detailed guide for audio transcription function.
