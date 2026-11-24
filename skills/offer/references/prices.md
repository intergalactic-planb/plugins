# Prices, counts, discounts and markups

- Every price is a real amount in the proposal currency. `319.90` means 319.90; never convert it to or from cents.
- For a new proposal, read its currency from the workspace. Tell the user when a catalog price amount is copied into a different currency without conversion.
- To change the currency of an existing proposal, use a code from list_supported_currencies. Say first that the prices keep their numbers and are not converted (100 USD becomes 100 EUR), and wait for a clear yes. If the user wants converted prices, say that this is not available and that the prices have to be changed one by one.
- `count` is the number of units. A product price and `finalPrice` are per unit; `totalPrice` is for all units.
- Use the proposal and group `total` returned by the service. Do not add product totals yourself, especially for option groups.

## Discount

- `PERCENT`: percent off one unit. A value of 10 means 10%.
- `MONEY`: amount off one unit. A value of 10 means 10 currency units, not the final price.
- The web may show a money discount as an equivalent percent. If `finalPrice` is correct, the discount is correct. Do not retry it.
- A money discount can be at most the unit price. Use the new price if both change together. If the discount is too high, ask the user for a corrected amount and wait for their answer before retrying. If the product was already added without its discount, report that and read the existing line. If the requested quantity was also skipped, send it and the agreed discount together in one change to that existing line.

## Markup

- `PERCENT`: percent added to one unit.
- `MONEY`: amount added to one unit.
- Give either a discount or a markup for one change, never both.

## Safe changes

- If price and a money discount or markup change together, send both in the same requested product change; the service adapter writes them safely in order.
- A catalog product's price in the proposal can differ from the catalog. Changing it does not change the catalog.
- After creation or update, compare the returned `price`, discount or markup, `finalPrice`, `totalPrice` and proposal `total` with the user's request.
