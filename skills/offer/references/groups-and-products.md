# Groups, options and products

## Catalog and custom products

- A catalog product keeps its catalog identity. Find it first; adding it to a proposal does not change the catalog.
- When the same name or article exists in several catalogs, use the catalog named by the user. If none was named, ask which product they mean.
- A custom product lives only in the proposal. Use the name and unit price the user gives. Ask for either if missing before adding the line.
- A bad image link does not remove a product: report that the product was added without the picture.
- A failed product does not cancel the other products. Report every item in `notAdded` with its reason.

## Groups

- One ordinary list: use one unnamed group, or name it only when the user gives a name.
- Several sections, rooms or stages: create ordered `groups`.
- Use a customer-visible group comment only when the user provides it.
- Do not send both `groups` and the top-level products or group name.
- Up to 20 groups and 50 products may be sent in one call. Do not silently drop the rest.

## Options

- `GENERAL`: all products count in the group total.
- `ALTERNATIVE`: the products are variants. Only the selected variant and its set count; when none is selected, the service uses its own default.
- Change `selectedItemId` only to a product of that same group.
- Use the total returned by the proposal. Products with `inTotal: false` do not count.

## Changes

- Read the proposal immediately before a change. Product and group ids are valid only inside that proposal.
- A new group is added after existing groups.
- New products go to the group the user named. Without one, use the first ordinary group, not an options group.
- An empty name or comment clears it. Omit a field to preserve it.
- Adding an image keeps existing images; it does not replace them.

## Outdated products

- `get_offer` marks a product with `status`: `OUTDATED` means its catalog product changed after it was added; `ORPHAN` means it is gone from the catalog; `INDEPENDENT` is a custom product. No status means no information, not "up to date".
- To bring outdated products up to date, list them by name with their current price and ask for a clear yes: the catalog data overwrites their name, price and description, including changes made in the proposal. Then use `actualizeProducts` with those ids only.
- A product that is `ORPHAN` or `INDEPENDENT` cannot be updated; say so. Do not update and change the same product in one call.
- After the update, read the returned proposal and report the changed prices and the new total.

## Deleting

- Delete only a group or product the user named. Use `removeProducts` for products and `removeGroups` for groups.
- Deleting a group deletes all its products too, and it cannot be undone. Before it, tell the user the group name and how many products it holds, and wait for a clear yes.
- A group to delete cannot get products or changes in the same call. To add products and delete groups together, give `groupId` of a group that stays.
- After the change, read the returned proposal and report what is gone and the new total.
