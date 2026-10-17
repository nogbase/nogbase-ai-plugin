# Changelog

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

## 1.1.0 — prepared 2026-09-27, not yet submitted

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
