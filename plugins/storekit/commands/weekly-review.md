---
description: Weekly trading review across your storekit stores, compared with the week before
argument-hint: "[store name or 'last week']"
---

Give the user a weekly trading review using the storekit-manager skill. $ARGUMENTS

1. Call `list-venues`. If the user named a store, review that store only. Otherwise review every store, one at a time if they use different currencies.
2. Use the last seven whole local days ending yesterday, unless the user asked for a different week. State the dates.
3. For each store, call `get-order-stats` for orders, revenue and average order value against the previous seven days.
4. For the top products, call `list-top-products` once for the whole group (no `venueId`) with `limit=5`, or with that store's `venueId` if the user named one. If the stores use different currencies, call it per store instead and say the lists are per store.
5. Call `get-feedback-summary` for the same week.
6. Call `list-refunds` for each store and pick out anything unusual, such as one large refund or more refunds than usual.

Reply with:

- One line per store: revenue, orders and average order value, each with the change on the previous week.
- The top five products across the group (or per store, if the stores use different currencies).
- Two or three things worth acting on, such as a store that dropped, a product that took off, low ratings or Google reviews waiting for a reply.

Lead with what went well. Keep it short enough to read on a phone. Don't make any changes.
