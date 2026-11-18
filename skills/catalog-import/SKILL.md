---
name: catalog-import
description: Import products into a catalog from a file. A new catalog from Excel, CSV, XML, PDF, a site or a link, or an update of a catalog that is there (prices, stock, new products, sections). Use when the user gives a price list or a file of a supplier and wants it in a catalog ("import this price list", "make a catalog from this file", "update the prices from this file", "upload the price list").
---

# Import a file into a catalog

Use only the tools of the catalog service (its MCP connection). Never open the service in a browser. Call the product by the name the user uses.
To the user, speak in business words: catalog, product, column, price. Never name tools, ids or fields. Never show raw errors: say what happened and what the user can do.

The answers of the tools have the exact rules ("next", "guide", a refusal with a reason). Do what they say. This skill gives the order of the steps; the files in references/ give the details. Read a reference when its case comes.

## 1. Choose the way

- A new catalog: go to section 2.
- A catalog that is there: go to section 3.
- Not clear which catalog: read the list of catalogs and ask the user.
- A PDF, a page of a site, a text, several files, a messy Excel, a cell with several properties, or a link: read references/sources.md first.

## 2. A new catalog

1. Read the file yourself: every sheet, the columns, the first rows, the number of rows.
   A cell with several properties ("220 V, 1.1 kW, cast iron", "Red / XL / cotton"): split it into columns, one property each, and import the table you made (references/sources.md). A product without one of the properties leaves that cell empty; never guess it.
2. Call get_connection_info: the currency of the workspace and the limits of the tariff.
3. Show the user a table: column of the file → column of the catalog → type (text, money, number, list). Column names stay in the language of the file. Pictures: a site or a feed with pictures gives their links (all of them, full https addresses, as the page or the file shows them); take them as one column of the kind list, and say in the table how many pictures the products have. The import attaches them to the products when the tariff allows; the answer of the import says if they stay links — then tell the user. Name the currency of the catalog; if some products have a price only in another currency, name them and say it goes to a separate column of the same name in that currency (the table shows the final names and currencies). Wait for "yes", even if the user already said "create". A change from the user: show a new table.
   How to name the columns and choose the currency: references/columns.md.
4. Call import_to_new_catalog. Run the command from its answer; do not ask again.
   The currency is refused (any source: a file, a site, a link, a PDF): never offer to relabel the prices with another currency or to convert them. Ask which allowed currency is only the technical currency of the catalog; the prices stay in their own currency in a money column named as the price, in XML (references/columns.md). Show the table again and wait for a yes.
5. Call import_status once. Not finished: tell the user the import still runs.
6. Call get_catalog and check the roles of the name, the price and the code. A role is missing or on a wrong column: tell the user; after "yes", fix it with update_catalog. A price column in another currency under a temporary name: rename it to the name of its price (references/columns.md).

## 3. An update of a catalog

1. Call get_catalog: columns, types, roles. Choose the key column: references/columns.md.
2. The file has a column that the catalog has not, and the user wants it: make it first with create_field.
3. Call get_catalog_snapshot with only the columns you need to match and to change.
4. Match the file with the snapshot by references/matching.md: with a script, not in the chat. A matching key makes candidates, not a final pair; compare the other available data before accepting one. For a supplied XML with sections, propose placing products as in the file. Keep existing sections when the user asks to update only prices, stock or other values, or when you made a flat XML from a PDF. Before preparing the file for section changes, read the section checks in references/sources.md.
5. Tell the user what you found: which products change and from what to what, exact pairs, doubtful pairs, new rows, skipped rows, problems of the file. Give the user the CSV of the pairs. List every row used for several products and every key repeated in the file; say what differs or which row you chose, and whether the user confirmed it. Ask about every unconfirmed choice and leave its products out until the user answers.
6. Call preview_import with matchReport. If its answer has "next", do it before anything else.
7. Tell the user the summary, and that it cannot be undone. Wait for a clear "yes".
8. Call commit_import with the code. Run the command. Call import_status once. After success, call get_catalog_snapshot with the key and the changed columns and check every affected row against the summary. Tell the user about any difference; do not try to correct it without a new preview and yes.

- Before asking whether to add new rows, say where each will go. If keeping sections, new products go to the root. If moving them as in the file, use their file section or the root if they have none. If adding memberships, name the existing sections they will join and any assignments that will be skipped. Add new rows only after the user says yes to adding them and only if they fit the tariff limit. Then show the final preview and ask for a yes to it.
- Sections change only from XML in the way shown in the approved preview. Moving removes all previous memberships. Adding memberships keeps the old ones and skips assignments to missing sections. Before the yes, name the skipped paths and the affected products.
- Apply a file always with the import, even when few products change.

## 4. How the file gets to the service

- You can run commands with network: the command from the answer of the tool (the default).
- The file is attached to the chat: give it in file.
- An XML or CSV you made, or a small file and no network: fileText.
- A link the user gave: fileUrl.

## 5. When it fails

- The import failed: tell the user in simple words what went wrong, and stop. Do not fill the catalog another way (columns and products one by one): the user said yes to this import, not to that.
- The limit of the tariff is reached: do not try again; tell the user.

## Never

- Never change data without a clear "yes" of the user to the summary.
- Never multiply or divide prices: the tools give real amounts.
- Never make up a value. An empty text cell stays empty. An empty money or number cell of a CSV becomes 0 in the catalog: tell the user "0", never "empty". To keep a price empty, make XML and omit `<price>` (references/sources.md).
