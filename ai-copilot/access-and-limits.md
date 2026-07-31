> For the complete documentation index, see [llms.txt](llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](ai-copilot/access-and-limits.md).

# AI Copilot — Access & Limits

#### Access Tiers <a href="#access-tiers" id="access-tiers"></a>

| Tier | Image upload cap | File upload cap | Messages / minute | Uploads / hour |
|---|---|---|---|---|
| **Free** | 2 MB | 1 MB | 5 | 10 |
| **Basic** | 5 MB | 5 MB | 20 | 60 |
| **Pro** | 10 MB | 10 MB | 60 | 200 |
| **Enterprise** | 15 MB | 10 MB | 120 | 500 |

Higher tiers also unlock access to more advanced AI models, where the platform designates certain models as requiring a minimum tier.

#### Hardened Uploads <a href="#hardened-uploads" id="hardened-uploads"></a>

Every uploaded file or image is checked against a strict extension, content-type, and file-signature match before being accepted, so a disguised file can't slip through under a fake extension.

#### What Igniz AI Copilot Can Help With — Full Index <a href="#full-index" id="full-index"></a>

| Area | Capability |
|---|---|
| **Spot Trading** | Open orders · order history · trade history |
| **Futures Trading** | Open positions · open orders · order history · trade history · leverage/margin settings |
| **Futures Account & Risk** | Wallet balances · liquidation-risk dashboard · funding payment history |
| **Wallet & Transfers** | Balances · transfer history · deposit history · withdrawal history |
| **Account & Referrals** | Account profile · referral overview · referral earnings ledger |
| **Market Intelligence** | Pairs & currencies · order book · best quote · trade tape · 24h ticker · funding rate · candles (13 timeframes) · gainers/losers/trending screeners |
| **Technical Analysis** | Indicator catalog discovery · chainable indicator computation (60+ indicators) |
| **Performance Coach** | Portfolio summary · pair performance · position breakdown · trade journal |
| **Strategy Lab** | Backtest/optimization status, reports, and history · parameter-space preview |

For placing or cancelling trades, transferring funds, or launching new backtests/optimizations, see [MCP](mcp.md) — the separate, explicitly-authorized capability set for AI agents that act on your behalf. See [Safety](ai-copilot/safety.md) for how the two are kept architecturally separate.
