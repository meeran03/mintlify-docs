# Implementation Plan: Advanced Features Documentation

## Overview

Document several advanced features that are implemented but not covered in docs: IVR Explorer, Learning & Memory System, Spam Detection, and Per-Assistant API Keys (BYO Keys).

## Files to Create

### 1. `ivr-explorer.mdx`

**Purpose:** Document the unique IVR mapping and conversion feature

**Sections:**

1. **Introduction**
   - What is IVR Explorer?
   - Unique value proposition: Map existing IVR phone trees automatically
   - Use cases:
     - Understanding competitor IVRs
     - Documenting legacy phone systems
     - Converting IVRs to AI assistants

2. **How It Works**
   ```
   1. Provide target phone number
   2. System calls and navigates the IVR
   3. Records and transcribes all prompts
   4. Builds visual tree map
   5. AI summarizes each menu option
   ```

3. **Exploration Strategies**
   | Strategy | Description | Best For |
   |----------|-------------|----------|
   | LLM-Driven | AI decides navigation path | Complex IVRs |
   | DTMF-Only | Systematic keypad exploration | Simple menus |
   | Hybrid | Combination approach | Most cases |

4. **Starting an Exploration**
   - API endpoint to initiate
   - Parameters (phone number, strategy, max depth)
   - Webhook notifications

5. **IVR Tree Visualization**
   - Node types:
     - Root: Entry point
     - Menu: Options menu
     - Submenu: Nested menus
     - Action: Terminal actions
     - Transfer: Human transfer points
     - Hold: Hold states
     - Loop: Return to previous menu
     - Dead End: No further options
   - Interactive tree view in dashboard

6. **Converting IVR to AI Assistant**
   - One-click conversion feature
   - What gets created:
     - Single assistant (simple IVRs)
     - Assistant graph (complex IVRs)
   - Human review and editing
   - Testing before deployment

7. **API Reference**
   - `POST /api/v1/ivr-explorer/explore` - Start exploration
   - `GET /api/v1/ivr-explorer/{exploration_id}` - Get status/results
   - `POST /api/v1/ivr-explorer/{exploration_id}/convert` - Convert to assistant
   - `GET /api/v1/ivr-explorer/trees` - List all explored trees

8. **Limitations**
   - Voice-only IVRs may have reduced accuracy
   - Some IVRs have anti-bot measures
   - Cost considerations (phone calls are billed)

**Source Reference:** `app/api/ivr_explorer.py`, `Features.MD` Section 12

---

### 2. `learning-memory.mdx`

**Purpose:** Document the Learning & Memory System

**Sections:**

1. **Introduction**
   - What is the Memory System?
   - How it enables personalized conversations
   - Privacy-first design

2. **Memory Types**
   | Type | Description | Example |
   |------|-------------|---------|
   | Semantic | General facts | "Customer prefers email contact" |
   | Episodic | Specific events | "Discussed order #123 on Jan 5" |
   | Procedural | How-to knowledge | "To escalate, transfer to ext 1234" |

3. **Memory Scopes**
   | Scope | Applies To | Use Case |
   |-------|-----------|----------|
   | Organization | All calls in org | Company policies |
   | Assistant | One assistant | Assistant-specific knowledge |
   | Location | Store/branch | Location-specific info |
   | Caller | Individual customer | Customer preferences |

4. **Memory Features**
   - Privacy-safe identifiers (hashed phone numbers)
   - Confidence scoring (0-1)
   - TTL (Time-to-Live) for automatic expiration
   - Soft delete for GDPR compliance
   - Vector embeddings for semantic search

5. **Memory Graph**
   - Relationships between memories:
     - Temporal (happened before)
     - Causal (caused by)
     - Resolution (resolved by)
     - Semantic (relates to)
     - Conflict (contradicts)
     - Supersedes (replaces)

6. **Caller Privacy**
   - Opt-out support
   - GDPR compliance
   - Memory write policies

7. **Prompt Versioning**
   - Lifecycle stages: Draft → Testing → Approved → Canary → Production → Retired
   - A/B testing prompts
   - Rollback capability

8. **Evaluation System**
   - Creating eval datasets
   - Test cases
   - Running evaluations
   - Metrics: keyword matching, intent accuracy, tool call accuracy

9. **PII Redaction**
   - Automatic detection before storage
   - Types detected: phone, email, SSN, credit card, addresses, DOB, IP
   - Replacement tokens

10. **API Reference**
    - Memory CRUD endpoints
    - Caller privacy endpoints
    - Prompt version endpoints
    - Eval dataset endpoints

**Source Reference:** `app/api/learning.py`, `Features.MD` Section 13

---

### 3. `spam-detection.mdx`

**Purpose:** Document the Spam Detection System

**Sections:**

1. **Introduction**
   - AI-powered real-time spam evaluation
   - Protects against spam/scam calls
   - Non-blocking design

2. **How It Works**
   ```
   Call starts → LLM evaluates transcript periodically →
   Aggregates verdicts → Terminates if spam confirmed
   ```

3. **Configuration**
   ```json
   {
     "spam_detection_config": {
       "enabled": true,
       "denied_scenarios": [
         "caller is trying to sell insurance",
         "caller is conducting a survey",
         "caller refuses to identify themselves"
       ],
       "allowed_scenarios": [
         "caller is a returning customer",
         "caller mentions a valid order number"
       ],
       "max_evaluations_per_call": 5,
       "min_evaluations": 2,
       "min_confidence": 0.75,
       "min_call_duration_before_termination": 15,
       "min_transcript_length": 50,
       "min_transcript_turns": 1
     }
   }
   ```

4. **Scenario Configuration**
   - Denied scenarios: What constitutes spam
   - Allowed scenarios: Whitelist legitimate patterns
   - Examples for common use cases

5. **Verdict Aggregation**
   - Minimum evaluations required
   - Confidence threshold
   - Consistency requirements

6. **Behavior**
   - Grace period before termination
   - Non-blocking (doesn't slow calls)
   - Fail-open design (errors allow calls)
   - Automatic termination when confirmed

7. **Best Practices**
   - Start with conservative settings
   - Monitor false positives
   - Tune based on call analytics

**Source Reference:** `app/services/spam_detection_service.py`, `Features.MD` Section 18.8

---

### 4. `byo-api-keys.mdx`

**Purpose:** Document Per-Assistant API Keys (Bring Your Own Keys)

**Sections:**

1. **Introduction**
   - What is BYO Keys?
   - Benefits:
     - Use your own provider accounts
     - Control costs directly
     - Use enterprise agreements
   - How it works: Assistant-level → Organization-level → System fallback

2. **Supported Providers**

   | Provider | Key Type | Fields |
   |----------|----------|--------|
   | ElevenLabs | API Key | `api_key` |
   | Deepgram | API Key | `api_key` |
   | Cartesia | API Key | `api_key` |
   | OpenAI | API Key | `api_key` |
   | Azure Speech | Key + Region | `api_key`, `region` |
   | Inworld | Bearer + Workspace | `bearer_token`, `workspace_id` |
   | Resemble AI | API Key | `api_key` |

3. **Configuration**

   **TTS Provider Keys:**
   ```json
   {
     "tts_settings": {
       "provider": "elevenlabs",
       "byo_api_keys": {
         "elevenlabs": {
           "api_key": "your-elevenlabs-key"
         }
       }
     }
   }
   ```

   **STT Provider Keys:**
   ```json
   {
     "stt_settings": {
       "provider": "deepgram",
       "byo_api_keys": {
         "deepgram": {
           "api_key": "your-deepgram-key"
         }
       }
     }
   }
   ```

   **Azure (requires region):**
   ```json
   {
     "tts_settings": {
       "provider": "azure",
       "byo_api_keys": {
         "azure": {
           "api_key": "your-azure-key",
           "region": "eastus"
         }
       }
     }
   }
   ```

4. **Priority Order**
   1. Assistant-level BYO keys
   2. Organization-level credentials
   3. System default keys (Burki managed)

5. **Security**
   - Keys encrypted at rest (AES-256)
   - Never exposed in API responses
   - Audit logging of key usage

6. **Cost Implications**
   - BYO: You pay provider directly
   - Managed: Burki bills with markup
   - Can mix per assistant

7. **Testing Keys**
   - How to verify keys work
   - Common errors and solutions

**Source Reference:** `Features.MD` Section 5.4, `app/services/credential_encryption_service.py`

---

## Files to Update

### 5. Update `advanced-features.mdx`

**Changes:**
- Add links to new dedicated pages
- Update sections to be summaries with "See [page] for details"
- Add new sections for:
  - IVR Explorer (link to dedicated page)
  - Learning & Memory (link to dedicated page)
  - Spam Detection (link to dedicated page)
  - BYO API Keys (link to dedicated page)

---

## Navigation Updates

Add to `docs.json` in Features or Advanced group:

```json
{
  "group": "Advanced",
  "pages": [
    "advanced-features",
    "ivr-explorer",
    "learning-memory",
    "spam-detection",
    "byo-api-keys"
  ]
}
```

---

## Implementation Steps

1. Create `ivr-explorer.mdx`
2. Create `learning-memory.mdx`
3. Create `spam-detection.mdx`
4. Create `byo-api-keys.mdx`
5. Update `advanced-features.mdx` with links
6. Update navigation in `docs.json`
7. Add cross-references from related pages
8. Test all examples

---

## Source Files to Reference

- `app/api/ivr_explorer.py` - IVR Explorer API
- `app/services/ivr_explorer/` - IVR Explorer services
- `app/api/learning.py` - Learning/Memory API
- `app/services/memory_service.py` - Memory system
- `app/services/spam_detection_service.py` - Spam detection
- `app/services/credential_encryption_service.py` - Key encryption
- `Features.MD` Sections 5.4, 12, 13, 18.8

---

## Estimated Effort

- IVR Explorer: 2 hours
- Learning & Memory: 3 hours (complex feature)
- Spam Detection: 1.5 hours
- BYO API Keys: 1 hour
- Update advanced-features.mdx: 0.5 hours
- Navigation + testing: 0.5 hours

**Total: ~8.5 hours**

---

## Dependencies

- None (can be implemented independently)

## Success Criteria

- Each feature has dedicated, comprehensive documentation
- Configuration examples are accurate
- API endpoints documented where applicable
- Security aspects clearly explained
- Navigation updated correctly
