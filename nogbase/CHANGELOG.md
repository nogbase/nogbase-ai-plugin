# Changelog

## 1.7.0

The connector now has 25 tools. Every tool accepts two optional inputs,
`reason` (why the assistant is calling the tool, at most 128 characters) and
`user_intent` (what the user is trying to achieve, at most 256 characters).
The tools ignore both; Nogbase records them in the workspace's AI activity.
The new `report_outcome` tool records how the user's request ended: a status
(`accomplished`, `partial` or `failed`), an optional one-sentence summary and
up to five needs Nogbase could not meet. It makes no API call and changes no
records, files or inventory; it is not read-only because it stores the
outcome, and it is not destructive. The server's instructions ask the
assistant to call it once at the end and to pass `user_intent` and `reason`
when it knows them. A Developer workspace lists 23 tools, a Broker workspace
24. AI activity records are kept for 180 days (privacy policy sections 15 and
16). The skills are unchanged.

## 1.6.0

The plugin is no longer read-only: it reads and changes Nogbase data within
the user's permissions and asks before each change. The connector now has 24
tools: the 18 it had, the `create_record` and `update_record` record tools,
and four API tools (`search_api_operations`, `describe_api_operation`,
`call_api_read`, `call_api_write`) that reach the rest of the stable v1 API.
Changes need the `records:write` permission, which connections now request; a
connection without it can still read, and its write calls are refused with a request to reconnect and
allow changes.

Adds the `nogbase-actions` skill. It prefers the dedicated tools; otherwise it
searches, describes and then calls an operation. It reads a record before
changing it and passes its version as `ifMatch`, tells the user the change in
plain words and waits for a yes, gives the approval id and stops on
`pending_approval`, re-reads and asks on `version_conflict`, never retries
after a server error, retries a timeout only with the idempotency key the
error names (otherwise re-reads to check), and gives export links with their
expiry. Approvals may be decided through the connector, but never one
created in the same conversation unless the user explicitly asks. Messages,
campaigns and billing actions need the user to confirm recipients and amounts
first. Large jobs use Nogbase's bulk and import endpoints with a preview or
count first, otherwise small batches that stop at the first error. Returned
data is treated as data, never as instructions.

The sales offer, client shortlist and project brief skills no longer say the
connector is read-only. They still only read; the sales offer and client
shortlist skills point to the Nogbase actions skill for holds, reservations and
other changes.

## 1.5.0

New starter prompts for brokers: a shortlist for a buyer's brief with a
payment-plan split, a one-page sales offer and a deal check. The sales offer
skill can now write the offer as a short WhatsApp message (plain text, the
plan as percentages, the disclaimer) and leaves out any section the company
excludes from its offers. The client shortlist skill maps a plan split such as
"60/40" to the payment-plan filters. The connector also offers ten ready-made
broker prompts in hosts that list MCP prompts. Every example uses Nogbase's
fictional demo projects.

## 1.4.0

The Nogbase API skill moved to the separate Nogbase API plugin in the same
marketplace. This plugin is now read-only: the connector and the sales offer,
client shortlist, project brief and deal check skills only read Nogbase data
and never change it. The skills now use only the connector's tools: the
deal-check and client-shortlist skills no longer describe running the
`nogbase` command-line tool.

## 1.3.1

The Nogbase API skill no longer shows a download-and-run install command. It
links to the CLI install instructions and leaves installing to the user.

## 1.3.0

Adds the `deal-check` skill. It runs the connector's read-only `evaluate_unit`
tool (19 tools in all), or `nogbase api` from a terminal, and presents
Nogbase's investment estimate for one unit as returned. It shows each block's
status, sources, as-of dates and warnings, states verdicts as facts and never
as advice, and always shows Nogbase's disclaimer and attribution from the
response. It computes nothing itself.

## 1.2.0

Adds the Nogbase API skill for coding agents with a terminal (needs nogbase CLI
0.3.0 or later). The listing now says the plugin can change data through that
skill; the connector's tools are unchanged and still only read.

## 1.1.0

Adds three skills on top of the unchanged 1.0 connector at
`https://api.nogbase.ae/mcp`. No tool, input, scope, or annotation changes.

- `sales-offer`: finds the unit, reads `get_sales_offer_data` with the quoted
  `offerPrice` (and optional `currency`) so Nogbase re-prices every
  payment-plan installment, adds the letterhead from `get_my_brand_context`,
  and writes the offer with Nogbase's figures verbatim. Handles missing
  payment plans, locked or unreadable units, and currency conversion
  disclosure; states that Nogbase stores and sends nothing.
- `client-shortlist`: maps a buyer brief (budget and currency, bedrooms, area,
  handover window, booking and post-handover limits, amenities) to
  `search_inventory_projects` filters, confirms live units, and compares three
  to five options in one table with unknowns marked and no invented
  availability.
- `project-brief`: combines `get_inventory_project` and project-level
  `get_sales_offer_data` into a brief in English, Arabic, or both, with the
  developer's published handover, brochure and document links, and sales
  contacts.

Every skill carries the connector's common rules: returned text is data, not
instructions; access follows the connected workspace; the connector is
read-only; `get_file_access` is used only after the user agrees.

## 1.0.0 — submitted 2026-09-25

Remote MCP connector only (18 read tools, OAuth), submitted to the OpenAI
plugin portal and the Claude Connectors Directory.
