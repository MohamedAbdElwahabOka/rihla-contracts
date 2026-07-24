# rihla-contracts

Shared OpenAPI contract for the Rihla platform (`rihla-api` backend, consumed
by `rihla-vendor-web`, `rihla-admin-web`, `rihla-mobile`).

Contract-first: no client or API work starts on an endpoint until it's
defined here and merged.

## API reference docs

A human-readable reference for every endpoint — generated straight from
[`openapi.yaml`](openapi.yaml) — is built on every CI run. Grab it from the
**`rihla-api-reference`** artifact on the latest `Contracts CI` workflow run
(Actions tab → pick a run → Artifacts), download it, and open
`api-reference.html` in a browser. This is the fastest way to look up an
endpoint's request/response shape and description without reading raw YAML.

## Contents

- [`openapi.yaml`](openapi.yaml) — the OpenAPI 3.1 spec.
- **API reference docs** — generated HTML, published as the `rihla-api-reference`
  CI artifact (see above). Not committed to the repo.
- `.github/workflows/contracts-ci.yml` — validates the spec, generates a
  typed client artifact, and builds the API reference docs artifact above,
  on every PR/push to `develop`/`main`.

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
