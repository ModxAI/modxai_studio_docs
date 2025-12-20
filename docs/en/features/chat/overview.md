# AI Chat

> v1.0.0
> 2025-12

This article introduces how to use the **Text Chat** and **Multimodal Chat** features in ModxAI Studio. It helps you complete tasks such as daily conversation reasoning, knowledge base Q&A, and image-text understanding.

## Table of Contents

- [Environment Requirements](#environment-requirements)
- [Prerequisites](#prerequisites)
- [Interface Layout](#interface-layout)
- [Conversation Management](#conversation-management)
- [Sending Messages](#sending-messages)
- [RAG Retrieval Augmented Generation](#rag-retrieval-augmented-generation)
- [Inference Parameter Settings](#inference-parameter-settings)
- [Message Operations](#message-operations)
- [Data Collection](#data-collection)
- [Notes](#notes)

---

## Environment Requirements

**No additional environment dependencies are required for this feature.**

Text chat and multimodal chat features use llama.cpp as the inference engine, so there is no need to install environment in **Settings** beforehand.

You only need to:
1. Download and load a chat model or multimodal model in the Model Library.
2. Start chatting once the model is successfully loaded.

---

## Prerequisites

Before using the chat feature, you need to load a model in the **Model Library** page:

- **Text Chat**: Load a "Chat Model".
- **Image-Text Chat**: Load a "Multimodal Model".

When no model is loaded, the chat page will prompt you to "Go to Model Library to load <Chat Model>".

---

## Interface Layout

The chat page is divided into two columns:

### Left - Session List

- **New Chat** button: Located at the top.
- **Session List**: Grouped by time (Today/Yesterday/Within 7 Days/Within 30 Days/Older).
- **Session Operations**: Option menu to rename or delete conversations.

### Right - Chat Area

- **Message List**: Displays the history of the current conversation.
- **Input Box**: Located at the bottom, containing the following features:
  - **Thinking** switch: Enables the display of AI thinking processes.
  - **Retrieval** switch: Enables RAG knowledge base retrieval.
  - Attachment button (displayed only for multimodal models).
  - Prompt Generation button (Magic Wand icon).
  - Inference Parameter Settings button (Slider icon).
  - Send button.

---

## Conversation Management

### New Chat

Click the **"New Chat"** button at the top left. The system will:
1. Automatically generate a temporary title (e.g., "New Chat 10:30").
2. Automatically generate a formal title after the first message is sent.

### Switch Chat

Click directly on any conversation in the session list on the left.

### Rename Chat

1. Click the menu on the right side of the session.
2. Select **"Rename"**.
3. Enter the new title and confirm.

### Delete Chat

1. Click the menu on the right side of the session.
2. Select **"Delete"**.
3. Confirm the operation (this action cannot be undone).

---

## Sending Messages

### Text Chat

1. Enter your question in the input box.
2. Click the Send button or press the `Enter` key.

### Image-Text Chat (Multimodal)

**Prerequisite**: A multimodal model must be loaded.

1. Click the **Attachment Button** (Link icon) on the left side of the input box.
2. Upload an image:
   - Click the "Add Attachment" button to select a file.
   - Or drag and drop the image directly into the attachment area.
3. **Supported Formats**: JPG, PNG.
4. Enter a related question.
5. Click Send.

**Note**:
- The image will be sent to the model along with the text.
- When no image is added, the multimodal model will work in text mode.

### Feature Switches

There are two important switches above the input box:

#### Thinking Switch
- **Function**: When enabled, the AI will display its thinking process.
- **Display**: Collapsible panel "💭 AI Thinking Process".
- **Usage**: To understand the AI's reasoning logic.

#### Retrieval Switch
- **Function**: Enables RAG knowledge base retrieval.
- **Prerequisite**: A knowledge base must be selected first (see the next section).
- **Effect**: The AI will answer questions based on the content of your knowledge base.

---

## RAG Retrieval Augmented Generation

RAG allows the AI to answer questions based on your own database.

### Preparation

Before use, you need to create a RAG dataset (knowledge base) in the **Data Module**.

### Usage Steps

1. **Open Inference Parameter Settings** (Click the slider icon above the input box).
2. **Expand the "RAG Model Configuration" collapsible panel**.
3. **Select Knowledge Base**: Select a created dataset from the dropdown menu.
4. **Configure Parameters** (Optional):
   - **Retrieval Method**: Hybrid (Recommended) / Vector / Vector + Rerank / Text.
   - **TopK**: Number of retrieved documents (default is 5).
   - **Score Threshold**: Similarity filter (0-1, default is 0.3).
   - **Max Context Length**: Maximum number of characters for a single retrieval.
   - **RAG Search Prompt**: Custom retrieval behavior instructions.
5. **Close the Drawer**.
6. **Turn on the "Retrieval" switch** above the input box.
7. Send messages normally.

### Managing Knowledge Bases

- **View list**: All available collections are shown in the parameter drawer  
- **Delete collection**: Click the delete button next to a collection to remove it from chat  
- **Delete note**: Deleting a collection here only removes its configuration and **does not delete stored vector files**  

### Re-import
You can re-import a deleted collection as long as the vector data files still exist. You need to select the corresponding import file (rag_dataset_***_meta.json) to import.

> The program directory contains a sample knowledge base generated from the official manual. You can use **Re-import** to import this knowledge base and try the RAG features.  
> Import file path: `<InstallationDrive>/ModxAI Studio/ModxAI-Backend/_internal/server/resources/example/rag_manual/rag_dataset_reimport_meta.json`

### Delete Knowledge Base

In the knowledge base dropdown of the inference parameter drawer, there is a delete button on the right side of each knowledge base.

---

## Inference Parameter Settings

Click the **Slider Icon** button above the input box to open the parameter drawer.

### Basic Parameters

#### Temperature
- **Range**: 0-1
- **Default**: 0.7
- **Function**:
  - Low Temperature (<0.5): Answers are more stable and conservative.
  - High Temperature (>0.8): Answers are more creative and random.
- **Adjustment**: Slider or direct numerical input.
- **Reset**: Click the reset button on the right to restore the default value.

#### Max Tokens
- **Range**: 1-8192
- **Default**: 1024
- **Function**: Limits the maximum length of a single reply.
- **Note**: Too small a value may cause the answer to be truncated.

#### System Prompt
- **Function**: Sets the role or task background for the AI.
- **Save Template**: Frequently used prompts can be saved as templates.
- **Delete Template**: Select a template and click the delete button.

### RAG Parameters (Collapsible Panel)

See the [RAG Retrieval Augmented Generation](#rag-retrieval-augmented-generation) section above.

---

## Message Operations

### User Messages (Sent by You)

There is a floating button group to the right of each user message:

- **Edit**: Modify the message content and save.
- **Copy**: Copy to clipboard.
- **Delete**: Delete the message.
- **Resubmit**: Delete the message and all subsequent conversations, then resend.

**Resubmit Explanation**:
- Will delete all content after this message.
- Suitable for correcting input errors and re-asking.
- Operation cannot be undone, please use with caution.

### AI Replies

There is a floating button group to the right of each AI reply:

- **Collect** (Thumbs up icon): Add to the collection dataset.
- **Dislike** (Thumbs down icon): Mark as unsatisfactory (remove from added collections).
- **Copy**: Copy the reply content.
- **Delete**: Delete the reply.

**View Thinking Process**:
- If the "Thinking" switch is enabled.
- A collapsible panel "💭 AI Thinking Process" will be displayed above the AI reply.
- Click to expand and view the reasoning steps.

---

## Data Collection

### Collection Method

Click the **Collect Button** (Thumbs up icon) on a satisfactory AI reply. The system will:
1. Mark the reply as "Collected".
2. Automatically add it to the collection dataset.
3. The floating button in the bottom left corner displays the number of collections.

### Manage Collections

Click the **Collection Icon Floating Button** in the bottom left corner to open the Data Manager:

- **View List**: All collected Q&A pairs.
- **Edit**: Modify the question or answer content.
- **Delete**: Remove unwanted data.
- **Export**: Export as a trainable dataset.
  - Set export name.
  - Configure quality filtering conditions.
  - Select export location.
  - One-click export to standard format.

**Export Usage**:
- Used for model fine-tuning training.
- Building new RAG knowledge bases.
- Backing up important conversation records.

---

## Notes

### Model Requirements

- **Text Chat**: Must load a chat model.
- **Image-Text Chat**: Must load a multimodal model.
- Feature switches will be automatically enabled or disabled based on the current model.

### RAG Usage Restrictions

- Must create a RAG dataset first.
- Must select a knowledge base after turning on the retrieval switch.
- An error will be prompted if no knowledge base is selected.

### Image Format Restrictions

- **Supported**: JPG, PNG.
- **Not Supported**: GIF, BMP, WebP, and other formats.
- Uploading unsupported formats will be rejected.

### Empty Reply Handling

In rare cases, the AI may fail to generate a valid reply, and the following will be displayed:
- "The model did not return a valid reply."

Suggestions:
- Rephrase the question.
- Check if the model is loaded correctly.
- Check if the inference parameters are reasonable.

### Context Management

- Each conversation maintains its context independently.
- The backend automatically manages the context window length.
- For very long conversations, it is recommended to start a new session to avoid context confusion.

### Performance Suggestions

- High temperature and long replies will increase inference time.
- Pay attention to resource consumption when deploying locally.
- Appropriately control the number of concurrent conversations.

---

After completing this chapter, you can continue to learn about:
- **RAG Data Generation**: How to create and manage knowledge bases.
- **Prompt Generation**: Use AI to assist in optimizing prompts.
- **Training Module**: Fine-tune models based on collected data.
