# RAG Knowledge Base Data Processing

> v1.0.0  
> 2025-12

This article explains in detail how to use the RAG (Retrieval-Augmented Generation) data processing feature in ModxAI Studio to convert documents into an intelligent knowledge base for AI conversational retrieval.

---

## Table of Contents

- [Environment Requirements](#environment-requirements)
- [Feature Overview](#feature-overview)
- [System Capabilities](#system-capabilities)
- [Processing Workflow](#processing-workflow)
- [Step-by-Step Details](#step-by-step-details)
- [Parameter Configuration](#parameter-configuration)
- [Output and Storage](#output-and-storage)
- [Using in Chat](#using-in-chat)
- [Exporting from Favorites](#exporting-from-favorites)
- [Notes](#notes)
- [FAQ](#faq)

---

## Environment Requirements

**This feature requires additional CPU or GPU environment dependencies to be installed in Settings.**

- Open the **Settings** page in ModxAI Studio  
- In the **Environment Management** tab, select and install the required environment (CPU/GPU)  
- After installation is complete, return to the data processing page to use the related features

---

## Feature Overview

The RAG knowledge base is a technique that allows AI to "look up references" when answering questions. By converting your documents into vector representations, the AI can retrieve relevant content during a conversation, providing more accurate and professional answers.

Unlike traditional full-text search, a RAG system understands the **semantic meaning** of questions. Even if the way you phrase your query is different from the wording in the documents, it can still find relevant content.

**Typical use cases:**

- Enterprise internal knowledge base Q&A  
- Product documentation intelligent support  
- Retrieval of professional domain materials  
- Personal knowledge management assistant  
- Fast lookup for technical documentation  

**Supported file formats:**

- Document: PDF, Word (.doc/.docx), PowerPoint (.ppt/.pptx), Excel (.xls/.xlsx)  
- Web: HTML (.html/.htm)  
- Text: TXT, Markdown, CSV, JSON, XML  
- Archives: ZIP (automatically unzipped and internal files processed recursively)

---

## System Capabilities

ModxAI Studio’s RAG system adopts multiple techniques to improve retrieval quality:

### Intelligent Document Structure Parsing

The system automatically recognizes document section structures (such as Markdown heading levels) and splits documents into logical units instead of simply using fixed-length chunks. This ensures:

- Each text block contains complete semantic information  
- The association between section titles and content is preserved  
- Configurable merge strategies based on heading levels  

### Multi-dimensional Retrieval Enhancements

During retrieval, the system combines multiple strategies to improve accuracy:

| Enhancement Strategy | Description |
|----------------------|-------------|
| **Vector semantic search** | Uses an embedding model to understand question semantics and find semantically similar content |
| **Section title matching** | When keywords in the question appear in section titles, the corresponding content gets a higher ranking weight |
| **AI summary matching** | Uses locally generated AI summaries for keyword matching to cover gaps in semantic search |
| **Section-level aggregation** | When multiple hits belong to the same section, the overall relevance of that section is increased |
| **Context expansion** | Automatically supplements neighboring paragraphs around hits to provide richer context |
| **Rerank optimization** | Uses a dedicated rerank model to finely reorder candidate results |

### Optional AI Analysis Enhancements

The system provides optional AI analysis steps that generate concise summaries for each text block. These summaries are considered during retrieval, helping the system better understand the topic of the content.

---

## Processing Workflow

RAG data processing consists of 8 steps, 2 of which are optional AI enhancement steps:

| Step ID | Name | Description | Required | Configurable |
|--------|------|-------------|----------|-------------|
| 0 | Conversion | Parses various file formats and extracts plain text | ✓ | - |
| 1 | Cleaning | Cleans invalid content and removes noise | ✓ | ✓ |
| 2 | Splitting | Splits documents into text blocks based on section structure | ✓ | ✓ |
| 3 | Analysis | Data quality analysis, deduplication, and complexity grouping | ✓ | - |
| 4 | AI Synthesis | (Optional) Uses AI to generate summaries for text blocks | - | ✓ |
| 5 | AI Parsing | (Optional) Parses and integrates AI-generated data | - | - |
| 11 | Vector Generation | Uses an embedding model to convert text into vectors | ✓ | ✓ |
| 12 | Vector Storage | Writes vector data to the vector database | ✓ | ✓ |

**About optional steps:**

- Steps 4–5 are AI enhancement steps and require a chat model to be loaded  
- Skipping these steps does not affect basic vector retrieval  
- Enabling them can improve retrieval accuracy but increases processing time

---

## Step-by-Step Details

### Step 0: Conversion

**Purpose:**  
Reads the files you select and converts formats such as PDF, Word, HTML, etc. into unified Markdown plain text.

**Processing:**

- PDF: Extracts text while preserving paragraph structure  
- Word/PPT/Excel: Reads document content and table data  
- HTML: Strips tags and keeps the main body text  
- Archives: Automatically unzips and recursively processes internal files  

**Notes:** This step is automatic and has no configurable parameters.

---

### Step 1: Cleaning

**Purpose:**  
Cleans invalid content from documents to improve data quality.

**Processing:**

- Removes repeated elements such as headers, footers, and navigation bars  
- Cleans special symbols and garbled text  
- Filters very short, meaningless paragraphs  

#### Configurable Parameters

| Parameter | Description | Default | Recommendation |
|----------|-------------|---------|----------------|
| **Minimum paragraph length** | Paragraphs with fewer characters will be removed | 3 | Adjust based on content type; set higher for technical docs |
| **Sample scope** | Scope where cleaning rules are applied | Structure | `Structure` – only structural areas such as headers/footers<br>`Full` – apply rules to the entire document |
| **Noise phrase match mode** | Matching mode for cleaning samples | plain | plain = prefix match; wildcard = `*` wildcard; regex = regular expressions |
| **Noise samples** | Custom content to be removed | Empty | Separate multiple samples with `\|`, e.g.: `\|Copyright\`<br>Each sample is recommended to be under 20 characters |
| **Link-line removal level** | Removal level for lines containing links | 2 | 0 = do not remove; 1 = remove local/private paths; 2 = remove all links |

**Advanced Parameters (collapsed):**

| Parameter | Description | Default | Recommendation |
|----------|-------------|---------|----------------|
| **Clean private IP links** | Whether to clean URLs containing private IPs | On | Recommended to avoid exposing intranet addresses |
| **Keep rich-text link lines** | Whether to keep lines with links but rich content | Off | Turn on to preserve valuable reference text |
| **Redact URLs in code blocks** | Replace URLs in code blocks with `[REDACTED_URL]` | Off | Decide based on security requirements |
| **Rich-text word threshold** | Word count threshold to determine rich text | 12 | Used to decide whether to keep link lines |
| **Rich-text length threshold** | Character length threshold for rich text | 80 | Used together with word threshold |

**Usage tips:**

- If documents share a common footer (e.g., company internal docs), add it to cleaning samples  
- For technical docs, set minimum paragraph length to 5–10 to avoid dropping code comments

---

### Step 2: Splitting

**Purpose:**  
Splits long documents into text blocks (chunks) suitable for retrieval. This is a key step in the RAG system and directly affects retrieval quality.

**Intelligent splitting logic:**

- Automatically recognizes Markdown heading levels (#, ##, ###, etc.) from the conversion step  
- Splits by logical sections to keep content complete  
- Supports configurable heading merge levels to control granularity  
- Automatically filters out navigation link lists, API function lists, and similar noise  
- Overly long sections are further split at paragraph and sentence boundaries  

#### Preserving Section Structure

Each resulting text block records its section information:


- **Section title**: Title text in the original document  
- **Hierarchy**: Heading nesting level  

This information is used for section aggregation and context expansion during retrieval.

#### Configurable Parameters

| Parameter | Description | Default | Recommendation |
|----------|-------------|---------|----------------|
| **Minimum paragraph length** | Minimum paragraph character length to keep during splitting | 3 | Too small will result in fragmented content |
| **Minimum document length** | Minimum required token count for a document | 10 | Very short documents are filtered out |
| **Minimum chunk length** | Minimum token count for a single text block | 15 | Recommended 15–50; too small hurts retrieval quality |

**About tokens:**  
Tokens are the basic unit of text for models. In Chinese, ~1 character ≈ 1–2 tokens; in English, ~1 word ≈ 1–3 tokens.

**Usage tips:**

- Q&A use: set minimum chunk length to 30–50 to ensure each chunk has complete information  
- Long-document retrieval: increase chunk length to improve context completeness  

---

### Step 3: Analysis

**Purpose:**  
Analyzes processed data quality, identifies duplicate/anomalous content, and groups by content complexity.

**Processing:**

- **Near-duplicate detection**: Finds highly similar text blocks and keeps representative ones  
- **Outlier detection**: Identifies content that significantly deviates from the main topics  
- **Complexity grouping**: Classifies content into high/medium/low complexity groups  

#### Embedding Model Dependency

Near-duplicate detection in this step can use an embedding model to improve accuracy. By default, the system uses the `all-MiniLM-L6-v2` model:

- **Local first**: Tries to load a locally downloaded model first  
- **Automatic fallback**: Falls back to TF-IDF if an embedding model is unavailable  
- **Optional configuration**: You can download this embedding model or use other compatible Sentence Transformer models in the model hub  

**Notes:** This step is automatic and has no configurable parameters. The embedding model is optional and does not affect basic functionality.

---

### Steps 4–5: AI Analysis (Optional)

**Purpose:**  
Uses an AI model to generate concise summaries for each text block to enhance subsequent retrieval.

**How it works:**

1. The system sends each text block to the loaded chat model  
2. The AI analyzes content and generates a core summary (by default up to 50 characters)  
3. The summary is stored alongside the original text in the vector database  
4. During retrieval, both the original text and summaries are matched to improve recall  

**Use cases:**

- Highly specialized documents that need core concept extraction  
- When user questions differ significantly from the document wording  
- When higher retrieval accuracy and relevance are desired  

#### Configurable Parameters

| Parameter | Description | Default | Recommendation |
|----------|-------------|---------|----------------|
| **System prompt** | Instruction used when AI generates summaries | Built-in template | Customize based on your document domain |

**Notes:**

- Requires a chat model to be loaded  
- Processing time is proportional to the number of text blocks  
- Skipping this step does not affect basic retrieval

---

### Step 11: Vector Generation

**Purpose:**  
Uses an embedding model to convert text blocks into high-dimensional vectors, which is the core of semantic retrieval.

**How it works:**  
The embedding model converts the "meaning" of text into mathematical vectors. Semantically similar content is close in vector space, allowing the AI to find content related to the question’s meaning rather than just matching keywords.

#### Configurable Parameters

| Parameter | Description | Default | Recommendation |
|----------|-------------|---------|----------------|
| **Embedding model** | Model used to generate vectors | Manual selection | Must select an imported embedding model from the model hub |
| **Vector normalization** | Whether to normalize vectors | On | Recommended to keep on; improves retrieval quality |

**Model selection suggestions:**

- Primarily Chinese: models like `qwen3-embedding`, `bge-large-zh`, `m3e-base`  
- Primarily English: models like `bge-large-en`, `e5-large`  
- Mixed languages: multilingual models like `bge-m3`  

**About normalization:**  
Normalized vectors have length 1, which is convenient for cosine similarity comparison. Keeping it on is recommended.

---

### Step 12: Vector Storage

**Purpose:**  
Stores generated vectors and corresponding text in a vector database for later retrieval.

**Stored data:**

- Text vectors (for semantic retrieval)  
- Original text content (for displaying results)  
- Section structure information (for aggregation and context expansion)  
- AI summaries (if steps 4–5 are enabled)  
- Other metadata (file name, block index, etc.)  

#### Configurable Parameters

| Parameter | Description | Default | Recommendation |
|----------|-------------|---------|----------------|
| **Collection name** | Identifier of the vector store | Auto generated | Use meaningful names like `product_manual_v1`, `tech_docs` |
| **Target dimension** | Vector dimensionality | 1024 | Must match model output dimension; usually no need to change |
| **Pooling strategy** | Pooling method for multi-token vectors | mean | Advanced option; usually keep default |

**About collection name:**

- The collection name is the unique identifier of a knowledge base and is used to select the KB in chat  
- Use concise and descriptive naming for easier management  
- New data with the same name is appended to the existing collection  

**Pooling strategy (advanced):**

- `mean`: average pooling, combines all features; most common  
- `max`: max pooling, keeps the most salient features  
- `cls`: uses the [CLS] or first-position vector  

---

## Output and Storage

### Intermediate Files

Data produced during processing is stored step by step in the output directory:

```text
output_dir/
├── ori/              # Step 0: raw parsed data
├── opt1/             # Step 1: cleaned data
├── opt2/             # Step 2: split data (with section structure)
├── opt3/             # Step 3: analyzed data
│   ├── high_complexity/
│   ├── mid_complexity/
│   └── low_complexity/
├── opt4/             # Step 4: AI analysis data (optional)
├── opt5/             # Step 5: AI-parsed data (optional)
└── rag_dataset/      # Vector database storage
    ├── embeddings/   # Vector files
    └── storage/      # Qdrant database files
```

### Vector Database

Final vector data is stored in an **independent vector database**, not in the app’s local database:

- **Location**: `rag_dataset/storage` folder under the output directory  
- **Database type**: Qdrant vector database (local mode)  
- **Data format**: vectors + original text + metadata (including section info and optional summaries)

**Important notes:**

- The vector database is stored separately from intermediate processing files  
- Each collection is automatically grouped by vector dimension, such as `product_manual_1024d`  
- The vector database contains all information required for retrieval, so intermediate files need not be kept

---

## Using in Chat

After RAG data processing is complete, you can enable knowledge-base retrieval in chat:

1. **Open parameter settings**: Click the parameter drawer in the chat interface  
2. **Select knowledge base**: Choose a created collection in the RAG configuration area  
3. **Configure retrieval mode**: Select an appropriate retrieval mode (see below)  
4. **Start chatting**: The AI will automatically retrieve relevant content to assist answers  

### Retrieval Modes

The system supports multiple retrieval modes, configurable in chat parameters:

| Mode | Description | Use case |
|------|-------------|----------|
| **Hybrid** | Vector search + rerank + multi-dimensional enhancements | **Recommended**, best overall performance |
| **Vector** | Vector semantic search only | Fast retrieval, no rerank model required |
| **Text** | Traditional keyword search | Fallback, no embedding model required |
| **Rerank** | Vector search + rerank | Requires rerank model |

**Rerank models:**  
For hybrid or rerank modes, import and load a rerank-type model (e.g., `qwen3-reranker`) in the model hub.

### Managing Knowledge Bases

- **View list**: All available collections are shown in the parameter drawer  
- **Delete collection**: Click the delete button next to a collection to remove it from chat  
- **Delete note**: Deleting a collection here only removes its configuration and **does not delete stored vector files**  
- **Re-import**: You can re-import a deleted collection as long as the vector data files still exist  

---

## Exporting from Favorites

Besides processing documents directly, you can export accumulated favorites from chat into RAG format:

### Steps

1. **Open Favorites management**: Click the "Favorites" button in the chat interface  
2. **Select data to export**: Check the conversations or liked content to export  
3. **Choose export format**: Select "RAG knowledge base format"  
4. **Configure export options**: Set name and other parameters  
5. **Run export**: After export, start from the **Vector Generation (Step 11)** step to complete subsequent processing  

### Use cases

- Turn high-quality AI answers into a knowledge base  
- Build a dedicated Q&A KB based on historical conversations  
- Convert liked content into searchable knowledge  

---

## Notes

### Environment Requirements

| Requirement | Description |
|------------|-------------|
| **Environment** | You must install an environment in **Settings** to run embedding models |
| **Embedding model** | At least one embedding-type model must be imported in the model hub |
| **Rerank model** (optional) | Recommended to import a rerank model to improve retrieval quality |
| **Chat model** (optional) | Required for AI analysis steps |
| **Disk space** | Vector databases consume disk space; ensure the output directory has enough free space |

### Quota Consumption

RAG data processing consumes free quotas:

- Step 0 (Conversion): free  
- Steps 1–3, 11–12: 1 quota unit per processed/generated record  
- Steps 4–5 (AI analysis): per-record cost according to AI generation quota  

### Performance Tips

- **First-time processing**: Large-scale first-time processing can take a long time; test with a small set of files first  
- **Incremental updates**: Supports incremental processing; newly added files do not require reprocessing all existing data  
- **GPU acceleration**: If a GPU is available, vector generation is significantly faster  
- **AI analysis**: Time for steps 4–5 depends on chat model inference speed  

---

## FAQ

**Q: Which embedding model should I choose?**

A: Choose based on your content language:

- Pure Chinese: `qwen3-embedding`, `bge-large-zh`, or `m3e-base`  
- Pure English: `qwen3-embedding`, `bge-large-en`, or `e5-large`  
- Mixed Chinese/English: `qwen3-embedding` or multilingual `bge-m3`  

**Q: Do I need to enable AI analysis steps?**

A: It depends:

- If documents are well-structured with clear headings, basic vector retrieval is usually enough  
- If content is highly specialized or user queries vary widely, enable AI analysis for better accuracy  
- AI analysis increases processing time; you can start without it and decide later based on retrieval results  

**Q: How to troubleshoot processing failures?**

A: Common causes:

1. Environment not installed in **Settings** – configure it first  
2. No embedding model imported – import an embedding-type model in the model hub  
3. Insufficient quota – check remaining quota or wait for reset  
4. AI analysis failure – check whether a chat model is loaded  

**Q: Can I modify existing knowledge-base content?**

A: Direct editing is not supported for now. To update:

1. Delete the old collection  
2. Modify the source documents and reprocess  
3. Or create a new collection with a new name for the updated version  

**Q: Does deleting a knowledge base in chat delete data?**

A: Deleting in the chat parameter drawer only removes the association and **does not delete actual vector data files**. To fully delete, manually remove vector storage files in the output directory.

**Q: What if retrieval quality is poor?**

A: Try the following:

1. Use hybrid retrieval and load a rerank model  
2. Enable AI analysis steps (4–5)  
3. Adjust chunk size to avoid overly small fragments  
4. Check whether cleaning removed valuable content  
5. Try a different embedding model  

**Q: Can I use Chinese in collection names?**

A: It is recommended to use English or Pinyin. Chinese names may cause compatibility issues in certain environments.

---

## Related Documents

- [Data Processing Overview](./overview.md) – Architecture of the data processing module  
- [SFT Data Processing](./sft.md) – Dataset generation for model fine-tuning  
- [Audio to Text](./audio2text.md) – Detailed guide to audio transcription
