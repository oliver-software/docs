# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Mintlify-based API documentation site for the Oliver Partner API (veterinary software). Content is written in MDX files with YAML front matter.

## Commands

```bash
# Install (global CLI)
npm i -g mintlify

# Dev server (http://localhost:3000)
mintlify dev

# Validate links
mintlify broken-links

# If dev fails, reinstall dependencies
mintlify install
```

Node version: v23.9.0 (see `.nvmrc`). Deployment is automatic on push to the default branch via the Mintlify GitHub App.

## Architecture

- **`docs.json`** — Central config: navigation structure, theme, logos, footer. Every page must be listed in the `navigation.tabs[0].groups` array here or it won't appear in the sidebar. See [Navigation](#navigation).
- **`api-reference/openapi.json`** — OpenAPI 3.1.0 spec for the Oliver Partner API. Endpoint MDX files reference operations from this spec via `openapi` front matter (e.g., `openapi: 'POST /v1/appointments'`).
- **`api-reference/<resource>/`** — MDX pages grouped by API resource (appointments, clients, patients, webhooks, etc.).

## Navigation

An MDX file that isn't listed in `docs.json` is invisible on the site — Mintlify renders nothing and reports no error, so the page looks "missing" with no clue why. Existing files are not exempt: several `api-reference/ai_outbound/` pages sat unlisted for weeks because only `create` had been registered.

**Before finishing any docs change, verify every page you touched (and its siblings) is in the nav:**

```bash
# Lists MDX pages that exist on disk but are absent from docs.json
comm -23 \
  <(find api-reference -name '*.mdx' | sed 's|\.mdx$||' | sort) \
  <(python3 -c "import json;d=json.load(open('docs.json'));[print(p) for g in d['navigation']['tabs'][0]['groups'] for p in g['pages']]" | sort)
```

Notes:
- Group a page by what it *is*, not by its directory — `api-reference/ai_outbound/outcome_webhook.mdx` lives under the **Webhooks** group next to `service_reminder`, not under AI Outbound Endpoints.
- Order within a group is the sidebar order. Conceptual pages (`overview`) go first, then endpoints.
- `mintlify dev` hot-reloads `docs.json`; no restart needed.
- Edit `docs.json` with a text edit or a `json.dump(..., indent=2, ensure_ascii=True)` round-trip — anything else reformats the whole file and buries the real change.

## Content Conventions

- MDX front matter links to OpenAPI operations: `openapi: 'METHOD /path'`
- Use Mintlify components for callouts: `<Note>`, `<Warning>`, `<Info>`, `<Tip>`
- Code examples use `<CodeGroup>` for multi-language blocks
- API base URL: `https://partner-api.getoliver.com`
- Auth: Bearer token + `X-Client-Id` header
