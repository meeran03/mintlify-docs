# Implementation Plan: SDK Documentation

## Overview

Create comprehensive documentation for the Burki SDKs (Python, JavaScript/TypeScript, Go) in Mintlify.

## Files to Create

### 1. `sdks/overview.mdx`

**Purpose:** SDK overview and comparison page

**Content:**
- Introduction to Burki SDKs
- Quick comparison table (Python, JS/TS, Go)
- Installation commands for each
- Feature support matrix
- Links to individual SDK pages

**Source Reference:** `burki-sdks/README.md`

---

### 2. `sdks/python.mdx`

**Purpose:** Complete Python SDK documentation

**Sections:**
1. Installation (`pip install burki`)
2. Quick Start
3. Authentication and Configuration
4. Resources:
   - Assistants (list, get, create, update, delete)
   - Calls (list, get, transcripts, recordings, metrics, initiate, terminate)
   - Phone Numbers (search, purchase, assign, release)
   - Documents/RAG (upload, list, status, delete)
   - Tools (list, create, assign, discover Lambda)
   - SMS (send, conversations)
   - Campaigns (create, start, pause, progress)
5. Real-time Streaming (WebSocket)
   - Live transcripts
   - Campaign progress
6. Error Handling
7. Async Usage

**Source Reference:** `burki-sdks/python/README.md`

---

### 3. `sdks/javascript.mdx`

**Purpose:** Complete JavaScript/TypeScript SDK documentation

**Sections:**
1. Installation (`npm install @burki.dev/sdk`)
2. Quick Start
3. Authentication and Configuration
4. Resources:
   - Assistants (full CRUD + status, export, cloned voices, providers)
   - Calls (initiate, list, transcripts, recordings, metrics, messages, terminate)
   - Phone Numbers (search, purchase, assign, release, webhooks, caller IDs)
   - Documents/RAG (upload, uploadFromUrl, list, status, reprocess, delete)
   - Tools (full CRUD + assign/unassign + Lambda discovery)
   - SMS (send, status, cancel, queue stats, conversations)
   - Campaigns (full CRUD + start/pause/resume/cancel + progress + contacts)
5. Real-time Streaming (WebSocket)
   - Live transcripts with event types
   - Campaign progress with event types
6. Error Handling (typed errors)
7. TypeScript Support (type imports)

**Source Reference:** `burki-sdks/js/README.md`

---

### 4. `sdks/go.mdx`

**Purpose:** Complete Go SDK documentation

**Sections:**
1. Installation (`go get github.com/burki-ai/burki-go`)
2. Quick Start
3. Authentication and Configuration
4. Resources:
   - Assistants (full CRUD with params structs)
   - Calls (initiate, list, transcripts, recordings, metrics, terminate)
   - Phone Numbers (search, purchase, assign, release)
   - Documents/RAG (upload file, upload from URL)
   - Tools (list, create, assign, Lambda discovery)
   - SMS (send, conversations)
   - Campaigns (create, start, progress)
5. Real-time Streaming (WebSocket)
   - Channel-based event handling
   - Ping/keepalive
6. Error Handling (type assertions)

**Source Reference:** `burki-sdks/go/README.md`

---

### 5. `sdks/realtime.mdx`

**Purpose:** Dedicated real-time/WebSocket documentation

**Sections:**
1. Overview of Real-time Features
2. Live Transcript Streaming
   - Connection setup
   - Event types (transcript, call_status, error)
   - Example implementations (Python, JS, Go)
3. Campaign Progress Streaming
   - Connection setup
   - Event types (progress, contact_completed, campaign_completed)
   - Example implementations
4. Connection Management
   - Reconnection strategies
   - Keepalive/ping
   - Error handling
5. Best Practices

**Source Reference:** SDK README files + `app/api/campaign_websocket.py`

---

## Navigation Updates

Add to `docs.json`:

```json
{
  "tab": "SDKs",
  "groups": [
    {
      "group": "SDK Documentation",
      "pages": [
        "sdks/overview",
        "sdks/python",
        "sdks/javascript",
        "sdks/go",
        "sdks/realtime"
      ]
    }
  ]
}
```

Or add as a group within Documentation tab:

```json
{
  "group": "SDKs",
  "pages": [
    "sdks/overview",
    "sdks/python",
    "sdks/javascript",
    "sdks/go",
    "sdks/realtime"
  ]
}
```

---

## Implementation Steps

1. Create `mintlify-docs/sdks/` directory
2. Create `sdks/overview.mdx` with comparison table and quick start
3. Create `sdks/python.mdx` adapting content from Python README
4. Create `sdks/javascript.mdx` adapting content from JS README
5. Create `sdks/go.mdx` adapting content from Go README
6. Create `sdks/realtime.mdx` consolidating WebSocket docs
7. Update `docs.json` to add SDKs navigation
8. Add cross-references from API Reference pages to SDK examples
9. Test all code examples for accuracy

---

## Estimated Effort

- Overview page: 1 hour
- Python SDK page: 2 hours
- JavaScript SDK page: 2 hours
- Go SDK page: 2 hours
- Real-time page: 1.5 hours
- Navigation + cross-refs: 0.5 hours

**Total: ~9 hours**

---

## Dependencies

- None (can be implemented independently)

## Success Criteria

- All SDK pages render correctly in Mintlify
- Code examples are accurate and copy-pasteable
- Navigation shows SDKs section
- Cross-references work between SDK docs and API Reference
