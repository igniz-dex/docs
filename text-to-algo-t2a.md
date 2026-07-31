> For the complete documentation index, see [llms.txt](/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](/text-to-algo-t2a.md).

# Text-to-Algo (T2A)

Text-to-Algo — **T2A** — turns a plain-English description of a trading idea into a fully-specified, validated, and live-managed trading algorithm. No code, no scripting, no manually babysitting orders around the clock.

### What T2A Is <a href="#what-t2a-is" id="what-t2a-is"></a>

You describe a strategy the way you'd explain it to another trader. T2A turns that description into a precise strategy definition, shows you exactly what it understood in plain English so you can confirm it, and then — once you approve it — runs it continuously: watching the market, arming entries, managing exits, cancelling sibling orders when one leg fires, and keeping you posted in real time.

### A Worked Example <a href="#a-worked-example" id="a-worked-example"></a>

You might type something like:

> *"Buy the dip on BTC/USDT when RSI is oversold on the 15-minute chart. Take profit at 2%, and once I'm in profit, trail my stop by 1%."*

T2A turns that into a strategy with a clear entry condition (RSI oversold on the 15-minute chart), a take-profit target (2%), and a trailing stop-loss (1% behind the best price reached since entry) — then shows you a plain-English confirmation of exactly that logic before anything runs.

### Markets & Timeframes <a href="#markets-timeframes" id="markets-timeframes"></a>

T2A is available on both **Spot** and **Futures (perpetuals)** markets, across every pair the exchange lists, on candle timeframes from **1 minute to 1 day**.

A strategy is always locked to the market kind it was built for — a spot strategy can never accidentally run on a futures pair, or vice versa. This is checked every time a strategy is launched, edited, or re-run, not just at creation.

### The Strategy Lifecycle <a href="#the-strategy-lifecycle" id="the-strategy-lifecycle"></a>

T2A is the first step in a larger lifecycle that carries a strategy from an idea to real, funded execution:

1. **Generate & Backtest** — describe the strategy in plain English, then check how it would have performed against historical market data.
2. **Optimize** *(optional)* — automatically search for better parameter values within the same strategy logic.
3. **Paper Trade** — run the strategy live, against real, real-time market data, with every fill, fee, and funding payment simulated against a virtual balance — no real capital at risk.
4. **Go Live** — flip the exact same strategy to real execution, with zero changes to its logic.

Each stage reuses the same underlying strategy definition and the same execution engine's decision logic — what you validate at each step is what actually runs live.

### Lifecycle Actions <a href="#lifecycle-actions" id="lifecycle-actions"></a>

A strategy isn't a one-shot creation — it's something you manage over time:

| Action | What it does |
|---|---|
| **Generate** | Describe entry/exit logic in plain English; the AI drafts a validated strategy. |
| **Preview** | Check whether each condition would already be true right now, and how close the market is to triggering it — at no cost to capital or generation quota. |
| **Launch** | Turn a validated strategy into a live, running order with a chosen size, expiry, and execution mode. |
| **Save** | Store a validated strategy's logic for later reuse, independent of any one run's sizing or expiry. |
| **Edit** | Fully replace a saved strategy's logic and/or pair; automatically re-validated. Past runs of the old version are unaffected, since a live order keeps its own frozen copy of the logic it launched with. |
| **Rename** | Update a saved strategy's display title. |
| **Delete** | Remove a saved strategy; running orders created from it are unaffected. |
| **Browse** | List all saved strategies, or fetch one's full logic and details. |
| **Re-launch** | Run a saved strategy again — on the same pair or a compatible one — with fresh sizing, expiry, and notification choices, without re-describing it in plain English. |
| **Monitor** | List all live and past orders, or pull one order's complete detail and audit-event history. |
| **Cancel** | Stop a single order, or an entire continuous/recurring run, on demand. |
| **Check quota** | See how many AI generations remain in the current window, and when it resets. |
| **Browse toolkit** | Explore the full catalog of indicators, structure presets, and parameters available for building strategies. |

### Rails & Limits <a href="#rails-limits" id="rails-limits"></a>

| Control | Limit |
|---|---|
| Saved strategies per user | Up to 100 |
| Free-text field length (entry / take-profit / stop-loss) | Up to 2,000 characters each |
| Market kind | A strategy is permanently locked to spot or futures — never both |

### Explore T2A <a href="#explore-t2a" id="explore-t2a"></a>

- [NLP Strategy Synthesis](/text-to-algo-t2a/nlp-strategy-synthesis.md) — how a plain-English description becomes a validated algorithm.
- [Backtesting](/text-to-algo-t2a/backtesting.md) — checking a strategy against historical market data.
- [Optimization](/text-to-algo-t2a/optimization.md) — automated parameter search over a strategy.
- [Paper Trading](/text-to-algo-t2a/paper-trading.md) — running a strategy live with simulated fills.
- [Live Activation](/text-to-algo-t2a/live-activation.md) — flipping a proven strategy to real execution.
