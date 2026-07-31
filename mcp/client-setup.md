> For the complete documentation index, see [llms.txt](llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](mcp/client-setup.md).

# MCP — Client Setup

This page walks through connecting an MCP-compatible AI client or agent to Igniz TradingMCP. For background on what TradingMCP is and how its two-tier authorization model works, see [Configuration](mcp/configuration.md).

#### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

- An **Igniz account** you can log into, since TradingMCP grants are issued against your own authenticated account.
- An **MCP-compatible client or agent** — any AI assistant, IDE integration, or autonomous agent framework that speaks the Model Context Protocol.
- A decision, before you start, about which authorization scope you intend to grant: **Ask & Analyze** (read-only) or **Ask & Analyze + Act & Trade** (full trading authority). You can always start narrower and grant more later.

#### Step 1 — Start the Connection <a href="#start-the-connection" id="start-the-connection"></a>

From your MCP client, initiate a connection to Igniz's TradingMCP endpoint. The exact steps differ by client, but every client will ask for an Igniz server address to connect to.

```
MCP server URL: <your-mcp-client-configured-igniz-endpoint>
```

> The value above is a placeholder — your specific TradingMCP connection endpoint is provided within the Igniz app when you begin the connection flow, not a fixed public URL to copy from documentation.

#### Step 2 — Authorize via OAuth <a href="#authorize-via-oauth" id="authorize-via-oauth"></a>

Starting a connection redirects you to Igniz's own login and consent screen — the agent itself never sees or handles your Igniz credentials.

1. Log in to your Igniz account if you aren't already.
2. Review the consent screen, which states plainly what the connecting client is requesting.
3. Choose the scope to grant:
   - **Ask & Analyze only** — the agent can read balances, positions, orders, trade history, market data, indicators, and performance/backtest reports, and nothing else.
   - **Ask & Analyze + Act & Trade** — the agent additionally gains the 9 account-changing actions: placing and cancelling orders, transferring funds between your own accounts, and launching new backtests or optimization runs.
4. Approve the request.

Igniz issues the client a scoped, time-limited access grant — never your login itself. If you only need research and analysis, granting **Ask & Analyze** alone is sufficient and keeps the agent structurally unable to touch an order or a transfer, regardless of what it's instructed to do.

#### Step 3 — Confirm the Connection <a href="#confirm-the-connection" id="confirm-the-connection"></a>

Once authorized, your MCP client should report a successful connection and list the tools now available to it, matching whichever scope you granted. If your client supports it, ask it to "list available tools" as a quick sanity check — a read-only grant should show no order-placement, cancellation, transfer, or backtest-launch tools at all.

#### Changing or Revoking Access <a href="#changing-or-revoking-access" id="changing-or-revoking-access"></a>

- **To grant additional scope** (for example, moving from Ask & Analyze to full trading authority), return to the connection flow and approve the broader request — this issues a new grant reflecting the wider scope.
- **To revoke access entirely**, remove the connected client from your Igniz account's connected-apps settings. Revocation takes effect immediately: any further calls from that client will fail rather than queue or partially succeed.
- There is no partial "leftover" access after revocation — both scopes are removed together, and reconnecting means going through authorization again from scratch.

#### Troubleshooting <a href="#troubleshooting" id="troubleshooting"></a>

| Symptom | Likely Cause |
|---|---|
| Client reports connected but trading tools are missing | Only **Ask & Analyze** was granted — this is expected behavior, not an error. Re-authorize with **Act & Trade** if you want trading tools available. |
| Agent's actions aren't visible anywhere | They should be — every action taken through TradingMCP lands in your normal account activity history. Check there rather than a separate "AI" log, since none exists. |
| Previously working client suddenly fails all calls | Access was likely revoked (by you or the platform) or the grant expired. Reconnect and re-authorize. |

#### Related Pages <a href="#related-pages" id="related-pages"></a>

| Page | Covers |
|---|---|
| [Configuration](mcp/configuration.md) | What TradingMCP is, the two-tier authorization model, and the full 54-capability catalog |
| [Tools](mcp/tools.md) | The individual tool catalog available under each scope |
| [AI Copilot](ai-copilot.md) | The in-app, always-on, read-only conversational counterpart to TradingMCP |
