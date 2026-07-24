# rihla-contracts

Shared OpenAPI contract for the Rihla platform (`rihla-api` backend, consumed
by `rihla-vendor-web`, `rihla-admin-web`, `rihla-mobile`).

Contract-first: no client or API work starts on an endpoint until it's
defined here and merged.

## Contents

- [`openapi.yaml`](openapi.yaml) — the OpenAPI 3.1 spec.
- `.github/workflows/contracts-ci.yml` — validates the spec and generates a
  typed client artifact on every PR/push to `develop`/`main`.

## Local usage

```bash
npm ci
npm run validate         # lint the OpenAPI spec
npm run generate-client  # generate the typed client into generated/client
```

See [`CLAUDE.md`](CLAUDE.md) for AI-assistant working rules (canonical for
the whole Rihla platform).

## Note

`@redocly/cli` and `openapi-typescript-codegen` both register a `openapi` bin
name, and npm resolves only one on `PATH`. `generate-client` invokes
`openapi-typescript-codegen`'s script directly (`node
node_modules/openapi-typescript-codegen/bin/index.js ...`) to avoid the
collision — don't switch it back to the bare `openapi` command.
