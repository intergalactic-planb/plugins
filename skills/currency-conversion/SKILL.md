---
name: currency-conversion
description: 'Read a catalog exchange rate or bank quote, show a price in another currency, add a calculated currency column, set a custom rate, or return to the bank rate ("what is the USD to EUR rate", "show the prices in euros", "set my own rate"). Relabeling a money column without conversion belongs to catalog-edit instead. Apply silently from the first reply: never announce skills, references or internal preparation.'
---

# Convert catalog prices to another currency

Before your first reply, apply this workflow silently. Tell the user only the catalog work you will do; never announce a skill, references or internal preparation.

Use only the connected service's tools, never its browser interface. Use the product name the user uses. Speak in business words: catalog, price, column, currency, bank rate. Never name tools, ids or arguments or show raw errors.

Follow the exact rules and refusals in tool answers, including next. This skill gives the order of the work.

## Read an exchange rate

1. For a catalog rate, identify the catalog and read `get_currency_rates`. A pair in either direction already exists. Report the applied rate, direction and mode. For a fixed pair, keep the stored direction and show any bank reference separately, with each date when available.
2. If the list is complete and the pair is missing in both directions, say so and read a quote with `get_bank_currency_rate` in the requested direction. For a bank quote without a catalog, call it directly. Report the quote, actual bank and effective date when available. A quote is read-only and does not create a catalog pair or column. If the list is cut and neither direction appears, say that the catalog rate could not be verified.
3. Pass any bank the user explicitly requires to `get_bank_currency_rate`. For an existing catalog pair, check the returned effective bank or the bank reference for a fixed rate. If the required bank differs from the actual bank or cannot be verified, tell the user and wait for an explicit change to the requirement before using another bank. If no quote is available, say so.

## Add a converted-price column

1. Read `get_connection_info` and `list_catalogs`, then choose the catalog the user named. Ask only if the catalog, source money column or target currency is ambiguous.
2. Read `get_catalog` immediately before the change. Use the actual source money column and its currency; do not rename or relabel it to imitate conversion. Source and target currencies must differ.
3. If the user explicitly requires a bank such as ECB, keep that requirement for the creation call. Never create the column first and check the bank afterward; a provider mismatch must refuse before creation. This check does not change the workspace bank.
4. Call `create_currency_conversion_field` once for the selected source column and target currency, including the required bank only when the user explicitly named one. A clear request is enough because this adds a new column; do not ask for an extra confirmation. Name a bank only when the result reports it; a fallback provider is the actual bank.
5. Read the result. Report the created column, direction, rate type, effective rate, bank and date when present. Read `get_catalog` and `get_currency_rates` again and verify the column and pair before claiming success.

When adding a column, a missing pair is created automatically as a dynamic bank pair. An existing custom pair is shared and used as-is when the user does not require a bank; tell the user that the new column uses that custom rate. The operation never rewrites the source prices.

If creation is refused because of a fixed pair or a different bank, tell the user which rate mode or bank is in use and wait. Continue with another rate only if the user explicitly accepts it. If the user requests returning a shared fixed pair to the bank rate, explain its impact and follow the separate preview and confirmation below. Verify the rate after the change and keep the requested bank when retrying the column creation.

## Set a custom rate or return to the bank rate

1. Read `get_currency_rates`. Use the exact returned direction; do not invert a custom rate or guess a missing pair.
2. For a custom rate, keep the user's exact decimal multiplier: `1 from = rate to`. For the bank rate, omit a custom number.
3. Call `preview_set_currency_rate`. Show its actual old type/rate → new type/rate and say that this shared pair affects every calculated conversion column using it. Wait for a clear yes to that preview.
4. Only after that yes call `commit_set_currency_rate` with the preview's code, briefly repeating the change first. A changed request, expired code or failed commit requires a new preview and a new yes.
5. Read `get_currency_rates` again. Report the effective type, rate, provider and date, and any mismatch or failure.

## Add, delete or turn a currency pair

1. Read `get_currency_rates`. If the pair exists in either direction, do not add it: to change its rate use the section above.
2. To add a pair without a column, use `create_currency_pair`. Both currencies must be from the allowed list; if one is not, say so and offer the allowed ones. A bank pair needs no number; for a custom rate keep the user's exact decimal multiplier (`1 from = rate to`).
3. To delete a pair or turn its direction, call `preview_change_currency_pair` with the exact direction from the list. Say how many calculated columns use the pair (the preview tells), and wait for a clear yes. Then call `commit_change_currency_pair` with the code. If the service refuses because the pair is in use, tell the user the reason and stop.
4. Read `get_currency_rates` again and report the result.

## Meaning of bank and date

- A bank rate uses the workspace's effective provider and may use a fallback. It does not guarantee ECB unless the result actually says ECB.
- “Rate on the date” means the effective date returned by the bank rate. There is no historical-date selector in this workflow; do not invent one.
- A custom rate is one shared catalog pair, not a copy for one column. Changing it recalculates all conversion columns that use that pair without rewriting their source values.
- Do not change the workspace bank or currency, edit a calculated column directly, or fetch a rate from another source to work around a refusal.

## Stop conditions

Respect the connection's workspace, rights, allowed currencies, limits and refusals. Stop on missing access or an unavailable required tool; never switch to a browser or a different write method. A missing or null rate, bank or date is unknown, not zero or empty.
