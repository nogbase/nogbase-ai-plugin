# Nogbase API

The Nogbase API skill lets coding agents with a terminal, such as Claude Code
and Codex, work with your Nogbase workspace through the Nogbase v1 API. The
agent uses the `nogbase` command-line tool with your own sign-in, so it reads
and changes records, schemas, the change feed, and audit history only when you
ask, and only within your own permissions.

For the Nogbase connector and the sales offer, client shortlist, project brief
and deal check skills, install the `nogbase` plugin from the same marketplace.

## Requirements

- A Nogbase account.
- The `nogbase` CLI, version 0.3.0 or later, installed with the instructions at
  https://github.com/nogbase/nogbase-cli#install.
- A signed-in CLI: run `nogbase login` yourself. It opens your browser.

## What it can do

- Read and change Nogbase data with your own permissions, at your request.
  Every change is recorded as yours.
- It asks before deleting anything, before changing more than a handful of
  records, and before any change to users, API keys, permissions or workspace
  settings.
- The model never sees a token or API key. The CLI keeps your sign-in and
  calls the API; the skill only tells the agent which `nogbase` commands to
  run.
- The plugin contains no scripts, hooks, or executables and declares no MCP
  server. It asks you to install the CLI yourself and never runs an installer.

## Install

Claude Code:

```text
/plugin marketplace add nogbase/nogbase-ai-plugin
/plugin install nogbase-api@nogbase-ai
```

Codex:

```bash
codex plugin marketplace add nogbase/nogbase-ai-plugin
codex plugin add nogbase-api@nogbase-ai
```

## Links

- Documentation: https://nogbase.ae/ai-connectors
- Privacy policy: https://nogbase.ae/privacy
- Terms of service: https://nogbase.ae/terms
- Support: support@nogbase.ae or https://nogbase.ae/support

See [CHANGELOG.md](CHANGELOG.md) for release notes.
