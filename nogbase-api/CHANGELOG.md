# Changelog

## 1.5.0

Scripts are now allowed when every Nogbase call goes through `nogbase`
commands, for example a shell or Python loop over `nogbase api ...`. The skill
still never uses curl or an HTTP library, never reads the CLI config, and never
puts a token in a script.

New "Bulk and long jobs" section: prefer Nogbase's server-side jobs
(`POST /v1/units/bulk-lifecycle`, project inventory import sessions, offering
import with preview, workflows) and show the preview or count first; otherwise
a script that pages with `meta.nextCursor`, works in batches, keeps a local
checkpoint file of finished ids, passes `--if-match` on updates, reuses an
idempotency key only on the exact retry of an uncertain write, stops on the
first non-retryable error, and prints a done/skipped/failed summary. The user
sees the plan and the record count, and says yes, before a script changes more
than a handful of records.

## 1.4.0

Split out of the `nogbase` plugin. The Nogbase API skill is now its own plugin
in the `nogbase-ai` marketplace, so the `nogbase` plugin stays read-only. It
needs the `nogbase` CLI, version 0.3.0 or later, and declares no MCP server.

The skill gains an Investment evaluation section: the read-only
`POST /v1/inventory/projects/<projectId>/evaluation` endpoint, presented as
Nogbase returns it, with the disclaimer and attribution from the response.
This replaces the terminal steps the deal-check skill used to carry.
