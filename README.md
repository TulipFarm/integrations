# TulipFarm Integrations

The official integrations marketplace for [TulipFarm](https://github.com/TulipFarm/tulipfarm).
TulipFarm instances browse this repository via `GET /api/v1/integrations/marketplace` and
install integrations through the Integrations UI.

Integrations connect TulipFarm agents to external services. V1 supports **MCP (Model Context Protocol)** integrations — each one spawns a local subprocess or connects to a remote SSE endpoint and exposes its tools directly to every agent in your instance.

## Layout

```text
registry.json                        # catalog manifest (verified badges + descriptions)
<name>/
  manifest.json                      # integration definition
```

Each `manifest.json` carries:

```json
{
  "name": "slug",
  "type": "mcp",
  "version": "1.0.0",
  "description": "One-line description.",
  "maintainer": "TulipFarm",
  "entry": {
    "transport": "sse",
    "url": "https://..."
  },
  "required_env": [
    { "name": "API_TOKEN", "label": "API Token", "secret": true }
  ]
}
```

- `name` — kebab-case slug (`^[a-z][a-z0-9-]*$`), must match the directory name.
- `type` — `mcp` (only type supported in V1).
- `entry.transport` — `stdio` (local subprocess) or `sse` (remote HTTP/SSE endpoint).
- `required_env` — fields the user must supply on the connect screen. Mark secrets `secret: true` so the UI masks them.

## `registry.json`

Optional root-level catalog that adds `verified` badges. Discovery works without it — it only enriches the marketplace listing.

```json
{
  "integrations": [
    { "name": "slack",  "verified": true },
    { "name": "github", "verified": true }
  ]
}
```

## Install flow

1. **Marketplace** — user browses `GET /api/v1/integrations/marketplace`, selects integrations, clicks Install. TulipFarm copies the `manifest.json` into the soul repo.
2. **Connect** — user opens the integration detail page, fills in any `required_env` fields, clicks Connect. TulipFarm writes `connection.yaml` to soul and starts the MCP server.
3. **Tools available** — all tools exposed by the MCP server are registered as `integration_<name>_<tool>` and are immediately available to agents.

## Adding an integration

1. Create `<name>/manifest.json` (kebab-case directory and `name` field).
2. Add an entry to `registry.json` (`verified: true` for officially maintained integrations).
3. Open a pull request — include a brief description of the service and any setup steps needed to obtain credentials.

## Contributing

Integrations in this repo are **officially maintained by TulipFarm**. For third-party integrations, users can point TulipFarm at any GitHub repo from the marketplace scan box.

Please keep `required_env` minimal — only ask for credentials the MCP server actually needs.

## License

Released under the [MIT License](LICENSE).
