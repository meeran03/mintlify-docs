# Implementation Plan: Navigation Restructuring

## Overview

Update `docs.json` to accommodate all new documentation pages and restructure navigation for better organization.

## Current Navigation Structure

```json
{
  "navigation": {
    "tabs": [
      {
        "tab": "Documentation",
        "groups": [
          { "group": "Getting Started", "pages": [...] },
          { "group": "Core Concepts", "pages": [...] },
          { "group": "AI Providers", "pages": [...] },
          { "group": "Features", "pages": [...] },
          { "group": "Advanced", "pages": [...] },
          { "group": "Help & Resources", "pages": [...] }
        ]
      },
      {
        "tab": "API Reference",
        "groups": [...]
      }
    ]
  }
}
```

## Proposed Navigation Structure

### Option A: Add SDKs as New Tab

```json
{
  "navigation": {
    "tabs": [
      {
        "tab": "Documentation",
        "groups": [...]
      },
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
      },
      {
        "tab": "API Reference",
        "groups": [...]
      }
    ]
  }
}
```

### Option B: Add SDKs as Group in Documentation Tab

```json
{
  "tab": "Documentation",
  "groups": [
    { "group": "Getting Started", ... },
    {
      "group": "SDKs",
      "pages": [
        "sdks/overview",
        "sdks/python",
        "sdks/javascript",
        "sdks/go",
        "sdks/realtime"
      ]
    },
    ...
  ]
}
```

**Recommendation:** Option A (separate tab) - SDKs are important enough to warrant their own tab.

---

## Complete Updated docs.json

```json
{
  "$schema": "https://mintlify.com/docs.json",
  "theme": "mint",
  "name": "Burki Voice AI Docs",
  "colors": {
    "primary": "#10B981",
    "light": "#A3FFAE",
    "dark": "#0D1117"
  },
  "favicon": "/favicon.svg",
  "navigation": {
    "tabs": [
      {
        "tab": "Documentation",
        "groups": [
          {
            "group": "Getting Started",
            "pages": [
              "introduction",
              "quickstart",
              "pricing",
              "getting-started",
              "configuration",
              "usage"
            ]
          },
          {
            "group": "Core Concepts",
            "pages": [
              "architecture",
              "ai-configuration"
            ]
          },
          {
            "group": "AI Providers",
            "pages": [
              "llm-providers",
              {
                "group": "TTS Providers",
                "pages": [
                  "tts-providers",
                  "tts-providers/elevenlabs",
                  "tts-providers/deepgram",
                  "tts-providers/cartesia",
                  "tts-providers/azure",
                  "tts-providers/kokoro",
                  "tts-providers/inworld",
                  "tts-providers/resemble",
                  "tts-providers/openai",
                  "tts-providers/voice-tuning",
                  "tts-providers/troubleshooting",
                  "tts-providers/best-practices"
                ],
                "icon": "microphone"
              },
              {
                "group": "STT Providers",
                "pages": [
                  "stt-providers",
                  "stt-providers/azure"
                ],
                "icon": "ear-listen"
              }
            ]
          },
          {
            "group": "Features",
            "pages": [
              "features",
              "telephony-providers",
              "messaging",
              "phone-number-management",
              "rag",
              "call-management",
              "tools",
              "campaigns",
              "assistant-graphs",
              "lambda-discovery",
              "live-transcript",
              "voice-cloning"
            ]
          },
          {
            "group": "Advanced",
            "pages": [
              "advanced-features",
              "ivr-explorer",
              "learning-memory",
              "spam-detection",
              "byo-api-keys",
              "integrations"
            ]
          },
          {
            "group": "Help & Resources",
            "pages": [
              "faq",
              "changelog"
            ]
          }
        ]
      },
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
      },
      {
        "tab": "API Reference",
        "groups": [
          {
            "group": "API Documentation",
            "pages": [
              "api-reference/introduction"
            ]
          },
          {
            "group": "Assistants",
            "pages": [
              "api-reference/assistants/create",
              "api-reference/assistants/list",
              "api-reference/assistants/get-by-id",
              "api-reference/assistants/update",
              "api-reference/assistants/delete",
              "api-reference/assistants/get-count",
              "api-reference/assistants/list-providers",
              "api-reference/assistants/get-organization",
              "api-reference/assistants/get-by-phone"
            ]
          },
          {
            "group": "Assistant Phone Numbers",
            "pages": [
              "api-reference/assistants/phonenumbers/list",
              "api-reference/assistants/phonenumbers/list-available",
              "api-reference/assistants/phonenumbers/assign-by-number",
              "api-reference/assistants/phonenumbers/bulk-assign",
              "api-reference/assistants/phonenumbers/unassign-by-number",
              "api-reference/assistants/phonenumbers/unassign-by-id"
            ]
          },
          {
            "group": "Assistant Documents (RAG)",
            "pages": [
              "api-reference/assistants/documents/upload",
              "api-reference/assistants/documents/list",
              "api-reference/assistants/documents/get-status",
              "api-reference/assistants/documents/delete"
            ]
          },
          {
            "group": "Voice Cloning",
            "pages": [
              "api-reference/assistants/voice-samples/upload",
              "api-reference/assistants/voice-samples/list",
              "api-reference/assistants/cloned-voices/create",
              "api-reference/assistants/cloned-voices/list",
              "api-reference/assistants/cloned-voices/delete",
              "api-reference/assistants/voice-cloning/providers"
            ]
          },
          {
            "group": "Calls",
            "pages": [
              "api-reference/calls/list",
              "api-reference/calls/get-by-id",
              "api-reference/calls/get-by-sid",
              "api-reference/calls/initiate",
              "api-reference/calls/terminate",
              "api-reference/calls/get-messages",
              "api-reference/calls/get-webhook-logs",
              "api-reference/calls/update-metadata",
              "api-reference/calls/get-count",
              "api-reference/calls/search",
              "api-reference/calls/get-analytics",
              "api-reference/calls/get-stats",
              "api-reference/calls/export"
            ]
          },
          {
            "group": "Call Data",
            "pages": [
              "api-reference/calls/data/get-transcripts-by-id",
              "api-reference/calls/data/get-transcripts-by-sid",
              "api-reference/calls/data/export-transcripts",
              "api-reference/calls/data/get-recordings-by-id",
              "api-reference/calls/data/get-recordings-by-sid"
            ]
          },
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
          },
          {
            "group": "Assistant Graphs",
            "pages": [
              "api-reference/assistant-graphs/create",
              "api-reference/assistant-graphs/list",
              "api-reference/assistant-graphs/get",
              "api-reference/assistant-graphs/update",
              "api-reference/assistant-graphs/delete",
              "api-reference/assistant-graphs/nodes",
              "api-reference/assistant-graphs/edges",
              "api-reference/assistant-graphs/sessions",
              "api-reference/assistant-graphs/assign-phone"
            ]
          },
          {
            "group": "Messaging",
            "pages": [
              "api-reference/messaging/send-sms"
            ]
          },
          {
            "group": "Phone Number Management",
            "pages": [
              "api-reference/phone-numbers/search",
              "api-reference/phone-numbers/purchase",
              "api-reference/phone-numbers/release",
              "api-reference/phone-numbers/countries",
              "api-reference/phone-numbers/update-webhooks",
              "api-reference/phone-numbers/get-webhooks"
            ]
          },
          {
            "group": "Organization",
            "pages": [
              "api-reference/organization/sync-phonenumbers",
              "api-reference/organization/list-phonenumbers",
              "api-reference/organization/list-available-phonenumbers"
            ]
          },
          {
            "group": "Tools & Custom Actions",
            "pages": [
              "api-reference/tools/create",
              "api-reference/tools/list",
              "api-reference/tools/get-by-id",
              "api-reference/tools/assign",
              "api-reference/tools/discover-lambda"
            ]
          },
          {
            "group": "WebSockets",
            "pages": [
              "api-reference/websockets/live-transcript",
              "api-reference/websockets/campaign-progress"
            ]
          },
          {
            "group": "Webhooks & System",
            "pages": [
              "api-reference/webhooks/twilio-twiml",
              "api-reference/webhooks/sms-webhooks",
              "api-reference/webhooks/recording-status",
              "api-reference/webhooks/initiate-call"
            ]
          }
        ]
      }
    ],
    "global": {
      "anchors": [
        {
          "anchor": "Support",
          "href": "mailto:info@burki.dev",
          "icon": "envelope"
        },
        {
          "anchor": "Community",
          "href": "https://discord.gg/burki",
          "icon": "discord"
        }
      ]
    }
  },
  "logo": {
    "light": "/logo/light.svg",
    "dark": "/logo/dark.svg"
  },
  "navbar": {
    "links": [
      {
        "label": "Support",
        "href": "mailto:info@burki.dev"
      }
    ],
    "primary": {
      "type": "button",
      "label": "Dashboard",
      "href": "https://burki.dev/dashboard"
    }
  },
  "footer": {
    "socials": {
      "x": "https://x.com/burki_ai",
      "linkedin": "https://linkedin.com/company/burki-ai"
    }
  }
}
```

---

## Changes Summary

### Documentation Tab Changes:

| Group | Changes |
|-------|---------|
| AI Providers > TTS | Add: cartesia, azure, kokoro |
| AI Providers | Add: STT Providers sub-group with azure |
| Features | Add: campaigns, assistant-graphs |
| Advanced | Add: ivr-explorer, learning-memory, spam-detection, byo-api-keys |

### New Tab:

| Tab | Groups |
|-----|--------|
| SDKs | SDK Documentation (overview, python, javascript, go, realtime) |

### API Reference Tab Changes:

| Group | Changes |
|-------|---------|
| Calls | Add: initiate, terminate, get-messages, get-webhook-logs, update-metadata |
| NEW: Campaigns | All campaign endpoints |
| NEW: Assistant Graphs | All graph endpoints |
| WebSockets | Add: campaign-progress |

---

## Implementation Steps

1. **Backup current docs.json**
   ```bash
   cp mintlify-docs/docs.json mintlify-docs/docs.json.backup
   ```

2. **Create required directories:**
   ```bash
   mkdir -p mintlify-docs/sdks
   mkdir -p mintlify-docs/stt-providers
   mkdir -p mintlify-docs/api-reference/campaigns
   mkdir -p mintlify-docs/api-reference/assistant-graphs
   ```

3. **Update docs.json incrementally:**
   - First add SDKs tab (after SDK docs created)
   - Add new TTS providers to navigation
   - Add STT providers sub-group
   - Add new feature pages
   - Add new advanced feature pages
   - Add new API reference groups

4. **Verify all pages exist before adding to navigation**
   - Each page path in docs.json must have corresponding .mdx file

5. **Test navigation:**
   - Run Mintlify dev server
   - Click through all navigation items
   - Verify no 404 errors

6. **Final validation:**
   - All links work
   - Logical grouping
   - No orphaned pages

---

## Dependencies

This plan should be executed **LAST** after all other documentation plans are complete, because:
- Pages must exist before being added to navigation
- Navigation references pages that will be created by other plans

---

## Execution Order

1. Wait for all page creation plans to complete
2. Verify all expected pages exist
3. Update docs.json incrementally
4. Test after each major change
5. Final full navigation test

---

## Validation Checklist

Before finalizing navigation:

- [ ] All SDK pages created: overview, python, javascript, go, realtime
- [ ] All TTS provider pages created: cartesia, azure, kokoro
- [ ] STT azure page created
- [ ] campaigns.mdx created
- [ ] assistant-graphs.mdx created
- [ ] ivr-explorer.mdx created
- [ ] learning-memory.mdx created
- [ ] spam-detection.mdx created
- [ ] byo-api-keys.mdx created
- [ ] All campaign API reference pages created
- [ ] All assistant graph API reference pages created
- [ ] All new call API reference pages created
- [ ] campaign-progress WebSocket page created

---

## Estimated Effort

- Update docs.json: 1 hour
- Testing and fixing: 1 hour
- Cross-reference verification: 0.5 hours

**Total: ~2.5 hours**

---

## Success Criteria

- All navigation items link to existing pages
- No 404 errors when clicking through navigation
- Logical organization of content
- SDKs easily discoverable
- New features visible in navigation
- API reference complete with new endpoints
