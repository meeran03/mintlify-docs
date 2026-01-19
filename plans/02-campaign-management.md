# Implementation Plan: Campaign Management Documentation

## Overview

Create comprehensive documentation for the Campaign Management feature, including conceptual guide and full API reference.

## Files to Create

### 1. `campaigns.mdx`

**Purpose:** Main Campaign Management guide

**Sections:**

1. **Introduction**
   - What are campaigns
   - Use cases (appointment reminders, sales outreach, surveys, etc.)

2. **Campaign Types**
   - Call Campaigns (outbound voice)
   - SMS Campaigns (bulk messaging)
   - Mixed Campaigns (voice + SMS)

3. **Contact Management**
   - CSV/JSON import
   - Column mapping
   - Duplicate detection
   - Custom fields
   - Do-Not-Contact lists

4. **Template System (Jinja2)**
   - Variable syntax: `{{name}}`, `{{company}}`
   - Template types: welcome_message, agenda, end_call_message, sms_message
   - Built-in filters: `phone_format`, `title_case`, `upper`, `lower`
   - Fallback values
   - System variables: `assistant_name`, `company`, `name`
   - Preview and testing

5. **Scheduling**
   - Schedule types: Immediate, Once, Recurring
   - Timezone support
   - Time windows (calling hours)
   - Day selection

6. **Execution Settings**
   - Concurrent call limits
   - Rate limiting (calls per minute)
   - Retry logic (max attempts, delay)
   - Voicemail detection (AMD)

7. **Monitoring & Analytics**
   - Real-time progress (WebSocket)
   - Metrics: completion rate, success rate, answer rate, avg duration, cost
   - Execution history per contact

8. **Best Practices**
   - Compliance (TCPA, GDPR)
   - Optimal scheduling
   - Template design

**Source Reference:** `Features.MD` Section 11, `app/services/campaign_executor.py`

---

### 2. `api-reference/campaigns/list.mdx`

**Endpoint:** `GET /api/v1/campaigns`

**Content:**
- Description
- Query parameters (status, campaign_type, assistant_id, search, page, per_page)
- Response schema
- Example request/response

---

### 3. `api-reference/campaigns/create.mdx`

**Endpoint:** `POST /api/v1/campaigns`

**Content:**
- Description
- Request body schema:
  - name, description
  - campaign_type (call, sms, mixed)
  - assistant_id
  - settings (concurrent_limits, rate_limiting, retry_logic)
  - schedule configuration
- Response schema
- Example request/response

---

### 4. `api-reference/campaigns/get.mdx`

**Endpoint:** `GET /api/v1/campaigns/{campaign_id}`

**Content:**
- Description
- Path parameters
- Response schema (full campaign with contacts count, metrics)
- Example request/response

---

### 5. `api-reference/campaigns/update.mdx`

**Endpoint:** `PUT /api/v1/campaigns/{campaign_id}`

**Content:**
- Description
- Path parameters
- Request body (partial update)
- Response schema
- Example request/response

---

### 6. `api-reference/campaigns/delete.mdx`

**Endpoint:** `DELETE /api/v1/campaigns/{campaign_id}`

**Content:**
- Description
- Path parameters
- Response (success message)
- Example request/response

---

### 7. `api-reference/campaigns/start.mdx`

**Endpoint:** `POST /api/v1/campaigns/{campaign_id}/start`

**Content:**
- Description
- Path parameters
- Response (updated campaign status)
- Prerequisites (contacts loaded, assistant configured)
- Example request/response

---

### 8. `api-reference/campaigns/pause.mdx`

**Endpoint:** `POST /api/v1/campaigns/{campaign_id}/pause`

**Content:**
- Description
- Path parameters
- Response
- Behavior (in-progress calls complete, new calls stop)
- Example request/response

---

### 9. `api-reference/campaigns/resume.mdx`

**Endpoint:** `POST /api/v1/campaigns/{campaign_id}/resume`

**Content:**
- Description
- Path parameters
- Response
- Example request/response

---

### 10. `api-reference/campaigns/cancel.mdx`

**Endpoint:** `POST /api/v1/campaigns/{campaign_id}/cancel`

**Content:**
- Description
- Path parameters
- Response
- Behavior (terminates campaign, marks remaining contacts as cancelled)
- Example request/response

---

### 11. `api-reference/campaigns/contacts.mdx`

**Endpoint:** Multiple contact endpoints

**Content:**
- `GET /api/v1/campaigns/{campaign_id}/contacts` - List contacts
- `POST /api/v1/campaigns/{campaign_id}/contacts` - Add contacts
- `POST /api/v1/campaigns/{campaign_id}/contacts/import` - Bulk import (CSV/JSON)
- `DELETE /api/v1/campaigns/{campaign_id}/contacts/{contact_id}` - Remove contact
- Query parameters (status filter)
- Contact schema (phone_number, name, variables, status)
- Example requests/responses

---

### 12. `api-reference/campaigns/progress.mdx`

**Endpoint:** `GET /api/v1/campaigns/{campaign_id}/progress`

**Content:**
- Description
- Path parameters
- Response schema:
  - total_contacts
  - completed_contacts
  - failed_contacts
  - pending_contacts
  - in_progress_contacts
  - success_rate
  - answer_rate
  - average_duration
  - total_cost
- Example request/response

---

### 13. `api-reference/campaigns/template-preview.mdx`

**Endpoint:** `POST /api/v1/campaigns/{campaign_id}/preview-template`

**Content:**
- Description
- Request body (template_type, variables)
- Response (rendered template)
- Example request/response

---

### 14. `api-reference/websockets/campaign-progress.mdx`

**Endpoint:** `WebSocket /ws/campaigns/{campaign_id}/progress`

**Content:**
- Connection setup
- Authentication
- Event types:
  - `progress` - Overall progress update
  - `contact_started` - Contact call initiated
  - `contact_completed` - Contact call finished
  - `contact_failed` - Contact call failed
  - `campaign_completed` - All contacts processed
- Event schemas
- Example client implementations (Python, JS)

---

## Navigation Updates

Add to `docs.json` in Features group:

```json
"campaigns"
```

Add new API Reference group:

```json
{
  "group": "Campaigns",
  "pages": [
    "api-reference/campaigns/list",
    "api-reference/campaigns/create",
    "api-reference/campaigns/get",
    "api-reference/campaigns/update",
    "api-reference/campaigns/delete",
    "api-reference/campaigns/start",
    "api-reference/campaigns/pause",
    "api-reference/campaigns/resume",
    "api-reference/campaigns/cancel",
    "api-reference/campaigns/contacts",
    "api-reference/campaigns/progress",
    "api-reference/campaigns/template-preview"
  ]
}
```

Add to WebSockets group:

```json
"api-reference/websockets/campaign-progress"
```

---

## Implementation Steps

1. Create `mintlify-docs/api-reference/campaigns/` directory
2. Create main `campaigns.mdx` guide
3. Create all API reference pages
4. Create WebSocket documentation
5. Update `docs.json` navigation
6. Add cross-references to SDK campaign sections
7. Test all examples

---

## Source Files to Reference

- `app/api/campaigns.py` - All endpoint implementations
- `app/api/schemas.py` - Request/response schemas
- `app/services/campaign_executor.py` - Execution logic
- `app/services/campaign_data_import.py` - Import logic
- `app/services/template_engine.py` - Jinja2 template system
- `app/api/campaign_websocket.py` - WebSocket implementation
- `Features.MD` Section 11

---

## Estimated Effort

- Main guide (campaigns.mdx): 3 hours
- API reference pages (13 pages): 4 hours
- WebSocket documentation: 1 hour
- Navigation + testing: 1 hour

**Total: ~9 hours**

---

## Dependencies

- None (can be implemented independently)
- Consider implementing after SDK docs (can reference SDK examples)

## Success Criteria

- Complete guide covers all campaign features
- All API endpoints documented with accurate schemas
- WebSocket events documented with examples
- Navigation correctly shows campaigns
- Code examples work correctly
