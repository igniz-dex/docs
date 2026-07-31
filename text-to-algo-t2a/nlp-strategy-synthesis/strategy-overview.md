> For the complete documentation index, see [llms.txt](llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](text-to-algo-t2a/nlp-strategy-synthesis/strategy-overview.md).

# Strategy Overview

Every T2A strategy — no matter how it's phrased in plain English — boils down to the same simple shape underneath: an entry, an optional take-profit, and an optional stop-loss. Understanding this shape makes it much easier to describe exactly the strategy you have in mind.

### The Three Logic Blocks <a href="#the-three-logic-blocks" id="the-three-logic-blocks"></a>

| Block | Required? | Shape |
|---|---|---|
| **Entry** | Yes | A single condition that fires the entry order (limit or market). |
| **Take-Profit** | No | A fractional ladder of up to 5 legs, each with its own trigger condition and its own share of the position. |
| **Stop-Loss** | No | Either a fixed condition (e.g. price back below an EMA) or a trailing stop that follows the best price reached since entry. |

An entry is the only required piece — a strategy with no take-profit or stop-loss simply enters and leaves you to manage the exit yourself. Most strategies, though, specify all three.

### How Conditions Evaluate <a href="#how-conditions-evaluate" id="how-conditions-evaluate"></a>

Each of these three blocks is built from **conditions** — logical trees that combine indicators, price data, comparisons, and boolean logic (AND / OR / NOT). Every condition is checked against a fresh, live snapshot of the market each time it's evaluated: the latest price, the order book, and the relevant candle history across every timeframe the strategy references.

This means a strategy is never checking stale data — an entry condition that depends on a 1-hour trend filter and a 5-minute trigger is evaluated against both timeframes' current state on every pass.

### Quick Index — What Can Appear Inside a Strategy <a href="#quick-index" id="quick-index"></a>

A condition tree can be built from any combination of:

- **Technical indicators** — 69 indicators across trend, momentum, composite, volatility, volume, and pivot/structure categories, each addressable on any of 10 timeframes with a configurable lookback.
- **Non-indicator market data** — live price, raw candle fields, order-book microstructure, simple arithmetic between values, "percent from my entry," and futures-only scalars like funding rate.
- **Comparators** — eight operators, including crossover detection, plus an optional "hold for N bars" persistence requirement.
- **Boolean logic** — AND / OR / NOT, nestable to a healthy depth.
- **Named market-structure & candlestick patterns** — 25 recognized patterns, evaluated only on fully closed candles.
- **Divergence detection** — price vs. any oscillator, in all four regular/hidden × bullish/bearish variants.
- **Session / time-of-day filters** — restrict a strategy to specific trading windows.
- **Cross-pair references** — conditions that read from a different pair than the one being traded (feature-gated).

The next few pages walk through each of these building blocks in detail — starting with the indicator and market-data toolkit.
