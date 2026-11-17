# Catalog, section and column settings

Use get_catalog immediately before preparing settings changes. update_catalog acts at once on a clear request, without a confirmation code. Product values and moving products are separate and require preview_update_products and commit_update_products.

## What can change

- Catalog: name, short description and full description. An empty description clears it; never send an empty value unless the user asks to clear it. Catalog currency is not editable here.
- Section: its name, using its id from this catalog. The root is the catalog itself; rename it through the catalog's name.
- Column: its name throughout the catalog, currency for a money column, and role. Two columns cannot have the same name. Renaming changes neither type nor values.

## Roles

A role identifies a column's purpose independently of its displayed name. One role belongs to one column; a column has one role.

| Column type | Allowed roles |
| --- | --- |
| text | name, description, longDescription, vendorCode, url, availability |
| money | price, purchase, target, mrp, cost |
| list | images |
| number | none |

Before assigning a role, read both the current holder and the target's existing role. Tell the user both losses before the call. For example, making Wholesale the main price removes the main-price role from Price and the purchase-price role from Wholesale. Do not ask for an extra yes when the user clearly requested this assignment.

Use role null only when the user asks to remove a role. Do not change a role just because a column was renamed. For import-discovered role corrections, follow catalog-import's requirement to ask first.

## Currency

A money column's currency is a label for its existing amounts. Changing EUR to USD leaves 100 as 100, without conversion. Say this BEFORE the call. Do not relabel currency to satisfy a request to convert prices. Use only an allowed currency; a refusal supplies the available choices.

## Restrictions and partial changes

The server refuses inherited catalogs, parent columns, calculated columns, invalid names, incompatible roles and busy catalogs. Preserve the refusal; do not route around it.

The server checks the whole request before writing, but the subsequent service writes are not one transaction. If a later step fails, done lists completed changes and notDone lists the remainder. Tell the user both. released lists roles removed from columns. Never claim that everything failed or succeeded based only on updated.

After a success or partial success, read get_catalog again. Verify the actual names, currencies and roles. Do not repeat completed steps; resolve a mismatch or retry only unfinished work under the user's existing requested scope.
