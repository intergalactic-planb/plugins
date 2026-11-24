# Company terms and publication

## Company, payment and delivery

- `get_company_details` gives legal entities, their payment methods and delivery methods of the workspace.
- A null part was not read; use `notRead` to explain why. An empty list means no available items were returned. Do not invent a default.
- `get_offer` gives the payment and delivery choices that this proposal offers to the customer. These are the facts for this proposal.
- A delivery can be fixed-price, free or calculated outside the service. A payment commission is a percent.
- Do not say that a legal entity, payment or delivery was selected or added unless the returned proposal says so.

## Changing the delivery and payment of a proposal

- To offer a delivery or payment method of the company, connect it by its id from get_company_details. A method already on the proposal cannot be connected again.
- A method only for this proposal needs a name; ask the user for it, and for the price of a delivery or the commission of a payment. A delivery is free or has a fixed price. Do not invent a price or a commission.
- Change or remove a method of the proposal by its id from get_offer. The proposal keeps its own copy: changing it does not change the company method.
- Do not invent a method for the company. If the user wants a company method that does not exist, say that it has to be created in the web first.
- Pictures and the order of the methods are not changed.
- After the change, read the returned proposal and report the methods it now offers.

## Before publication

Read the proposal again and check the company details. Tell the user:

- proposal and customer;
- groups and product positions;
- counts, final prices, discounts or markups;
- total and currency;
- payment and delivery choices on the proposal, with each missing or unread detail called out.

Then call the publication preview. Its summary is authoritative.

## Publication and republication

- Publication creates the customer-visible version and cannot be undone.
- A changed draft does not change the version already visible to the customer.
- Republishing makes a new version at the same link.
- Publication does not open access when access by the link is closed. Say so, and after publishing ask whether to open it.
- Republishing can clear the customer's answer to the previous version. Say which answer will be cleared.
- Only a clear yes after the preview authorizes publication.
- The publication result checks the current proposal. Give the public link in chat when the result confirms access and includes the link. The public address must be available.
- If access is closed, report that the proposal was published, then ask whether to open access by the link. On a clear yes call open_offer_access and give its link; on a no leave it closed. Never open access unasked. If access could not be checked, read the proposal again. If access is open and the public link is unavailable, report that. Never publish a second version just to check access or get a link.
- To close access by the link, use close_offer_access only when the user clearly asks to close or stop it. Say that the customer will not be able to open the proposal until access is opened again, and that the proposal stays published. Never close access unasked, and do not close it to "unpublish". After it, say that access is closed; if the user wants it open again, call open_offer_access.
- History can show publication as `VERSION_ADDED` without a separate `OFFER_PUBLISHED` event. Do not call that a failed publication when the current proposal is published.
- `productCount` in the list and history is the total number of units. The publication preview counts product positions. Name the measure when the numbers differ.
