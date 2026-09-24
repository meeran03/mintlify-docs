# Burki Voice AI — public documentation

The integration guides, architecture overview, and API reference for **[Burki](https://burki.dev)**, the voice AI platform built by [Muhammad Meeran](https://meeran.dev).

**[Read the live documentation →](https://docs.burki.dev)**

Burki grew from a replacement for a voice provider into a platform for configuring and operating phone agents. This repository is the public documentation surface. The production backend and customer configurations are private.

## Start with the engineering

| Area | Read |
| --- | --- |
| System structure and conversation pipeline | [Architecture](architecture.mdx) |
| First assistant and call | [Quickstart](quickstart.mdx) |
| Carrier and SIP integration | [Telephony providers](telephony-providers.mdx) · [Bring your own SIP trunk](byo-sip-trunk.mdx) |
| Agent actions and integrations | [Tools](tools.mdx) · [Custom tools](tools/custom-tools-deep-dive.mdx) |
| Streaming operational visibility | [Live transcripts](live-transcript.mdx) |
| HTTP API contracts | [API introduction](api-reference/introduction.mdx) · [OpenAPI](api-reference/openapi.json) |
| Provider configuration | [LLMs](llm-providers.mdx) · [Speech recognition](stt-providers.mdx) · [Speech synthesis](tts-providers.mdx) |

## Preview locally

```bash
git clone https://github.com/meeran03/mintlify-docs.git
cd mintlify-docs
npm install -g mint
mint dev
```

The CLI prints the local preview URL. Navigation and site settings live in `docs.json`; documentation is written in MDX. Keep credentials, customer data, and private infrastructure details out of examples.

## Contributing

For a documentation correction, open an issue or pull request with the affected page and the expected behavior. For account or product support, use [info@burki.dev](mailto:info@burki.dev).

This repository intentionally retains its existing name so incoming links and documentation integrations keep working.
