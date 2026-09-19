<img src="https://ucarecdn.com/20d662e7-f923-4257-abce-3cb8e7db1d7e/-/format/auto/" alt="StoreKit" height="48" />

# StoreKit Claude Code plugin

A [Claude Code plugin marketplace](https://code.claude.com/docs/en/plugin-marketplaces)
containing the **storekit** plugin, which connects Claude Code to the StoreKit
MCP server at `https://mcp.storekit.com/mcp` using OAuth.

## Install

```
/plugin marketplace add store-kit/claude-plugin
/plugin install storekit@storekit
```

Then run `/mcp`, select **storekit** and click **Authenticate**. Claude Code opens
your browser; sign in with your StoreKit dashboard account and you are done.

## Layout

```
.claude-plugin/marketplace.json          Marketplace catalogue (one plugin: storekit)
plugins/storekit/
  .claude-plugin/plugin.json             Plugin manifest
  .mcp.json                              Remote HTTP MCP server definition
  skills/storekit-mcp/SKILL.md           Usage notes Claude loads when working with the tools
```

## How the OAuth flow works

The plugin ships no credentials. `.mcp.json` only declares the remote server:

```json
{
  "mcpServers": {
    "storekit": { "type": "http", "url": "https://mcp.storekit.com/mcp" }
  }
}
```

Everything else is standard MCP authorization (OAuth 2.1), discovered at runtime:

1. Claude Code calls `POST https://mcp.storekit.com/mcp` with no token. The server
   answers `401` with
   `WWW-Authenticate: Bearer resource_metadata="https://mcp.storekit.com/.well-known/oauth-protected-resource"`.
2. Claude Code fetches that protected-resource metadata. It lists StoreKit's
   authorization server, `bearer_methods_supported: ["header"]` and
   `scopes_supported: ["read"]`.
3. Claude Code reads the authorization server's
   `/.well-known/oauth-authorization-server` metadata, registers itself with
   Dynamic Client Registration and starts the authorization-code + PKCE flow in the
   browser.
4. The authorization server sends the browser to the StoreKit dashboard, where the
   user signs in with their normal dashboard credentials. The dashboard completes
   the login and the browser is redirected to Claude Code's local callback with the
   authorization code.
5. Claude Code exchanges the code for an access token and sends it as
   `Authorization: Bearer <token>` on every subsequent request. The MCP server
   verifies the token and scopes every tool call to the user's account.

Tokens are stored by Claude Code per server URL. `claude mcp remove` / plugin
uninstall deletes them.

## Local development

```
claude --plugin-dir ./plugins/storekit
claude plugin validate ./plugins/storekit
```

Edit `.mcp.json` to point at a local or staging MCP server if needed
(`http://localhost:3001/mcp` for `npm run mcp` in corona-api).
