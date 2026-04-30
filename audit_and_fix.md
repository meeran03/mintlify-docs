# Burki Docs API Audit & Fix Log

Started: 2026-04-30  
Owner: Codex (with user direction)  
Scope: API accuracy audit + immediate corrections in docs

## Source of truth used for validation

- `burki-backend/app/api/assistants.py`
- `burki-backend/app/api/calls.py`
- `burki-backend/app/api/schemas.py`
- `burki-backend/app/core/auth.py`
- `burki-backend/app/api/main_routers/transcript.py`
- `burki-backend/app/services/webhook_service.py`
- `burki-backend/app/services/sms/sms_webhook_service.py`
- `burki-backend/app/services/llm_service.py`
- `burki-backend/app/services/tts/tts_factory.py`
- `burki-backend/app/services/stt/stt_factory.py`
- `burki-backend/app/services/tts/tts_openai.py`
- `burki-backend/app/services/tts/tts_inworld.py`

## Completed fixes

### 15) SIP trunking and SIP bridge docs added

- **Docs files added:**
  - `byo-sip-trunk.mdx`
  - `api-reference/sip-trunks/overview.mdx`
  - `api-reference/webhooks/sip-webhooks.mdx`
- **Docs files updated:**
  - `docs.json`
- **Status:** Added
- **Problems found:**
  - No first-class BYO SIP trunking guide existed.
  - No API reference covered `/api/v1/sip-trunks`.
  - No docs covered root SIP bridge endpoints (`/sip-webhook`, `/sip-status`, `/sip-prewarm`).
  - SIP bridge sync contract and phone-number `sip_trunk_id` assignment were undocumented.
- **Fix applied:**
  - Added BYO SIP trunking concept/operations guide.
  - Added consolidated SIP Trunks API page covering CRUD, enable/disable, sync, and phone-number `sip_trunk_id` assignment.
  - Added SIP webhooks/prewarm reference.
  - Added SIP pages to Documentation and API Reference navigation.
- **Backend references used:**
  - `app/api/sip_trunks.py`
  - `app/api/main_routers/sip.py`
  - `app/api/assistants.py` (`PATCH /organization/phone-numbers/{phone_number_id}`)
  - `app/services/sip_bridge_service.py`
  - `sip-bridge/bridge/sync_api.py`
  - `sip-bridge/bridge/outbound_api.py`

### 16) Advanced SMS API coverage added

- **Docs files added:**
  - `api-reference/messaging/sms-conversations.mdx`
  - `api-reference/messaging/sms-logs.mdx`
  - `api-reference/messaging/sms-compliance.mdx`
  - `api-reference/messaging/sms-10dlc.mdx`
- **Docs files updated:**
  - `docs.json`
  - `messaging.mdx`
- **Status:** Added
- **Problems found:**
  - SMS conversations, sessions, logs, debug timeline, compliance, and 10DLC carrier campaign APIs were implemented but not documented.
  - SMS compliance uses `/v1/sms-compliance` and JWT-only auth, unlike most SMS endpoints.
  - 10DLC helper APIs have provider-specific prefixes under `/api/telnyx`, `/api/twilio-10dlc`, and `/api/vonage-10dlc`.
- **Fix applied:**
  - Added advanced SMS conversation/session API page.
  - Added SMS logs and debug timeline page.
  - Added SMS compliance page with JWT-only auth callout.
  - Added SMS 10DLC campaign helper API page.
  - Added links in Messaging overview and API Reference navigation.
- **Backend references used:**
  - `app/api/sms_sessions.py`
  - `app/api/sms_conversations.py`
  - `app/api/sms_logs.py`
  - `app/api/sms_audit_logs.py`
  - `app/api/sms_compliance.py`
  - `app/api/telnyx_campaigns.py`
  - `app/services/sms/*`

### 17) Account and organization API coverage added

- **Docs files added:**
  - `api-reference/account/overview.mdx`
- **Docs files updated:**
  - `docs.json`
  - `byo-api-keys.mdx`
- **Status:** Added
- **Problems found:**
  - Users, invitations, organization settings, user API keys, provider credentials, fallback keys, LLM presets, and concurrency settings were implemented but not documented in API Reference.
  - Secret-bearing responses needed explicit warnings.
  - API key creation response exposes the plaintext `key` once and required documentation.
  - LLM preset apply can return unmasked stored API keys.
- **Fix applied:**
  - Added consolidated Account and Organization API page covering current user, org users/invitations, organization get/create/update, Twilio validation, user API keys, LLM presets, configuration status, fallback keys, and concurrency settings.
  - Added secret-handling warnings.
  - Linked BYO API Keys to account/org API and clarified assistant-level keys vs org fallback keys.
  - Added page to API Reference navigation.
- **Backend references used:**
  - `app/api/users.py`
  - `app/api/schemas.py`
  - `app/core/auth.py`

### 18) Learning and memory API coverage added

- **Docs files added:**
  - `api-reference/learning/overview.mdx`
- **Docs files updated:**
  - `learning-memory.mdx`
  - `docs.json`
- **Status:** Added
- **Problems found:**
  - Conceptual learning guide had only partial API tables.
  - Full `/api/learning` route surface was absent from API Reference.
  - Route prefix differs from `/api/v1`, and this needed to be explicit.
  - Call traces and learning data can contain conversation content and stable caller hashes, requiring a sensitivity warning.
- **Fix applied:**
  - Added consolidated Learning and Memory API page for memories, search, memory edges, prompt lifecycle, canary/gates, eval datasets/cases/import/publish/from-trace, eval run, stats, graph, DSPy, and call traces.
  - Updated the conceptual guide to link to the full API page and list missing endpoints.
  - Added Learning & Memory API navigation.
- **Backend references used:**
  - `app/api/learning.py`
  - `app/db/learning_models.py`

### 19) OpenAPI regenerated from backend

- **Docs files updated:**
  - `api-reference/openapi.json`
- **Status:** Regenerated
- **Problems found:**
  - OpenAPI referenced by Mintlify was stale and did not include newly audited surfaces like SIP trunking, advanced SMS, account/org, and learning routes.
- **Fix applied:**
  - Generated OpenAPI directly from `app.main:app` using the backend virtualenv.
  - New schema contains 336 paths.
- **Generation notes:**
  - Command used: `.venv/bin/python` importing `app.main` and writing `app.openapi()` to `burki-docs/api-reference/openapi.json`.
  - FastAPI emitted existing duplicate-operation-id warnings for some Vonage and costs routes.
  - Pydantic emitted a warning for the SIP `register` field shadowing `BaseModel.register`; schema generation still completed.

### 20) Call recording playback and metrics coverage added

- **Docs files added:**
  - `api-reference/calls/data/play-recording.mdx`
  - `api-reference/calls/data/metrics.mdx`
- **Docs files updated:**
  - `docs.json`
- **Status:** Added
- **Problems found:**
  - Authenticated recording playback endpoint was implemented but not documented.
  - Per-call metrics get/recalculate endpoints were implemented but not documented.
- **Fix applied:**
  - Added recording playback API page with binary response and processing/error status behavior.
  - Added call metrics API page covering `GET` and `PATCH` metrics endpoints and the `CallMetricsResponse` shape.
  - Added both pages to Call Data navigation.
- **Backend references used:**
  - `app/api/calls.py` (`play_call_recording`, `get_call_metrics`, `recalculate_call_metrics`)
  - `app/api/schemas.py` (`CallMetricsResponse`)

### 21) Final IA cleanup and validation

- **Docs files updated:**
  - `index.mdx`
  - `introduction.mdx`
  - `live-transcript.mdx`
  - `audit_and_fix.md`
- **Status:** Fixed and validated
- **Problems found:**
  - Home page and Platform Overview still overlapped.
  - Live Transcript guide duplicated protocol reference content from the WebSocket API page.
- **Fix applied:**
  - Reframed Home as a documentation map.
  - Reframed Platform Overview as the conceptual platform explanation.
  - Rewrote Live Transcript guide as an integration guide and linked the API Reference as canonical protocol detail.
  - Ran JSON parse validation and `npx mintlify broken-links`.
- **Validation result:**
  - `docs.json`: valid JSON
  - `api-reference/openapi.json`: valid JSON
  - `npx mintlify broken-links`: passed with no broken links found

### 22) SDK package verification

- **Docs files updated:**
  - `sdks/overview.mdx`
  - `sdks/go.mdx`
- **Status:** Partially verified
- **Problems found:**
  - SDK overview claimed Python, JavaScript/TypeScript, and Go SDKs were all available.
  - Public package checks verified Python and npm packages, but the Go module path could not be verified.
- **Fix applied:**
  - Marked Python package `burki` as publicly verified at version `0.1.2`.
  - Marked npm package `@burki.dev/sdk` as publicly verified at version `0.1.0`.
  - Marked Go module `github.com/burki-ai/burki-go` as unverified and added a warning to the Go SDK page.
- **Verification commands/results:**
  - `pip index versions burki`: found `burki (0.1.2)`.
  - `npm view @burki.dev/sdk name version --json`: found `@burki.dev/sdk` version `0.1.0`.
  - `go list -m -versions github.com/burki-ai/burki-go`: could not run because `go` is not installed locally.
  - Web/package search did not verify the Go module as public.

### 23) LLM/STT/TTS provider documentation alignment

- **Docs files updated:**
  - `llm-providers.mdx`
  - `stt-providers.mdx`
  - `tts-providers.mdx`
  - `tts-providers/openai.mdx`
  - `tts-providers/inworld.mdx`
  - `tts-providers/voice-tuning.mdx`
  - `tts-providers/troubleshooting.mdx`
  - `tts-providers/best-practices.mdx`
  - `ai-configuration.mdx`
  - `configuration.mdx`
  - `quickstart.mdx`
  - `getting-started.mdx`
  - `features.mdx`
  - `architecture.mdx`
  - `introduction.mdx`
  - `byo-api-keys.mdx`
- **Status:** Fixed
- **Problems found:**
  - LLM docs omitted `azure` and understated `custom_websocket`.
  - LLM model examples included stale marketing names like Grok 2 instead of backend/API model IDs.
  - STT docs covered only Deepgram, ElevenLabs, and Azure, omitting Uplift, Speechmatics, Telnyx, Soniox, and Deepgram Flux.
  - STT enum/model mapping mentions OpenAI Whisper and Assembly, but they are not registered in the active factory and should not be documented as supported realtime providers.
  - TTS docs incorrectly marked OpenAI TTS as “Coming Soon” even though `OpenAITTSService` is registered.
  - TTS overview omitted backend-supported Kokoro, Uplift, Murf, and Soniox.
  - Inworld docs used display labels (`TTS-1`, `TTS-1-Max`) instead of exact backend model IDs and omitted 1.5 models.
  - Shared setup pages still listed old shortened provider sets.
- **Fix applied:**
  - Added Azure OpenAI and custom WebSocket details to LLM provider docs.
  - Rewrote OpenAI TTS as a live supported provider page with supported models, voices, speed, and instructions.
  - Expanded STT provider docs to include Deepgram Flux, Uplift, Speechmatics, Telnyx, and Soniox, with warning not to configure unsupported OpenAI/Assembly STT.
  - Expanded TTS provider overview to include OpenAI, Kokoro, Uplift, Murf, and Soniox, with warning not to document AWS Polly/Google TTS until wired.
  - Updated Inworld model IDs to `inworld-tts-1`, `inworld-tts-1-max`, `inworld-tts-1.5-max`, and `inworld-tts-1.5-mini`.
  - Updated common provider lists across configuration, quickstart, getting started, features, architecture, and BYO API key docs.
- **Backend references used:**
  - `app/services/llm_service.py` (`LLMService.PROVIDERS`)
  - `app/services/stt/stt_factory.py` (`STTFactory._providers`, Flux routing, model map)
  - `app/services/tts/tts_factory.py` (`TTSFactory._providers`, provider setting branches)
  - `app/services/tts/tts_openai.py` (OpenAI models/voices/options)
  - `app/services/tts/tts_inworld.py` (Inworld model IDs/language support)
- **Validation result:**
  - `docs.json`: valid JSON
  - `api-reference/openapi.json`: valid JSON
  - `npx mintlify broken-links`: passed with no broken links found
  - Cursor lints on edited docs: no errors

### 5 Calls API list/filter/detail correction

- **Docs files updated:**
  - `api-reference/calls/list.mdx`
  - `api-reference/calls/get-by-id.mdx`
  - `api-reference/calls/get-by-sid.mdx`
  - `api-reference/calls/search.mdx`
  - `api-reference/calls/get-count.mdx`
  - `api-reference/calls/export.mdx`
- **Status:** Fixed
- **Problems found:**
  - `search.mdx` documented a non-existent `GET /api/v1/calls/search` endpoint.
  - List/export docs omitted backend filters `call_sid` and `assistant_name`.
  - Detail examples used stale fields (`metadata`, `cost`, `created_at`, `updated_at`) instead of `call_meta` and cost breakdown fields.
  - `get-by-sid.mdx` described `call_sid` as Twilio-only despite multi-provider support.
  - `get-count.mdx` claimed the same filters as List Calls, but backend count supports only `status`, `assistant_id`, `date_from`, and `date_to`.
- **Fix applied:**
  - Reframed `search.mdx` as a filter guide for `GET /api/v1/calls`.
  - Added `call_sid` and `assistant_name` to List Calls and Export Calls.
  - Documented AND semantics for call filters.
  - Updated get-by-ID/SID examples to `CallResponse` fields (`call_meta`, `total_cost`, `llm_cost`, `tts_cost`, `stt_cost`, `telephony_cost`, `cost_currency`, `assistant_name`, `flow_name`).
  - Reworded `call_sid` as Burki call SID / provider call identifier.
  - Corrected count filter scope.
- **Backend references used:**
  - `app/api/calls.py` (`get_calls`, `get_calls_count`, detail handlers, export filters)
  - `app/api/schemas.py` (`CallResponse`, `PaginatedCallsResponse`)

### 6) Calls data/export/terminate correction

- **Docs files updated:**
  - `api-reference/calls/data/get-transcripts-by-id.mdx`
  - `api-reference/calls/data/get-transcripts-by-sid.mdx`
  - `api-reference/calls/data/get-recordings-by-id.mdx`
  - `api-reference/calls/data/get-recordings-by-sid.mdx`
  - `api-reference/calls/data/export-transcripts.mdx`
  - `api-reference/calls/terminate.mdx`
- **Status:** Fixed
- **Problems found:**
  - Transcript examples used stale fields (`text`, `timestamp`) instead of `content`, `created_at`, segment timing, confidence, and provider timing fields.
  - SID transcript/recording docs described `call_sid` as Twilio-only.
  - Recording examples used `url` instead of `recording_url` and omitted `file_path`, `format`, `recording_type`, `recording_source`, and `status`.
  - Transcript export examples had wrong JSON shape and CSV column names.
  - Terminate call docs did not mention verified-user requirement and used “BYO SIP Trunk” wording where backend provider id is SIP bridge.
- **Fix applied:**
  - Updated transcript examples to `TranscriptResponse` fields.
  - Updated recording examples to `RecordingResponse` fields.
  - Corrected transcript export examples for `txt`, `json`, and `csv`.
  - Reworded SID references as Burki call SID / provider call identifier.
  - Added verified account note for terminate call and changed provider label to SIP bridge.
- **Backend references used:**
  - `app/api/schemas.py` (`TranscriptResponse`, `RecordingResponse`)
  - `app/api/calls.py` (transcript/recording/export/terminate handlers)

### 7) Messaging/SMS API correction

- **Docs files updated:**
  - `api-reference/messaging/send-sms.mdx`
  - `messaging.mdx`
  - `api-reference/webhooks/sms-webhooks.mdx`
- **Status:** Fixed
- **Problems found:**
  - Queued SMS response example used a provider SID-like `SM...` ID, but queued sends return a Burki queue UUID.
  - Immediate-send docs said success could return `status: "failed"` even though backend raises an error response on send failure.
  - Send SMS docs omitted `403` for cross-organization `from_phone_number`.
  - Messaging overview only mentioned Twilio/Telnyx and described a Telnyx-first provider priority that does not match provider selection.
  - Interactive workflow example used the wrong payload shape for customer `sms_webhook_url`.
  - SMS webhook docs had typo host `api.burki.devv`, omitted Vonage, and documented legacy/provider webhook URLs as primary.
- **Fix applied:**
  - Updated queued response example to a UUID.
  - Clarified immediate failures return error responses.
  - Added `403 Forbidden` example and related endpoints (`GET /sms/{message_id}/status`, `DELETE /sms/{message_id}`, `GET /sms/queue/stats`).
  - Updated provider support to Twilio, Telnyx, and Vonage.
  - Rewrote provider selection around the sender phone number provider rather than priority/fallback.
  - Updated customer webhook example to the normalized `sms_received` payload.
  - Corrected webhook hostname and canonical provider webhook paths.
- **Backend references used:**
  - `app/api/main_routers/outbound/sms.py` (`/sms/send`, status/cancel/stats companions, error behavior)
  - `app/api/schemas.py` (`SendSMSRequest`, `SendSMSResponse`, `SMSStatusResponse`)
  - `app/api/sms_webhook.py` (unified `/webhooks/sms/{provider}` and legacy aliases)
  - `app/services/sms/sms_queue_service.py` (queued UUID message ID)

### 8) Messaging/SMS missing coverage logged

- **Status:** Partially fixed
- **Docs files added:**
  - `api-reference/messaging/get-sms-status.mdx`
  - `api-reference/messaging/cancel-sms.mdx`
  - `api-reference/messaging/queue-stats.mdx`
- **Docs files updated:**
  - `docs.json`
- **Fix applied:**
  - Added API reference pages for SMS status, cancel, and queue stats.
  - Expanded the Messaging API Reference navigation.
- **Backend references used:**
  - `app/api/main_routers/outbound/sms.py`
  - `app/api/schemas.py` (`SMSStatusResponse`, `SMSCancelResponse`, `SMSQueueStatsResponse`)
- **Missing docs found:**
  - SMS sessions API pages (`/api/v1/sms-sessions/...`).
  - SMS conversations API pages (`/api/v1/sms-conversations/...`).
  - SMS logs API pages (`/api/v1/sms-logs/...`).
  - SMS audit/debug timeline page (`/api/v1/sms/{call_id}/logs`).
  - SMS compliance API page (`/v1/sms-compliance/...`), including its JWT-auth caveat.
  - 10DLC management API coverage for Telnyx, Twilio, and Vonage carrier campaign endpoints.
- **Next action:** Add these as dedicated API Reference pages and expand the Messaging group in `docs.json`.

### 9) Billing/pricing narrative correction

- **Docs files updated:**
  - `pricing.mdx`
  - `faq.mdx`
  - `usage.mdx`
  - `byo-api-keys.mdx`
- **Status:** Fixed
- **Problems found:**
  - Pricing/FAQ described separate platform-minute and carrier-subsidy trial buckets, but backend uses a unified trial-minute bucket.
  - Pricing described a fixed Burki Cloud minimum balance that is not consistently enforced by live call/SMS gating paths.
  - Voice duration FAQ did not mention that any non-zero connected call is billed as at least 1 minute.
  - Usage page documented the wrong SMS path/body shape (`/api/v1/messaging/sms/send`, `to/from/body`) instead of `/sms/send` with `from_phone_number`, `to_phone_number`, and `message`.
  - BYO docs omitted organization-level managed-service preferences exposed through billing API.
- **Fix applied:**
  - Rewrote trial copy around one 200 connected-minute bucket.
  - Reworded managed costs as absorbed while trial minutes remain.
  - Removed the specific Burki Cloud `$2.00` minimum balance table and documented the call-start wallet requirement generically.
  - Clarified 1-minute minimum billing for non-zero connected calls.
  - Corrected SMS example in `usage.mdx` and added `/v1/billing/wallet`.
  - Added managed-services API note to BYO docs.
- **Backend references used:**
  - `app/db/billing_models.py` (`BillingAccount.trial_minutes_remaining`)
  - `app/services/billing/wallet_service.py` (wallet and minimum balance constants)
  - `app/services/billing/usage_ingestion_service.py` (billable minute rounding)
  - `app/api/main_routers/outbound/sms.py` (`POST /sms/send`)
  - `app/api/billing.py` (`/v1/billing/...`)

### 10) Billing API reference added

- **Docs files updated:**
  - `api-reference/billing/overview.mdx`
  - `docs.json`
- **Status:** Added
- **Problems found:**
  - Backend exposes `/v1/billing` REST endpoints, but docs had no Billing API Reference group.
  - Billing base path is `/v1/billing`, not `/api/v1/billing`.
- **Fix applied:**
  - Added a Billing API page covering wallet balance, top-up, auto top-up, ledger, usage summary, charges summary, statements, managed services, and admin credit grants.
  - Added the Billing API page to the API Reference navigation.
- **Backend references used:**
  - `app/api/billing.py` (all route paths, request/response models, auth caveats)

### 11) Organization phone-number API correction

- **Docs files updated:**
  - `api-reference/organization/sync-phonenumbers.mdx`
  - `api-reference/organization/list-phonenumbers.mdx`
  - `api-reference/organization/list-available-phonenumbers.mdx`
- **Status:** Fixed
- **Problems found:**
  - Sync docs described Twilio-only sync, but backend syncs Twilio, Telnyx, and Vonage when configured.
  - List docs omitted `include_inactive`.
  - Phone-number response examples omitted `provider`, `is_verified_caller_id`, `sip_trunk_id`, `allowed_country_codes`, and `flow`.
  - Example text implied `twilio_sid` is Twilio-only; backend keeps this legacy field as provider phone ID compatibility.
- **Fix applied:**
  - Updated sync copy and example message for multi-provider sync.
  - Added `include_inactive`.
  - Expanded organization phone number response examples to match `OrganizationPhoneNumberResponse`.
- **Backend references used:**
  - `app/api/assistants.py` (`sync_organization_phone_numbers`, `list_organization_phone_numbers`, `list_available_phone_numbers`)
  - `app/api/schemas.py` (`SyncPhoneNumbersResponse`, `OrganizationPhoneNumberResponse`)

### 12) Organization/account missing coverage logged

- **Status:** Logged, not yet created
- **Missing docs found:**
  - User profile endpoints (`/api/v1/users/me`, password change).
  - Organization user listing/invite/accept-invitation flow.
  - Organization get/create/update settings API.
  - Organization API key/provider credential management, including fallback keys.
  - LLM preset CRUD/apply flows.
  - SIP organization settings and sync behavior.
- **Next action:** Add an Organization & Account API section with safe examples that avoid exposing secret values.

### 13) Information architecture and repo hygiene cleanup

- **Docs files updated:**
  - `README.md`
  - `docs.json`
  - `snippets/snippet-intro.mdx` (removed)
- **Status:** Fixed
- **Problems found:**
  - `README.md` local dev instructions pointed to a non-existent `mintlify-docs` directory.
  - Getting Started navigation put `pricing` before implementation/setup tasks.
  - Generic Mintlify snippet boilerplate existed in the published docs tree and did not belong in Burki product docs.
- **Fix applied:**
  - Updated local dev command to run from `/Users/meeran/Startup/burki-docs`.
  - Reordered Getting Started to be task-first: home, quickstart, setup, configuration, usage, platform overview.
  - Moved `pricing` into Help & Resources.
  - Deleted the boilerplate snippet.
- **Remaining IA notes:**
  - `index.mdx` and `introduction.mdx` still overlap and should eventually be merged or sharply differentiated.
  - `live-transcript.mdx` and `api-reference/websockets/live-transcript.mdx` still overlap; the guide should become shorter and link to the API reference as canonical protocol detail.

### 14) Quickstart trial wording correction

- **Docs file updated:** `quickstart.mdx`
- **Status:** Fixed
- **Problem found:** Quickstart still said “200 free platform minutes,” which conflicted with the unified connected-minute trial wording now used in Pricing and FAQ.
- **Fix applied:** Replaced with “200 connected trial minutes.”
- **Backend references used:**
  - `app/db/billing_models.py` (`BillingAccount.trial_minutes_remaining`)

### 1) API introduction accuracy pass

- **Docs file updated:** `api-reference/introduction.mdx`
- **Status:** Fixed
- **Problems found:**
  - Authentication section implied API key only.
  - Response format implied a single universal `{ data, message }` shape.
  - Pagination documented `page` / `page_size` instead of `skip` / `limit`.
  - Webhook events listed dotted event names not matching webhook payload message types.
  - Rate-limit section claimed universal tier/header behavior without endpoint-level context.
- **Fix applied:**
  - Rewrote auth section to reflect bearer token usage with API key or JWT.
  - Replaced response format with practical patterns (list and paginated object).
  - Replaced pagination docs with `skip` / `limit` and `items/total/skip/limit` shape.
  - Updated webhook section to typed payload model (`status-update`, `end-of-call-report`, `sms_received`).
  - Reframed rate limits as endpoint-specific and removed incorrect universal header claims.
- **Backend references used:**
  - `app/core/auth.py` (`get_current_user_flexible`)
  - `app/api/assistants.py` (`skip`, `limit`)
  - `app/api/calls.py` (`skip`, `limit`)
  - `app/api/schemas.py` (`PaginatedCallsResponse`)
  - `app/services/webhook_service.py` (`status-update`, `end-of-call-report`)
  - `app/services/sms/sms_webhook_service.py` (`sms_received`)

### 2) List assistants endpoint type/parameter correction

- **Docs file updated:** `api-reference/assistants/list.mdx`
- **Status:** Fixed
- **Problems found:**
  - Response example used string IDs for `id`, `user_id`, `organization_id`.
  - Optional query flags were missing from docs (`active_only`, `my_assistants_only`, `include_stats`).
- **Fix applied:**
  - Converted ID fields in response example to integer values.
  - Added missing query parameters and their behavior.
  - Added stats fields in response example to match `include_stats` behavior.
- **Backend references used:**
  - `app/api/assistants.py` (`get_assistants` query params)
  - `app/api/schemas.py` (`AssistantResponse` integer IDs)

### 3) Live transcript WebSocket reference correction

- **Docs file updated:** `api-reference/websockets/live-transcript.mdx`
- **Status:** Fixed
- **Problems found:**
  - Endpoint showed `ws://` instead of production `wss://`.
  - `call_sid` described as Twilio-specific.
  - Authorization-header example implied browser support.
  - Hard-coded WebSocket rate limits listed without backend confirmation.
- **Fix applied:**
  - Updated endpoint examples to `wss://api.burki.dev/live-transcript/...`.
  - Updated `call_sid` description to carrier-agnostic Burki call SID.
  - Clarified header auth is for non-browser clients only.
  - Removed unverified hard-coded rate-limit claims.
- **Backend references used:**
  - `app/api/main_routers/transcript.py` (endpoint path and auth methods)

### 4) Live transcript guide consistency update

- **Docs file updated:** `live-transcript.mdx`
- **Status:** Fixed
- **Problems found:**
  - Guide mixed `ws://burki.dev` and `wss://api.burki.dev`.
  - `call_sid` described as Twilio-specific.
  - Header auth note lacked browser/client distinction.
  - SDK helper snippets used `ws://` defaults.
- **Fix applied:**
  - Standardized to `wss://api.burki.dev` in endpoint guidance and helper snippets.
  - Updated call SID wording to Burki call SID.
  - Clarified Authorization header usage for server-side clients.

### 25) Final remaining docs correctness and brand alignment pass

- **Docs files updated:**
  - `api-reference/tools/discover-lambda.mdx`
  - `lambda-discovery.mdx`
  - `api-reference/tools/create.mdx`
  - `voice-cloning.mdx`
  - `api-reference/assistants/voice-samples/upload.mdx`
  - `api-reference/assistants/voice-samples/list.mdx`
  - `api-reference/assistants/voice-cloning/providers.mdx`
  - `api-reference/assistants/cloned-voices/create.mdx`
  - `api-reference/assistants/cloned-voices/list.mdx`
  - `api-reference/assistants/cloned-voices/delete.mdx`
  - `api-reference/calls/data/update-metadata.mdx`
  - `features.mdx`
  - `faq.mdx`
  - `messaging.mdx`
  - `byo-api-keys.mdx`
  - `advanced-features.mdx`
  - `stt-providers.mdx`
  - `stt-providers/elevenlabs.mdx`
  - `stt-providers/azure.mdx`
  - `tts-providers.mdx`
  - `tts-providers/deepgram.mdx`
  - `tts-providers/elevenlabs.mdx`
  - `tts-providers/cartesia.mdx`
  - `tts-providers/azure.mdx`
  - `tts-providers/troubleshooting.mdx`
  - `tts-providers/voice-tuning.mdx`
  - `ai-configuration.mdx`
  - `changelog.mdx`
  - `sdks/overview.mdx`
  - `api-reference/account/overview.mdx`
  - `phone-number-management.mdx`
  - `api-reference/phone-numbers/search.mdx`
  - `api-reference/phone-numbers/purchase.mdx`
  - `api-reference/phone-numbers/release.mdx`
  - `api-reference/phone-numbers/countries.mdx`
  - `api-reference/phone-numbers/get-webhooks.mdx`
  - `docs.json`
  - `images/hero-light.svg`
  - `images/hero-dark.svg`
- **Status:** Fixed
- **Problems found:**
  - Old `api.burkivoice.ai` examples remained in Lambda discovery docs.
  - Old BurkiVoice naming remained in an AWS IAM user example and a tool user-agent sample.
  - Voice-cloning examples and related API pages mixed live `/api/v1` paths with unprefixed `/assistants/...` examples.
  - Voice-cloning conceptual content still referenced "Future Providers", `Coming Soon`, and `TBD` table cells.
  - Phone-number API reference pages and guide examples used unprefixed `/phone-numbers/...` paths even though OpenAPI/backend paths are `/api/v1/phone-numbers/...`.
  - `release.mdx` pointed users to the old `organization/phonenumbers` route instead of the current organization phone-number inventory route.
  - Several pages had unqualified claims around fixed latency, uptime/SLA, HIPAA compliance, global coverage counts, and "enterprise-grade" security/reliability.
  - Docs theme still used Mintlify emerald/blue defaults and old blue hero SVGs.
  - Docs footer exposed a personal X handle instead of only company-branded social links.
- **Fix applied:**
  - Standardized stale hostnames and branding to `api.burki.dev` and Burki naming.
  - Corrected voice-cloning paths to `/api/v1/assistants/...`.
  - Updated voice-cloning provider docs to reflect currently wired voice-cloning providers: ElevenLabs, Resemble, and Cartesia.
  - Corrected all phone-number API frontmatter and example URLs to `/api/v1/phone-numbers/...`.
  - Replaced fixed/unverified marketing claims with configuration-dependent or provider-attributed wording.
  - Updated `docs.json` to Burki mint `#66D38F`, switched to a sharper Mintlify theme, and removed personal X social from docs footer.
  - Rebuilt `hero-light.svg` and `hero-dark.svg` as black/mint/gold neo-brutalist Burki docs visuals.
- **Validation:**
  - `npx mintlify broken-links` passed.
  - `npx mintlify validate` passed.

## Open audit items / residual risk

### API correctness

- [x] Regenerated `api-reference/openapi.json` from the backend app for route parity.
- [x] Added manual coverage for the major missing API surfaces found in this audit.
- [x] Checked SDK package names/install commands against available public registries.
- [x] Final sweep completed for stale hostnames, missing `/api/v1` route prefixes, unfinished `Coming Soon`/`TBD` copy, risky hard claims, and old visual/theme assets.

### Coverage gaps

- [x] SIP trunks and SIP bridge endpoints.
- [x] Billing / wallet endpoints.
- [x] SMS status/cancel/queue stats.
- [x] SMS sessions, conversations, logs, debug timeline, compliance, and 10DLC flows.
- [x] Learning/memory APIs.
- [x] Call recording playback and metrics endpoints.
- [x] Org/team/API key/provider credential/fallback key management endpoints.
- [x] LLM/STT/TTS provider lists and major provider options aligned with backend factories/services.

### Information architecture / quality

- [x] Reordered docs navigation for implementation-first journey.
- [x] Added missing API groups to API Reference navigation.
- [x] Reduced overlap between `index.mdx` and `introduction.mdx`.
- [x] Consolidated overlap between `live-transcript.mdx` and `api-reference/websockets/live-transcript.mdx`.

## Change history

- 2026-04-30: Initial API audit started.
- 2026-04-30: First wave of API corrections applied (intro, assistants list, live transcript reference + guide).
- 2026-04-30: Calls, SMS, billing, organization phone-number, and IA cleanup batches applied.
- 2026-04-30: Validation run: `npx mintlify broken-links` passed with no broken links found.
- 2026-04-30: Added SMS status/cancel/queue stats API pages; validation rerun passed with no broken links found.
- 2026-04-30: Added SIP, advanced SMS, account/org, learning, call metrics/playback docs; regenerated OpenAPI; validation rerun passed with no broken links found.
- 2026-04-30: Trimmed Home vs Platform Overview and Live Transcript Guide vs API Reference overlap.
- 2026-04-30: Aligned LLM/STT/TTS provider docs with backend provider factories and service options.
- 2026-04-30: Completed final correctness/theme sweep; fixed phone-number paths, voice-cloning placeholders, stale branding/hosts, unqualified hard claims, docs theme colors, and hero visuals. Validation passed (`npx mintlify broken-links`, `npx mintlify validate`).
