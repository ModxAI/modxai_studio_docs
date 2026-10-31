# Memory Management

> v1.1.10
> 2026-10

Memory management provides a unified view for reviewing and managing the **long-term memory** accumulated by the agent during execution, as well as manually **bookmarked** Q&A entries. You can preview, edit, archive, delete, or export memories as data formats suitable for training or knowledge base use.

## Table of Contents

- [Memory Sources & Categories](#memory-sources--categories)
- [Browse & Filter](#browse--filter)
- [View Memory Details](#view-memory-details)
- [Edit Memory](#edit-memory)
- [Archive & Unarchive](#archive--unarchive)
- [Delete Memory](#delete-memory)
- [Export Memory](#export-memory)
- [Notes](#notes)

---

## Memory Sources & Categories

The list uses three categories:

| Category | Description |
|----------|-------------|
| **Active memory** | Saved by the agent while it runs a task |
| **Manual bookmarks** | Q&A the user bookmarks or marks with 👍 |
| **Auto organization** | Written by a scheduled automatic memory-organization task |

### Memory Types

The list types are: summary, insight, task result, context, daily note, and bookmarked Q&A.

---

## Browse & Filter

### Statistics Panel

The top of the page shows memory statistics:

| Statistic | Description |
|-----------|-------------|
| Total Memories | Total number of memories under the current filter |
| Active memory | Count of entries in the Active memory category |
| Manual Bookmarks | Number of manually bookmarked memories |
| Exportable SFT | Number of memories exportable as SFT format |
| Exportable RAG | Number of memories exportable as RAG format |
| Archived | Number of archived memories in the full dataset |

### Filter Conditions

Filter by the following conditions:

| Filter | Options | Description |
|--------|---------|-------------|
| **Search Box** | Free text | Search by title, summary, content, or Q&A text |
| **Category** | All / Active memory / Manual bookmarks / Auto organization | Filter by memory source |
| **Memory Type** | Summary / Insight / Task result / Context / Daily note / Bookmarked Q&A | Filter by memory type |
| **Archive Status** | Not Archived / All / Archived | Filter active or archived memories |

The list and the detail panel both show **category** and **memory type**. The filters use those two fields.

---

## View Memory Details

Click the **Preview** button on a memory entry to open a detail panel showing complete information:

| Field | Description |
|-------|-------------|
| Title | Memory title |
| Category | Active memory / Manual bookmarks / Auto organization |
| Type | Summary / Insight / Task result / Context / Daily note / Bookmarked Q&A |
| Tags | Associated tag list |
| Export Status | Exported / Not Exported |
| Archive Status | Active / Archived |
| Question (Bookmarked) | Question in bookmarked Q&A |
| Answer (Bookmarked) | Answer in bookmarked Q&A |
| Summary | Memory summary |
| Content | Full memory content |
| Created At | Time the memory was generated |
| Updated At | Time of last modification |
| Last Exported At | Time of most recent export |

---

## Edit Memory

> **Note**: Editing **directly overwrites** the original memory content, which is irreversible. Please confirm before saving.

Click the **Edit** button on a memory entry to open the edit dialog.

The edit form displays different fields depending on the memory category:

| Category | Editable Fields |
|----------|-----------------|
| Active memory | Title, Summary, Content |
| Auto organization | Title, Summary, Content |
| Manual Bookmarks | Title, Question, Answer |

After saving, a second confirmation is required before the memory content is updated.

---

## Archive & Unarchive

### Archive

Archived memories are **invisible** to the agent but remain in the management page for viewing and exporting.

- **Single Archive**: Click the archive button on a memory entry and confirm.
- **Batch Archive**: Select multiple entries and click **Archive Selected**.

### Unarchive

After unarchiving, the memory becomes visible to the agent again.

- **Single Unarchive**: Click the unarchive button in the archived view.
- **Batch Unarchive**: Select multiple archived entries and click **Unarchive Selected**.

> **Note**: Archiving only affects records in the Active memory category. Bookmarked Q&A entries do not participate in archiving.

---

## Delete Memory

> **Deleted memories cannot be recovered.** Please proceed with caution.

Click the **Delete** button on a memory entry and confirm to delete.

---

## Export Memory

You can export memories as data files for **model fine-tuning (SFT)** or **knowledge base construction (RAG)**.

### Prerequisites

- At least one memory record selected.
- Specify an export name and output directory.
- Selected records must be compatible with the export format (see below).

### Export Steps

1. Select the memories to export in the list.
2. Click the **Export** button to open the export settings panel.
3. Enter an export name.
4. Select an output directory.
5. Choose an export format:
   - **SFT**: Exports as Q&A training data, suitable for model fine-tuning.
   - **RAG**: Exports as data ready for the **Generate Vectors** step, suitable for knowledge base construction.

### Export Compatibility

| Format | Compatible With | Incompatible With |
|--------|-----------------|-------------------|
| **SFT** | Only manual bookmark memories | Agent automatic memories |
| **RAG** | Memories with body content | Title-only memories without body content |

> **Tip**: The export panel shows real-time counts of "Exportable" and "Will Be Skipped" to help determine format suitability.

After successful export, you can choose whether to **open the output directory** to view the exported files.

### Export Status

After a memory is exported, it is marked as "Exported." Subsequent exports do not re-mark it as unexported.

---

## Notes

- Search supports fuzzy matching on title, summary, content, and Q&A text.
- Archiving directly affects the agent's memory retrieval behavior — archived memories will not be recalled in context.
- SFT export only supports the "Manual Bookmarks" category. If agent automatic memory content is suitable for fine-tuning, it must first be converted to bookmarked memory via the bookmark feature.
- RAG export produces data ready for the **Generate Vectors** step.
