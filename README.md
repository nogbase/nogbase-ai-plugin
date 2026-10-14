# Nogbase AI connector plugins (1.4, skills)

**Status (2026-10-14): published at https://github.com/nogbase/nogbase-ai-plugin; directory submissions pending.** This directory is the
version 1.4 marketplace for the Nogbase remote MCP connector
(`https://api.nogbase.ae/mcp`). It holds two plugins:

- `nogbase`: the connector plus four chat skills (sales offer, client
  shortlist, project brief, deal check). It only reads Nogbase data. The
  deal-check skill uses the read-only `evaluate_unit` tool added on the server
  in October 2026.
- `nogbase-api`: the Nogbase API skill alone, for terminal agents such as
  Claude Code and Codex. It uses the separately installed `nogbase` CLI
  (0.3.0 or later), can change data at the user's request, and declares no MCP
  server.

See [Publishing](#publishing) for the directory submissions.

This directory is laid out to be published as **its own public GitHub
repository**. Anthropic's directory reads plugins only from GitHub
repositories that are public before the listing goes live and under 50 MiB as
GitHub archives them; `nogbase/nogbase-core` is private and far larger, so it
cannot be the source. Copy this directory's contents to the new repository's
root unchanged.

## Layout

```text
ai-connector-plugin/                  repository root (= marketplace root)
├── .claude-plugin/marketplace.json   Claude Code marketplace listing ./nogbase and ./nogbase-api
├── .agents/plugins/marketplace.json  Codex / ChatGPT desktop marketplace listing ./nogbase and ./nogbase-api
├── README.md                         this file (maintainers)
├── nogbase/                          read-only plugin folder (submit this path to Anthropic)
│   ├── .claude-plugin/plugin.json    Claude plugin manifest
│   ├── .mcp.json                     Claude: remote server, type "http"
│   ├── plugin.json                   Agent Plugins manifest + extensions.com.openai
│   ├── mcp.json                      Agent Plugins: remote server, type "streamable-http"
│   ├── skills/
│   │   ├── sales-offer/SKILL.md          + agents/openai.yaml
│   │   ├── client-shortlist/SKILL.md     + agents/openai.yaml
│   │   ├── project-brief/SKILL.md        + agents/openai.yaml
│   │   └── deal-check/SKILL.md           + agents/openai.yaml
│   ├── assets/logo.svg               square canvas around the official Nogbase mark
│   ├── README.md                     listing text (Anthropic shows it as the description)
│   ├── CHANGELOG.md
│   └── LICENSE
└── nogbase-api/                      terminal-agent plugin folder (no MCP server)
    ├── .claude-plugin/plugin.json    Claude plugin manifest
    ├── plugin.json                   Agent Plugins manifest (no extensions.com.openai)
    ├── skills/
    │   └── nogbase-api/SKILL.md          + agents/openai.yaml (needs the nogbase CLI, not the connector)
    ├── README.md
    ├── CHANGELOG.md
    └── LICENSE
```

Each plugin folder serves both vendors: Claude Code reads
`.claude-plugin/plugin.json` and `.mcp.json`; Codex and ChatGPT read the
portable root `plugin.json`, `mcp.json`, and `skills/`, and ignore the Claude
files. `nogbase-api/` has neither MCP file. Both read the same
`skills/<name>/SKILL.md` files, whose frontmatter uses only `name` and
`description` so it is valid for every surface.

## Vendor formats (read 2026-09-27)

Anthropic (Claude Code, claude.ai, Cowork):

- Manifest at `.claude-plugin/plugin.json`; only `name` is required; set
  `version`, `description`, `author` to avoid warnings; `homepage` must parse
  as a URL. [Plugin manifest reference](https://code.claude.com/docs/en/plugins/manifest-reference)
- Skills at `skills/<name>/SKILL.md`; frontmatter `description` is a single
  string; outside Claude Code only `name`, `description`, `license`,
  `compatibility`, `metadata`, `allowed-tools` are accepted.
  [Skills](https://code.claude.com/docs/en/skills),
  [Agent Skills specification](https://agentskills.io/specification)
- A remote MCP server is referenced in `.mcp.json` by `type: "http"` and an
  `https://` URL, never with secrets; use the same URL as the directory
  connector so users see one set of tools; on claude.ai and Cowork it appears
  on the plugin's Connectors tab and signs in through the server's OAuth.
  [Plugin structure and testing](https://claude.com/docs/plugins/build),
  [Add components](https://code.claude.com/docs/en/plugins/components)
- Directory submission: a GitHub repository (plugin path optional), README of
  at least 40 words and a LICENSE (or `license`) in the plugin folder, files
  under 256 KiB, no binaries other than images and fonts.
  [Submit your plugin](https://claude.com/docs/plugins/submit),
  [Pre-submission checklist](https://claude.com/docs/plugins/pre-submission-checklist),
  [Publish and distribute](https://code.claude.com/docs/en/plugins/publish)
- Marketplace: `.claude-plugin/marketplace.json` with `name`, `owner`,
  `plugins`. [Marketplace reference](https://code.claude.com/docs/en/plugins/marketplace-reference)

OpenAI (ChatGPT and Codex share one Plugins Directory):

- Portable manifest: root `plugin.json` declaring
  `https://agent-plugins.org/schemas/1.0.0/plugin.schema.json`; OpenAI listing
  fields under `extensions.com.openai.interface`; skills are discovered in
  `skills/`, MCP servers in root `mcp.json` (`type: "streamable-http"`).
  `.codex-plugin/plugin.json` remains a compatibility fallback.
  [Package your plugin](https://developers.openai.com/plugins/build/plugins),
  [plugin schema](https://agent-plugins.org/schemas/1.0.0/plugin.schema.json),
  [MCP schema](https://agent-plugins.org/schemas/1.0.0/mcp.schema.json)
- Skills are supported (skills-only, MCP-only, or MCP plus skills). A skill's
  MCP dependency and its display metadata go in
  `skills/<name>/agents/openai.yaml`.
  [Build skills](https://developers.openai.com/plugins/build/skills)
- A directory plugin with MCP is submitted **With MCP**: the portal takes the
  server URL itself and scans it; an existing integration cannot be
  referenced. Skills are uploaded as a bundle into the same draft (or imported
  from the MCP server, which would need server changes and is not used here).
  Listing limits: display name and short description at most 30 characters,
  at most 3 starter prompts of at most 128 characters.
  [Submit plugins](https://developers.openai.com/plugins/deploy/submission),
  [Submission errors](https://developers.openai.com/plugins/deploy/submission-errors),
  [Submit your Claude Code plugin to OpenAI](https://developers.openai.com/plugins/guides/submit-claude-plugin)

## Test locally

Checks that need no sign-in (run from this directory):

```bash
claude plugin validate --strict ./nogbase       # plugin manifest, .mcp.json, skills
claude plugin validate --strict ./nogbase-api   # plugin manifest, skill
claude plugin validate --strict .               # marketplace
claude --plugin-dir ./nogbase plugin details nogbase           # 4 skills, 1 MCP server
claude --plugin-dir ./nogbase-api plugin details nogbase-api   # 1 skill, no MCP server
```

### Claude Code

1. Load the plugins for one session:
   `claude --plugin-dir ./nogbase --plugin-dir ./nogbase-api`
   (or install them: `claude plugin marketplace add ./`, then
   `claude plugin install nogbase@nogbase-ai` and
   `claude plugin install nogbase-api@nogbase-ai`).
2. The skills appear as `/nogbase:sales-offer`, `/nogbase:client-shortlist`,
   `/nogbase:project-brief`, `/nogbase:deal-check` (plugin `nogbase`) and
   `/nogbase-api:nogbase-api` (plugin `nogbase-api`), and load automatically
   when a request matches.
3. Sign-in: the Nogbase authorization server accepts OAuth client ID
   metadata documents, so Claude Code signs in to the plugin's server with
   its own published identity: run `/mcp`, pick `nogbase`, and authenticate,
   then choose the workspace on the Nogbase page.

### Codex

1. Add the marketplace and install:

   ```bash
   codex plugin marketplace add ./
   codex plugin add nogbase@nogbase-ai
   codex plugin add nogbase-api@nogbase-ai
   ```

   `codex mcp list` then shows the `nogbase` plugin's `nogbase` server, and the
   skills load as `nogbase:sales-offer`, `nogbase:client-shortlist`,
   `nogbase:project-brief`, `nogbase:deal-check` and
   `nogbase-api:nogbase-api`.
2. Sign-in: Codex signs in to the plugin's server with its own published
   identity (OAuth client ID metadata document); approve the Nogbase page
   when Codex opens it and choose the workspace.
3. ChatGPT desktop reads the same marketplace from
   `.agents/plugins/marketplace.json`; restart the app to pick it up.

### Prompts to run (reviewer personas from `docs/release/remote-ai-connectors-submission.md`)

| Skill | Prompt | Expect |
| --- | --- | --- |
| client-shortlist | "Find projects in Dubai Marina with two-bedroom units under AED 3,000,000." (P1) | `search_inventory_locations` → `search_inventory_projects` with `locationId`, `bedrooms`, `priceMax`, `currencyCode`; table with unknowns marked |
| client-shortlist | "Compare three projects for a client who wants a one-bedroom with at most 10% on booking." | `maxBookingPercent: 10`; availability only from unit rows or counts |
| project-brief | "Tell me about Peace Lagoons: developer, location, unit mix and starting price." (P3) | `get_inventory_project` + `get_sales_offer_data` (`purpose: project_brief`); handover as the developer's published Q2 2027 |
| project-brief | "Write a project brief for Peace Lagoons in Arabic." | `locale: ar-AE` labels; translated text marked as the model's |
| sales-offer | "Prepare a one-page sales offer for unit B827 in Peace Lagoons at AED 1,000,000 with my company letterhead and every payment-plan installment." (P4) | `get_sales_offer_data` with `unitId`, `offerPrice: "1000000"`, `purpose: sales_offer_one_page`; `get_my_brand_context`; every plan re-priced (20% = AED 200,000); "Nogbase stored nothing" |
| deal-check | "Is unit B827 in Peace Lagoons a good investment if it rents for AED 90,000 a year? Show what the buyer would make selling at handover." (P6) | `search_inventory_units` → `evaluate_unit` with `assumptions.annualRent` `"90000"` + `currencyCode` AED; blocks with status and sources; verdicts as facts; disclaimer and attribution from the response |
| sales-offer (negative) | "Mark unit B827 in Peace Lagoons as sold." (N1) | No tool call; explains the connector is read-only |

## Open points before submission

- **License**: `nogbase/LICENSE` grants use with a Nogbase account and no
  redistribution. Anthropic requires a license file; its terms are an owner
  decision.
- **Icon**: `nogbase/assets/logo.svg` is the brand app icon (the blade-towers
  mark in a 260 × 260 brand-blue tile), written by
  `docs/design/2026-09-30-premium-pass/generate_brand.py`; regenerate it there
  rather than editing the SVG. `logo` and `composerIcon` in `plugin.json` both
  point at it.
- **OpenAI package name**: the portal's package name is
  `app-6ab582e45b5481919e74c6db3f72a18f` (the 1.0 listing was made with the
  old form, in Codex format). A new version must keep it
  (`plugin_name_mismatch`), so the ZIP recipe below rewrites `name` from
  `openai-listing.json`; the repository keeps `nogbase`.

## Publishing

Anthropic (plugin bundle):

Submit only plugin path `nogbase`. Compliance acknowledgement #3 says the
plugin "does not exfiltrate credentials or execute code outside its declared
MCP servers", and the Nogbase API skill drives a local CLI, so `nogbase-api`
stays out of the directory and is installed from the marketplace only.

1. Create the public repository from this directory; push; tag `nogbase--v1.4.0`
   (`claude plugin tag ./nogbase`) and `nogbase-api--v1.4.0`
   (`claude plugin tag ./nogbase-api`).
2. `claude plugin validate --strict ./nogbase` in the new repository.
3. [claude.ai/directory/manage](https://claude.ai/directory/manage) → Submit
   new → **Plugin bundle** → repository, plugin path `nogbase`, tracked branch
   or tag → **Validate** → fix any Blocking finding → listing details (read
   from `plugin.json` and `nogbase/README.md`) → data handling (the declared
   connector only reads Nogbase data and never changes it; the plugin stores
   nothing) → compliance → Submit for review → Publish when it passes.

OpenAI (new version of the existing Nogbase plugin):

1. In the [plugin portal](https://platform.openai.com/plugins), open the
   published Nogbase plugin and start a new version (**With MCP**, same
   universal URL `https://api.nogbase.ae/mcp`), then **Scan Tools**.
2. Skills: upload a ZIP whose single top-level directory is `nogbase/`. It
   holds all five skills, because Codex shares this directory and has a
   terminal. The recipe:
   - copies `nogbase/` and removes `.mcp.json`, `mcp.json` (the portal carries
     the server), `.claude-plugin/`, `README.md` and `CHANGELOG.md` (the Agent
     Plugins format needs only `plugin.json`, `skills/` and `assets/`);
   - copies `nogbase-api/skills/nogbase-api` into `nogbase/skills/`;
   - overlays `openai-listing.json` onto `plugin.json`: the portal's package
     name, and the five-skill listing text and capabilities. The repository's
     `nogbase/plugin.json` describes only the four read-only skills.

   From this directory:

   ```bash
   out="$PWD/nogbase-1.4.0.zip"
   tmp="$(mktemp -d)"
   cp -R nogbase "$tmp/nogbase"
   rm -rf "$tmp/nogbase/.mcp.json" "$tmp/nogbase/mcp.json" "$tmp/nogbase/.claude-plugin" \
     "$tmp/nogbase/README.md" "$tmp/nogbase/CHANGELOG.md"
   cp -R nogbase-api/skills/nogbase-api "$tmp/nogbase/skills/nogbase-api"
   jq --slurpfile o openai-listing.json '.name = $o[0].name
     | .extensions["com.openai"].interface.capabilities = $o[0].capabilities
     | .extensions["com.openai"].interface.longDescription = $o[0].longDescription' \
     nogbase/plugin.json > "$tmp/nogbase/plugin.json"
   (cd "$tmp" && zip -r "$out" nogbase)
   rm -rf "$tmp"
   unzip -l "$out"   # plugin.json, LICENSE, assets/, five skills under skills/
   ```

   Wait for the skill scans (up to 2 hours).
3. Starter prompts: the three in `plugin.json`. Test cases: P1-P6 and N1, N3,
   N4 from the submission pack, noting which skill each exercises.
4. Release notes: "Update 1.4.0: adds the read-only Evaluate a unit tool (an
   investment estimate for one unit from DLD sales, with sources, as-of dates
   and a disclaimer) and the Sales offer, Client shortlist, Project brief, Deal
   check and Nogbase API skills. The Nogbase API skill can change data through
   the user's own nogbase CLI sign-in. Other tools and annotations
   unchanged."
5. Submit for review; publish after approval.
