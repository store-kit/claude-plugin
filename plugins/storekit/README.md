# storekit for Claude

Ask Claude about your storekit restaurants and stores in plain English, and make quick changes during service.

The plugin connects Claude to the storekit connector at `https://mcp.storekit.com/mcp` and adds a storekit manager skill. The skill tells Claude which report answers which question, how to read amounts and dates, how to handle guests' personal details, and to read every change back to you before making it.

## What you can ask

- "How did sales this Saturday compare with last Saturday at each store?"
- "Show me every refund over £20 this week, with the order and the reason."
- "What are guests saying in this week's feedback and Google reviews?"
- "The chicken burger has sold out at Soho. Snooze it until 6pm."

## Commands

- **weekly-review**: sales, orders, top products, feedback and refunds for the last week, compared with the week before.
- **end-of-day**: today's takings, matching the dashboard's End of day (Z) report, plus refunds, rejected orders and open bills.
- **sold-out**: snooze a sold-out item until a set time, then list everything that's snoozed.
- **promo-code**: set up a discount code, checking existing codes first.

In Claude Code and Cowork, run them as `/storekit:weekly-review` and so on. In Claude chat, just ask ("do my weekly review").

## Connect

In Claude, add **storekit** from Customize > Plugins. In Claude Code, run:

```
/plugin install storekit --marketplace store-kit/claude-plugin
```

After adding the plugin, open its Connectors tab, connect **storekit** and sign in with your storekit dashboard login. Claude only sees the stores and data that login can see in the dashboard. In Claude Code, run `/mcp`, pick **storekit** and select **Authenticate**.

## What Claude can change

Claude can create discount codes and payment links, snooze and unsnooze menu items, snooze and reopen a store, and edit a menu item's name, description or "popular" flag. It asks you to approve each change first. It can't refund, cancel or capture orders, or change prices, settings, users or payouts.

## Support

Email mcp@storekit.com. Guides: https://storekit.com/docs/guides/ai/connect/claude. Privacy policy: https://storekit.com/privacy-policy.
