# Quotalogic plugin for ChatGPT, Codex and Claude

Skills that help the agent work with your Quotalogic catalogs and commercial proposals.

| Skill | What it does |
| --- | --- |
| `catalog-import` | Makes a new catalog from a price list (Excel, CSV, XML, PDF, a site or a link) or updates a catalog from a file: prices, stock, new products, sections. The agent shows you how it understood the file and changes nothing without your yes. |
| `catalog-edit` | Edits catalogs by hand: product values and moves after confirmation, column roles and currencies, names, sections, extra columns, and listed new products. Files use catalog-import. |
| `currency-conversion` | Adds a calculated price column in another currency, uses the effective bank rate, or changes the shared pair to a confirmed custom rate and back. |
| `offer` | Finds, creates and changes a commercial proposal: products, groups or options, quantities, discounts and markups, company terms, and publication after your confirmation. |

## Before you install

The plugin brings both the skills and the Quotalogic connection (`.mcp.json`, the staging server for now). After install, sign in once:

- Claude app, **Code** tab: type only `/mcp`, then click **Connect** next to `quotalogic`.
- Claude Code in a terminal: run `/mcp`, choose `plugin:quotalogic:quotalogic`, then **Authenticate**.
- Codex: `codex mcp login quotalogic`.
- Claude chat and Cowork: open the plugin's **Connectors** tab and connect it. On Team and Enterprise an Owner adds the connector for the organization first.
- ChatGPT web in a managed workspace: connect the separately provided **Quotalogic MCP** app.

If you already added the same server by hand, Claude Code keeps one copy, but Codex shows two. In Codex remove the manual one: `codex mcp remove <name>`, with its name from `codex mcp list` (not `quotalogic`).

Install from this GitHub repository; no GitHub account is needed.

For Alice AI, follow the [personal MCP plugin guide](docs/connect/en/alice-act.md).

## Install in Codex and ChatGPT desktop

```bash
codex plugin marketplace add intergalactic-planb/plugins
codex plugin add quotalogic@quotalogic-plugins
codex plugin list --marketplace quotalogic-plugins
```

Restart ChatGPT desktop or start a new Codex chat after installation.

## Use in Codex and ChatGPT desktop Work

Type `$` and select a skill such as `$quotalogic:catalog-import`. On this surface, `@` searches files and previous chats; it does not invoke a skill.

For an import check, make the agent show the column mapping and wait for a separate yes. After the import, ask it to read the created product and compare the stored values with the file. A known backend import bug can store a fractional price one cent lower (`19.99` as `19.98`) while reporting success with no warnings. Use a whole-number test price when checking only the plugin, or verify every fractional price until the backend is fixed.

## Install in ChatGPT web

The GitHub marketplace provides the skills package. Workspace members also need access to **Quotalogic MCP** and must sign in with their own Quotalogic account to read catalogs. See the [setup steps for admins and members](docs/connect/en/codex-desktop.md#set-up-quotalogic-for-a-chatgpt-workspace).

Test with a whole-number price, require a separate yes after the column mapping, and read the created product back after import. The check is complete only when both the skill and Quotalogic MCP tools are available. A loaded skill with missing tools proves only that the GitHub package was imported.

If **Admin → Plugins** is unavailable, a workspace admin must perform the import. This GitHub marketplace is not a public-directory release and cannot currently be self-installed in an ordinary web account. A **Desktop only** plugin cannot run on the web.

Workspace marketplaces sync daily. Admins can request an immediate update under **Admin → Plugins → Marketplaces → Sync now**; start a new chat afterward. Deleting the marketplace removes every plugin imported from it, but does not disconnect a separately installed MCP app.

See OpenAI's [plugin guide](https://learn.chatgpt.com/docs/plugins) and [GitHub marketplace import guide](https://learn.chatgpt.com/docs/enterprise/plugin-management).

## Install in Claude Code

```bash
claude plugin marketplace add intergalactic-planb/plugins
claude plugin install quotalogic@quotalogic-plugins
```

## Update

Updates do not come by themselves. In Codex:

```bash
codex plugin marketplace upgrade quotalogic-plugins
codex plugin remove quotalogic@quotalogic-plugins
codex plugin add quotalogic@quotalogic-plugins
```

In Claude Code:

```bash
claude plugin update quotalogic@quotalogic-plugins
```

Or turn on auto-update in Claude: `/plugin` → **Marketplaces** → `quotalogic-plugins` → **Enable auto-update**.

Then ask the agent in your words, for example "make a catalog from this price list", "update the prices in my catalog from this file" or "make and send a proposal for this customer".

The GitHub marketplace is a repository distribution channel for Codex and managed ChatGPT workspaces. It does not publish the plugin to the universal ChatGPT and Codex directory.
