# API Reference Documentation Todos (Fully Accurate to Codebase)

## Current Status
This file now lists **all** API endpoints that are actually implemented in your FastAPI backend, based on a comprehensive code scan. Use this as the definitive source of truth for documentation.

---

## 🎯 Assistants API (`/api/v1/assistants`)

- `POST /` — Create a new assistant.
- `GET /` — List all assistants for the organization.
- `GET /count` — Get the total count of assistants.
- `GET /providers` — List supported LLM, TTS, and STT providers.
- `GET /me/organization` — Get organization info for the current user.
- `GET /{assistant_id}` — Get a specific assistant by ID.
- `PUT /{assistant_id}` — Update an assistant's configuration.
- `DELETE /{assistant_id}` — Delete an assistant.
- `GET /by-phone/{phone_number}` — Get an assistant by their assigned phone number.

### Assistant - Phone Number Management
- `GET /{assistant_id}/phone-numbers` — List phone numbers assigned to an assistant.
- `GET /{assistant_id}/available-phone-numbers` — List organization phone numbers available for assignment.
- `POST /{assistant_id}/phone-numbers/assign-by-number` — Assign a phone number using the number string (recommended).
- `POST /{assistant_id}/phone-numbers/bulk-assign` — Assign multiple phone numbers by their IDs.
- `POST /{assistant_id}/phone-numbers/unassign-by-number` — Unassign a phone number using the number string.
- `POST /{assistant_id}/phone-numbers/{phone_number_id}/unassign` — Unassign a phone number by its ID.

### Assistant - Documents (RAG)
- `POST /{assistant_id}/documents/upload` — Upload a document to an assistant's knowledge base.
- `GET /{assistant_id}/documents` — List all documents for an assistant.
- `GET /{assistant_id}/documents/{document_id}/status` — Check the processing status of a document.
- `DELETE /{assistant_id}/documents/{document_id}` — Delete a document.

---

## 🎯 Calls API (`/api/v1/calls`)

- `GET /` — List calls with comprehensive filtering and pagination.
- `GET /count` — Get the count of calls with filtering.
- `GET /export` — Export a list of calls to CSV.
- `GET /analytics` — Get call analytics (e.g., total calls, duration).
- `GET /stats` — Get high-level call statistics.
- `GET /search` — Search for calls.
- `GET /{call_id}` — Get a specific call by its ID.
- `GET /sid/{call_sid}` — Get a specific call by its Twilio SID.

### Call Data
- `GET /{call_id}/transcripts` — Get all transcripts for a call.
- `GET /sid/{call_sid}/transcripts` — Get all transcripts for a call by Twilio SID.
- `GET /{call_id}/transcripts/export` — Export transcripts for a call to CSV.
- `GET /{call_id}/recordings` — Get all recordings for a call.
- `GET /sid/{call_sid}/recordings` — Get all recordings for a call by Twilio SID.

---

## 🎯 Organization API

- `POST /api/v1/assistants/organization/phone-numbers/sync` — Sync phone numbers from the Twilio account.
- `GET /api/v1/assistants/organization/phone-numbers` — List all phone numbers in the organization.
- `GET /api/v1/assistants/organization/phone-numbers/available` — List unassigned phone numbers in the organization.

---

## 🎯 Webhooks & System Endpoints

- `POST /twiml` — Main webhook for handling incoming Twilio calls.
- `POST /recording-status` — Webhook for receiving recording status updates from Twilio.
- `POST /calls/initiate` — (Internal/System) Endpoint to initiate outbound calls.

---

## 🎯 Web Interface Endpoints (Not for Public API Docs)

These are routes that render HTML pages for the web dashboard. They should generally **not** be included in the public API reference documentation.

### Web - General
- `GET /`
- `GET /dashboard`
- `GET /profile`
- `GET /organization`
- `GET /docs`
- `GET /api-reference`

### Web - Assistants
- `GET /assistants`
- `GET /assistants/new`
- `POST /assistants/new`
- `GET /assistants/{assistant_id}`
- `GET /assistants/{assistant_id}/edit`
- `POST /assistants/{assistant_id}/edit`
- `GET /assistants/{assistant_id}/delete`
- `GET /assistants/export`
- `POST /assistants/bulk-action`
- `POST /assistants/fetch-phone-numbers`

### Web - Calls
- `GET /calls`
- `GET /calls/{call_id}`
- `GET /calls/{call_id}/recording/{recording_id}`
- `GET /calls/{call_id}/recording/{recording_id}/play`
- `GET /calls/{call_id}/transcripts/export`
- `GET /calls/export`
- `POST /calls/bulk-action`

### Web - Organization & Phone Numbers
- `GET /organization/phone-numbers`
- `POST /organization/phone-numbers/sync`
- `POST /organization/phone-numbers/{phone_number_id}/assign`
- `POST /organization/phone-numbers/{phone_number_id}/delete`
- `GET /organization/settings`
- `POST /organization/settings/twilio`

### Web - Auth
- `GET /auth/login`
- `POST /auth/login`
- `POST /auth/google`
- `GET /auth/register`
- `POST /auth/register`
- `POST /auth/google-register`
- `GET /auth/logout`
- `GET /auth/api-keys`
- `POST /auth/api-keys/create`
- `POST /auth/api-keys/{key_id}/delete`

---

## Notes
- Only endpoints that are actually present in the codebase are listed above.
- If you add new endpoints in the backend, update this file to keep your docs plan in sync.
- For each endpoint, document request parameters, authentication, and response format in your Mintlify docs.

---

## Next Steps
- Use this complete list to build out your public API reference in Mintlify, focusing on the sections marked for the public API.
- Ensure the navigation in `docs.json` reflects these categories accurately. 