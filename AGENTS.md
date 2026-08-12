# Documentation project instructions

## About this project

- This is the documentation site for **Feather**, a unified customer experience platform API.
- Built on [Mintlify](https://mintlify.com). Pages are MDX files with YAML frontmatter; configuration lives in `docs.json`.
- The backend that this documents is `agentic-platform-backend` (internal name "CX Platform API"). Its FastAPI app is the source of truth for the API reference — see the **API Reference** section below for how the spec is regenerated after a release.
- Use the Mintlify MCP server, `https://mcp.mintlify.com`, to edit content and settings via MCP.
- Use the Mintlify docs MCP server, `https://www.mintlify.com/docs/mcp`, to query information about using Mintlify via MCP.

## Terminology

- **Assistant** — the AI actor customers build. The API resource is `/v1/assistants`. An assistant bundles an agent (persona + prompt) and an optional workflow as one unit. Prefer "assistant"; do **not** call it an "agent" in prose (the backend renamed `/v1/agents` → `/v1/assistants`). The words `agent` still appear as literal API enum/field values (e.g. workflow node `type: "agent"`, policy point `agent_response`) — keep those verbatim in code/JSON.
- **Revision** — an immutable version of an assistant's configuration. You activate a revision (`.../revisions/{rev}/activate`); there is no "publish" verb.
- **Conversation** / **session** — a runtime interaction, created via `POST /v1/conversations` (not `/v1/v2/conversations`). Use "conversation".
- **Channel** — how a conversation reaches the assistant: chat/webchat, SMS, email, voice, or API.
- **Knowledge base** (KB) — an indexed document collection. API paths are doubled: `/v1/knowledge-base/knowledge-bases/...`.
- **End user** — the person the assistant talks to (`end_user_id`). Reserve "user"/"member" for people in the customer's own org (`/v1/identity/members`).
- **Policy**, **Team**, **Tool**, **Integration**, **Evaluator** — use as named by their API resources.

## Style preferences

- Use active voice and second person ("you")
- Keep sentences concise — one idea per sentence
- Use sentence case for headings
- Bold for UI elements: Click **Settings**
- Code formatting for file names, commands, paths, and code references
- Ground every endpoint, field name, and enum value in the current OpenAPI spec (`api-reference/openapi.source.json`) — do not invent request/response shapes
- Base URL in examples is the sandbox: `https://api-sandbox.featherhq.com`; auth header is `x-api-key: <key>`

## API Reference

- The API Reference tab is auto-generated from an OpenAPI spec — one interactive page per endpoint.
- `api-reference/openapi.source.json` is the full upstream spec (source of truth).
- `api-reference/openapi.json` is the **curated** spec Mintlify renders. Do not edit it by hand — it is generated.
- `scripts/build-docs-openapi.py` produces the curated spec from the source: it removes operational / vendor-webhook / internal endpoints (Twilio callbacks, SMS inbound/status, health probes, `/_internal/`, etc.) and applies rendering fixes.
- **To update the API reference after a backend release:** overwrite `openapi.source.json` with the new spec, then run `python3 scripts/build-docs-openapi.py`.
- To change which endpoints are hidden, edit the `HIDE_*` rules at the top of that script.

## Content boundaries

- Document only customer-facing, API-key-callable endpoints. Internal / staff / operational endpoints are stripped from the rendered spec by `scripts/build-docs-openapi.py` (via `HIDE_TAGS`, `HIDE_OPERATIONS`, and the `/_internal/` · `/internal/` · `/twilio/` path fragments). Do not write prose for anything hidden there — e.g. `voice-admin`, `voice-capacity-override`, `platform-config`, `benchmarking-internal`, vendor webhooks, Twilio callbacks, health/readiness probes, and the internal analytics endpoints.
- Public browser-side endpoints (the webchat public loader/session/turn dispatcher, mock public dispatcher) are not part of the API-key surface — describe them conceptually only, don't document them as callable API endpoints.
- Use the sandbox base URL in examples; never include real API keys, org IDs, or secrets.
