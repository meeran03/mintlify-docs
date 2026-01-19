# Mintlify Documentation Update Plans

This directory contains implementation plans for updating the Burki Voice AI Mintlify documentation.

## Overview

The current documentation is significantly outdated. These plans identify 9 separate documentation work packages that can be implemented independently.

## Plans Summary

| # | Plan | File | Estimated Hours | Priority | Status |
|---|------|------|-----------------|----------|--------|
| 1 | SDK Documentation | [01-sdk-documentation.md](./01-sdk-documentation.md) | 9 hours | HIGH | |
| 2 | Campaign Management | [02-campaign-management.md](./02-campaign-management.md) | 9 hours | HIGH | |
| 3 | API Reference Updates | [03-api-reference-updates.md](./03-api-reference-updates.md) | 5 hours | HIGH | |
| 4 | New TTS Providers | [04-tts-providers.md](./04-tts-providers.md) | 6.5 hours | MEDIUM | |
| 5 | Vonage Telephony | [05-vonage-telephony.md](./05-vonage-telephony.md) | 2.5 hours | MEDIUM | ✅ DONE |
| 6 | Assistant Graphs | [06-assistant-graphs.md](./06-assistant-graphs.md) | 7.5 hours | MEDIUM | |
| 7 | Azure STT | [07-azure-stt.md](./07-azure-stt.md) | 3 hours | LOW | |
| 8 | Advanced Features | [08-advanced-features.md](./08-advanced-features.md) | 8.5 hours | LOW | |
| 9 | Navigation Restructure | [09-navigation-restructure.md](./09-navigation-restructure.md) | 2.5 hours | LAST | |

**Total Estimated Effort: ~53 hours**

## Recommended Implementation Order

1. **SDK Documentation** - Highest user impact, enables programmatic access
2. **Campaign Management** - Major missing feature with full API
3. **API Reference Updates** - Outdated docs cause integration issues
4. **New TTS Providers** - Cartesia is frequently used
5. **Vonage Telephony** - Completes telephony coverage
6. **Assistant Graphs** - Complex feature needs documentation
7. **Azure STT** - Expands STT options
8. **Advanced Features** - Lower priority features
9. **Navigation Restructure** - Final cleanup after content is added

## How to Use These Plans

1. Select a plan to implement
2. Read the plan file for detailed instructions
3. Follow the implementation steps
4. Mark sections as complete as you go
5. Run Mintlify dev server to verify changes
6. Move to next plan

## Key Source Files

These files are referenced across multiple plans:

- `Features.MD` - Comprehensive feature list
- `app/api/schemas.py` - All API request/response schemas
- `burki-sdks/` - SDK implementations and READMEs
- `app/services/` - Service implementations
- `docs/` - Internal documentation

## Notes

- Plans can be implemented independently (except Navigation which is last)
- Each plan includes source file references for accurate documentation
- Estimated hours are rough approximations
- Test all changes with Mintlify dev server before committing
