# Attachments & Knowledge Bases

> v1.1.11
> 2026-10

The agent supports attaching files, images, audio, and other resources when sending messages, as well as connecting to RAG knowledge bases. This topic covers the usage and limitations of each attachment type.

## Table of Contents

- [Attachment Types](#attachment-types)
- [Local Attachments](#local-attachments)
- [Knowledge Base Connection](#knowledge-base-connection)
- [Workspace Reference](#workspace-reference)

---

## Attachment Types

The attachment menu on the left side of the input box supports the following types:

| Type | Icon / Entry | Description |
|------|--------------|-------------|
| Image | Attachment Menu → Image | Upload images for the agent to analyze |
| Audio | Attachment Menu → Audio | Upload audio files for processing |
| File | Attachment Menu → File | Upload any file for reading |
| Video | Attachment Menu → Video | Upload video files |
| Local Attachment | Attachment Menu → Local Attachment | Comprehensive entry, supports multiple formats |
| Knowledge Base Connection | Attachment Menu → Knowledge Base | Link a RAG knowledge base |
| Workspace Reference | Attachment Menu → Workspace Reference (Read-only) | Reference other workspace directories |
---

## Local Attachments

### Adding Attachments

1. Click the attachment button on the left side of the input box.
2. Select the appropriate attachment type.
3. Choose files from the local file system.

Image attachments can be pasted from the clipboard.

### Supported Formats

| Category | Supported Formats |
|----------|-------------------|
| Image | JPG, PNG, and other common image formats |
| Audio | Common audio formats |
| File | Text, PDF, Word, code files, etc. |
| Video | Common video formats |

### Usage Notes

- Attachments are sent to the agent along with the message.
- The agent can read attachment content for analysis.
- Attachments are isolated by session; the attachment list is automatically restored when switching sessions.
- Multimodal models support image understanding (requires loading a multimodal model category).

---

## Knowledge Base Connection

Link a RAG knowledge base to the current session, allowing the agent to prioritize answering questions based on your private knowledge base.

### Prerequisites

- A RAG knowledge base must already be created or imported.
- It is recommended to load both embedding and reranking models to improve retrieval quality.

### Connecting a Knowledge Base

1. Select **Knowledge Base Connection** from the attachment menu.
2. In the dialog, select an existing knowledge base from the dropdown list.
3. After confirmation, the connected knowledge base name is displayed next to the input box.

### Disconnecting a Knowledge Base

Click the close button next to the connected knowledge base name to disconnect.

Chat and the agent can attach the same knowledge base. Each side keeps its own retrieval settings. The underlying data is shared.

### Knowledge Base Parameters

The following parameters are available when connecting a knowledge base:

| Parameter | Description |
|-----------|-------------|
| Retrieval Method | Hybrid (recommended) / Vector / Vector+Rerank / Text |
| Retrieval Count | Number of documents returned by retrieval |
| Rerank Candidate Count | May improve recall quality |
| Score Threshold | Similarity filter (0~1) |
| Max Context Length | Maximum character count per single retrieval |
| Retrieval Prompt | Custom retrieval behavior instructions |

---

## Workspace Reference

You can attach an external directory. The agent can read files there and cannot write. `load_external_image` can also read images from those directories.

### Prerequisites

- Existing other workspace directories
- Maximum of 10 workspace directories can be referenced at the same time

### Referencing a Workspace

1. Select **Workspace Reference** from the attachment menu.
2. In the dialog, select an existing workspace directory from the dropdown list.
3. After confirmation, the referenced workspace directory is displayed next to the input box.

### Disreferencing a Workspace

Click the close button next to the referenced workspace directory to disconnect.