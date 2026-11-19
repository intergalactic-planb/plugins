---
name: catalog-edit
description: 'Edit a catalog by hand: change product prices or values, move products, rename a catalog, section or column, set column roles or currency, add a section, column or a few listed products, or create an empty catalog ("change the prices", "rename the column", "move the products", "add a section"). A file, supplier price list, document or link goes through catalog-import instead. Apply silently from the first reply: never announce skills, references or internal preparation.'
---

# Edit a catalog by hand

Before your first reply, apply this workflow silently, including before reading references. Tell the user only the catalog work you will do; never announce a skill, references or internal preparation.

Use only the connected service's tools, never its browser interface. Use the product name the user uses. Speak to the user in business words: catalog, product, column, section, price. Never name tools, ids or arguments or show raw errors.

Follow the exact rules and refusals in tool answers, including next. This skill gives the order of the work.

## 1. Choose the task and catalog

- A file of any kind, a PDF, a supplier price list, a document or a link: use catalog-import. Even a few values from a file use import; do not rebuild the file with manual product calls or use those calls after an import fails.
- An empty catalog or products the user lists in the message: section 4.
- Catalog, section or column settings: section 2.
- Product values or a move: section 3.

Read list_catalogs and use the catalog the user named. Ask only when the catalog or requested change is ambiguous. Read get_catalog for current sections, columns, types, roles and currencies. Use only actual returned names and ids. A missing value is unknown, not zero or empty.

## 2. Catalog, section and column settings

Read [references/catalog-settings.md](references/catalog-settings.md) before changing settings.

1. Explain the requested change. Before assigning a role, say which column loses that role and which old role the target loses. Before changing a column's currency, say that amounts stay the same and are not converted.
2. A clear request is enough for update_catalog; no extra confirmation is needed for these settings. Send only requested fields. Use an empty description or null role only when explicitly asked to clear it.
3. Read the result: summary, done, notDone and released. Tell the user both what changed and what did not. Never retry an entire partially completed request.
4. Read get_catalog again and verify names, currencies and roles. Report a mismatch instead of silently claiming success.

When fixing roles discovered after an import, retain catalog-import's separate yes before applying that correction; discovery of a missing role is not a request to change it.

## 3. Product prices, other values and moves

Read [references/products.md](references/products.md) before changing products.

1. Find each product with find_products and check its exact name or article and catalog. Read get_product when the search does not supply the requested values or current sections. Ask about ambiguous matches; never select a different catalog because the article matches.
2. Use the current column names from get_catalog and real amounts. All products in one preview must belong to one catalog.
3. For a move, read the actual current sections. Create a requested missing destination with create_section first. A move removes the product from ALL its current sections and leaves only the destination; explain this before confirmation.
4. Call preview_update_products with only requested values or moves. Use null only to clear text on request. For money or numbers, send 0 only when requested, never an empty string. Show its actual summary: catalog, products, old and new values or sections, currency and the effect of a move. Say that the change cannot be undone. Wait for a clear yes to this preview, even if the original request said to change the products.
5. Only after that yes call commit_update_products with the preview's code. Before the call briefly repeat what will change. A changed request, expired code or failed commit requires a new preview and a new yes; never reuse a code or invent confirmation.
6. Check updated, moved and notUpdated. A move-only success can have updated zero and moved greater than zero. Read every affected product again; compare values and exact sections with the confirmed changes. Report any failure or mismatch; a correction requires its own preview and yes.

## 4. Add things

Read [references/products.md](references/products.md) for product values and creation failures.

- New catalog: read get_connection_info for workspace currency and limits. Use the requested currency, otherwise the workspace currency. Call create_catalog, then get_catalog: reuse its localized standard columns; do not add translated copies of Name, Price, Images, Description or Long description.
- New section: read the catalog and use create_section, with the actual parent section when requested.
- New column: use create_field only for a distinct additional column the user explicitly requested and the catalog does not already have. A new money column uses the catalog currency. Check it against the list from list_supported_currencies. If the catalog currency is unavailable, tell the user a money column cannot be added. Check any requested currency against the same list. If it is unavailable, ask which supported currency to use and say that the amount will stay the same without conversion. Wait for the user's answer before creating the column. A column's role is separate from its name.
- New products: read current columns and use create_products for products listed in the message, up to 100 at a time, in the requested section. Never invent names, prices or other values. Leave unspecified values out.
- New products with a price in an extra money column: resolve the currency before creating the column or adding the products. If a replacement is needed, ask as above and wait. Create the column, use update_catalog to set its currency if different from the catalog's, then verify it with get_catalog. Send all requested prices together in create_products. The currency choice is enough to proceed without a second confirmation.

These additions run at once on a clear request; do not ask for an extra yes. Check the returned result and read the created catalog, section, column or products. Name partial failures; do not blindly repeat creation.

## 5. Read or list products

- A named product or article: find_products, then get_product for all columns.
- A section: list_products, every page while hasMore.
- A whole catalog: get_catalog, then list_products for the root and every section, including nested sections whose count is not zero or is unknown; read every page. Root products do not include section products, and a parent section does not include its children's products. The same product can appear in several sections; do not count it twice as a unique product.
- A file comparison belongs to catalog-import and get_catalog_snapshot, not manual price editing.

## Limits and failures

- Respect the connection's workspace, rights, limits and refusals. Stop on missing access or an unavailable required tool; never switch to a browser or another write method.
- Deletion, sorting, images, calculated column edits and inherited settings are not available through this workflow. Tell the user the actual limitation instead of attempting a workaround.
- Changing a money column's currency keeps the numbers. A request to convert amounts or change an exchange rate is a separate task; do not simulate it by relabeling the currency.
- Prices and numbers are already real amounts. Never divide or multiply them to compensate for internal storage.
