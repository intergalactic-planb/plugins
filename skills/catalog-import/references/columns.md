# Columns, roles and currency

## Which columns to take (a new catalog)

- Give the columns the names the user wants.
- The columns of a catalog are limited by the tariff (get_connection_info: limits.perCatalog.fields). From a file with many fields take the main ones and show the set before the import. Keep the code or id that finds the products again: the next updates need it.
- Kinds: money for prices, number for counts (stock), list for links to pictures, text for the rest.

## Roles

The import gives the standard roles (name, price, code, description, pictures…) by the NAMES of the columns. When several columns fit one role, the import picks one by itself (a name like "id" beats "article"). So:

- name the main price plainly: "Price", "Цена";
- name the other prices without the word price or цена: "Wholesale", "Опт", "Закупка";
- make only one column look like the code, the name or another role.

After the import read the columns with get_catalog. If the price, the code or the name has no role, or has it on another column, tell the user; after a yes, fix it with update_catalog.

## The key of an update

- The key is the column with the role vendorCode.
- The file has no such column: take the column with the role name, do not ask the user. Then say in the summary that the products are found by the exact name: rows whose name is written differently, or fits several products, are skipped.
- No column of the catalog has unique values: see references/matching.md.

## The currency of a new catalog

- The file clearly states one currency (руб, ₽, RUB, €, $… at the prices or in the header): give it as "currency".
- It states none: the currency of the workspace (get_connection_info, read it always).
- Several currencies, or a doubt (the prices look like another currency): ask the user.
- The user names another one: take it.
- Before the yes tell the user the currency of the catalog, and when it is not the one of the workspace, both ("the catalog will be in RUB, the workspace is in EUR").
- Not every currency is allowed: a refusal lists the allowed ones. Tell the user and ask; never convert the prices.

## The currency of a money column

- Excel and XML: a money column gets its currency from the file (the format of the cell, the attribute of the price).
- CSV: it can not. If such a column is clearly in another currency, tell the user it will be in the currency of the catalog and ask what to do.
