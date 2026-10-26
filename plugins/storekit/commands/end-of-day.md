---
description: End-of-day takings check for a storekit store, matching the dashboard's End of day (Z) report
argument-hint: "[store name] [date]"
---

Run an end-of-day check using the storekit-manager skill. $ARGUMENTS

1. Call `list-venues`. If the user didn't name a store and there is more than one, check each store.
2. Use today at each store, in its local time, unless the user gave a date.
3. Call `get-end-of-day-report` for the day. Report gross sales, card and cash totals with their transaction counts, refunds (count and total), tips, discounts, service charge and average order value.
4. Call `list-refunds` for the same day and list each refund with its amount, time and order.
5. Call `list-orders` with `dateBasis=creation` for the day, including rejected and cancelled orders, and count how many were rejected or cancelled.
6. If the store uses pay-at-table, call `list-bills` and flag any bill still open with an amount due.

Reply with the takings in one short block per store, then anything the manager should check before closing: open bills, refunds, rejected orders. Remind them that this report covers online orders only, not pay-at-table bills. Don't make any changes.
