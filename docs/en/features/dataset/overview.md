# Data Processing

> v1.0.0
> 2025-12

This article introduces how to use the **Data Processing** feature in ModxAI Studio, helping you convert raw data into standardized datasets for SFT fine-tuning, RAG retrieval, and audio transcription.

## Table of Contents

- [Environment Requirements](#environment-requirements)
- [Feature Overview](#feature-overview)
- [Interface Layout](#interface-layout)
- [Dataset Types](#dataset-types)
- [Processing Flow](#processing-flow)
- [Quota Consumption Rules](#quota-consumption-rules)
- [Output Directory and Files](#output-directory-and-files)
- [AI Node Features](#ai-node-features)
- [Resume from Breakpoint](#resume-from-breakpoint)
- [Notes](#notes)

---

## Environment Requirements

**This feature requires installing CPU or GPU environment dependencies in the settings.**
- Go to the **Settings** page of ModxAI Studio
- In the **Environment Management** tab, select and install the required environment (CPU/GPU)
- After installation, return to the data processing page to use the related features

---

## Feature Overview

The Data Processing module is one of the core features of ModxAI Studio, providing a complete conversion process from raw data to training datasets. Key features include:

- **Multi-Data Type Support**: Supports text, code, RAG knowledge bases, audio-to-text, and other data types.
- **Pipeline Processing**: Adopts a step-by-step pipeline architecture, where each step can be configured independently.
- **AI Enhancement Capabilities**: Implements advanced functions such as intelligent data synthesis and quality filtering through AI nodes.
- **Resume from Breakpoint**: Allows restarting from a specific step after a failure, avoiding re-execution of completed steps.
- **Quota Management**: Each data generation during processing consumes free quota.

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

Used to generate supervised fine-tuning training data, supporting two source data types:

**Text Dataset**:
- Supported formats: PDF, Word (.doc/.docx), PowerPoint (.ppt/.pptx), Excel (.xls/.xlsx), HTML, Text files (.txt/.md/.csv/.json/.xml), ZIP archives.
- Output format: Standardized JSONL training data.

**Code Dataset**:
- Supported formats: C/C++ (.c/.cpp/.cc/.h/.hpp), Shader (.hlsl/.glsl/.cg/.ush/.usf).
- Output format: Structured code training data.

### RAG Dataset

Used to generate vectorized knowledge bases required for retrieval augmentation:

- Supported formats: Same as text datasets.
- Output format: Vector embedding data, stored in vector database.

### Audio to Text

Transcribe audio files into structured text:

- Supported formats: MP3, WAV, M4A, WMA.
- Output formats: JSON, SRT subtitles, HTML, Markdown.

---

## Processing Flow

### SFT Text/Code Processing Steps

| Step | Name | Description | Required |
| :--- | :--- | :--- | :--- |
| 0 | Convert | Parse input files and convert to unified format. | ✓ |
| 1 | Clean | Clean and preprocess data. | ✓ |
| 2 | Split | Split long data chunks. | ✓ |
| 3 | Analyze | Quality analysis and complexity assessment. | ✓ |
| 4 | AI Synthesis | Use AI models to synthesize enhanced data. | Optional |
| 5 | AI Synthesis Parse | Parse AI-generated data. | Optional |
| 6 | Convert to Pretrain | Convert to pre-training data format. | Optional |
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
| 4 | Generate Vector | Use embedding model to generate embeddings. | ✓ |
| 5 | Store Vector | Parse and store in vector database. | ✓ |

### Audio to Text Processing Steps

| Step | Name | Description | Required |
| :--- | :--- | :--- | :--- |
| 0 | Load Model | Load audio-to-text model. | ✓ |
| 1 | Transcribe | Transcribe audio to text. | ✓ |
| 2 | Output | Output formatted text data. | ✓ |

### Workflow Templates

The system presets multiple workflow templates for quick selection of common step combinations:

- **AI Enhancement Mode**: Includes AI synthesis steps, suitable for scenarios needing data expansion.
- **AI Quality Mode**: Includes AI synthesis and AI filtering steps, suitable for pursuing high-quality data.
- **Pretrain Mode**: Generate pre-training format data.
- **Custom Mode**: Manually select steps to execute.

---

## Quota Consumption Rules

The data processing process consumes free quota according to the following rules:

### Free Steps

The following steps **do not consume** quota:
- Step 0 (Convert/Parse)
- Basic operations of Step 1 (Clean)

### Quota Consuming Steps

Data generation operations after Step 1 consume **1 quota unit per generated data record**:
- Each data chunk generated after splitting.
- Each data record generated by AI synthesis.
- Each data record processed by AI filtering.
- Each final training data record output.

### View Quota

Quota usage can be viewed in the application top status bar.

---

## Output Directory and Files

### SFT Dataset Output

After processing is complete, data is stored in subdirectories of the output directory by step:

```
Output Directory/
├── ori/          # Step 0: Raw parsed data
├── opt1/         # Step 1: Cleaned data
├── opt2/         # Step 2: Split data
├── opt3/         # Step 3: Analyzed data
├── opt4/         # Step 4: AI synthesized data
├── opt5/         # Step 5: AI synthesis parsed data
├── opt6/         # Step 6: Pre-training data
├── opt7/         # Step 7: Integrated data
├── opt8/         # Step 8: AI filtered data
├── opt9/         # Step 9: AI filter parsed data
└── final/        # Step 10: Final training data
    ├── train/    # Training set
    │   └── final_dataset.jsonl
    ├── eval/     # Validation set
    │   └── final_dataset.jsonl
    └── test/     # Test set
        └── final_dataset.jsonl
```

**Training Usage**: In the training module, select `final/train/final_dataset.jsonl` as the training data file.

### Output File Naming

Unified naming rules for output files of each step:

| Step | Filename |
| :--- | :--- |
| Step 0 | `code_dataset.jsonl` / Named by file type |
| Step 1-9 | `opt{N}_dataset.jsonl` |
| Step 10 | `final_dataset.jsonl` |

### Output Format Selection

Step 10 supports multiple output formats:
- `.jsonl` (Default, recommended for SFT training)
- `.json`
- `.npy` (NumPy format, suitable for direct loading after preprocessing)

---

## AI Node Features

### Model Requirements

Using AI nodes (Step 4, 8) requires ensuring:

1. **Available Chat Model in Model Library**: AI nodes use chat models for data synthesis and filtering.
2. **Select Model in Step Parameters**: Click the settings button of the step to select the AI model to use.

### Model Reuse Mechanism

AI nodes in SFT data processing support reusing loaded chat models:

- If a model is already loaded in the chat interface and the configuration (Base Model + LoRA) matches what the AI node needs.
- The system automatically detects and reuses the model, and requests enter the queue for processing.
- Multiple requests are processed serially through the queue mechanism without interference.
- **Model is not unloaded after SFT task completion**, and your chat session is unaffected.

### RAG Embedding Model

Step 4 (Generate Vector) of RAG data processing requires:

1. **Embedding Model in Model Library**.
2. **Install Environment in Settings**: Embedding models require CPU or GPU environment support.
3. Select the embedding model in step parameters.

---

## Resume from Breakpoint

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

**Q: Can I continue processing after the quota is used up?**

A: After the quota is used up, steps consuming quota cannot be executed. You can upgrade your account to get more quota or wait for quota reset.

---

## Subsequent Documentation

- [SFT Data Processing](./sft.md) - Detailed guide for SFT text/code data processing.
- [RAG Data Processing](./rag.md) - Detailed guide for RAG knowledge base generation.
- [Audio to Text](./audio2text.md) - Detailed guide for audio transcription function.
