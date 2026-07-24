# CLAUDE.md — rihla-contracts

Instructions for AI assistants (Claude Code) working in this repo. This is the
**canonical** AI rules file for the Rihla platform — `rihla-api`,
`rihla-vendor-web`, `rihla-admin-web`, and `rihla-mobile` each keep a short
`CLAUDE.md` that links back here and adds only repo-specific instructions.

## What this repo is

The single source of truth for the Rihla backend API contract: OpenAPI spec
(`openapi.yaml`) plus the CI that validates it and generates typed clients.
No business logic, no runtime code, no database access lives here.

## Contract-first rule

No client or API implementation work starts on an endpoint until it is
defined and merged here (or explicitly approved for mocking). This repo is
upstream of `rihla-api` and all three client repos.

## Commands

- Package manager: `npm`
- Install: `npm ci`
- Lint/validate the spec: `npm run validate` (Redocly)
- Generate the typed client locally: `npm run generate-client` → outputs to
  `generated/client` (gitignored — never commit generated output)

## Architecture boundaries

- `openapi.yaml` is the only hand-edited contract source.
- `generated/` is build output. **Never hand-edit generated code** —
  regenerate from `openapi.yaml` via `npm run generate-client` or the CI
  artifact.
- Downstream repos (`rihla-api`, web portals, mobile) consume the generated
  client artifact; they must not hand-write or copy API client types.

## Current contract version

`openapi.yaml` → `info.version: 0.1.0` (Demo 1 scope: `POST /v1/listings`,
`GET /v1/listings`). Bump this on every contract change; treat it as the
version downstream repos pin against.

## Required checks before opening a PR

1. `npm run validate` passes (no lint errors).
2. `npm run generate-client` completes without error.
3. Changes are additive where possible (new optional fields/endpoints).
   Breaking changes (removed/renamed fields, changed types, removed
   endpoints) require an ADR and explicit approval before merging.

## Git flow

- `main` is protected — never push directly.
- `develop` is the staging integration branch.
- Work happens on a feature branch off `develop` → PR into `develop`.
- Abdulwahab is a required reviewer on every PR.
- Merge to `develop` publishes a prerelease contract version; merge to
  `main` publishes a stable release.

## Security

- Never print secrets or credentials in examples, commits, or output.
- Never use production data — examples in the spec use obviously fake
  values (see `openapi.yaml` examples).
- Redact PII in any example payloads, logs, or test fixtures.

## Working with Claude Code

- Claude Code output is a draft. Review every diff and generated file before
  committing — it is not evidence of correctness on its own.
- Never run Claude Code in unattended/"skip permissions" mode in this repo.
- Narrow, bounded delegation (e.g. writing a lint rule, investigating a
  failing CI check) is fine as a subagent task. Contract design decisions
  (new endpoints, breaking changes, schema shape) are not delegated — they
  go through the author + Abdulwahab's review.
- The GitHub Action in this repo is CI only (lint/validate/generate/diff).
  It has no merge, deploy, or secrets access.
