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

- Pictures of products: when the user gives links to pictures for products, find the products and call add_product_images. A picture is added to the existing ones. When the user attached a picture file to the chat, call upload_product_image with it instead (only when a file is really attached; otherwise ask for a link). To take a picture off a product, read the product first: its pictures have ids there; delete only the one the user points at with delete_product_image and say that it cannot be brought back. Reordering is not available. A link that is not a picture, is bigger than 10 MB, or cannot be downloaded is skipped: name each such link and its reason. If the catalog has no column for pictures, say so and ask which list column should hold them; it needs the role images (update_catalog) first.

These additions run at once on a clear request; do not ask for an extra yes. Check the returned result and read the created catalog, section, column or products. Name partial failures; do not blindly repeat creation.

## 5. Read or list products

- A named product or article: find_products, then get_product for all columns.
- A section: list_products, every page while hasMore.
- A whole catalog: get_catalog, then list_products for the root and every section, including nested sections whose count is not zero or is unknown; read every page. Root products do not include section products, and a parent section does not include its children's products. The same product can appear in several sections; do not count it twice as a unique product.
- A file comparison belongs to catalog-import and get_catalog_snapshot, not manual price editing.

## Limits and failures

- Respect the connection's workspace, rights, limits and refusals. Stop on missing access or an unavailable required tool; never switch to a browser or another write method.
- Copies and sections of products: to copy products, find them first and call duplicate_products with the target catalog and section (the section is optional: the top level is used). It makes independent copies; the originals stay. To copy all the products of a section, give the source catalog and section instead of product ids; the server does not say how many were copied, so read the target section before saying a number and never repeat the call (it copies again). To show one product in several sections, or to take it out of some, read its current sections (get_product), build the FULL list of sections it must be in and call set_product_sections. A section left out of the list loses the product (the product itself is not deleted). Moving to one other section is a normal move through preview_update_products.
- Deleting products: only when the user clearly asks. Find the products first, then preview the deletion. Tell the user how many products go (and which ones, for a few) and that a deleted product leaves every section it was in and cannot be brought back from here; wait for a clear yes to exactly that. For all the products of a section, the number comes from the preview: say it. After the confirmation, read the products or the section again and report what was deleted and what was not. Never repeat the deletion on your own.
- Sections: to move a section into another section, to the top level or to another place among its neighbours, read the sections (get_catalog) and call move_section with the place counted from 1. A section cannot be moved into itself or its own subsection. To copy a section with its subsections and products into another section or catalog, call copy_section; the copy is independent and the original stays. A repeated call copies again: never repeat it; read the target and report what came.
- Formula columns (a markup, a discount, a price with a margin): a formula is a money column calculated from another money column with a modifier. Read the columns first: calculated columns show their base column and modifier. To add one, call create_formula_field with the base column, the kind (percent, amount or multiplier) and the number as the user said it (a discount is a negative percent). To change one, call update_formula_field with the new kind and number, or reset to take the modifier of the section above. Say before changing that the prices of every product of the section and its subsections change at once. The base column never changes. A currency conversion column is changed with the rate tools, not here.
- The card of a product (get_product) shows the modifier of the product in each formula column: its own, or "section" when it takes the one of the section. Read it before changing a modifier of one product.
- One product with its own price or markup in a formula column: set_product_modifier with the section the product is shown in (percent, amount, multiplier, or exact for a final price); clear returns the product to the modifier of the column.
- To make a text or number column money (change_field_type), say first that values that are not a number may be lost and that the column loses its role, and wait for a clear yes. After it, tell the user the lost role and offer to set it again.
- Deleting a catalog, a section or a column: only when the user clearly asks, with the exact name. Preview first, then say what goes: for a catalog how many products, for a section how many products and subsections, for a column that its values in all the products go and which calculated columns are based on it. Say that it cannot be undone from here, wait for a clear yes to exactly that, then commit. A catalog taken from another company and the top level of a catalog are not deleted. After the commit read the catalogs or the columns again and report. Never repeat a deletion on your own.
- Showing or hiding a column: preview_set_column_visibility with true to show and false to hide; a hidden column has "hidden" in the columns. It only changes how the table looks for the user, no data is lost; still preview, say which column and that, wait for a yes. A column that is already so is refused: tell the user.
- Sorting, deleting or reordering pictures, the order and width of columns, and inherited settings are not available through this workflow. Tell the user the actual limitation instead of attempting a workaround.
- Changing a money column's currency keeps the numbers. A request to convert amounts or change an exchange rate is a separate task; do not simulate it by relabeling the currency.
- Prices and numbers are already real amounts. Never divide or multiply them to compensate for internal storage.
