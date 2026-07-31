> For the complete documentation index, see [llms.txt](llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](ai-copilot/performance-and-strategy.md).

# AI Copilot — Performance & Strategy

#### AI Performance Coach — Your Personal Trading Journal <a href="#ai-performance-coach" id="ai-performance-coach"></a>

Fully available in-chat — arguably the richest thing you can ask Copilot to do. Covers both spot and futures activity, over any custom date range, always normalized to USD and computed with the exact same math the exchange's matching engine uses internally.

- **Portfolio Summary** — a full financial and behavioral check-up: net P&L, win rate, profit factor, expectancy, drawdown and recovery metrics, hold-time habits (including a flag for cutting winners short), risk discipline (stop-loss usage, leverage, risk-multiple distribution), and behavioral signals like **revenge-trading detection** — how quickly you re-enter after a loss, and whether position size creeps up afterward.
- **Pair Performance** — the same data rolled up per trading pair, ranked and sortable, to answer "which coins am I actually good at trading?"
- **Position Breakdown** — a trade-by-trade drill-down with **Maximum Favorable/Adverse Excursion** analysis (did that trade almost go further, either way, than what was realized?), automatic **stop-loss detection and R-multiple** scoring, and a snapshot of market conditions (momentum, volatility, trend) captured at the exact moment each position was opened.
- **Trade Journal** — the raw, fill-by-fill ledger underneath everything else, for full execution-level transparency.

Every metric is built to say "insufficient data" rather than present a misleading number whenever there isn't enough history to compute something honestly.

> *Ask Igniz:* "How did I do this month?" · "Which of my pairs is losing me money?" · "Show me my last 20 closed trades with the R-multiple on each." · "Do I have a revenge-trading problem?"

#### Strategy Lab — Research & Review <a href="#strategy-lab" id="strategy-lab"></a>

Copilot can help you understand and review strategy testing that's already been run:

- Check the status of any in-progress backtest or optimization job.
- Retrieve and explain the full report of a completed backtest or optimization — headline results, quality flags, and mandatory disclosures, in plain language.
- List your full history of past backtests and optimizations.
- **Preview a strategy's tunable parameter space** — see exactly which numeric settings can be optimized, with sensible ranges, to help plan a run before committing any usage quota.

> Launching a brand-new backtest or optimization job is reserved for the platform's Strategy Lab interface or an authorized agent connected via [MCP](mcp.md) — not something Copilot submits directly from a chat conversation. Once a run exists, though, Copilot can pull it up, explain it, and help you decide what to try next.

> *Ask Igniz:* "How's my backtest doing?" · "Explain the results of that optimization run." · "What parameters could I even tune on this strategy?"
