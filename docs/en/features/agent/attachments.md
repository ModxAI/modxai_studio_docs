# Attachments & Knowledge Bases

> v1.1.11
> 2026-11

Click **+** on the left of the agent input to add local files, connect a knowledge base, or reference another directory. Check the selected files and directories before sending the task.

## Local Attachments

1. Click **+ → Add Files** and choose one or more local files.
2. Wait for them to appear beside the input. You can also drag files into the input area or paste files from the clipboard.
3. Describe which files to use and the result you want, then send the task.

Remove unwanted attachments before sending. Each conversation keeps its own attachment draft when you switch conversations.

Common types include:

| Type | Common formats |
|------|----------------|
| Image | PNG, JPG, JPEG, WebP, BMP |
| Audio | MP3, WAV, OGG, M4A, AAC, FLAC |
| Video | MP4, WebM, MOV, M4V, AVI, MKV |
| File | TXT, Markdown, PDF, Office documents, code, ZIP, and others |

Attaching a file does not mean the current model can understand all of its contents directly. Image understanding needs a suitable multimodal model. Audio, video, documents, and archives also depend on available tools and file contents. Folders cannot be uploaded as attachments; use a workspace reference for a directory.

## Knowledge Base Connection

A knowledge base provides searchable material for the task. First create and import one through [data processing](../dataset/rag.md), then connect it to a conversation.

### Connect or Disconnect

1. Click **+ → Knowledge Base Link**.
2. Select an existing knowledge base and adjust retrieval settings as needed.
3. Confirm. Its name appears beside the input.
4. Send your question and describe which material to consult.

Use the close button on its tag to disconnect without deleting the knowledge base. Chat and the agent can use the same underlying knowledge base while keeping separate retrieval settings.

### Retrieval Settings

- **Retrieval method**: Hybrid retrieval (recommended), Dual-Path Fusion Retrieval, Embedding retrieval, Embedding + re-ranking retrieval, or Text retrieval.
- **Retrieval count**: Number of results to return.
- **Rerank candidate count**: Number of candidates passed to reranking.
- **Score threshold**: Filters out results below the threshold.
- **Maximum context length**: Limits the text returned by one retrieval.
- **Retrieval prompt**: Additional retrieval instructions.

Embedding retrieval needs an embedding model that matches the knowledge base. Reranking also needs a reranking model. If no results appear, check that the knowledge base is imported, required models are ready, and the threshold is not too high.

## Workspace Reference

**Workspace Reference(Read-only)** lets the agent read a directory outside the current workspace, such as an existing project. It does not make that directory the task's writable workspace.

1. Click **+ → Workspace Reference(Read-only)**.
2. Choose an existing directory in the system directory picker.
3. Check the reference shown beside the input, then send the task.

Repeat to add other directories. Use the close button on a reference tag to remove it. References are kept per conversation. Avoid selecting a parent directory containing private material unrelated to the task.

To create or modify files in a directory, set the current workspace in **Runtime Settings**. See [Workspace Browser](./workspace.md).
