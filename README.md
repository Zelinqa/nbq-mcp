# NBQ MCP Server

Planned open-source MCP integration for the Zelinqa NBQ API.

## Status

Not implemented for V1 yet. This repository is a placeholder and must not be
advertised as an available integration.

## Required architecture

```text
MCP tool -> official SDK -> public NBQ REST API
```

The server must never import the private engine or access PostgreSQL directly.
It should expose a small stateful-session tool surface, validate inputs, return
typed errors and preserve the API’s idempotency semantics.

## Acceptance before release

- generated/aligned with the V1 OpenAPI contract;
- tested with a temporary real staging key;
- all scopes and revoked-key behavior verified;
- no key, token or conversation content written to logs;
- installation and examples documented in French and English;
- published only after the SDKs themselves are accepted.

See `../docs/platform/api-routes.md` for the current internal route summary and
`../nbq-docs/` for the future public guide.
