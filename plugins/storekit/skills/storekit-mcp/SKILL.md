---
name: storekit-mcp
description: How to use the StoreKit MCP tools (venues, orders, menus, stats, discounts, refunds, bills, payment links). Use when the user asks about their StoreKit venues, orders, sales, menus or payment links, or when a StoreKit tool call returns an error or "Needs authentication".
---

# StoreKit MCP

All tools are scoped to the signed-in user's StoreKit account. Tools are named
`mcp__plugin_storekit_storekit__<tool>` (e.g. `...__list-venues`).

## Authentication

The server uses OAuth 2.1. If a tool call fails with 401 or the
server shows `Needs authentication`, tell the user to run `/mcp`, pick
**storekit** and complete the sign-in in the browser using their StoreKit
dashboard login. Tokens are stored by Claude Code; nothing else is needed.

## Response shape

Every successful call returns `{ data, meta }`:

- Detail tools put the record in `data`; list tools use `data.results`.
- `meta.pagination` carries `page`, `limit`, `hasMore`, `nextPage`, `total`
  (`total: null` means not counted, not zero). Follow `nextPage` when
  `hasMore` is true.
- `meta.dateRange` echoes the resolved UTC bounds and `meta.timezone` the
  timezone used; `meta.warnings` lists anything that was defaulted or clipped.
- Errors come back as `{ error: { code, message, nextSteps, retryable } }` in
  the text block with `isError: true`. Never retry a write automatically.

## Conventions

- Amounts are in minor currency units (pence/cents). Use `currencyCode` from
  the result; never assume a currency.
- Dates accept `YYYY-MM-DD` or ISO datetimes with `Z` / an explicit offset.
  Date-only `to` values include the whole local day. Max range is 90 days.
- Venue-scoped queries default to the venue's timezone; account-wide queries
  default to UTC. Pass `timezone` (IANA) to override.
- Order `status` values are the stored strings, e.g. `Ready For Pickup`,
  `Out for Delivery`.
- Start with `list-venues` to resolve a venue name to `venueId` before calling
  venue-scoped tools.
- Write tools (`create-discount-code`, `create-payment-link`) mutate live
  data. Confirm parameters with the user before calling them.
