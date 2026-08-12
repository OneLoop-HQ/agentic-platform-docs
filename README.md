# Feather Docs

The documentation site for [Feather](https://featherhq.com) — a unified customer experience platform API. Built on [Mintlify](https://mintlify.com).

## Structure

- `docs.json` — site configuration and navigation.
- `introduction.mdx`, `authentication.mdx`, `quickstart.mdx` — Get Started pages.
- `concepts/*.mdx` — Core Concepts (assistants, knowledge bases, conversations, channels, memory, workflows, policies, privacy, analytics).
- `guides/*.mdx` — task-based guides and per-channel guides.
- `api-reference/` — the OpenAPI-driven API reference (see below).
- `AGENTS.md` — writing conventions and terminology for contributors and AI tools.

## API reference

The **API Reference** tab is generated from an OpenAPI spec — one interactive page per endpoint.

- `api-reference/openapi.source.json` — the full upstream spec exported from the backend (source of truth).
- `api-reference/openapi.json` — the **curated** spec Mintlify renders. Generated; do not edit by hand.
- `scripts/build-docs-openapi.py` — produces the curated spec from the source, stripping internal/operational endpoints.

To refresh after a backend release:

1. Export the spec from the backend (`scripts/export_openapi.py`) and copy it to `api-reference/openapi.source.json`.
2. Run `python3 scripts/build-docs-openapi.py`.
3. Review the diff and adjust the `HIDE_*` rules in the script if the backend added new internal tags.

## Local development

Install the [Mintlify CLI](https://www.npmjs.com/package/mint) and run it from the repo root (where `docs.json` lives):

```bash
npm i -g mint
mint dev
```

Preview at `http://localhost:3000`. Check for broken links with `mint broken-links`.

## Publishing

Changes deploy automatically when merged to the default branch via the Mintlify GitHub app.
