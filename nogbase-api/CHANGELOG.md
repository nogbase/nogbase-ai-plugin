# Changelog

## 1.4.0

Split out of the `nogbase` plugin. The Nogbase API skill is now its own plugin
in the `nogbase-ai` marketplace, so the `nogbase` plugin stays read-only. It
needs the `nogbase` CLI, version 0.3.0 or later, and declares no MCP server.

The skill gains an Investment evaluation section: the read-only
`POST /v1/inventory/projects/<projectId>/evaluation` endpoint, presented as
Nogbase returns it, with the disclaimer and attribution from the response.
This replaces the terminal steps the deal-check skill used to carry.
