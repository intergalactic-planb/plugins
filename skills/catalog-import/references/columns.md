# Columns, roles and currency

## Which columns to take (a new catalog)

- Give the columns the names the user wants; otherwise the names of the file, in its language. Never translate them: the examples here are in English only for the example.
- A cell with several properties goes to several columns, one property each (a split column is named by the property: power, voltage, material, in the language of the file).
- The columns of a catalog are limited by the tariff (get_connection_info: limits.perCatalog.fields). From a file with many fields take the main ones and show the set before the import. Keep the code or id that finds the products again: the next updates need it.
- Kinds: money for prices, number for counts (stock), list for links to pictures, text for the rest.
- A blank cell in a mapped money or number column of a CSV becomes 0. Before the yes, name the products and columns and explain which zeros come from empty cells and which were explicit in the file. If the price of a new product must remain unset, use XML and omit `<price>`.

## Roles

Set the agreed roles explicitly in the prepared XML of a new catalog, using references/sources.md. Existing catalogs keep their roles. When the source has several candidates for one role:

- name the main price plainly, in the language of the file ("Price" for an English file);
- name the other prices without the word "price" in any language: "Wholesale", "Purchase";
- make only one column look like the code, the name or another role;
- a price in another currency is the same price: after the import it gets the same name as that price (see below).

After the import read the columns with get_catalog. If the price, the code or the name has no role, or has it on another column, tell the user which column currently holds the role and which should get it. Ask separately and wait for a yes before changing a role with update_catalog.

## The key of an update

- Start with the column with the vendorCode role. If the file has no such column, try the column with the name role and tell the user the match is by exact name.
- Before writing, verify that the actual catalog key reaches only the approved products. If an article or name is shared by several products and only one is approved, use an existing unique catalog key for that product. If no such key is usable, leave the row out and report why. See references/matching.md.

## The currency of a new catalog

- The file clearly states one currency (₽, RUB, €, $, a currency word… at the prices or in the header): give it as "currency".
- It states none: the currency of the workspace (get_connection_info, read it always).
- Several currencies, or a doubt (the prices look like another currency): ask which one is the currency of the catalog.
- The user names another one: take it.
- Before the yes tell the user the currency of the catalog, and when it is not the one of the workspace, both ("the catalog will be in RUB, the workspace is in EUR").
- Not every currency is allowed. If the requested catalog currency is refused, tell the user, list the choices in the refusal, and ask which to use. Wait for the answer and then show the revised table for a separate yes. Do not choose the workspace currency or start an import while waiting. If the list is unavailable, explain that the catalog cannot be created now.
- The requested catalog currency can differ from the prices in the source. A chosen catalog currency is technical for amounts outside it. Use an explicit field definition for the main price, just as for any other price column. Keep amounts and supported source currencies as below. Change an unsupported currency only after the user has chosen its replacement.

## Check currencies before creating a catalog or price column

- Read supportedCurrencies from get_connection_info. Use this list for the new catalog and every new price column, including columns from native exports. Compare the source currencies after normalizing aliases such as RUR to RUB, BYB or BYR to BYN, and RMB to CNY.
- If a new price column has an unsupported currency, name the column and its source currency. Ask which supported currency to use and say that the numbers in the column will stay the same. Wait for the choice before starting the import or creating the column. Do not assume the catalog or workspace currency is the choice. If the list is empty, tell the user the service has no configured currencies and wait.
- After the choice prepare the XML with the agreed currency in the field definition. Keep the numbers unchanged. Show the affected columns, source and replacement currencies, and the same amounts. Wait for a separate yes before writing.
- Do not look up currency pairs or recalculate amounts for this replacement.
- When adding a money column by hand in an existing catalog, its initial currency is the catalog currency. If the catalog currency is unavailable, explain that the column cannot be added this way and wait.

## The currency of a money column

- Read the source currency from its header, cell format, or XML markers. Ask if it is unclear. Prepare XML with the chosen currency in the definition of each money field (references/sources.md). Use a separate field for each kind of price and currency. Put numbers in the offers and leave out missing prices.
- Keep names in the language of the file. A price in another currency can have the same final name, for example "Price" in EUR and "Price" in CHF. A different kind of price keeps its own name, for example "Wholesale". If the price kind or main currency is unclear, ask before the yes.
- Keep the plain name for the main source currency of a price kind. Give an additional currency of that same price a temporary distinct name, for example "Price CHF". Use a separate field ID and an explicit currency. After success rename it to the final name agreed in the preview, then check the amounts and currencies. Ask separately before correcting a role.
- When two columns have one name, the tools show them with the currency: "Price (RUB)", "Price (USD)" (a column in the catalog currency keeps the plain name). Use these labels in the tools; the real name is "Price". To the user say "Price" in USD, never "Price (USD)" or "Price USD", and do not rename again.
- Before the yes the table shows the final names and currencies, not the temporary name: "Price" — RUB, "Price" — USD. Tell the user plainly which products have a price only in another currency and where it goes: "all prices are in rubles; these products have a price only in dollars, so it is in a separate column 'Price' in USD".
- An update: such a price goes into the column of the same name in its currency (other tools show it as "Price (USD)"). The catalog has none: add it before the import (create_field, then update_catalog for its currency and name) after the user's yes.
- Import prices from CSV, Excel, XML, attachments or links through prepared XML. A price update must use the same currency as its destination column. If they differ, resolve the destination or the replacement with the user before the preview.
