---
description: Something has sold out. Snooze the item at a storekit store until a set time, then list what's already snoozed
argument-hint: "[item] at [store] until [time]"
---

Help the user take a sold-out item off sale using the storekit-manager skill. $ARGUMENTS

1. Call `list-venues` and find the store. Ask if it's unclear.
2. Call `get-menu` for that store and find the item. If several items match (for example "chicken burger" and "spicy chicken burger"), ask which ones.
3. If the user didn't say when it's back, suggest the end of today's service and ask.
4. Read the change back in one sentence with the item, store and end time in the store's local time, and wait for a yes.
5. Call `snooze-menu-items` once with the item IDs and the end time as an ISO timestamp with the store's UTC offset.
6. Call `list-snoozed-items` for the store.

Reply with what was snoozed and until when, then the full list of items snoozed at that store right now. Offer to unsnooze any of them. If the whole store needs to close instead, offer `snooze-venue` and ask for the reason first.
