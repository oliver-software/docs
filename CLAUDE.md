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

- **`docs.json`** — Central config: navigation structure, theme, logos, footer. All new pages must be added to the `navigation.tabs[0].groups` array here or they won't appear in the sidebar.
- **`api-reference/openapi.json`** — OpenAPI 3.1.0 spec for the Oliver Partner API. Endpoint MDX files reference operations from this spec via `openapi` front matter (e.g., `openapi: 'POST /v1/appointments'`).
- **`api-reference/<resource>/`** — MDX pages grouped by API resource (appointments, clients, patients, webhooks, etc.).

## Content Conventions

- MDX front matter links to OpenAPI operations: `openapi: 'METHOD /path'`
- Use Mintlify components for callouts: `<Note>`, `<Warning>`, `<Info>`, `<Tip>`
- Code examples use `<CodeGroup>` for multi-language blocks
- API base URL: `https://partner-api.getoliver.com`
- Auth: Bearer token + `X-Client-Id` header
