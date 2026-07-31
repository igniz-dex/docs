> For the complete documentation index, see [llms.txt](/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](/mcp/configuration.md).

# MCP — Configuration

#### What TradingMCP Is <a href="#what-tradingmcp-is" id="what-tradingmcp-is"></a>

TradingMCP is Igniz's AI trading layer — the connective tissue that lets an authorized AI agent act on your account with the same depth and precision you have yourself, built on the [Model Context Protocol](/mcp.md). An agent connected through TradingMCP can check a balance, read the order book, compute a technical indicator, review your performance history, place an order, manage risk on a leveraged position, or run a strategy backtest — all using the exact same live systems, prices, and math that power the exchange itself.

TradingMCP is the full capability set behind Igniz's AI-native trading story: **54 individually addressable capabilities**, spanning spot trading, futures and perpetuals, wallet and fund management, account and referral information, real-time market intelligence, a 60+-indicator technical analysis engine, an AI performance coach, and a full strategy backtesting and optimization lab. It is the superset that both the in-app [AI Copilot](/ai-copilot.md) and any external AI client you choose to connect draw from — see [Tools](/mcp/tools.md) for the individual tool catalog.

#### Two-Tier Authorization Model <a href="#two-tier-authorization" id="two-tier-authorization"></a>

Every capability in the TradingMCP catalog falls into exactly one of two authorization tiers, and you choose which tiers an agent receives when you connect it:

| Tier | Scope | Availability |
|---|---|---|
| **Ask & Analyze** | Read-only — balances, positions, order/trade history, market data, indicators, performance analytics, backtest/optimization reports | Always-on for a connected agent once granted; the same scope the in-app [AI Copilot](/ai-copilot.md) runs under by default |
| **Act & Trade** | Account-changing — placing and cancelling orders, transferring funds between your own accounts, launching new backtests or optimization runs | A separate, explicit, revocable grant — never bundled into "Ask & Analyze" by default |

The **Ask & Analyze** scope covers 45 read-only capabilities. **Act & Trade** adds exactly **9** account-changing actions on top of it, for the full 54. Those 9 actions are precisely what separates a connected agent from the in-app Copilot, which is architecturally read-only and never exposes them at all — see the [AI Copilot Safety](/ai-copilot/safety.md) page for that comparison.

No permission level — not even full "Act & Trade" authority — ever includes initiating a withdrawal off the platform or generating a new deposit address. Moving funds off the exchange always requires you to act directly.

#### Granting Access — OAuth-Secured Consent <a href="#granting-access" id="granting-access"></a>

An AI agent never sees your password. Connecting an agent to TradingMCP goes through a standards-based OAuth consent flow:

1. You initiate a connection from your chosen MCP-compatible client or agent.
2. You're redirected to Igniz to review exactly what's being requested — which scope(s), and for how long.
3. You approve (or decline) deliberately, choosing anywhere from "just let it look at my portfolio and market data" up to "let it trade for me."
4. Igniz issues the agent a scoped, time-limited access grant — never a login credential.

Nothing beyond what you explicitly approve is ever switched on. If you grant only **Ask & Analyze**, the agent has no path to place an order, cancel one, move funds, or start a new backtest, regardless of what it's asked to do.

> Concrete connection details (authorization endpoint, client identifiers, redirect URIs) are specific to each MCP client integration — see [Client Setup](/mcp/client-setup.md) for the connection walkthrough.

#### Revoking Access <a href="#revoking-access" id="revoking-access"></a>

Access can be cut off at any moment, by you or by the platform, and it takes effect immediately — there is no lingering window where a revoked agent can still act. Revoking a grant removes both scopes at once; reconnecting an agent means going through consent again and choosing scopes fresh.

#### Auditable By Design <a href="#auditable-by-design" id="auditable-by-design"></a>

Every trade an agent places, every order it cancels, and every transfer it makes lands in your own activity history — the same record you'd see if you'd clicked the buttons yourself. There is no separate, invisible "AI trail."

#### Related Pages <a href="#related-pages" id="related-pages"></a>

| Page | Covers |
|---|---|
| [Client Setup](/mcp/client-setup.md) | Connecting an MCP client, choosing scope, and revoking access step by step |
| [Tools](/mcp/tools.md) | The individual tool catalog available under each scope |
| [AI Copilot](/ai-copilot.md) | The in-app, always-on, read-only conversational counterpart to TradingMCP |
