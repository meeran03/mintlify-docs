# Implementation Plan: Azure STT Documentation

## Overview

Add Azure Speech STT as a provider option. Currently, only Deepgram is documented for STT.

## Files to Create

### 1. `stt-providers/azure.mdx`

**Purpose:** Azure Speech STT provider documentation

**Sections:**

1. **Overview**
   - Introduction to Azure Speech STT
   - Key features:
     - 100+ languages supported
     - Real-time transcription
     - Custom speech models
     - Phrase lists (like keyterms)
     - Speaker diarization
     - Multi-channel support
   - Best for: Enterprise, Microsoft ecosystem, multi-language support

2. **Setup**
   - Azure portal setup
   - Create Speech resource
   - Get subscription key and region
   - Environment variables:
     ```env
     AZURE_SPEECH_KEY=your_subscription_key
     AZURE_SPEECH_REGION=eastus
     ```
   - Per-assistant configuration:
     ```json
     {
       "stt_settings": {
         "provider": "azure",
         "azure_config": {
           "subscription_key": "...",
           "region": "eastus"
         }
       }
     }
     ```

3. **Available Models**

   | Model | Description | Use Case |
   |-------|-------------|----------|
   | Standard | Default Azure model | General transcription |
   | Custom | Organization-trained | Domain-specific terminology |

4. **Language Support**
   - List major supported languages
   - Language code format (e.g., "en-US", "es-ES")
   - Auto language detection option

5. **Configuration Options**

   ```json
   {
     "stt_settings": {
       "provider": "azure",
       "model": "standard",
       "language": "en-US",
       "azure_config": {
         "subscription_key": "...",
         "region": "eastus",
         "enable_diarization": false,
         "phrase_list": ["Burki", "AI assistant"]
       }
     }
   }
   ```

   - **Phrase Lists**: Boost recognition of specific terms
   - **Diarization**: Speaker identification
   - **Profanity Filter**: Options for masking

6. **Comparison with Deepgram**

   | Feature | Azure Speech | Deepgram |
   |---------|--------------|----------|
   | Languages | 100+ | 30+ |
   | Real-time | Yes | Yes |
   | Custom Models | Yes | Limited |
   | Latency | ~200ms | ~100ms |
   | Keywords | Phrase Lists | Keywords/Keyterms |
   | Diarization | Yes | Yes |
   | Best For | Enterprise, Multi-language | Speed, Phone calls |

7. **Best Practices**
   - Regional selection for latency
   - Using phrase lists for domain terms
   - When to use Azure vs Deepgram

8. **Troubleshooting**
   - Authentication errors
   - Region mismatch issues
   - Language detection problems

**Source Reference:** `app/services/stt_azure.py`

---

## Files to Update

### 2. Update `stt-providers.mdx`

**Current State:** Only documents Deepgram

**Changes:**

1. **Update Title/Description:**
   ```mdx
   ---
   title: STT Providers & Settings
   description: Configure Speech-to-Text providers including Deepgram and Azure Speech
   ---
   ```

2. **Add Provider Selection:**
   ```mdx
   <Callout type="info">
   Burki Voice AI supports multiple STT providers. Choose Deepgram for speed and phone optimization, or Azure Speech for enterprise features and broader language support.
   </Callout>
   ```

3. **Add Provider Comparison Table:**
   ```mdx
   ## Provider Comparison

   | Feature | Deepgram | Azure Speech |
   |---------|----------|--------------|
   | **Speed** | Ultra-fast (~100ms) | Fast (~200ms) |
   | **Languages** | 30+ | 100+ |
   | **Models** | Nova 2, Nova 3 | Standard, Custom |
   | **Keywords** | Keywords, Keyterms | Phrase Lists |
   | **Best For** | Phone calls, Speed | Enterprise, Multi-language |
   ```

4. **Add Azure Section:**
   ```mdx
   ## Azure Speech

   <Accordion title="Azure Speech Configuration">
   Azure Speech provides enterprise-grade speech recognition with broad language support.

   **Setup:**
   1. Create Azure Speech resource
   2. Get subscription key and region
   3. Configure in assistant settings

   **Configuration:**
   ```json
   {
     "stt_settings": {
       "provider": "azure",
       "language": "en-US",
       "azure_config": {
         "subscription_key": "...",
         "region": "eastus"
       }
     }
   }
   ```

   See [Azure Speech STT Guide](/stt-providers/azure) for detailed documentation.
   </Accordion>
   ```

5. **Update the intro to mention multiple providers**

---

## Navigation Updates

Update `docs.json`:

Option A - Create STT Providers group (like TTS):
```json
{
  "group": "STT Providers",
  "pages": [
    "stt-providers",
    "stt-providers/azure"
  ]
}
```

Option B - Add as sub-page under existing:
```json
// In AI Providers group
"stt-providers",
"stt-providers/azure"
```

---

## Implementation Steps

1. Create `mintlify-docs/stt-providers/` directory
2. Create `stt-providers/azure.mdx`
3. Update `stt-providers.mdx` to be multi-provider aware
4. Update navigation in `docs.json`
5. Add cross-references from assistant configuration docs
6. Test configuration examples

---

## Source Files to Reference

- `app/services/stt_azure.py` - Azure STT implementation
- `app/services/stt_base.py` - STT provider enum, base classes
- `app/services/stt_factory.py` - Provider selection
- `Features.MD` Section 4.1

---

## Key Information from Source

From `app/services/stt_azure.py`:

**Available Models:**
```python
_available_models = {
    "standard": ModelInfo(
        id="standard",
        name="Standard",
        description="Standard Azure Speech model with good accuracy",
        supported_languages=[...100+ languages...],
        supports_keywords=False,
        supports_keyterms=True,  # Via phrase lists
        supports_diarization=True,
        supports_multichannel=True,
        real_time=True,
    )
}
```

**Key Features:**
- Real-time transcription
- Phrase lists for term boosting
- Speaker diarization
- Multi-channel support
- Wide language support

---

## Estimated Effort

- Azure STT page: 1.5 hours
- Update stt-providers.mdx: 1 hour
- Navigation + testing: 0.5 hours

**Total: ~3 hours**

---

## Dependencies

- None (can be implemented independently)

## Success Criteria

- Azure STT page documents all configuration options
- stt-providers.mdx reflects multi-provider support
- Comparison table is accurate
- Configuration examples work correctly
- Navigation is updated
