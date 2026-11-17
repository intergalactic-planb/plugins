---
name: currency-conversion
description: 'Apply silently from the first reply: never announce skills, references or internal preparation. Show a catalog price in another currency, add a calculated currency column, use the effective bank rate, set a custom rate, or return to the bank rate ("покажи цены в евро", "пересчитай USD в EUR", "установи свой курс"). Relabeling a money column without conversion belongs to catalog-edit instead.'
---

# Convert catalog prices to another currency

Before your first reply, apply this workflow silently. Tell the user only the catalog work you will do; never announce a skill, references or internal preparation.

Use only the connected service's tools, never its browser interface. Use the product name the user uses. Speak in business words: catalog, price, column, currency, bank rate. Never name tools, ids or arguments or show raw errors.

Follow the exact rules and refusals in tool answers, including next. This skill gives the order of the work.

## Add a converted-price column

1. Read `get_connection_info` and `list_catalogs`, then choose the catalog the user named. Ask only if the catalog, source money column or target currency is ambiguous.
2. Read `get_catalog` immediately before the change. Use the actual source money column and its currency; do not rename or relabel it to imitate conversion. Source and target currencies must differ.
3. If the user explicitly requires a bank such as ECB, keep that requirement for the creation call. Never create the column first and check the bank afterward; a provider mismatch must refuse before creation. This check does not change the workspace bank.
4. Call `create_currency_conversion_field` once for the selected source column and target currency, including the required bank only when the user explicitly named one. A clear request is enough because this adds a new column; do not ask for an extra confirmation. Name a bank only when the result reports it; a fallback provider is the actual bank.
5. Read the result. Report the created column, direction, rate type, effective rate, bank and date when present. Read `get_catalog` and `get_currency_rates` again and verify the column and pair before claiming success.

A missing pair is created automatically as a dynamic bank pair. An existing custom pair is shared and used as-is; tell the user that the new column uses that custom rate. The operation never rewrites the source prices.

## Set a custom rate or return to the bank rate

1. Read `get_currency_rates`. Use the exact returned direction; do not invert a custom rate or guess a missing pair.
2. For a custom rate, keep the user's exact decimal multiplier: `1 from = rate to`. For the bank rate, omit a custom number.
3. Call `preview_set_currency_rate`. Show its actual old type/rate → new type/rate and say that this shared pair affects every calculated conversion column using it. Wait for a clear yes to that preview.
4. Only after that yes call `commit_set_currency_rate` with the preview's code, briefly repeating the change first. A changed request, expired code or failed commit requires a new preview and a new yes.
5. Read `get_currency_rates` again. Report the effective type, rate, provider and date, and any mismatch or failure.

## Meaning of bank and date

- A bank rate uses the workspace's effective provider and may use a fallback. It does not guarantee ECB unless the result actually says ECB.
- “Rate on the date” means the effective date returned by the bank rate. There is no historical-date selector in this workflow; do not invent one.
- A custom rate is one shared catalog pair, not a copy for one column. Changing it recalculates all conversion columns that use that pair without rewriting their source values.
- Do not change the workspace bank or currency, edit a calculated column directly, or fetch a rate from another source to work around a refusal.

## Stop conditions

Respect the connection's workspace, rights, allowed currencies, limits and refusals. Stop on missing access or an unavailable required tool; never switch to a browser or a different write method. A missing or null rate, bank or date is unknown, not zero or empty.
