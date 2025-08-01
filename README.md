# Project Setup Guide

This document outlines the steps to get started with the project and configure the necessary global settings. This setup is required to enable core features such as transcript extraction, video scoring, voiceover generation, and YouTube API interaction.

---

## 1. Prerequisites

Before starting, ensure you have the following:

- A Python 3.10+ environment
- Access to `userdata.get()` credentials (environment manager or secrets loader)
- API keys for:
  - OpenAI (2 separate project keys)
  - Supadata (Pro plan or higher)
  - ElevenLabs (Developer plan or higher)
  - YouTube Data API v3 (with domain verified for increased quota)
- Optional: Proxy credentials if routing requests via proxy. THIS IS REQUIRED IN PRODUCTION!!

---

## 2. Globals Configuration

The Cloab has a **Globals** cell or module. This holds all key configurations. Please refer to it. An example of what that cell looks like is below. 

### Example Global Configuration

```python
# Toggle this to True once you've added all credentials and followed setup
I_ADDED_ALL_OF_THIS_AND_FOLLOWED_INSTRUCTIONS = False

# Two separate OpenAI API keys from different projects (to bypass rate limit)
API_KEY_1 = userdata.get("API_KEY_1")
API_KEY_2 = userdata.get("API_KEY_2")

# Supadata API Key - requires Pro plan or higher
SUPADATA_API_KEY = userdata.get("SUPADATA_API_KEY")

# Eleven Labs API Key - requires Developer plan or higher
ELEVEN_LABS_API_KEY = userdata.get("ELEVEN_LABS_API_KEY")

# YouTube API Key - must have domain verified for 10K quota
YOUTUBE_API_KEY = userdata.get("YOUTUBE_API_KEY")

# Optional Proxy (used only if setup is confirmed)
if I_ADDED_ALL_OF_THIS_AND_FOLLOWED_INSTRUCTIONS:
    PROXY_USERNAME = userdata.get("PROXY_USERNAME")
    PROXY_PASSWORD = userdata.get("PROXY_PASSWORD")
    PROXY_URL = f"http://{PROXY_USERNAME}:{PROXY_PASSWORD}@127.0.0.1:5000"
else:
    PROXY_URL = None

# Eleven Labs default voice configuration
DEFAULT_VOICE_ID = "JKbdwi8BFwQlr1n3fwoT"
ELEVEN_LABS_VOICE_SPEED = 0.90

# Default GPT model used for all OpenAI calls
DEFAULT_GPT_MODEL = "gpt-3.5-turbo"  # or "gpt-4o"

# Output directory for downloaded clips
DOWNLOAD_OUTPUT_DIR = "clips"

# Default weights for video segment scoring
DEFAULT_WEIGHTS = {
    "text": 0.23,
    "semantic": 0.15,
    "diversity": 0.04,
    "entity": 0.17,
    "dual": 0.08,
}

# Custom weights for advanced segment scoring
CUSTOM_WEIGHTS = {
    "text": 0.45,
    "visual": 0.40,
    "semantic": 0.20,
    "audio": 0.05,
    "ocr": 0.05,
    "diversity": 0.04,
    "entity": 0.14,
    "dual": 0.10,
}

# Embedding model used for semantic similarity
EMBEDDER_MODEL_NAME = "BAAI/bge-small-en"
```

---

## 3. Notes

- Do not change weight values unless you fully understand their role in clip scoring.
- Always toggle `I_ADDED_ALL_OF_THIS_AND_FOLLOWED_INSTRUCTIONS` to `True` after setup. This enables proxy and secures conditional logic.
- The system expects these keys to be present or it may crash on startup or during pipeline execution.

---

## 4. Running the Pipeline

Once the globals are configured, the main APIs `/script` and `/video-gen` can be called using your client of choice (Postman, cURL, frontend app).

If you are missing configuration, the API will return meaningful error responses to guide you.
