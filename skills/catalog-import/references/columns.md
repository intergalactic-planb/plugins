# Columns, roles and currency

## Which columns to take (a new catalog)

- Give the columns the names the user wants.
- The columns of a catalog are limited by the tariff (get_connection_info: limits.perCatalog.fields). From a file with many fields take the main ones and show the set before the import. Keep the code or id that finds the products again: the next updates need it.
- Kinds: money for prices, number for counts (stock), list for links to pictures, text for the rest.

## Roles

The import gives the standard roles (name, price, code, description, pictures…) by the NAMES of the columns. When several columns fit one role, the import picks one by itself (a name like "id" beats "article"). So:

- name the main price plainly: "Price", "Цена";
- name the other prices without the word price or цена: "Wholesale", "Опт", "Закупка";
- make only one column look like the code, the name or another role;
- a price in another currency is the same price: after the import it gets the same name as that price (see below).

After the import read the columns with get_catalog. If the price, the code or the name has no role, or has it on another column, tell the user; after a yes, fix it with update_catalog.

## The key of an update

- The key is the column with the role vendorCode.
- The file has no such column: take the column with the role name, do not ask the user. Then say in the summary that the products are found by the exact name: rows whose name is written differently, or fits several products, are skipped.
- No column of the catalog has unique values: see references/matching.md.

## The currency of a new catalog

- The file clearly states one currency (руб, ₽, RUB, €, $… at the prices or in the header): give it as "currency".
- It states none: the currency of the workspace (get_connection_info, read it always).
- Several currencies, or a doubt (the prices look like another currency): ask which one is the currency of the catalog.
- The user names another one: take it.
- Before the yes tell the user the currency of the catalog, and when it is not the one of the workspace, both ("the catalog will be in RUB, the workspace is in EUR").
- Not every currency is allowed: a refusal lists the allowed ones. Tell the user and ask which allowed currency to use for the catalog; never convert the prices.
- If a source currency was refused, the allowed catalog currency is only technical for values outside it. For a product with no price in the chosen catalog currency, leave the main price empty and keep every source amount in its own money column of its own currency, as below. Never relabel a source amount with the allowed currency.

## The currency of a money column

- Excel and XML: a money column gets its currency from the file (the format of the cell, the attribute of the price).
- When you make XML and a source price is outside the catalog currency, including a source currency refused for the catalog, keep it in its own money column of its currency. Never drop, convert or relabel the value; omit the param only when that product has no such price.
- Such a column is the same price in another currency, so in the catalog it has the same name as that price: "Цена" in RUB and "Цена" in USD, "Оптовая" in RUB and "Оптовая" in USD. The currency next to the name tells them apart; do not write the currency into the name. A different kind of price (wholesale, purchase, recommended) keeps its own name in every currency. If it is not clear which price a source amount is, ask before the yes.
- The main currency is the one most prices of the file are in, even when the catalog has another, technical currency (a refused one). Its column keeps the plain name and the role: `<param name="Цена" unit="RUB">8490</param>` (or `<price>` when it is the catalog currency).
- The import joins columns of one name into one column, so only the other currency gets a temporary name, the price name and the currency: `<param name="Цена USD" unit="USD">95</param>`, mapped as `Цена USD → Цена USD → money`. After the import read get_catalog: the role stays on the column in the main currency (fix it with update_catalog if not), then rename the temporary column to the name of its price with update_catalog. This rename is a setting: it needs no extra yes. Read get_catalog again and check both currencies.
- When two columns have one name, the tools show them with the currency: "Цена (RUB)", "Цена (USD)" (a column in the catalog currency keeps the plain name). Use these labels in the tools; the real name is "Цена". To the user say «Цена» in USD, never «Цена (USD)» or «Цена USD», and do not rename again.
- Before the yes the table shows the final names and currencies, not the temporary name: «Цена» — RUB, «Цена» — USD. Tell the user plainly which products have a price only in another currency and where it goes: "all prices are in rubles; these products have a price only in dollars, so it is in a separate column «Цена» in USD".
- An update: such a price goes into the column of the same name in its currency (other tools show it as "Цена (USD)"). The catalog has none: add it before the import (create_field, then update_catalog for its currency and name) after the user's yes.
- CSV: it can not. If such a column is clearly in another currency, tell the user it will be in the currency of the catalog and ask what to do.
