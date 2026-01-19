# Implementation Plan: API Reference Updates

## Overview

Update outdated API reference documentation and add missing endpoints to ensure docs match the current implementation.

## Files to Update

### 1. Update `api-reference/webhooks/initiate-call.mdx`

**Current State:** Missing `from_phone_number`, `assistant_id` is shown as required

**Updated Schema:**

```json
{
  "to_phone_number": "+15559876543",        // Required
  "from_phone_number": "+15551234567",       // Optional - source phone number
  "assistant_id": 123,                       // Optional - override default assistant
  "welcome_message": "Hello {{name}}",       // Optional - supports templates
  "agenda": "Discuss {{topic}}",             // Optional - supports templates
  "variables": {                             // Optional - template variables
    "name": "John Doe",
    "topic": "appointment"
  }
}
```

**Changes:**
- Add `from_phone_number` parameter documentation
- Change `assistant_id` from required to optional
- Add explanation of assistant resolution logic:
  1. If `assistant_id` provided, use that assistant
  2. If `from_phone_number` provided, use assistant assigned to that number
  3. Error if neither can resolve to an assistant
- Add `variables` parameter documentation
- Update examples

**Source Reference:** `app/api/schemas.py` lines 885-898

---

### 2. Create `api-reference/calls/initiate.mdx`

**Purpose:** Move initiate call from webhooks to calls section (it's an API endpoint, not a webhook)

**Endpoint:** `POST /api/v1/calls/initiate`

**Content:**
- Description: Initiate an outbound call from an AI assistant
- Request body schema (as above)
- Response schema:
  ```json
  {
    "message": "Call initiated successfully",
    "call_sid": "CAxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
  }
  ```
- Example requests with different parameter combinations
- Error responses (assistant not found, no phone number, etc.)

---

### 3. Create `api-reference/calls/terminate.mdx`

**Endpoint:** `POST /api/v1/calls/{call_sid}/terminate`

**Content:**
- Description: Terminate an ongoing call
- Path parameters: `call_sid`
- Response schema
- Use cases (admin intervention, balance depletion, etc.)
- Example request/response

**Source Reference:** `app/api/calls.py`

---

### 4. Update `api-reference/calls/list.mdx`

**Review and update:**
- Verify all query parameters match implementation
- Check response schema includes:
  - `assistant_graph_id` and `assistant_graph_name` (if applicable)
  - All cost fields
  - All metadata fields
- Update examples

**Source Reference:** `app/api/calls.py` lines 32-100

---

### 5. Update `api-reference/calls/get-by-id.mdx`

**Review and update:**
- Verify response schema is complete
- Include all nested objects (transcripts, recordings, metrics)
- Check optional fields

---

### 6. Update `api-reference/assistants/create.mdx`

**Review and update for new fields:**
- `byo_api_keys` section (per-assistant API keys)
- `spam_detection_config`
- `disclosure_settings`
- `structured_data_schema`
- Updated `tts_settings` with new providers (cartesia, azure, kokoro)
- Updated `stt_settings` with Azure option

---

### 7. Update `api-reference/assistants/update.mdx`

**Same updates as create.mdx**

---

### 8. Update `api-reference/messaging/send-sms.mdx`

**Review and update:**
- Verify `from_phone_number` parameter
- Check `queue` parameter for rate-limited sending
- Verify response schema

---

## Files to Create

### 9. `api-reference/calls/get-messages.mdx`

**Endpoint:** `GET /api/v1/calls/{call_id}/messages`

**Content:**
- Description: Get LLM conversation messages for a call
- Path parameters
- Response schema (list of ChatMessage objects)
- Example

---

### 10. `api-reference/calls/get-webhook-logs.mdx`

**Endpoint:** `GET /api/v1/calls/{call_id}/webhook-logs`

**Content:**
- Description: Get webhook delivery logs for a call
- Path parameters
- Response schema
- Example

---

### 11. `api-reference/calls/update-metadata.mdx`

**Endpoint:** `PATCH /api/v1/calls/{call_id}/metadata`

**Content:**
- Description: Update call metadata
- Path parameters
- Request body (arbitrary JSON object)
- Response schema
- Example

---

## Schema Verification Checklist

Review `app/api/schemas.py` and verify these schemas are accurately documented:

- [ ] `InitiateCallRequest` - verify all fields documented
- [ ] `InitiateCallResponse` - verify response documented
- [ ] `CallResponse` - verify all fields in list/get responses
- [ ] `AssistantCreateRequest` - verify new fields
- [ ] `AssistantUpdateRequest` - verify new fields
- [ ] `SendSMSRequest` - verify all fields
- [ ] `TranscriptResponse` - verify structure
- [ ] `RecordingResponse` - verify structure
- [ ] `CallMetricsResponse` - verify all metrics

---

## Navigation Updates

Update `docs.json`:

1. Move `api-reference/webhooks/initiate-call` to `api-reference/calls/initiate` (or keep both with redirect)

2. Add new call endpoints to Calls group:
```json
{
  "group": "Calls",
  "pages": [
    "api-reference/calls/list",
    "api-reference/calls/get-by-id",
    "api-reference/calls/get-by-sid",
    "api-reference/calls/initiate",      // NEW
    "api-reference/calls/terminate",     // NEW
    "api-reference/calls/get-messages",  // NEW
    "api-reference/calls/get-webhook-logs", // NEW
    "api-reference/calls/update-metadata",  // NEW
    "api-reference/calls/get-count",
    "api-reference/calls/search",
    "api-reference/calls/get-analytics",
    "api-reference/calls/get-stats",
    "api-reference/calls/export"
  ]
}
```

---

## Implementation Steps

1. Review `app/api/schemas.py` for current request/response schemas
2. Review `app/api/calls.py` for all call endpoints
3. Update `initiate-call.mdx` with new parameters
4. Create `calls/initiate.mdx` (proper location)
5. Create `calls/terminate.mdx`
6. Create other missing call endpoint docs
7. Update assistant create/update docs for new fields
8. Update navigation
9. Verify all examples work

---

## Source Files to Reference

- `app/api/calls.py` - Call endpoints
- `app/api/schemas.py` - All request/response schemas
- `app/api/assistants.py` - Assistant endpoints
- `app/api/root.py` - Webhook handlers

---

## Estimated Effort

- Update initiate-call.mdx: 0.5 hours
- Create new call endpoints (4 files): 2 hours
- Update assistant endpoints: 1 hour
- Schema verification: 1 hour
- Navigation + testing: 0.5 hours

**Total: ~5 hours**

---

## Dependencies

- Should be done early as other docs reference API endpoints

## Success Criteria

- All documented schemas match `app/api/schemas.py`
- All call endpoints are documented
- Initiate call shows new parameters correctly
- Examples are accurate and copy-pasteable
- No broken links in navigation
