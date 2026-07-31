> For the complete documentation index, see [llms.txt](https://igniz.gitbook.io/igniz-docs/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://igniz.gitbook.io/igniz-docs/risks.md).

# Risks

> **This page is not exhaustive.** Trading crypto derivatives on a decentralized, AI-native exchange carries risks beyond what's listed here. Nothing on this page is financial advice, and you should only trade with capital you can afford to lose. Read this page alongside [Fund Safety](https://igniz.gitbook.io/igniz-docs/igniz-trade/fund-safety.md) and [Security](https://igniz.gitbook.io/igniz-docs/security.md) before trading on Igniz.

#### Smart Contract Risk <a href="#smart-contract-risk" id="smart-contract-risk"></a>

Igniz's trading, margin, and settlement logic depends on smart contracts and cross-chain bridge infrastructure. Like any on-chain system, correctness depends on that code being free of implementation flaws — a bug or an exploited vulnerability in a contract or bridge could put user funds at risk. Igniz undergoes independent security review (see [Audit Report](https://igniz.gitbook.io/igniz-docs/security/audit-report.md)), but no amount of auditing eliminates smart contract risk entirely.

#### Blockchain & Cross-Chain Risk <a href="#blockchain-and-cross-chain-risk" id="blockchain-and-cross-chain-risk"></a>

Igniz operates across multiple L1/L2 networks and relies on IBC and other cross-chain bridge mechanisms to move assets and data between them. Consensus failures, validator downtime, network congestion, or issues with a bridge can cause delays, failed transactions, or temporary service interruptions that are outside Igniz's direct control. Bridge infrastructure in particular is a common target of exploits industry-wide.

#### Market Liquidity Risk <a href="#market-liquidity-risk" id="market-liquidity-risk"></a>

Order book depth varies by market, and newer or lower-volume pairs can be thin. Trading into thin liquidity — especially with market orders or large size — can produce meaningful slippage and move the price against you more than expected. Always check the live [order book](https://igniz.gitbook.io/igniz-docs/igniz-trade/order-book.md) depth before sizing a trade in a less-liquid market.

#### Oracle / Price-Feed Risk <a href="#oracle-price-feed-risk" id="oracle-price-feed-risk"></a>

Igniz's [Mark Price](https://igniz.gitbook.io/igniz-docs/igniz-trade/mark-price.md) and [Index Price](https://igniz.gitbook.io/igniz-docs/igniz-trade/index-price.md) — the composite feeds that drive margin, liquidation, and funding calculations — are published by validators and aggregate external exchange data. A compromised, delayed, or manipulated price feed anywhere upstream could, in principle, trigger premature or incorrect liquidations. Igniz mitigates this by design: Mark Price is a **median across multiple independent sources** (an adjusted reference price, the platform's own order book median, and a weighted external perpetuals aggregate), with **exponential-moving-average smoothing** specifically intended to filter momentary spikes and resist single-source manipulation — see [Mark Price](https://igniz.gitbook.io/igniz-docs/igniz-trade/mark-price.md) for the full composition. This reduces, but cannot completely eliminate, oracle risk.

#### Leverage & Liquidation Risk <a href="#leverage-and-liquidation-risk" id="leverage-and-liquidation-risk"></a>

Igniz offers leverage under a progressive, per-market framework where the permissible maximum decreases as position size increases (see [Leverage](https://igniz.gitbook.io/igniz-docs/igniz-trade/leverage.md)). Leverage magnifies both gains and losses, and a highly leveraged position can be liquidated on a comparatively small adverse price move. In fast-moving or illiquid conditions, the platform's order book liquidation attempt may not fully absorb a position, in which case it can be routed to the Insurance Vault backstop — and in extreme conditions, positions can be subject to **auto-deleveraging (ADL)**. See [Liquidation Mechanism](https://igniz.gitbook.io/igniz-docs/igniz-trade/liquidation-mechanism.md) and [ADL](https://igniz.gitbook.io/igniz-docs/igniz-trade/adl.md) for the full mechanics, and consider your leverage and stop-loss usage accordingly.

#### AI-Suite Risks <a href="#ai-suite-risks" id="ai-suite-risks"></a>

Igniz's AI trading suite is a differentiator, and it carries its own category of risk distinct from ordinary market risk:

- **T2A automated execution.** [Text-to-Algo](https://igniz.gitbook.io/igniz-docs/text-to-algo-t2a.md) turns a plain-English strategy description into a live, automated trading algorithm. A strategy that is misspecified, or that is run in market conditions it wasn't designed for, can still lose money — automation executes the rules it was given literally and continuously, without the judgment a discretionary trader would apply in the moment.
- **Backtest and optimization overfitting.** Strong historical [backtest](https://igniz.gitbook.io/igniz-docs/text-to-algo-t2a/backtesting.md) or optimization results do not guarantee live performance. Despite Igniz's overfitting-protection stack (robustness scoring, out-of-window degradation reporting, and pass/fail reality-check gates), a strategy that looks excellent on historical data can still underperform once traded live.
- **AI Copilot is advisory and read-only.** [AI Copilot](https://igniz.gitbook.io/igniz-docs/ai-copilot.md) cannot place a trade, cancel an order, or move funds under any circumstance — it is structurally read-only. Its analysis, like any AI-generated output, can be incomplete or wrong. Trading decisions, and responsibility for them, remain yours.
- **AI Chart Analysis is visualization, not advice.** [AI Chart Analysis](https://igniz.gitbook.io/igniz-docs/ai-chart-analysis.md) produces deterministic technical-analysis annotations — market structure, support/resistance, Fibonacci levels, and similar — from your natural-language request. It is not a signal service and not trade advice, and its output is explicitly not phrased as a recommendation to buy, sell, enter, or exit.

#### Risk Mitigation <a href="#risk-mitigation" id="risk-mitigation"></a>

Igniz builds a number of protections into the platform specifically to reduce the risks above:

- **Manipulation-resistant pricing.** [Mark Price](https://igniz.gitbook.io/igniz-docs/igniz-trade/mark-price.md) is median-based across multiple independent sources with EMA smoothing, so no single feed or momentary spike can drive margin or liquidation decisions.
- **Wash trade protection.** Igniz automatically cancels a resting order that would otherwise self-match against your own incoming order, preventing accidental self-trading — see [Wash Trade Protection](https://igniz.gitbook.io/igniz-docs/igniz-trade/wash-trade-protection.md).
- **Liquidation and ADL backstops.** A dual-stage liquidation process (order book first, then the Insurance Vault) and auto-deleveraging as a last resort are both designed to preserve platform solvency without imposing clearance fees on liquidated traders — see [Liquidation Mechanism](https://igniz.gitbook.io/igniz-docs/igniz-trade/liquidation-mechanism.md) and [ADL](https://igniz.gitbook.io/igniz-docs/igniz-trade/adl.md).
- **Honesty over confidence, across the AI suite.** Every AI-suite feature — Copilot, T2A, backtesting, optimization, and Chart Analysis — is built to report "insufficient data" or an explicit disclosure rather than present a fabricated or misleadingly confident number.
- **Self-custody.** Igniz is non-custodial — you control your own funds via your own wallet, and no AI agent, at any permission level, can ever initiate a withdrawal off the platform or generate a new deposit address. See [AI Copilot Safety](https://igniz.gitbook.io/igniz-docs/ai-copilot/safety.md) for how that boundary is enforced architecturally, not just by policy, across both Copilot's 45 read-only capabilities and the full 54-capability MCP catalog.
