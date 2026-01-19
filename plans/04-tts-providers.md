# Implementation Plan: New TTS Providers Documentation

## Overview

Add documentation for three TTS providers that are implemented but not documented: Cartesia Sonic 3, Azure Speech, and Kokoro.

## Files to Create

### 1. `tts-providers/cartesia.mdx`

**Purpose:** Complete Cartesia Sonic 3 TTS documentation

**Sections:**

1. **Overview**
   - Introduction to Cartesia Sonic 3
   - Key features: 42 languages, low latency, voice cloning
   - Best for: Multilingual agents, real-time conversations

2. **Setup**
   - Sign up at Cartesia
   - Get API key
   - Environment variable: `CARTESIA_API_KEY`
   - Per-assistant configuration

3. **Available Models**
   | Model | Description |
   |-------|-------------|
   | sonic-3 | Latest model (auto-updates) |
   | sonic-3-2025-10-27 | Stable snapshot |

4. **Available Voices**
   | Voice | Gender | Description |
   |-------|--------|-------------|
   | katie | Female | Stable, realistic for agents |
   | kiefer | Male | Stable, realistic for agents |
   | tessa | Female | Emotive, expressive |
   | kyle | Male | Emotive, expressive |
   | sarah | Female | Professional, clear |

5. **Language Support**
   - List all 42 supported languages with codes
   - Language configuration

6. **Voice Cloning**
   - Requirements (~5 seconds of clean audio)
   - Supported formats: MP3, WAV, FLAC, M4A, OGG
   - API workflow for cloning
   - UI workflow

7. **Configuration Options**
   - Speed control
   - Context continuation for natural flow
   - Flush tags for immediate speech

8. **Pricing**
   - Cost per character (~$0.015 per 1000 chars)

9. **Comparison with Other Providers**
   - Latency vs ElevenLabs vs Deepgram
   - Quality comparison
   - Use case recommendations

**Source Reference:** `docs/CARTESIA_TTS.md`, `app/services/tts_cartesia.py`

---

### 2. `tts-providers/azure.mdx`

**Purpose:** Azure Speech TTS documentation

**Sections:**

1. **Overview**
   - Introduction to Azure Speech TTS
   - Key features: 500+ neural voices, 100+ languages
   - Best for: Enterprise, Microsoft ecosystem integration

2. **Setup**
   - Azure portal setup
   - Create Speech resource
   - Get API key and region
   - Environment variables: `AZURE_SPEECH_KEY`, `AZURE_SPEECH_REGION`
   - Per-assistant configuration

3. **Available Voices**
   
   **English Voices:**
   | Voice | ID | Gender | Description |
   |-------|-----|--------|-------------|
   | Jenny | en-US-JennyNeural | Female | Clear, professional |
   | Aria | en-US-AriaNeural | Female | Warm, natural |
   | Guy | en-US-GuyNeural | Male | Confident |
   | Davis | en-US-DavisNeural | Male | Professional |
   
   **Other Languages:**
   - Document key voices for major languages
   - Link to full Azure voice gallery

4. **Configuration Options**
   - Voice selection
   - SSML support
   - Speaking rate
   - Pitch adjustment

5. **Pricing**
   - Azure pricing tiers
   - Free tier limits

6. **Best Practices**
   - Regional selection for latency
   - Voice consistency

**Source Reference:** `app/services/tts_azure.py`

---

### 3. `tts-providers/kokoro.mdx`

**Purpose:** Kokoro TTS (self-hosted) documentation

**Sections:**

1. **Overview**
   - Introduction to Kokoro TTS
   - Key feature: Self-hosted option
   - Best for: Privacy-sensitive deployments, on-premise

2. **Architecture**
   - Kokoro runs as separate microservice
   - Communication via HTTP/WebSocket
   - Docker deployment

3. **Setup**
   - Docker compose configuration
   - Environment variable: `KOKORO_TTS_URL`
   - Health check endpoints

4. **Available Voices**
   
   **English Voices:**
   | Voice | Gender | Description |
   |-------|--------|-------------|
   | af_heart | Female | Warm, expressive |
   | af_bella | Female | Clear, natural |
   | af_sarah | Female | Professional |
   | am_adam | Male | Clear, confident |
   | am_michael | Male | Natural |

5. **Configuration Options**
   - Server URL configuration
   - Timeout settings
   - Fallback behavior

6. **Deployment Guide**
   - Docker deployment
   - Resource requirements (CPU/GPU)
   - Scaling considerations

7. **Limitations**
   - Self-hosted only (no cloud option)
   - Limited voice selection vs cloud providers
   - Requires infrastructure management

**Source Reference:** `app/services/tts_kokoro.py`, `kokoro/` directory

---

## Files to Update

### 4. Update `tts-providers.mdx`

**Changes:**

1. **Add to Provider Comparison Cards:**

```mdx
<Card title="🎯 Cartesia Sonic 3" icon="bullseye" href="/tts-providers/cartesia">
  **Multilingual Excellence**
  
  42 languages, voice cloning, low latency
  
  *Best for: Multilingual agents, global deployments*
</Card>

<Card title="☁️ Azure Speech" icon="microsoft" href="/tts-providers/azure">
  **Enterprise Scale**
  
  500+ neural voices, 100+ languages, Microsoft integration
  
  *Best for: Enterprise, Azure ecosystem*
</Card>

<Card title="🏠 Kokoro" icon="server" href="/tts-providers/kokoro">
  **Self-Hosted**
  
  On-premise option, privacy-focused
  
  *Best for: Privacy-sensitive, on-premise deployments*
</Card>
```

2. **Update Feature Matrix:**

| Provider | Latency | Languages | Voice Cloning | Streaming | Best For |
|----------|---------|-----------|---------------|-----------|----------|
| ElevenLabs | ~250ms | 70+ | Advanced | WebSocket | Premium quality |
| Deepgram | ~75ms | English+ | No | WebSocket | Speed & phone calls |
| Inworld | ~200ms | 11 | Zero-shot | HTTP/WS | Emotional expression |
| Resemble | ~300ms | English | Custom | WebSocket | Brand voices |
| **Cartesia** | ~150ms | **42** | **Yes** | WebSocket | **Multilingual** |
| **Azure** | ~200ms | **100+** | No | HTTP | **Enterprise** |
| **Kokoro** | Varies | English | No | HTTP | **Self-hosted** |

3. **Update Quick Start Tabs:**

Add new tabs for Cartesia, Azure, Kokoro use cases.

---

## Navigation Updates

Add to `docs.json` in TTS Providers group:

```json
{
  "group": "TTS Providers",
  "pages": [
    "tts-providers",
    "tts-providers/elevenlabs",
    "tts-providers/deepgram",
    "tts-providers/cartesia",     // NEW
    "tts-providers/azure",        // NEW
    "tts-providers/kokoro",       // NEW
    "tts-providers/inworld",
    "tts-providers/resemble",
    "tts-providers/openai",
    "tts-providers/voice-tuning",
    "tts-providers/troubleshooting",
    "tts-providers/best-practices"
  ],
  "icon": "microphone"
}
```

---

## Implementation Steps

1. Create `tts-providers/cartesia.mdx` (most detailed, has existing docs)
2. Create `tts-providers/azure.mdx`
3. Create `tts-providers/kokoro.mdx`
4. Update `tts-providers.mdx` comparison table and cards
5. Update navigation in `docs.json`
6. Add cross-references from assistant configuration docs
7. Test all links and examples

---

## Source Files to Reference

- `docs/CARTESIA_TTS.md` - Existing Cartesia documentation
- `app/services/tts_cartesia.py` - Cartesia implementation
- `app/services/tts_azure.py` - Azure implementation
- `app/services/tts_kokoro.py` - Kokoro implementation
- `kokoro/README.md` - Kokoro deployment docs
- `Features.MD` Section 5.1

---

## Estimated Effort

- Cartesia page: 2 hours (good existing docs)
- Azure page: 1.5 hours
- Kokoro page: 1.5 hours
- Update tts-providers.mdx: 1 hour
- Navigation + testing: 0.5 hours

**Total: ~6.5 hours**

---

## Dependencies

- None (can be implemented independently)

## Success Criteria

- All three new provider pages render correctly
- Comparison table accurately reflects capabilities
- Voice lists are accurate
- Setup instructions are complete
- Cross-references work
