# API Documentation

This API allows users to generate a script for a YouTube-style video and generate a full video using AI and YouTube clips.

---

## Endpoints Overview

| Endpoint        | Method | Description                        |
|-----------------|--------|------------------------------------|
| `/script`       | POST   | Generates a script based on prompt and duration |
| `/video-gen`    | POST   | Generates a video based on the script and settings |
| `/health`       | GET    | Simple health check (returns JSON) |

---

## POST `/script`

Generate a sectioned script for a video based on a given prompt and target duration.

### Request

**Content-Type:** `application/json`

```json
{
  "prompt": "How do solar panels work?",
  "duration": 30
}
```

### Parameters

| Name      | Type   | Required | Description                                 | Min | Max | Notes |
|-----------|--------|----------|---------------------------------------------|-----|-----|-------|
| prompt    | string | ✅        | A short topic prompt                        | 10  | 200 | Must be a meaningful, searchable idea |
| duration  | int    | ✅        | Desired video duration (in seconds)         | 5   | 60  | Free users: 5–15s, Paid users: up to 60s |

### Response

```json
{
  "script": {
    "prompt": "How do solar panels work?",
    "duration": 30,
    "sections": [
      {
        "start": 0,
        "end": 7,
        "text": "Solar panels capture sunlight using photovoltaic cells."
      },
      ...
    ]
  }
}
```

---

## POST `/video-gen`

Run the full video generation pipeline using an enriched script and a user prompt.

### Request

**Content-Type:** `application/json`

```json
{
  "prompt": "How do solar panels work?",
  "script": {
    "sections": [
      {
        "start": 0,
        "end": 7,
        "text": "Solar panels capture sunlight using photovoltaic cells."
      }
    ]
  },
  "isVertical": false,
  "isMuted": false,
  "hasVoiceover": true,
  "ttsVoiceId": "EXAMPLE_VOICE_123",
  "fallbackVoiceId": "DEFAULT_VOICE_456"
}
```

### Parameters

| Name            | Type    | Required | Default | Description |
|-----------------|---------|----------|---------|-------------|
| prompt          | string  | ✅        | —       | Original user prompt to guide the visuals |
| script          | object  | ✅        | —       | Must contain `sections` array |
| isVertical      | boolean | ❌        | false   | Format video as vertical (e.g., Shorts or Reels) |
| isMuted         | boolean | ❌        | false   | If true, disables all audio in the video |
| hasVoiceover    | boolean | ❌        | false   | Enable TTS voiceover for each section |
| ttsVoiceId      | string  | ❌        | fallbackVoiceId | Preferred ElevenLabs voice ID |
| fallbackVoiceId | string  | ❌        | system default | Backup voice ID if `ttsVoiceId` is missing |

### Response

```json
{
  "success": true,
  "videoUrl": "We dont have this implemented yet"
}
```

### Error Responses

| Code | Description                                 |
|------|---------------------------------------------|
| 400  | Invalid or missing JSON fields              |
| 500  | Pipeline failure or internal server error   |

---

## GET `/health`

A simple health check endpoint.

### Response

```json
{
  "success": "working fine"
}
```
