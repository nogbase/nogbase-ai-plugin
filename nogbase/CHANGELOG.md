# Changelog

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
