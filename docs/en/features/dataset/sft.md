# SFT Data Processing (Text/Code)

> v1.1.0
> 2026-5

This article details how to use the SFT (Supervised Fine-Tuning) data processing function of ModxAI Studio to convert raw text or code files into high-quality datasets that meet training format requirements.

---

## Table of Contents

- [Environment Requirements](#environment-requirements)
- [Feature Introduction](#feature-introduction)
- [Data Type Selection](#data-type-selection)
- [Workflow Templates](#workflow-templates)
- [Processing Flow Overview](#processing-flow-overview)
- [Step-by-Step Details](#step-by-step-details)
- [AI Node Configuration](#ai-node-configuration)
- [Incremental Processing and Error Repair](#incremental-processing-and-error-repair)
- [Output Formats](#output-formats)
- [Notes](#notes)
- [FAQ](#faq)

---

## Environment Requirements

**This feature requires installing CPU or GPU environment dependencies in the settings.**
- Go to the **Settings** page of ModxAI Studio
- In the **Environment Management** tab, select and install the required environment (CPU/GPU)
- After installation, return to the data processing page to use the related features

---

## Feature Introduction

SFT data processing is used to convert raw text or code into structured datasets usable for model fine-tuning training. The system supports two data types: **Text** and **Code**, and provides multiple preset workflow templates to help you quickly complete data preparation.

**Applicable Scenarios**:
- Domain knowledge fine-tuning dataset construction
- Code completion and analysis model training
- Automatic generation of Q&A pair data
- Pre-training corpus preparation

**Core Features**:
- Supports AI-assisted data synthesis, automatically generating high-quality Q&A pairs
- Multi-threaded parallel processing, fully utilizing hardware resources
- Incremental processing support, resumable from breakpoints
- Comprehensive error recovery mechanism

---

## Data Type Selection

### Text Data (Text)

Suitable for processing various documents, articles, technical materials, and other text content.

**Supported File Formats**:
- Documents: PDF, Word (.doc/.docx), PowerPoint (.ppt/.pptx), Excel (.xls/.xlsx)
- Web Pages: HTML (.html/.htm)
- Text: TXT, Markdown, CSV, JSON, XML
- Archives: ZIP (automatically decompresses and processes)

**Structured Data (JSON)**:

Select "JSON" from the file type dropdown to switch to structured data input mode. Suitable for JSON/JSONL data with fixed fields (e.g., news, business records), allowing direct field mapping to standard formats without going through the unstructured cleaning pipeline.

When structured data is selected, the conversion step displays the following parameters:

| Parameter | Description | Default |
|-----------|-------------|---------|
| Limit Count | Process only the first N files or records; 0 means no limit | 0 |
| Record ID Field | Unique identifier field name for each record | id |
| Title Field | Source field mapped to document title | title |
| Content Field | Source field mapped to document content (required) | content |
| Summary Field | Source field mapped to document summary | description |
| Keyword Fields | Source fields mapped to keywords, comma-separated | keywords,tag_key |
| Category Fields | Source fields mapped to category, comma-separated | catname,areaname |
| Author Field | Source field mapped to author | author_key |
| Source Field | Source field mapped to source | copyfrom |
| Published Time Field | Source field mapped to publish time | inputtime |

### Code Data (Code)

Suitable for processing source code files. Currently supports two code types:

#### C/C++ Code (Clang)

Uses Clang compiler frontend for syntax analysis, capable of accurately identifying code structures such as functions, classes, and variables.

**Supported File Formats**: `.c`, `.cpp`, `.cc`, `.h`, `.hpp`

**Dependency Requirements**: Requires system installation of libclang library. The program automatically searches the following locations:
- Path specified by environment variable `LIBCLANG_PATH`
- `runtime/packages/clang` directory
- Python clang package directory

#### Shader Code (Shader)

Supports multiple shader languages, specifically optimized for Unreal Engine formats.

**Supported File Formats**: `.hlsl`, `.glsl`, `.cg`, `.ush`, `.usf`, `.h`

**Parsed Content**:
- File top comments
- `#include` directives
- Macro definitions (`#define`)
- Struct definitions
- Function definitions
- Conditional compilation blocks (`#if/#endif`)

---

## Workflow Templates

To simplify operations, the system presets several common workflow templates. You can choose according to your needs:

| Template Name | Included Steps | Applicable Scenario | Description |
| :--- | :--- | :--- | :--- |
| **AI Synthesis** | 0→1→2→3→4→5→7→10 | Need AI to generate Q&A pairs | Most common complete flow |
| **AI Quality** | 0→1→2→3→4→5→7→8→9→10 | Need AI to filter low-quality data | Includes quality assessment filtering |
| **Custom** | Select as needed | Special needs | Manually select steps to execute |

> Pre-training no longer has a separate step. Set the **Export Format** to `pretrain` in the AI synthesis step node, and subsequent steps will output plain text corpus in pre-training format.

---

## Processing Flow Overview

SFT data processing includes 11 steps (0-10), some of which are optional:

| Step | Name | Description | Required | Configurable |
| :--- | :--- | :--- | :--- | :--- |
| 0 | Convert/Parse | Read and parse source files | ✓ | Code configurable |
| 1 | Clean | Clean invalid content | ✓ | ✓ |
| 2 | Split | Split into appropriately sized chunks | ✓ | ✓ |
| 3 | Analyze/Organize | Quality assessment and complexity analysis | ✓ | - |
| 4 | AI Synthesis | Use AI to generate Q&A pairs | Optional | ✓ |
| 5 | AI Synthesis Parse | Parse AI generation results | Optional | - |
| 7 | Integrate Data | Aggregate all data and format | ✓ | - |
| 8 | AI Filter | AI quality assessment filtering | Optional | ✓ |
| 9 | AI Filter Parse | Parse filtering results | Optional | - |
| 10 | Output | Generate final training data | ✓ | ✓ |

**Step Relationship Explanation**:
- Steps 5/9 are automatically triggered after executing 4/8, no manual selection needed.
- Steps 8-9 require the output of Step 7 as input.

---

## Step-by-Step Details

### Step 0: Convert/Parse

**Function**: Read source files and extract content. Text and code are processed differently.

#### Text Data Processing

Automatically identify file formats and extract plain text content, preserving original paragraph structure.

#### Code Data Processing

Use dedicated parsers to analyze code structure:

- **Clang Parser**: Identify functions, classes, structs, variable declarations, etc.
- **Shader Parser**: Identify macros, structs, functions, include dependencies, etc.

**Configurable Parameters (Code)**:

| Parameter | Description | Default |
| :--- | :--- | :--- |
| **Code Version** | Label code version information (e.g., project version number) | Empty |
| **Exclude Directories** | Skip specified directories, separated by commas | Empty |

---

### Step 1: Clean

**Function**: Clean noisy content in data to improve data quality.

**Processing Content**:
- Remove repetitive content such as headers, footers, navigation bars.
- Clean special symbols and garbled characters.
- Filter out meaningless paragraphs that are too short.

**Configurable Parameters**:

| Parameter | Description | Default | Suggestion |
| :--- | :--- | :--- | :--- |
| **Min Paragraph Length** | Paragraphs below this character count will be filtered | 3 | Technical documents can be set to 5-10 |
| **Sample Scope** | Scope of cleaning rules application | Structure | `Structure` - Only clean structural areas<br>`Full Text` - Apply to full text |
| **Noise Phrases Mode** | Matching mode for cleaning samples | plain | plain=prefix match; wildcard=supports *; regex=regex match |
| **Noise Samples** | Custom content to be removed | Empty | Separate multiple samples with `\|` |
| **Link Line Removal Level** | Deletion level for lines containing links | 2 | 0=keep all; 1=remove local/private paths; 2=remove all links |

**Advanced Parameters (Collapsible)**:

| Parameter | Description | Default | Suggestion |
| :--- | :--- | :--- | :--- |
| **Strip Private IP URLs** | Whether to strip lines containing private IP URLs | Enabled | Recommended to enable to avoid internal address leakage |
| **Preserve Rich Link Lines** | Whether to preserve lines with links but rich content | Disabled | Enable to preserve valuable reference descriptions |
| **Strip URLs in Code Blocks** | Whether to replace URLs in code blocks with [REDACTED_URL] | Disabled | Decide based on security requirements |
| **Rich Words Threshold** | Words threshold to determine rich text | 12 | Used to judge whether to preserve link lines |
| **Rich Length Threshold** | Character length threshold for rich text | 80 | Works with words threshold for comprehensive judgment |

---

### Step 2: Split

**Function**: Split long text/code into chunks suitable for processing.

**Processing Logic**:
- Split by natural boundaries (titles, paragraphs, functions).
- Maintain semantic/code integrity.
- Super long chunks will be split again, and associated hash recorded.

**Configurable Parameters**:

| Parameter | Description | Default | Suggestion |
| :--- | :--- | :--- | :--- |
| **Min Paragraph Length** | Minimum character count for a single chunk | 3 | - |
| **Doc Min Tokens** | Minimum Token count for the entire document | 10 | Documents too short will be skipped |
| **Chunk Min Tokens** | Minimum Token count for a single chunk | 15 | Avoid generating overly fragmented chunks |

---

### Step 3: Analyze/Organize

**Function**: Perform quality analysis on data chunks and group by complexity.

**Text Data Analysis Dimensions**:
- Effective content ratio (after removing stop words)
- Vocabulary richness (TTR indicator)
- Sentence complexity (average sentence length)
- Structural degree (number of titles, lists, code blocks)

**Code Data Analysis Dimensions**:
- Cyclomatic complexity (control flow complexity)
- Cognitive complexity
- Nesting depth
- Token density

**Complexity Grouping**:
- `high`: High complexity, suitable for generating in-depth analysis content.
- `mid`: Medium complexity, suitable for regular Q&A.
- `low`: Low complexity, suitable for simple Q&A or skipping.

#### Configurable Parameters

| Parameter | Description | Default | Recommendation |
|----------|-------------|---------|----------------|
| **Enable quality filtering** | Whether to use an embedding model for quality filtering | On | Recommended to enable for better data quality. Can be disabled if false positives occur. |

#### Embedding Model Dependency

Near-duplicate detection in this step can use an embedding model to improve accuracy. By default, the system uses the `all-MiniLM-L6-v2` model, if it cannot be installed, an alternative algorithm is used for near-duplicate detection:

- **Local first**: Tries to load a locally downloaded model first  
- **Automatic fallback**: Falls back to TF-IDF if an embedding model is unavailable  

> **Resource Tip**: This step is multi-threaded parallel processing, which will consume significant CPU and memory resources. Please pay attention to system load when processing large amounts of data.

---

### Step 4: AI Synthesis (Optional)

**Function**: Use AI models to analyze data chunks and generate Q&A pairs or analysis content for training.

Automatically convert raw text/code into structured data such as analysis, Q&A pairs, or even custom-styled structured data through AI capabilities.

**Task Types**:

| Type | Description | Output Content |
| :--- | :--- | :--- |
| **Analysis** | In-depth interpretation of content | Structured analysis text |
| **QA** | Generate Q&A pairs | Question-Answer pairs |
| **All** | Execute both tasks simultaneously | Analysis + QA |

**Complexity Matching**:

The system matches different AI prompts for data chunks of different complexities based on the complexity grouping in Step 3:

| Complexity | Analysis Focus | QA Generation Quantity |
| :--- | :--- | :--- |
| high | In-depth analysis of core concepts and implementation principles | 5-8 pairs |
| mid | Explain main functions and usage | 3-5 pairs |
| low | Briefly describe basic information | 1-3 pairs |

See [AI Node Configuration](#ai-node-configuration) chapter for detailed AI parameter configuration.

---

### Step 5: AI Synthesis Parse (Optional)

**Function**: Parse AI output from Step 4 and extract structured data. Exclude invalid content.

This step is recommended to be executed after Step 4 completes, cleaning the AI output to ensure high-quality data for subsequent steps.

#### Configurable Parameters

| Parameter | Description | Default | Recommendation |
|----------|-------------|---------|----------------|
| **Enable quality filtering** | Whether to use an embedding model for quality filtering | On | Recommended to enable for better data quality. Can be disabled if false positives occur. |

#### Embedding Model Dependency

Near-duplicate detection in this step can use an embedding model to improve accuracy. By default, the system uses the `all-MiniLM-L6-v2` model, if it cannot be installed, an alternative algorithm is used for near-duplicate detection:

- **Local first**: Tries to load a locally downloaded model first  
- **Automatic fallback**: Falls back to TF-IDF if an embedding model is unavailable  

---

### Export Format & Pre-training

Pre-training is no longer a standalone step. Set the **Export Format** parameter in Step 4 (AI Synthesis) to control the final output:

| Export Format | Output Content | Use Case |
|---------------|----------------|----------|
| sft_chatml | ChatML dialogue format (system/user/assistant) | SFT fine-tuning |
| pretrain | Plain text corpus (single assistant message) | Pre-training / Continued pre-training |
| dpo | Preference pair format (prompt/chosen/rejected) | DPO preference alignment training |

---

### Step 7: Integrate Data

**Function**: Aggregate output from previous steps and convert to ChatML conversation format.

**Processing Content**:
- Merge multiple data chunks from the same source.
- Construct System-User-Assistant conversation structure.
- Handle chunking and overlapping of super long content.

**Output Format Example**:
```json
{
  "messages": [
    {"role": "system", "content": "You are a helpful assistant."},
    {"role": "user", "content": "Question content..."},
    {"role": "assistant", "content": "Answer content..."}
  ]
}
```

---

### Step 8: AI Filter (Optional, Use with Caution)

**Function**: Use AI to assess the quality of integrated data and filter low-quality entries.

**Assessment Criteria**:
- Content accuracy and relevance
- Expression clarity
- Completeness and consistency

**Output Judgment**:
- `<P>`: Pass, retain this data entry.
- `<F>`: Fail, filter out this data entry.

> ⚠️ **Important Warning**:
> 
> AI filtering requires using a **model with strong analysis capabilities** (e.g., models with 4B+ parameters). Using a model with insufficient capabilities may:
> - Incorrectly mark high-quality data as low quality.
> - Retain actually problematic data.
> - Lead to a decline in final dataset quality.
>
> **Suggestion**: Unless you have sufficient computing resources and high-quality models, it is recommended to skip this step and control output quality by adjusting prompt parameters in Step 4.

---

### Step 9: AI Filter Parse (Automatic)

**Function**: Parse filtering results from Step 8 and remove entries marked as `<F>`.

---

### Step 10: Output

**Function**: Generate final training data files, supporting multiple formats and dataset splitting.

**Configurable Parameters**:

| Parameter | Description | Default | Options |
| :--- | :--- | :--- | :--- |
| **Dataset Split** | Train/Eval/Test set ratio | 0.8, 0.1, 0.1 | Sum of three ratios should be 1 |
| **Parallel Threads** | Data processing parallelism | 16 | 1-64 |
| **Tokenizer Path** | Tokenizer for Token calculation | Built-in | Can specify custom path |
| **Save Format** | Output file format | .jsonl | `.jsonl` / `.json` / `.npy` |
| **System Prompt** | System message added to all conversations | Empty | Customizable |

**Output Files**:
- `train.jsonl`: Training set
- `eval.jsonl`: Validation set
- `test.jsonl`: Test set

---

## AI Node Configuration

Step 4 (AI Synthesis) and Step 8 (AI Filter) both require configuring AI model parameters.

### Basic Parameters

| Parameter | Description | Default |
| :--- | :--- | :--- |
| **AI Model** | Select model to use | Required |
| **Task Type** | Analysis / QA / All | Analysis |
| **Complexity Match** | Which complexity levels of data to process | All |
| **Export Format** | Output format for final training data | sft_chatml |
| **Chain of Thought Mode** | Inject CoT reasoning prompt to guide model thinking process | Off |

**Export Format Options**: `sft_chatml` (SFT ChatML dialogue format), `pretrain` (pre-training plain text), `dpo` (DPO preference pair format).

**Chain of Thought Mode**: When enabled, automatically injects content reasoning CoT prompts and disables the model's native thinking function to avoid dual thinking tag pollution. Suitable for analysis tasks where the model needs to show reasoning steps.

### Advanced Parameters (Collapsible Area)

| Parameter | Description | Default | Range |
| :--- | :--- | :--- | :--- |
| **Temperature** | Generation diversity | 0.7 | 0-1 |
| **Max Tokens** | Maximum output length | 1024 | 1-8192 |
| **Top K** | Sampling candidates | 20 | 0-100 |
| **Top P** | Nucleus sampling threshold | 0.95 | 0-1 |
| **Repeat Penalty** | Avoid repetition | 1.1 | 0-2 |
| **Min P** | Minimum probability threshold | 0.0 | 0-1 |
| **Enable Thinking** | Use model native thinking | No | - |
| **Server Address** | Remote inference service URL, format `http://ip:port/v1/chat/completions` | Empty | - |

### Custom System Prompt

You can customize AI behavior guidance in the **System Prompt** field. If left empty, the system uses default prompts based on task type:

**Analysis Task Default Prompt** (Overview):
- Act as a professional technical analyst.
- Perform structured in-depth analysis of content.
- Output includes core concepts, key points, code examples (if applicable).

**QA Task Default Prompt** (Overview):
- Act as a professional technical documentation writer.
- Generate high-quality Q&A pairs.
- Questions should cover different difficulty levels.

**Code Analysis Prompt additionally includes**:
- Analysis of function/method parameters and return values.
- Code implementation principle explanation.
- Potential optimization suggestions.

---

## Incremental Processing and Error Repair

AI Synthesis step (Step 4) supports incremental processing and automatic error repair, which is very useful when processing large amounts of data.

### Incremental Processing

When you pause processing or restart after an unexpected program interruption:

1. **Auto Detect**: System detects processed data chunks.
2. **Skip Completed**: Successfully processed chunks will not be requested again.
3. **Continue Processing**: Only process new or incomplete data.

**Implementation Mechanism**:
- Use WAL (Write-Ahead Logging) to record each processing result.
- Track processing status of each data chunk via Hash value.
- Periodic automatic backup (every 15 minutes).

### Error Repair Mechanism

When an AI request fails, the system automatically retries and repairs:

**Retry Strategy**:
- Max retries: 3 times
- Retry interval: 60 seconds
- Timeout: Dynamically adjusted based on Token count (120-600 seconds)

**Error Recovery**:
- Failed requests are recorded in the error list.
- Automatically attempt to repair previous errors on next execution.
- Support manual triggering of error repair.

**Status Tracking**:
```json
{
  "total_groups": 100,
  "processed_count": 85,
  "processed_groups": {
    "file_a.txt__abc123": {
      "total_chunks": 10,
      "processed_chunks": ["hash1", "hash2", ...],
      "error_chunks": ["hash9"],
      "is_done": false
    }
  }
}
```

---

## Output Formats

### JSONL Format (Recommended)

One JSON object per line, conforming to ChatML format:

```jsonl
{"messages": [{"role": "system", "content": "..."}, {"role": "user", "content": "..."}, {"role": "assistant", "content": "..."}]}
{"messages": [{"role": "system", "content": "..."}, {"role": "user", "content": "..."}, {"role": "assistant", "content": "..."}]}
```

### JSON Format

Complete JSON array:

```json
[
  {"messages": [...]},
  {"messages": [...]}
]
```

### NPY Format

NumPy binary format, suitable for direct loading by training frameworks:
- `train.npy`: Token ID array
- `train.npy.shape.json`: Shape metadata

---

## Notes

### Performance and Resources

1. **Multi-threaded Processing**: Step 0 and Step 1 use multi-threaded parallel processing, consuming significant CPU and memory.
2. **AI Request Overhead**: Steps 4 and 8 require massive AI requests; processing time depends on data volume and model speed.
3. **Disk Space**: Intermediate files occupy some disk space; suggest reserving sufficient space.

### Code Processing

1. **Clang Dependency**: Processing C/C++ code requires libclang library support.
2. **Encoding Issues**: Recommend source code use UTF-8 encoding.
3. **Exclude Directories**: For large projects, suggest excluding build, vendor, etc., directories.

### AI Model Selection

1. **Synthesis Step**: Recommend using models with 4B+ parameters.
2. **Filter Step**: Recommend using models with 4B+ parameters.

---

## FAQ

### Q: What if processing is interrupted?

A: Directly re-execute the same steps. The system automatically detects processed content and continues from the breakpoint.

### Q: AI synthesis content quality is not ideal?

A: Try the following methods:
1. Adjust temperature parameter (lowering increases consistency).
2. Customize system prompt.
3. Use a larger model.
4. Adjust complexity matching, process high-complexity data first.

### Q: Why is there no separate pre-training entry?

A: The processing flow for pre-training data is highly similar to SFT. Main differences:
- No AI Q&A generation (Steps 4-5).
- Output format is plain text instead of conversation format.

Therefore, it is integrated into the same interface. Set the **Export Format** to `pretrain` in Step 4 (AI Synthesis) to output in pre-training format.

### Q: AI filtering causes significant data reduction?

A: This usually means:
1. The model used has insufficient capabilities; suggest switching to a stronger model.
2. Original data quality indeed has issues.
3. Can temporarily skip AI filtering step and manually spot-check data quality.

### Q: How to process ultra-large datasets?

A: Suggest batch processing:
1. Split data into multiple subdirectories.
2. Process each subdirectory separately.
3. Finally merge output files.
4. Utilize incremental processing function to execute in multiple batches.

### Q: Which engine formats does Shader code support?

A: Mainly supports:
- Standard HLSL/GLSL
- Unreal Engine Shaders (.ush/.usf)
- CG Shaders
- Compatibility headers (.h)

---

*For other questions, please refer to project documentation or ask in the community.*

