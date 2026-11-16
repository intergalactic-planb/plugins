# Groups, options and products

## Catalog and custom products

- A catalog product keeps its catalog identity. Find it first; adding it to a proposal does not change the catalog.
- When the same name or article exists in several catalogs, use the catalog named by the user. If none was named, ask which product they mean.
- A custom product lives only in the proposal. It needs a name and a price from the user.
- A bad image link does not remove a product: report that the product was added without the picture.
- A failed product does not cancel the other products. Report every item in `notAdded` with its reason.

## Groups

- One ordinary list: use one unnamed group, or name it only when the user gives a name.
- Several sections, rooms or stages: create ordered `groups`.
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
