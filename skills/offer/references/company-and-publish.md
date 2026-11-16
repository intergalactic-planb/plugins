# Company terms and publication

## Company, payment and delivery

- `get_company_details` gives legal entities, their payment methods and delivery methods of the workspace.
- A null part was not read; use `notRead` to explain why. An empty list means no available items were returned. Do not invent a default.
- `get_offer` gives the payment and delivery choices that this proposal offers to the customer. These are the facts for this proposal.
- A delivery can be fixed-price, free or calculated outside the service. A payment commission is a percent.
- Do not say that a legal entity, payment or delivery was selected or added unless the returned proposal says so.

## Before publication

Read the proposal again and tell the user:

- proposal and customer;
- groups and product positions;
- counts, final prices, discounts or markups;
- total and currency;
- payment and delivery choices, or that none were returned.

Then call the publication preview. Its summary is authoritative.

## Publication and republication

- Publication creates the customer-visible version and cannot be undone.
- A changed draft does not change the version already visible to the customer.
- Republishing makes a new version at the same link.
- Publication does not open access when access by the link is closed. Say so.
- Republishing can clear the customer's answer to the previous version. Say which answer will be cleared.
- Only a clear yes after the preview authorizes publication.
- After publication, read the proposal again. `isPublished` means a version exists; `linkOpen` means the customer can open it now. This current state is the publication check.
- History can show publication as `VERSION_ADDED` without a separate `OFFER_PUBLISHED` event. Do not call that a failed publication when the current proposal is published.
- `productCount` in the list and history is the total number of units. The publication preview counts product positions. Name the measure when the numbers differ.
