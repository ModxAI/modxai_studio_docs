# Voice Input

> v1.1.0
> 2026-05

The agent supports voice input via microphone. The system automatically transcribes speech to text and fills it into the input box.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Usage](#usage)
- [Status Descriptions](#status-descriptions)
- [Read Aloud](#read-aloud)
- [Notes](#notes)

---

## Prerequisites

> \u26a0\ufe0f **An STT model must be loaded and a microphone device connected** for voice input to function.

### Required Models

| Model | Purpose | Requirement |
|-------|---------|-------------|
| STT Model | Converts speech to text | Must be loaded for voice input |
| TTS Model | Converts text to speech (read aloud) | Optional, only needed for read-aloud |

### Loading Models

1. Import or download a **Speech-to-Text** model in the **Model Library**.
2. Load the **Speech-to-Text** model.
3. For read-aloud functionality, also load a **Text-to-Speech** model.

> When no STT model is loaded, clicking the microphone button displays the prompt "Speech recognition model not ready."

---

## Usage

### Start Recording

1. Click the **Microphone button** on the left side of the input box.
2. Recording begins; the button state changes to recording.
3. The interface displays a "Recording" indicator.

### Stop Recording & Transcribe

1. Click the microphone button again to stop recording.
2. The system automatically sends the audio to the STT model for transcription.
3. The interface displays a "Transcribing" indicator.
4. Once complete, the transcribed text is automatically filled into the input box.

### Recording Duration

- Single recording is limited to **120 seconds** (2 minutes).
- Recording ends automatically on timeout and transcription begins.

### Cancel Recording

You can cancel the current recording during recording (no transcription will be performed).

---

## Status Descriptions

Voice input has the following states:

| Status | Interface Prompt | Description |
|--------|------------------|-------------|
| Idle | "Click microphone to start voice input" | Waiting for user to start recording |
| Recording | "Listening for voice input" | Audio is being recorded |
| Transcribing | "Transcribing voice input" | Audio is being converted to text |
| Transcription Complete | Text auto-filled in input box | Ready to edit and send or send directly |
| Empty Result | "No valid speech content detected" | No valid speech detected in recording |
| Transcription Failed | "Speech transcription failed" | STT model processing error |

---

## Read Aloud

Agent responses support a **Read Aloud** feature (TTS speech synthesis).

### Prerequisites

- A **TTS (Text-to-Speech) model** must be loaded.
- The read-aloud button is hidden when no TTS model is loaded.

### Usage

Click the **Read Aloud button** (🔊) next to the agent's response to synthesize and play the response as speech.

---

## Notes

### Model Readiness Check

- The microphone button is unavailable when no STT model is loaded.
- The read-aloud button is unavailable when no TTS model is loaded.
- Model loading status can be checked on the Model Library page.

### Recording Quality

- Ensure the microphone device is working properly.
- Recording in a quiet environment yields more accurate transcription results.
- Supports the system default microphone device.

### Transcription Accuracy

- Transcription accuracy depends on STT model quality and recording clarity. The `faster-whisper-large-v3` model is recommended.
- Transcribed text is automatically filled into the input box and can be manually edited before sending.
- If transcription accuracy is unsatisfactory, try switching to a different STT model.
