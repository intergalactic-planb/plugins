# Quotalogic plugin for ChatGPT, Codex and Claude

Skills that help the agent work with your Quotalogic catalogs and commercial proposals.

| Skill | What it does |
| --- | --- |
| `catalog-import` | Makes a new catalog from a price list (Excel, CSV, XML, PDF, a site or a link) or updates a catalog from a file: prices, stock, new products, sections. The agent shows you how it understood the file and changes nothing without your yes. |
| `catalog-edit` | Edits catalogs by hand: product values and moves after confirmation, column roles and currencies, names, sections, extra columns, and listed new products. Files use catalog-import. |
| `currency-conversion` | Adds a calculated price column in another currency, uses the effective bank rate, or changes the shared pair to a confirmed custom rate and back. |
| `offer` | Finds, creates and changes a commercial proposal: products, groups or options, quantities, discounts and markups, company terms, and publication after your confirmation. |

## Full install guides

Step by step for each app: install, sign in, update, turn off and remove. Start page in [English](docs/connect/en/README.md), [Russian](docs/connect/ru/README.md), [Chinese](docs/connect/cn/README.md), or open the clickable page [docs/connect/index.html](docs/connect/index.html) in a browser (all three languages). Team notes (tests, plugin files for internal testing): [docs/build-archives.md](docs/build-archives.md).

The sections below are a short summary.

## Before you install

The plugin brings both the skills and the Quotalogic connection (`.mcp.json`, the staging server for now). After install, sign in once:

- Claude Code: run `/mcp`, choose `plugin:quotalogic:quotalogic`, then **Authenticate**.
- Codex: `codex mcp login quotalogic`.
- Claude chat and Cowork: open the plugin's **Connectors** tab and connect it. On Team and Enterprise an Owner adds the connector for the organization first.
- ChatGPT web: not confirmed that the bundled connection works; use the separate Quotalogic app (see below).

If you already added the same server by hand, Claude Code keeps one copy, but Codex shows two. In Codex remove the manual one: `codex mcp remove <name>`, with its name from `codex mcp list` (not `quotalogic`).

Install from this GitHub repository; no GitHub account is needed.

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

The package includes the Quotalogic connection, but it is not confirmed that ChatGPT web uses a connection bundled in a plugin. Until that is checked, users on the web also need a separately available and authenticated Quotalogic app with MCP tools.

A workspace admin can import this GitHub marketplace:

1. Open **Admin → Plugins → Add → Import marketplace**.
2. Set **Source** to `https://github.com/intergalactic-planb/plugins`, leave **Path** empty, and set **Branch, tag, or commit** to `main`.
3. Import the marketplace (allow GitHub access if asked) and review **Import results**.
4. Open `Quotalogic` and set its installation policy to **Available** or **Installed** for the required roles. Make the separate Quotalogic MCP app available to the same roles.

Users then open **Plugins**, select their workspace tab, install `Quotalogic` and the Quotalogic MCP app if needed, and start a new Chat or Work conversation. Type `@` and select the plugin or its `catalog-import` skill. On ChatGPT web, `@` invokes plugins and skills; `$` is for Codex.

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
