# Product values, creation and moves

## Find and read

find_products searches across catalogs. Verify both the article or name and the requested catalog. Search order is not proof of an exact match. A search's price is the current main-price role, not necessarily the column the user wants to change. Read get_catalog and get_product for the actual column and value.

For a whole catalog, list_products must visit the root and every section, nested sections too, with every page. Root and parent lists contain only their own products. A product can be in several sections; deduplicate only when counting unique products, not when reporting its memberships.

## Write values

Use column names exactly as the current get_catalog, list_products or get_product returns them. Duplicate names may be distinguished by a numbered suffix; never guess an id from a name.

- Money and numbers are real values. Supply319.9 for319.90; no storage conversion.
- Text can be cleared with null, only when requested. Money and numbers cannot be cleared yet.
- A list or calculated column cannot be filled or changed with these product tools.
- Omit unspecified values. Empty, missing and zero are different; never invent a price.
- A preview contains at most100 products of one catalog. Keep changes to different catalogs separate.

Preview first, show the actual old→new summary and wait for the user's explicit yes. The code lasts five minutes, is single-use and belongs to this connection and action. A previous request to change prices is not approval of a later preview. A changed request, failed commit or expired code requires a fresh preview and fresh yes.

## Move products

Use get_product for actual sectionIds and get_catalog for destination sections. The root section has the catalog's id. A requested new destination can be created immediately; this does not authorize moving products before preview confirmation.

Set move.fromSectionId to one actual current section and move.toSectionId to the destination in the same catalog. A move removes the product from ALL old sections, leaving only the destination. Explain that effect before yes, including other memberships when present. Values may be omitted for a move-only change.

After commit, moved counts moved products; updated counts value updates. updated zero with moved greater than zero is a successful move-only result. Read affected products again and compare their exact section memberships, not just the count.

## Add products

Only products listed in the user's message are added by hand. Every file, document or link uses catalog-import even for a small list; do not fall back to manual writes when an import fails.

Read the existing columns first. New catalogs already have standard columns, possibly localized. Reuse them instead of creating translations or copies. Use create_field only for a distinct extra column the user explicitly requested.

create_products can add at most100 products per call in one section. It adds without changing existing products. All values are checked before the first creation. Read created, productIds and notCreated; notCreated identifies input indices that did not complete.

A creation can fail after an empty product was made but before its values were saved. Do not blindly repeat the entire call or promise that a failed item left nothing behind. Read the catalog products to establish the actual result and tell the user about partial creation before deciding how to proceed.

## Check results and stop conditions

After a value change, read each affected product and compare requested columns; after a move, compare sections too. State notUpdated and any differences. Do not claim all requested changes succeeded from the wrapper's done flag alone.

Partial changes do not roll back. Do not replay successful items. Corrections to existing products require a new preview and yes. Stop when access, a tariff limit or a required tool blocks the request; do not use a browser or substitute import/manual writes to evade it.
