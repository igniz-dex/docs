> For the complete documentation index, see [llms.txt](https://igniz.gitbook.io/igniz-docs/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://igniz.gitbook.io/igniz-docs/text-to-algo-t2a/nlp-strategy-synthesis/strategy-examples.md).

# Strategy Examples

T2A works from plain English. You describe what you want in ordinary trading language, and the AI drafts a fully-specified, validated strategy from it. For example:

> *"Buy the dip on BTC/USDT when RSI is oversold on the 15-minute chart. Take profit at 2%, and once I'm in profit, trail my stop by 1%."*

This page catalogs common trader and algo concepts, and how each maps onto T2A's building blocks — useful as a quick reference for "yes, T2A can build this."

#### Trend & momentum patterns <a href="#trend-momentum-patterns" id="trend-momentum-patterns"></a>

| Pattern | How T2A builds it |
|---|---|
| **Golden / Death Cross** | A faster moving average (e.g. 20-period EMA) crossing above or below a slower one (e.g. 50-period SMA), on any timeframe. |
| **RSI mean-reversion** | Enter when RSI drops below 30 (oversold); exit when RSI climbs back above 70, or at a fixed percentage target from entry. |
| **MACD momentum entry** | Enter when the MACD histogram crosses above zero, or when the MACD line crosses above its signal line. |
| **Multi-timeframe confluence** | Combine a fast trigger (e.g. a 5-minute entry signal) with a slower trend filter (e.g. a 1-hour or 4-hour trend condition) — both must agree before the strategy enters. |
| **Trend-following with a computed stop** | Enter on a SuperTrend or Parabolic SAR flip; protect the position with a trailing stop, or a stop-loss condition that references an ATR-based stop level (such as ATR Stop or Volatility Stop). |

#### Volatility & breakout patterns <a href="#volatility-breakout-patterns" id="volatility-breakout-patterns"></a>

| Pattern | How T2A builds it |
|---|---|
| **Bollinger Band squeeze breakout** | Wait for the bands to compress to a multi-bar low, then enter once price breaks back above the upper band. |
| **Donchian / "Turtle" breakout** | Enter when price closes above the highest high of the last N bars (a classic channel breakout). |
| **Noise-filtered breakout** | Any entry condition can require the underlying condition to hold for several consecutive bars before firing, so a single one-tick wick can't trigger a false breakout. |

#### Risk & sizing patterns <a href="#risk-sizing-patterns" id="risk-sizing-patterns"></a>

| Pattern | How T2A builds it |
|---|---|
| **ATR-based risk sizing** | Size the position using risk-percent sizing, with the stop-loss distance informed by ATR (for example, entry price minus 2× ATR). |
| **Multi-leg profit-taking** | A take-profit ladder — for example, 50% off at +1%, 30% at +2%, and the remaining 20% managed by a trailing stop. |
| **Fixed-dollar limit buy** | "Buy $20 of BTC at 64,000" becomes a limit entry at 64,000, sized as a fixed $20 quote-currency spend — a stated price makes the entry a limit order, and a stated dollar amount sizes it in quote currency. |

#### Repeating & scaling patterns <a href="#repeating-scaling-patterns" id="repeating-scaling-patterns"></a>

| Pattern | How T2A builds it |
|---|---|
| **DCA-style re-entry** | Continuous Re-Arm execution mode with a fixed or percentage-based size and a cooldown between cycles — the strategy automatically resets and waits for the next signal after each cycle completes. |
| **Pyramiding into a trend** | Continuous (Overlapping) execution mode with fixed-quantity sizing and a concurrency cap, so the strategy can add to a winning position as the signal re-fires. |

#### Price-level & mean-reversion patterns <a href="#price-level-mean-reversion-patterns" id="price-level-mean-reversion-patterns"></a>

| Pattern | How T2A builds it |
|---|---|
| **VWAP reversion / momentum** | Compare live price against VWAP for a reversion play, or compute a momentum indicator like RSI directly on top of VWAP for a smoothed read. |
| **Order-book imbalance scalp** | Enter on a heavy bid/ask imbalance in the live order book, combined with a tight price trigger. |
| **Daily-pivot bounce / fade** | Enter on a bounce off a classic daily pivot support level, or fade a move into a Camarilla resistance/support extreme. |

#### Smart-money & structure patterns <a href="#smart-money-structure-patterns" id="smart-money-structure-patterns"></a>

| Pattern | How T2A builds it |
|---|---|
| **Break of Structure (BOS) continuation** | Enter on a bullish break of structure, filtered by a momentum or trend condition; place the stop below the last confirmed swing low. |
| **Change of Character (CHoCH) reversal** | Enter on a genuine swing-pivot change of character after a prevailing trend — a first-class structure event, not an approximation. |
| **Liquidity sweep + fair value gap confluence** | Combine a liquidity sweep (a stop-hunt-and-reclaim) with price trading back into an unfilled fair value gap; take profit at the next pivot resistance level. |
| **Candlestick pattern at a key level** | Enter on a candlestick pattern (for example, a bearish engulfing candle) occurring exactly at a key Fibonacci retracement level. |
| **Order-block pullback** | Enter when price returns to retest the order block that produced the most recent break of structure. |

---

Every pattern above is built purely from the indicators, structure events, and conditions described across the T2A documentation — none require custom code. For the full catalog of building blocks, see [Indicators & Market Data](https://igniz.gitbook.io/igniz-docs/text-to-algo-t2a/nlp-strategy-synthesis/indicators-and-market-data.md) and [Conditions & Trading Signals](https://igniz.gitbook.io/igniz-docs/text-to-algo-t2a/nlp-strategy-synthesis/conditions-and-trading-signals.md); for the exact platform limits, see [Limitations & Technical Reference](https://igniz.gitbook.io/igniz-docs/text-to-algo-t2a/nlp-strategy-synthesis/limitations-and-technical-reference.md).
