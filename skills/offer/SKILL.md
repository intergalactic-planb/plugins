---
name: offer
description: 'Create, find, review, change and publish a commercial proposal or quote. Use when the user asks for an offer, proposal or quote: add catalog or custom products, groups or options, quantities, discounts or markups, customer details, payment or delivery terms, edit an existing offer, compare its history, or send/publish it to the customer ("make a quote", "change the offer", "send it to the customer"). Apply silently from the first reply: never announce skills, references or internal preparation.'
---

# Work with a commercial proposal

Before your first reply, apply this workflow silently, including before reading references. Describe only the proposal work you will do; never announce a skill, its instructions, references or internal preparation. For example: “I will prepare the offer for the customer and check its items.”

Use only the tools of the connected service. Never open the service in a browser. Call the product by the name the user uses.
To the user, speak in business words: proposal, customer, group, product, price, discount, payment, delivery. Never say that you use a skill. Never name tools, ids or fields. Never show raw errors.

The answers of the tools have the exact rules and refusals. Follow them. This skill gives the order; references/ gives details.

## 1. Choose the task

- Find or list proposals: use find_offers, then get_offer for the one the user means. For an outdated check, if `checked` is less than `total`, say that older proposals were not checked.
- Explain what changed or what the customer answered: read the proposal, then get_offer_history.
- Create a proposal: section 2.
- Change a proposal: section 3.
- Publish, send or share it: section 5.

If several proposals or products fit, show their business names and ask which one. Never expose ids.

## 2. Create a proposal

1. Read references/groups-and-products.md and read `get_connection_info` for the new proposal currency.
2. For every catalog product, call find_products. Match the name or article and the catalog the user named. Ask when the match is not unique.
3. For a custom product, use the name and unit price the user gives. Ask for either if missing before adding it. Use a group comment only if the user provides one.
4. Read references/prices.md when there is a discount, markup, changed price or more than one unit.
5. Before creation, tell the user the proposal name, customer, workspace currency, groups, products, counts, prices and discounts. If a catalog price uses a different currency, explain that its numeric amount is copied into the proposal currency without conversion. A clear request to create is enough; do not ask for another confirmation unless something is ambiguous.
6. Call create_offer once. Use groups when the user asked for groups or options; otherwise use the single-group arguments.
7. If the user asked for alternative options or a markup, use the ids returned by creation and call update_offer once: set only the requested groups to `ALTERNATIVE` and their selected products when requested, and set only the requested product markups. Check the proposal returned by this change.
8. Check the final returned proposal. Report its total and every partial result, including products added without a discount or an image. Check whether a line is already in the proposal before retrying it.

## 3. Change a proposal

1. Find the proposal and call get_offer immediately before the change. Use only its product and group ids.
2. Tell the user how you understood the requested changes. Ask only about an ambiguity; a clear request is enough to change the draft.
3. Read the relevant reference:
   - groups, options, products and pictures: references/groups-and-products.md;
   - price, count, discount or markup: references/prices.md.
4. Call update_offer once with only the requested fields. Send a new price and its requested money discount or markup in the same product change. Ask for missing names or unit prices before adding custom products. Use an empty string only to clear a supported text field on request. The proposal itself needs a nonempty name.
5. Check the returned proposal: the changed values, groups, total, products not added and pictures not added.
6. If the proposal was published before, say that the customer still sees the previous published version until the proposal is published again.

## 4. Company, payment and delivery

Read references/company-and-publish.md.

- Call get_company_details when the user asks for the company's details, payment or delivery, or asks to prepare a proposal for sending.
- Read get_offer to see which payment and delivery choices this proposal actually gives the customer.
- Do not claim that a legal entity, payment or delivery was applied unless the returned proposal shows it.

## 5. Publish

1. Read references/company-and-publish.md.
2. Call get_offer again. Check the customer, groups, products, counts, discounts or markups, total, payment and delivery. Tell the user what is missing; never fill it in yourself.
3. Call preview_publish_offer.
4. Show its summary in business words: customer, number of product positions, total and currency; whether this is a new version; whether access by the link is closed; whether an old customer answer will be cleared.
5. Say that publication cannot be undone. Wait for a clear yes to this exact summary.
6. Call commit_publish_offer with the code. Never publish on an earlier or vague yes.
7. Give the public link if returned. Call get_offer once more and report whether the proposal is published and whether the customer can open it now.

## Never

- Never publish before the user confirms the publication summary.
- Never make up a product, price, customer, company detail, payment or delivery.
- Never multiply or divide prices: the tools give real amounts.
- Never recalculate a returned proposal total yourself.
- Never retry a money discount merely because the web shows its equivalent percent.
