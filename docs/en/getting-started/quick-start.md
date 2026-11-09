# Quick Start

> v1.1.11
> 2026-11

Follow these steps to start your first local chat. Installation path examples use Windows notation.

---

## Step 1: Launch the Application

1. Get the installer from the [download page](https://www.modxai.net/en/download), install it, and open the app.
2. On the welcome screen, choose **Get started**. You can skip the first environment install. Skip it when you only want llama.cpp text chat.
3. If the install directory needs administrator rights (for example `C:\Program Files`), start the app as administrator. Otherwise it will not start.

The first launch fetches initialization data once. If that fails, check the network and start again.

The main page can move backward and forward through pages you have already opened.

---

## Step 2: Get a Model

For the first use, you need to import or download a chat model.

### Method 1: Online Download (Recommended)

1. Open the **Model Center** submenu.
2. Choose **Download**.
3. Select a download source (Users in Chinese mainland are recommended to select **ModelScope**).
4. Select a model in the Chat Model category (e.g., Qwen3.5-9B-GGUF).
5. Select a quantized version (Recommended Q4_K_M, balancing effect and resource usage).
6. Click **"Start Download"** and wait for the download to complete.

> 💡 Model files are large, and download time depends on network speed.

### Method 2: Local Import

If you already have a local model file (.gguf format):

1. Open **Model Center** and click **Import**.
2. Select the model file.
3. Confirm import information and complete.

---

## Step 3: Load the Model

1. In the **"Chat Models"** tab of the Model Library.
2. Check the model you just downloaded/imported.
3. Click the **"Load"** button at the top.
4. Wait for the model loading to complete (Status changes to "Loaded").

---

## Step 4: Start Chatting

1. Open the **Agent** submenu and choose **Chat**.
2. Click **New Conversation** or **directly enter a question**.
3. Enter your question in the input box.
4. Click Send or use the shortcut selected in **Settings → General → Editor**. `Shift + Enter` inserts a new line.

🎉 Congratulations! You have successfully completed your first AI chat!

---

## Advanced Features

After completing the basic chat, you can explore more features:

| Feature | Description | Documentation |
|---------|-------------|---------------|
| **Agent** | Let AI autonomously execute tasks: web search, file operations, terminal commands | [Agent](../features/agent/overview.md) |
| **Workspace** | Preview files the agent reads or writes, share notes, link earlier tasks | [Workspace Browser](../features/agent/workspace.md) |
| **Memory** | Review, edit, and export long-term memories | [Memory](../features/memory/overview.md) |
| **Scheduled Tasks** | Run an agent, organize memory, or refresh a knowledge base on a schedule | [Scheduled Tasks](../features/scheduler/overview.md) |
| **External Access** | Reach the agent from Telegram, Discord, DingTalk, or Feishu, or call the local gateway HTTP API | [External Access](../features/gateway/overview.md) |
| **RAG Retrieval** | Let AI answer questions based on your knowledge base | [AI Chat](../features/chat/overview.md) |
| **Image Generation** | Generate images using SD models | [SD Chat](../features/sdchat/overview.md) |
| **Data Processing** | Convert documents into training data | [Data Processing](../features/dataset/overview.md) |
| **RSI Self Improvement** | Test and improve a system prompt or Skill, then compare and export results | [RSI](../features/rsi/overview.md) |
| **Model Training** | Fine-tune your exclusive model | [Model Training](../features/training/overview.md) |
| **LAN API** | Automatically provides OpenAI-compatible interface after loading model | [Model Library](../features/models/overview.md#lan-inference-service) |

---

## FAQ

### Model download is very slow?

- Try switching the download source (Hugging Face ↔ ModelScope).
- Check network connection.

### Model loading failed?

- Check if system memory is sufficient.
- Try selecting a smaller quantized version (e.g., Q2_K).

### No response in chat?

- Confirm the model has been successfully loaded (Status shows "Loaded").
- Open **Model Center** and check resource usage at the top right.

---

## Next Steps

- [Model Library](../features/models/overview.md) - Learn complete model management features.
- [AI Chat](../features/chat/overview.md) - Explore advanced features like RAG and Multimodal.
- [Global Settings](../features/settings/overview.md) - Configure runtime environment to unlock more features.
