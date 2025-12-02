# RAG Knowledge Base Data Processing

> v1.0.0
> 2025-12

This article details how to use the RAG (Retrieval-Augmented Generation) data processing function of ModxAI Studio to convert documents into a vector knowledge base usable for AI conversation retrieval.

---

## Table of Contents

- [Environment Requirements](#environment-requirements)
- [Feature Introduction](#feature-introduction)
- [Processing Flow](#processing-flow)
- [Step-by-Step Details](#step-by-step-details)
- [Parameter Configuration](#parameter-configuration)
- [Output and Storage](#output-and-storage)
- [Using in Chat](#using-in-chat)
- [Exporting from Personalized Data](#exporting-from-personalized-data)
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

RAG knowledge base is a technology that allows AI to "consult materials" to answer questions. By converting your documents into vector form for storage, AI can retrieve relevant content during conversation, thereby providing more accurate and professional answers.

**Applicable Scenarios**:
- Enterprise internal knowledge base Q&A
- Product documentation intelligent customer service
- Professional field data retrieval
- Personal knowledge management assistant

**Supported File Formats**:
- Documents: PDF, Word (.doc/.docx), PowerPoint (.ppt/.pptx), Excel (.xls/.xlsx)
- Web Pages: HTML (.html/.htm)
- Text: TXT, Markdown, CSV, JSON, XML
- Archives: ZIP (automatically decompresses and processes internal files)

---

## Processing Flow

RAG data processing includes 6 steps. The first 4 steps are shared with SFT text processing, and the last 2 steps are specifically for vectorization:

| Step | Name | Description | Configurable |
| :--- | :--- | :--- | :--- |
| 0 | Convert | Parse various file formats and extract plain text content. | - |
| 1 | Clean | Clean invalid content and remove noisy data. | ✓ |
| 2 | Split | Split long documents into text chunks suitable for retrieval. | ✓ |
| 3 | Analyze | Data quality analysis, grouping by complexity. | - |
| 4 | Generate Vector | Use embedding model to convert text into vectors. | ✓ |
| 5 | Store Vector | Write vector data to vector database. | ✓ |

---

## Step-by-Step Details

### Step 0: Convert

**Function**: Read the files you selected and convert various formats such as PDF, Word, HTML into unified plain text.

**Processing Content**:
- PDF: Extract text content, preserving paragraph structure.
- Word/PPT/Excel: Read document content and table data.
- HTML: Clear tags, preserve body text.
- Archives: Automatically decompress and recursively process internal files.

**Note**: This step is automatic processing and requires no parameter configuration.

---

### Step 1: Clean

**Function**: Clean invalid content in documents to improve data quality.

**Processing Content**:
- Remove repetitive content such as headers, footers, navigation bars.
- Clean special symbols and garbled characters.
- Filter out meaningless paragraphs that are too short.

#### Configurable Parameters

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

**Usage Suggestions**:
- If the document source is single (e.g., company internal documents), you can add unified footer information as cleaning samples.
- For technical documents, it is recommended to set the minimum paragraph length to 5-10 to avoid filtering out code comments.

---

### Step 2: Split

**Function**: Split long documents into text chunks suitable for retrieval. This is a key step in the RAG system, directly affecting retrieval effectiveness.

**Processing Logic**:
- Prioritize splitting by natural boundaries of titles and paragraphs.
- Maintain semantic integrity, avoid breaking in the middle of sentences.
- Super long paragraphs will be further split by sentences.

#### Configurable Parameters

| Parameter | Description | Default | Suggestion |
| :--- | :--- | :--- | :--- |
| **Min Paragraph Length** | Minimum paragraph character count retained during splitting. | 3 | Too small will produce fragmented content. |
| **Min Document Length** | Minimum Token count required for a document. | 10 | Documents too short will be filtered. |
| **Min Chunk Length** | Minimum Token count for a single text chunk. | 15 | Suggest 15-50; too small affects retrieval quality. |

**Token Explanation**: Token is the basic unit for model processing text. Chinese approx. 1 character ≈ 1-2 Tokens, English approx. 1 word ≈ 1-3 Tokens.

**Usage Suggestions**:
- Q&A Scenario: Set minimum chunk length to 30-50 to ensure each chunk contains complete information.
- Long Document Retrieval: Can appropriately increase chunk length to improve context integrity.

---

### Step 3: Analyze

**Function**: Perform quality analysis on processed data, identify duplicates and anomalies, and group by content complexity.

**Processing Content**:
- **Near-Duplicate Detection**: Find highly similar text chunks and retain representative content.
- **Outlier Detection**: Identify abnormal content that deviates significantly from the topic.
- **Complexity Grouping**: Classify and store content by High/Mid/Low complexity.

#### Embedding Model Dependency

The near-duplicate detection function of this step supports using embedding models to improve detection accuracy. The system defaults to using the `all-MiniLM-L6-v2` model:

- **Local Priority**: The system will prioritize attempting to load locally downloaded models.
- **Auto Downgrade**: If the embedding model is unavailable, it will automatically use the TF-IDF algorithm for detection.
- **Optional Configuration**: You can download this embedding model in the Model Library or use other compatible Sentence Transformer models.

**Note**: This step is automatic processing and requires no parameter configuration. The embedding model is optional and does not affect basic functions.

---

### Step 4: Generate Vector

**Function**: Use an Embedding Model to convert text chunks into high-dimensional vectors. This is the core step for achieving semantic retrieval.

**Working Principle**: The vector model converts the "meaning" of text into mathematical vectors. Content with similar semantics is also close in vector space, allowing AI to find content relevant to the question.

#### Configurable Parameters

| Parameter | Description | Default | Suggestion |
| :--- | :--- | :--- | :--- |
| **Embedding Model** | Select the model used to generate vectors. | Manual Selection Required | Must select an imported vector model (Embedding type) from the Model Library. |
| **Vector Normalization** | Whether to normalize vectors. | Enabled | Recommended to keep enabled to improve retrieval effectiveness. |

**Model Selection Suggestions**:
- Chinese dominant: Select models supporting Chinese, such as `qwen3-embedding`, `bge-large-zh`, `m3e-base`.
- English dominant: Can select `bge-large-en`, `e5-large`.
- Multi-language mixed: Select multi-language models, such as `bge-m3`.

**Vector Normalization Explanation**: Normalized vectors have a unified length of 1, facilitating retrieval comparison using cosine similarity. Recommended to keep enabled.

---

### Step 5: Store Vector

**Function**: Store generated vectors and corresponding text into the vector database for subsequent retrieval use.

#### Configurable Parameters

| Parameter | Description | Default | Suggestion |
| :--- | :--- | :--- | :--- |
| **Collection Name** | Identifier name of the vector library. | Auto Generated | Suggest using meaningful names, e.g., `Product_Manual_v1`, `Tech_Docs`. |
| **Target Dimension** | Dimension size of the vector. | 1024 | Must match model output dimension, usually no need to modify. |
| **Vector Pooling Strategy** | Merging method for multi-dimensional vectors. | mean | Advanced option, generally keep default. |

**Collection Name Explanation**:
- Collection name is the unique identifier of the knowledge base, used to select the knowledge base to retrieve during chat.
- Suggest using concise and clear naming for easy identification and management.
- New data with the same collection name will be appended to the existing collection.

**Pooling Strategy Explanation** (Advanced Option):
- `mean`: Mean pooling, integrates all features, most commonly used.
- `max`: Max pooling, retains most significant features.
- `cls`: Uses the feature vector of the first position.

---

## Output and Storage

### Intermediate Files

Data during processing is stored in the output directory by step:

```
Output Directory/
├── ori/              # Step 0: Raw parsed data
├── opt1/             # Step 1: Cleaned data
├── opt2/             # Step 2: Split data
├── opt3/             # Step 3: Analyzed data
│   ├── high_complexity/
│   ├── mid_complexity/
│   └── low_complexity/
└── opt4/             # Step 4: Vector data (Incremental directory)
```

### Vector Database

The final vector data is stored in an **independent vector database**, not the application's local database:

- **Storage Location**: Vector storage folder under the output directory.
- **Database Type**: Qdrant Vector Database (Local Mode).
- **Data Format**: Vector + Raw Text + Metadata.

**Important Note**:
- Vector database is stored separately from intermediate processing files.
- Each collection is automatically grouped by vector dimension, e.g., `Product_Manual_1024d`.
- Vector database contains complete information required for retrieval, no need to keep intermediate files.

---

## Using in Chat

After completing RAG data processing, you can enable knowledge base retrieval in chat:

1. **Open Parameter Settings**: Click the parameter drawer in the chat interface.
2. **Select Knowledge Base**: Select the created collection in the RAG configuration area.
3. **Start Conversation**: AI will automatically retrieve relevant content to assist in answering.

### Manage Knowledge Base

- **View List**: You can see all available knowledge base collections in the parameter drawer.
- **Delete Collection**: Click the delete button next to the collection to remove the knowledge base.
- **Delete Note**: Delete operation only removes collection configuration, **does not delete stored vector data files**.
- **Re-import**: Supports re-importing deleted collections, provided the vector data files still exist.

---

## Exporting from Personalized Data

In addition to directly processing documents, you can also export personalized data accumulated in chat to RAG format:

### Operation Steps

1. **Enter Collection Data Management**: Click the "Collection" button in the chat interface.
2. **Select Export Data**: Check the conversation or liked content to export.
3. **Select Export Format**: Select "RAG Knowledge Base Format".
4. **Configure Export Options**: Set name and other parameters.
5. **Execute Export**: After export, complete processing from the (**Generate Vector**) node to generate vectors and store them in the vector database.

### Usage Scenarios

- Accumulate high-quality AI answers into a knowledge base.
- Build exclusive Q&A libraries based on historical conversations.
- Convert liked high-quality content into retrievable knowledge.

---

## Notes

### Environment Requirements

| Requirement | Description |
| :--- | :--- |
| **Environment** | Must configure environment in **Settings** to run vector models. |
| Vector Model | Must import at least one Embedding type model in the Model Library. |
| Disk Space | Vector database will occupy some storage space, please ensure sufficient space in the output directory. |

### Quota Consumption

RAG data processing consumes free quota:
- Step 0 (Convert): Free.
- Step 1-5: Consumes 1 quota unit per processed/generated data record.

### Performance Suggestions

- **First Processing**: Processing a large number of documents for the first time may take a long time; suggest testing with a small number of files first.
- **Incremental Update**: Supports incremental processing; new files do not require re-processing all data.
- **GPU Acceleration**: If GPU is available, vector generation speed will be significantly improved.

---

## FAQ

**Q: Which vector model should I choose?**

A: Choose based on your content language:
- Pure Chinese content: Recommend `qwen3-embedding`, `bge-large-zh`, or `m3e-base`.
- Pure English content: Recommend `qwen3-embedding`, `bge-large-en`, or `e5-large`.
- Chinese-English mixed: Recommend `qwen3-embedding`, `bge-m3` multi-language models.

**Q: How to troubleshoot processing failure?**

A: Common reasons:
1. Environment not configured in **Settings** - Please configure in **Settings**.
2. Vector model not imported - Please import Embedding type model in Model Library first.
3. Insufficient quota - Check remaining quota or wait for reset.

**Q: Can I modify the content of an existing knowledge base?**

A: Direct editing is currently not supported. If update is needed, suggest:
1. Delete old collection.
2. Re-process after modifying source documents.
3. Or create an updated version using a new collection name.

**Q: Does deleting the knowledge base in chat affect data?**

A: Deleting a knowledge base collection in the chat parameter drawer only cancels the association, **does not delete actual vector data files**. To delete thoroughly, you need to manually clean the vector storage files in the output directory.

**Q: Is it normal for Step 3 analysis to take a long time?**

A: If the data volume is large, quality analysis does take some time. If an embedding model is available, vector similarity will be used for near-duplicate detection, which is more effective but takes slightly longer. The system automatically selects the optimal plan.

**Q: Can collection names use Chinese?**

A: Suggest using English or Pinyin naming. Chinese may cause compatibility issues in some system environments.

---

## Subsequent Documentation

- [Data Processing Overview](./overview.md) - Understand the overall architecture of the data processing module.
- [SFT Data Processing](./sft.md) - Dataset generation for model fine-tuning.
- [Audio to Text](./audio2text.md) - Detailed guide for audio transcription function.
