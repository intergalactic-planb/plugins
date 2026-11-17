# Quotalogic plugin for ChatGPT, Codex and Claude

Skills that help the agent work with your Quotalogic catalogs and commercial proposals.

| Skill | What it does |
| --- | --- |
| `catalog-import` | Makes a new catalog from a price list (Excel, CSV, XML, PDF, a site or a link) or updates a catalog from a file: prices, stock, new products, sections. The agent shows you how it understood the file and changes nothing without your yes. |
| `catalog-edit` | Edits catalogs by hand: product values and moves after confirmation, column roles and currencies, names, sections, extra columns, and listed new products. Files use catalog-import. |
| `currency-conversion` | Adds a calculated price column in another currency, uses the effective bank rate, or changes the shared pair to a confirmed custom rate and back. |
| `offer` | Finds, creates and changes a commercial proposal: products, groups or options, quantities, discounts and markups, company terms, and publication after your confirmation. |

## Before you install

Connect Quotalogic to your agent first: the plugin has the skills, while the connection gives the agent access to your workspace. In Quotalogic open the workspace settings, find the section about agents, and follow the steps for your app.

The repository is private for now: you need read access to it on GitHub, and git on your computer signed in to GitHub (for example `gh auth login`).

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

The GitHub marketplace is a local/repository distribution channel. It does not publish the plugin to the universal ChatGPT and Codex directory.
