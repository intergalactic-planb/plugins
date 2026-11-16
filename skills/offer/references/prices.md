# Prices, counts, discounts and markups

- Every price is a real amount in the proposal currency. `319.90` means 319.90; never convert it to or from cents.
- `count` is the number of units. A product price and `finalPrice` are per unit; `totalPrice` is for all units.
- Use the proposal and group `total` returned by the service. Do not add product totals yourself, especially for option groups.

## Discount

- `PERCENT`: percent off one unit. A value of 10 means 10%.
- `MONEY`: amount off one unit. A value of 10 means 10 currency units, not the final price.
- The web may show a money discount as an equivalent percent. If `finalPrice` is correct, the discount is correct. Do not retry it.
- An amount off greater than the unit price is invalid. Do not replace it with another number.

## Markup

- `PERCENT`: percent added to one unit.
- `MONEY`: amount added to one unit.
- Give either a discount or a markup for one change, never both.

## Safe changes

- If price and a money discount or markup change together, send both in the same requested product change; the service adapter writes them safely in order.
- A catalog product's price in the proposal can differ from the catalog. Changing it does not change the catalog.
- After creation or update, compare the returned `price`, discount or markup, `finalPrice`, `totalPrice` and proposal `total` with the user's request.
