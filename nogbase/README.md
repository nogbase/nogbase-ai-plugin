# Nogbase

Nogbase helps real-estate brokers and property developers work with their
Nogbase inventory, including off-plan projects, from their AI assistant. This
plugin pairs the Nogbase connector with four skills that turn common sales
tasks into repeatable steps, and adds a Nogbase API skill for coding agents:

| Skill | What it does |
| --- | --- |
| Sales offer | A branded offer for one unit at the quoted or list price, with every payment-plan installment computed by Nogbase, the offer terms and fees, and your company letterhead. |
| Client shortlist | Turns a buyer's brief (budget and currency, bedrooms, area, handover window, booking and post-handover limits) into filtered searches and compares three to five options in one table. |
| Project brief | A project summary with the developer's published handover, unit mix and starting prices, payment plans, amenities, brochures, and sales contacts, in English, Arabic, or both. |
| Deal check | Nogbase's investment estimate for one off-plan unit: entry cost, the payment schedule, price against DLD sales, value scenarios with the sale at handover, break-even and, from a rent you give, the yield. Every figure shows its source and as-of date, with Nogbase's disclaimer; it is an estimate, not financial advice. |
| Nogbase API | For coding agents with a terminal, such as Claude Code and Codex: reads and changes your Nogbase workspace data through the `nogbase` CLI with your own sign-in, at your request, and asks before deleting or bulk changes. |

Try: "Prepare a one-page sales offer for unit B827 in Peace Lagoons at AED
1,000,000 with my company letterhead", "Compare three projects for a client who
wants a one-bedroom with at most 10% on booking", "Write a project brief for
this project in Arabic", or "Is unit B827 in Peace Lagoons a good investment if it
rents for AED 90,000 a year?"

## Requirements

A Nogbase broker or developer account. When you first use the plugin, your AI
application asks you to connect Nogbase: you sign in on the Nogbase page and
choose the workspace (company) the connection may read.

The Nogbase API skill also needs the `nogbase` CLI, version 0.3.0 or later,
installed on your machine and signed in with `nogbase login`.

## What the plugin connects to and what it does

- The connector tools connect to one service, the Nogbase connector at
  `https://api.nogbase.ae/mcp`, over OAuth, and only read. They cannot
  reserve, mark sold, change prices, save, or send anything in Nogbase. Offers
  and briefs are written in your conversation; Nogbase does not store them.
- The Nogbase API skill runs the `nogbase` CLI on your machine. The CLI is
  installed separately, and you sign it in with `nogbase login`. With that
  sign-in it can read and change anything your Nogbase account may, when you
  ask. It asks before deleting or bulk changes. The plugin itself contains no
  scripts, hooks, or executables.
- Results include only the projects and units your workspace may see. Locked
  projects show a short card until access is granted.
- Opening a file stored in Nogbase creates a five-minute link and an audit
  record, so the skills ask you before opening one.
- Nogbase sends your AI application only what the tools return: inventory and
  project data your workspace may read, file names and short-lived links, and
  your company's public branding. The connector never sends personal email,
  bank, or tax details; the API skill sees only what your own account can read.

## Links

- Documentation: https://nogbase.ae/ai-connectors
- Privacy policy: https://nogbase.ae/privacy
- Terms of service: https://nogbase.ae/terms
- Support: support@nogbase.ae or https://nogbase.ae/support

See [CHANGELOG.md](CHANGELOG.md) for release notes.
