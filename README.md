# Quotalogic plugin for Claude

Skills that help Claude work with your Quotalogic catalogs and commercial proposals.

| Skill | What it does |
| --- | --- |
| `catalog-import` | Makes a new catalog from a price list (Excel, CSV, XML, PDF, a site or a link) or updates a catalog from a file: prices, stock, new products, sections. Claude shows you how it understood the file and changes nothing without your yes. |
| `catalog-edit` | Edits catalogs by hand: product values and moves after confirmation, column roles and currencies, names, sections, extra columns, and listed new products. Files use catalog-import. |
| `currency-conversion` | Adds a calculated price column in another currency, uses the effective bank rate, or changes the shared pair to a confirmed custom rate and back. |
| `offer` | Finds, creates and changes a commercial proposal: products, groups or options, quantities, discounts and markups, company terms, and publication after your confirmation. |

## Before you install

Connect Quotalogic to Claude first: the plugin has the skills, the connection gives Claude access to your workspace. In Quotalogic open the settings of the workspace, find the section about agents and follow the steps for your Claude app.

## Install

In Claude Code:

```bash
claude plugin marketplace add intergalactic-planb/plugins
claude plugin install quotalogic@quotalogic-plugins
```

The repository is private for now: you need read access to it on GitHub, and git on your computer signed in to GitHub (for example `gh auth login`).

## Update

Updates do not come by themselves. To get a new version:

```bash
claude plugin update quotalogic@quotalogic-plugins
```

Or turn on auto-update: in Claude run `/plugin` → **Marketplaces** → `quotalogic-plugins` → **Enable auto-update**.

Then ask Claude in your words, for example "make a catalog from this price list", "update the prices in my catalog from this file" or "make and send a proposal for this customer".
