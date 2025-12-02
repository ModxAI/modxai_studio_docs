# Data Collection

> v1.0.0
> 2025-12

This article introduces how to use the **Data Collection** feature of ModxAI Studio to collect, manage, and export conversation data for model fine-tuning or RAG knowledge base construction.

## Table of Contents

- [Environment Requirements](#environment-requirements)
- [Prerequisites](#prerequisites)
- [Feature Introduction](#feature-introduction)
- [Collecting Data](#collecting-data)
- [Managing Data](#managing-data)
- [Exporting Data](#exporting-data)
- [Export Format Explanation](#export-format-explanation)
- [Important Notes](#important-notes)
- [Usage Tips](#usage-tips)

---

## Environment Requirements

### Basic Features (Collection and Management)

**No additional environment dependencies required**

- Data collection and management features do not depend on models or Python environments.

### Advanced Features (Export SFT Format)

**Python environment installation is recommended for best results**

If you need to export SFT format data for model fine-tuning:

| Environment Status | Tokenization Method | Effect | Description |
| :--- | :--- | :--- | :--- |
| **Python Environment Installed** | Transformers Precise Tokenization | ⭐⭐⭐⭐⭐ Accurate | Uses the model's official tokenizer to ensure accurate token boundaries for training data. |
| **Python Environment Not Installed** | Lightweight Estimated Tokenization | ⭐⭐⭐ Usable | Uses character length estimation, which may have token boundary deviations. |

**Suggestion**:
- If you plan to perform model fine-tuning training, please download the Python environment in "Settings".
- If you only use it for RAG format export, no Python environment is needed.

---

## Feature Introduction

**Core functions of the Data Collection feature**:

1. **Collect High-Quality Conversation Data**: Filter high-quality Q&A pairs from actual conversations.
2. **Unified Management of Training Materials**: Edit, view, and delete collected data.
3. **Export Professional Format Data**:
   - **SFT Format**: Used for model fine-tuning training.
   - **RAG Format**: Used for building RAG knowledge bases.

### Typical Usage Scenarios

#### Scenario 1: Model Fine-tuning Training
```
1. In daily conversations, like high-quality AI answers.
2. Accumulate a sufficient number of Q&A pairs (recommended 100+).
3. Export SFT format data.
4. Use the training function to fine-tune the model.
```

#### Scenario 2: RAG Knowledge Base Construction
```
1. Collect professional Q&A pairs in a specific field.
2. Export RAG format data.
3. Put it into the RAG data processing flow to generate vectors.
4. Use for RAG retrieval augmented conversation.
```

---

## Collecting Data

### Method 1: Like in Conversation (Recommended)

In the chat interface, evaluate the AI's answer:

1. **Like Icon** (👍): Collect this Q&A pair.
   - Question: The message you sent.
   - Answer: The AI's reply.
   - Thinking Process (Optional): If the chain of thought is enabled, it will be saved together.

2. **Unlike**: Click the 👍 icon again to cancel the collection.

**Auto Save**:
- Saved to the local database immediately after liking.
- The number on the collection button in the bottom right corner updates in real-time.

### Method 2: Edit and Supplement Data

In the data management interface, you can:
- Manually edit the content of questions and answers.
- Adjust data status (Pending/Exported/Trained, etc.).

---

## Managing Data

### Open Data Management Interface

Click the **Collection Data Button** (showing number statistics) in the bottom right corner of the chat page:

```
📊 12 items
```

### Data List Features

#### View Data
- **Question Column**: Displays user question content (hover to view full content).
- **Answer Column**: Displays AI reply content (hover to view full content).
- **Status Column**: Displays data status.
  - 🟠 **Pending**: Newly collected data.
  - 🔵 **Exported**: Exported but not trained.
  - 🟢 **Trained**: Used for training.
  - 🔴 **Failed**: Data processing failed.
- **Create Time**: Collection time.

#### Filter and Sort
- **Status Filter**: Click the status column header to filter data by status.
- **Time Sort**: Click the create time column header to sort by time.
- **Quick Select**:
  - Select All: Select all data on the current page.
  - Invert Selection: Invert current selection.
  - Select Pending Items: Select only data with "Pending" status.

#### Data Operations
- **Details**: View complete questions, answers, and metadata.
- **Edit**: Modify questions, answers, or status.
- **Delete**: Delete unwanted data (cannot be undone).

**Important**: Deleting data will automatically cancel the like status of the corresponding message.

---

## Exporting Data

### Export Process

#### 1. Select Data
In the data management interface, check the data to be exported:
- Can select across pages.
- Recommended single export quantity: 50-200 items.

#### 2. Click Export Button
Click the **"Export"** button below the table to open the export configuration dialog.

#### 3. Configure Export Parameters

##### Basic Configuration

**Export Name** (Required):
```
Example: customer_service_qa
Note: Only supports letters, numbers, underscores, and hyphens.
```

**Output Directory** (Required):
```
Click the folder icon to select the save location.
Suggestion: Create a dedicated training data directory.
```

**Quantity Limit**:
```
Default: 100
Range: 1 to Total Selected Data
Note: Limits the number of data items exported this time.
```

**Enable Quality Filtering**:
```
☑ Enable: Filter out Q&A pairs that are too short or meaningless.
☐ Disable: Export all selected data.

Sub-options (displayed when enabled):
- Min Q&A Character Length: Default 2, Range 1-100.
```

##### Export Format (Critical)

**SFT Training Format**:
```
Usage: Model fine-tuning training.
Output: Two files.
  1. {name}_{batch}_{count}.jsonl (Plain text messages format)
  2. chat_dataset/{name}_{batch}_{count}_train.jsonl (Vectorized format, can be used directly for training)

Format Example:
{"messages": [
  {"role": "user", "content": "What is machine learning?"},
  {"role": "assistant", "content": "Machine learning is..."}
]}
```

**RAG Pipeline Format**:
```
Usage: Building RAG knowledge base.
Output: One file.
  opt3_dataset_rag_{name}_{batch}.json

Format: Compatible with RAG data processing flow analysis node output.
Can be directly put into **Generate Vector** node for processing.

Format Example:
{
  "base_path": "abc123",
  "file_name": "qa_001", 
  "chunk_count": 1,
  "chunks": ["Question: ...\n\nAnswer: ..."],
  "metadata": {
    "source": "personalized_training",
    "question": "...",
    "answer": "..."
  }
}
```

#### 4. Start Export

Click the **"Start Processing"** button:
- Wait for processing.
- Prompt success upon completion and display file path.

---

## Export Format Explanation

### SFT Format Deep Dive

#### Generated Files

```
Output Directory/
├── mymodel_20231130120000_50.jsonl          # Plain text format
└── chat_dataset/
    └── mymodel_20231130120000_50_train.jsonl # Vectorized format (ready for training)
```

#### Plain Text File Content

Each line is a JSON object containing the messages field:

```json
{"messages": [
  {"role": "user", "content": "User Question"},
  {"role": "assistant", "content": "AI Answer"}
]}
```

If there is a thinking process, it will be integrated into the assistant's answer:

```json
{"messages": [
  {"role": "user", "content": "Complex Question"},
  {"role": "assistant", "content": "<thinking>\nThinking Process\n</thinking>\n\nFinal Answer"}
]}
```

#### Vectorized File Content

Contains input_ids and label, ready for training:

```json
{
  "input_ids": [1, 2, 3, ...],  // token IDs
  "label": [-100, -100, 5, ...]  // Training labels (-100 means loss is not calculated)
}
```

#### Data Processing Logic

1. **Model Family Detection**:
   - Automatically detect target model type (Qwen/LLaMA).
   - Select tokenizer template based on model.

2. **Content Filtering**:
   - Enable quality filtering: Remove content that is too short or duplicated.
   - Sensitive word filtering: Automatically block inappropriate content.

3. **Smart Chunking**:
   - If a single Q&A exceeds the **Max Output Length Tokens** in global settings.
   - Automatically split the answer into multiple chunks.
   - Keep the question complete, only chunk the answer.

4. **Deduplication**:
   - Use content hash to detect duplicates.
   - Automatically skip existing Q&A pairs.

5. **Vectorization** (SFT format only):
   - Use tokenizer to convert text to token IDs.
   - Generate training labels (user part marked as -100, assistant part normal).

#### Model Template Compatibility (Important)

**Currently Supported Model Families**:
- **Qwen Series** (Default): Qwen/Qwen2/Qwen3, etc.
- **LLaMA Series**: LLaMA2/LLaMA3, etc.

**Detection Rules**:
1. Read config.json in the model directory.
2. Judge based on `architectures` or `model_type` fields.
3. If unable to judge, default to Qwen template.

**Template Difference Example**:

```python
# Qwen Template
<|im_start|>user
User Question<|im_end|>
<|im_start|>assistant
AI Answer<|im_end|>

# LLaMA Template  
[INST] User Question [/INST] AI Answer

```

**⚠️ Critical Note**:
- **Consequences of Template Mismatch**: Confirm the target model type when exporting. Incorrect training data format leads to training failure or poor results.

### RAG Format Deep Dive

#### Generated Files

```
Output Directory/
└── opt3_dataset_rag_myexport_20231130120000.json
```

#### File Content Structure

A JSON array, each element represents a QA pair:

```json
[
  {
    "base_path": "a1b2c3d4",
    "file_name": "qa_12345",
    "chunk_count": 1,
    "chunks": [
      "Question: What is machine learning?\n\nAnswer: Machine learning is a branch of artificial intelligence..."
    ],
    "chunk_hashes": ["a1b2c3d4"],
    "chunk_tokens": [250],
    "metadata": {
      "source": "personalized_training",
      "question": "What is machine learning?",
      "answer": "Machine learning is a branch of artificial intelligence...",
      "thinking": null,
      "original_id": "12345"
    }
  }
]
```

#### Subsequent Processing Flow

```
1. Put the exported JSON file into the RAG processing flow.
2. Check only the (**Generate Vector**) node.
3. Generate vectors and store them in the vector database.
4. Enable RAG retrieval in conversation to use.
```

#### Differences from SFT Format

| Feature | SFT Format | RAG Format |
| :--- | :--- | :--- |
| Usage | Model fine-tuning training | Knowledge base retrieval |
| Output Files | 2 (.jsonl) | 1 (.json) |
| Content Structure | messages array | chunk array |
| Vectorization | Automatic | Requires subsequent processing |
| Python Environment Needed | Recommended | Not needed |
| Model Template Dependency | Strong dependency | No dependency |

---

## Important Notes

### 1. Model Template Matching Issue

**Problem Description**:
The system defaults to the Qwen model template. If your target training model is not the Qwen series, a template mismatch may occur.

**Impact**:
- Incorrect training data format.
- Model cannot learn correctly.
- Training loss is abnormal or does not converge.

**Solutions**:

#### Solution 1: Use Qwen Series Models (Recommended)
```
Pros: No configuration needed, works out of the box.
Cons: Limits model selection.
```

#### Solution 2: Check Model Family Detection
```
1. View logs when exporting SFT data.
2. Confirm if "Detected model family" is correct.
3. If incorrect, it means the model config.json information is non-standard.
```

#### Solution 3: Bypass Issue with RAG Format
```
If only used for RAG knowledge base:
1. Select export RAG format (no template dependency).
2. Process in RAG flow subsequently.
```

**Future Improvement Directions**:
- Support user-defined template selection.
- Extend automatic detection for more model families.
- Provide template configuration interface.

### 2. Tokenization Precision Issue

**Limitations of Lightweight Tokenization**:

When Python environment is not installed, estimated tokenization is used:
- Rough estimation of token count based on character length.
- May lead to inaccurate chunk boundaries.
- Slight impact on training effect.

**Suggestion**:
- If data quality requirements are high, install Python environment.
- If only used for RAG, no need to worry about this issue.

### 3. Data Quality Control

**Role of Quality Filtering**:

After enabling quality filtering:
- ✅ Keep: Meaningful complete Q&A pairs.
- ❌ Skip: Content that is too short (e.g., "Okay", "Thanks").
- ❌ Skip: Duplicate Q&A pairs.

**Min Character Length**:
```
Default: 2 characters
Suggestion: Adjust according to actual needs.
- General conversation: 2-5 characters.
- Professional Q&A: 10 characters or more.
```

### 4. Data Volume Suggestions

| Usage | Min Data Volume | Recommended Data Volume | Description |
| :--- | :--- | :--- | :--- |
| Test Training Flow | 10 items | 50 items | Verify feature availability. |
| Style Fine-tuning | 100 items | 500 items | Adjust answer style and tone. |
| Domain Fine-tuning | 500 items | 2000 items | Learn specific domain knowledge. |
| RAG Knowledge Base | 50 items | Unlimited | The more the better, focus on quality. |

### 5. File Management

**Output File Naming Rules**:

SFT Format:
```
{export_name}_{batch}_{count}.jsonl
Example: customer_service_20231130120000_150.jsonl
```

RAG Format:
```
opt3_dataset_rag_{export_name}_{batch}.json
Example: opt3_dataset_rag_customer_service_20231130120000.json
```

**Recommended Directory Structure**:
```
Training Data/
├── Customer Service Domain/
│   ├── customer_service_v1_20231130120000_200.jsonl
│   └── chat_dataset/
│       └── customer_service_v1_20231130120000_200_train.jsonl
├── Technical Support/
│   └── tech_support_v1_20231201100000_300.jsonl
└── RAG Knowledge Base/
    └── opt3_dataset_rag_faq_20231202080000.json
```

---

## Usage Tips

### 1. Progressive Data Collection

**Phased Accumulation**:
```
Week 1: Collect 10-20 items, test export flow.
Week 2: Collect 50-100 items, try training verification.
Week 3: Collect 200+ items, formally fine-tune the model.
```

**Topic Focus**:
- Do not mix data from multiple domains.
- Data from the same export should be consistent in topic.

### 2. Data Quality First

**Collection Criteria**:
- ✅ Answer is accurate and logically clear.
- ✅ Length is moderate (50-500 words).
- ✅ Question is representative.
- ❌ Chat or meaningless conversation.
- ❌ Answer is wrong or vague.

**Edit and Improve**:
- Can enter management interface to edit after liking.
- Correct small errors in AI answers.
- Supplement missing key information.

### 3. Reasonable Use of Two Formats

**Scenarios for Choosing SFT Format**:
```
✓ Want the model to learn a specific answer style.
✓ Need the model to remember professional knowledge.
✓ Have sufficient data volume (100+ items).
✓ Willing to invest training time and resources.
```

**Scenarios for Choosing RAG Format**:
```
✓ Data volume is small (<100 items).
✓ Knowledge updates frequently.
✓ Do not want to modify the base model.
✓ Need explainable retrieval sources.
```

### 4. Batch Processing Strategy

**Batch Export**:
```
Scheme 1: Batch by Topic
- Batch 1: Product Consultation (100 items)
- Batch 2: After-sales Service (150 items)
- Batch 3: Technical Issues (80 items)

Scheme 2: Batch by Time
- Export once a week.
- Train after accumulating a certain amount.
```

**Version Management**:
```
Include version information in the export name:
- customer_service_v1
- customer_service_v2_refined
- customer_service_v3_final
```

### 5. Verify Export Results

**Check Plain Text File**:
```bash
# View file line count
wc -l {export_file}.jsonl

# View first few lines of data
head -n 3 {export_file}.jsonl | jq
```

**Check Vectorized File** (SFT only):
```bash
# View vectorized file
head -n 1 chat_dataset/{export_file}_train.jsonl | jq
```

**Verification Points**:
- Is the number of data items as expected?
- Is the messages format correct?
- Does the input_ids array exist (SFT)?

---

## FAQ

### Export button disabled?

**Reason**: No data selected.

**Solution**: Check at least one data item, and the export button will be enabled.

---

### Export failed: Output directory not selected?

**Reason**: Output directory field is required.

**Solution**: Click the folder icon to select the save location.

---

### Vectorized file not generated?

**Possible Reasons**:
1. Selected RAG format (does not generate vectorized file).
2. Python environment not installed and lightweight tokenization failed.
3. Tokenizer path configuration error.

**Solution**:
- Check export format selection.
- Retry after installing Python environment.

---

### Format error prompted during training?

**Most Likely Reason**: Model template mismatch.

**Troubleshooting Steps**:
1. Confirm the type of the target training model.
2. If mismatched:
   - Use Qwen series models, or
   - Export RAG format to bypass the issue.

---

### How to confirm data is correctly collected?

**Verification Method**:
1. After liking, the 👍 icon becomes solid.
2. The number on the collection button in the bottom right corner +1.
3. Open the data management interface, and you can see the new data.

---

### How to use RAG format files?

**Usage Flow**:
```
1. Open "Datasets" page.
2. Select RAG data processing flow.
3. Put the exported JSON file into the input directory.
4. Run **Generate Vector** node.
5. Enable RAG retrieval in conversation settings after completion.
```

---

After completing this chapter, you can continue to learn about:
- **Model Fine-tuning Training**: How to use exported SFT data to train models.
- **RAG Data Processing**: Complete RAG Pipeline flow.
- **Training Parameter Tuning**: Tips for improving training results.
