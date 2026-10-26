---
description: Set up a storekit discount code, checking existing codes first and confirming every detail before it goes live
argument-hint: "[e.g. 10% off this weekend at Soho]"
---

Set up a discount code using the storekit-manager skill. $ARGUMENTS

1. Call `list-venues` and `list-discount-codes`. Check the code the user wants doesn't already exist, and mention any similar code that is still running.
2. Work out, asking only for what's missing:
   - the code (it is saved in lower case)
   - the type: a percentage off, a fixed amount off, buy one get one free, or free delivery
   - the amount
   - which stores it applies to
   - when it starts and ends
   - any minimum spend, maximum discount, total use limit or limit per guest
3. Convert amounts for the tool. A percentage is in tenths of a percent (10% is `100`). A fixed amount, minimum spend and maximum discount are in minor units (£5 is `500`). Send start and end times as ISO timestamps with the store's UTC offset.
4. Read the whole code back in plain English, e.g. "SUMMER10: 10% off, Soho and Shoreditch, from now until Sunday 11pm, one use per guest. Create it?", and wait for a yes.
5. Call `create-discount-code` once.

Reply with the code exactly as saved, where it works and when it ends. Don't offer to email or post it anywhere; the connector can't do that.
