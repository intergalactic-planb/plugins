# Groups, options and products

## Catalog and custom products

- A catalog product keeps its catalog identity. Find it first; adding it to a proposal does not change the catalog.
- When the same name or article exists in several catalogs, use the catalog named by the user. If none was named, ask which product they mean.
- A custom product lives only in the proposal. Use the name and unit price the user gives. Ask for either if missing before adding the line.
- A bad image link does not remove a product: report that the product was added without the picture.
- A picture the user attached to the chat is added to a product with add_offer_product_image, only when the file is really attached. If the assistant has no attached file, ask for a link to the picture and use the link instead. Add to the product the user names, after reading the proposal.
- To remove a picture, read the proposal first: the pictures of a product have ids there. Remove only the picture the user points at, by delete_offer_product_image, and say that it cannot be brought back. The order of pictures cannot be changed.
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
- To make a group of options, create or change a group with type `ALTERNATIVE`. To choose the variant, set `selectedItemId` to the product the user named (the main product of a set chooses the whole set). Tell the user which variant counts in the total.

## Sets

- Products with the same `bundleId` are one set; the first of them (`mainOfSet`) is its main product. A set is chosen and counted as a whole in a group of options.
- To take a set apart, give its `bundleId` in `disbandBundles`: the products stay in the offer as separate ones.
- To delete a set with all its products, give its `bundleId` in `removeBundles`. It cannot be undone: tell the user the main product and how many products go, and wait for a clear yes.
- A set to delete cannot have its products changed, removed or in a group to delete in the same call.
- To build a set, give the main product and the products to add in `makeSets`; they go right after the main one in the order given. A product that is the main one of another set brings its whole set. To change the main product of a set, give the product in `makeMain` (it must be in a set and not be the main one).
- After building or changing a set, read the offer again and check the order and the total.

## Order

- To move a product to another group or to a place in its group, use `moveProducts` with the group and a place counted from 1 among the other products of that group. The main product of a set moves the whole set; any other product of a set leaves its set: say so before moving it. Place 1 in a group of options makes that variant (or set) the main one.
- To change the order of groups, use `moveGroups` with the place among the groups, counted from 1; it is done after the other changes.
- Do not move a product in the same call in which it is changed, removed, or put in a set. After moving, read the offer again and check the order.

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

- Delete only a group, set or product the user named. Use `removeProducts` for products, `removeBundles` for sets and `removeGroups` for groups.
- Deleting a group deletes all its products too, and it cannot be undone. Before it, tell the user the group name and how many products it holds, and wait for a clear yes.
- A group to delete cannot get products or changes in the same call. To add products and delete groups together, give `groupId` of a group that stays.
- After the change, read the returned proposal and report what is gone and the new total.
