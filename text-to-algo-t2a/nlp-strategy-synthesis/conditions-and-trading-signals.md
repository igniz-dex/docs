> For the complete documentation index, see [llms.txt](https://igniz.gitbook.io/igniz-docs/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://igniz.gitbook.io/igniz-docs/text-to-algo-t2a/nlp-strategy-synthesis/conditions-and-trading-signals.md).

# Conditions & Trading Signals

Indicators and market data (see the [previous page](https://igniz.gitbook.io/igniz-docs/text-to-algo-t2a/nlp-strategy-synthesis/indicators-and-market-data.md)) are the raw values a strategy reads. This page covers how those values get turned into actual trading signals: comparisons, boolean logic, recognized chart patterns, divergence, and time-based filters.

### Comparison Operators <a href="#comparison-operators" id="comparison-operators"></a>

A condition compares two values using one of eight operators:

`<`, `<=`, `>`, `>=`, `==`, `!=`, **crosses above**, **crosses below**

`==` and `!=` are only meaningful for exact matches and are rejected when comparing continuous market values (like price or an indicator reading), since floating-point values essentially never land on an exact number — threshold comparisons or crossover detection are used instead.

**Crosses above / crosses below** require both sides of the comparison to carry bar history (price, an indicator, or a raw candle field) — an order-book metric has no history and can't be used in a crossover check.

### Holding for Persistence <a href="#holding-for-persistence" id="holding-for-persistence"></a>

Any comparison can add a **"hold for N bars"** requirement, so the condition must stay true for a chosen number of consecutive checks before it counts — useful for filtering out one-bar noise, e.g. "RSI above 70 for 3 bars running" instead of a single-tick spike. This doesn't apply to crossover comparisons, since a cross is inherently a one-bar event.

### Boolean Logic <a href="#boolean-logic" id="boolean-logic"></a>

Conditions combine with standard boolean logic:

- **AND** / **OR** — 1 to 8 operands each, nestable to build arbitrarily rich "and/or/but not" logic.
- **NOT** — negates a single condition.
- **Always** — unconditionally true; valid only as an entry condition, for immediate "buy now" style strategies.

### Named Patterns (25) <a href="#named-patterns" id="named-patterns"></a>

T2A recognizes chart patterns the way a discretionary trader would — not just raw indicator math. A pattern only counts once it has fully confirmed on a **closed candle**; the currently-forming candle never triggers a false signal. Every pattern's sensitivity is independently tunable (e.g. how many bars confirm a swing pivot, how large a wick must be relative to the body), so you aren't locked into one rigid definition.

#### Smart Money Concepts (12) <a href="#smart-money-concepts" id="smart-money-concepts"></a>

| Pattern | What It Detects |
|---|---|
| **Bullish Break of Structure** | Price closes above the last confirmed swing high while the trend is already up — a continuation signal. |
| **Bearish Break of Structure** | Price closes below the last confirmed swing low while the trend is already down — a continuation signal. |
| **Bullish Change of Character** | Price closes above the last swing high while the trend was down — an early reversal-up signal. |
| **Bearish Change of Character** | Price closes below the last swing low while the trend was up — an early reversal-down signal. |
| **Bullish Liquidity Sweep** | Price pierces below the last swing low on the wick but closes back above it — a classic stop-hunt-and-reclaim. |
| **Bearish Liquidity Sweep** | Price pierces above the last swing high on the wick but closes back below it. |
| **Bullish Fair Value Gap Created** | A three-candle imbalance forms where a gap is left below current price — an untested demand zone. |
| **Bearish Fair Value Gap Created** | The mirror-image imbalance forms above current price — an untested supply zone. |
| **Price in Bullish Fair Value Gap** | Price is currently trading back inside an unfilled bullish gap — a common pullback-entry zone. |
| **Price in Bearish Fair Value Gap** | Price is currently trading back inside an unfilled bearish gap. |
| **Bullish Order Block Tap** | Price returns to retest the candle that launched the most recent bullish break of structure. |
| **Bearish Order Block Tap** | Price returns to retest the candle that launched the most recent bearish break of structure. |

#### Candlestick Patterns (13) <a href="#candlestick-patterns" id="candlestick-patterns"></a>

| Pattern | What It Detects |
|---|---|
| **Bullish Engulfing** | An up candle whose body fully swallows the prior down candle's body. |
| **Bearish Engulfing** | A down candle whose body fully swallows the prior up candle's body. |
| **Hammer** | A long lower wick with a small body near the top — rejection of lower prices. |
| **Shooting Star** | A long upper wick with a small body near the bottom — rejection of higher prices. |
| **Doji** | Open and close are nearly identical — a classic sign of market indecision. |
| **Bullish Marubozu** | An up candle with almost no wicks at all — one-sided, aggressive buying. |
| **Bearish Marubozu** | A down candle with almost no wicks at all — one-sided, aggressive selling. |
| **Morning Star** | A three-candle bullish reversal: a long down candle, a small indecisive candle, then a strong up candle. |
| **Evening Star** | The bearish mirror image of the Morning Star. |
| **Three White Soldiers** | Three consecutive strong up candles — powerful bullish continuation. |
| **Three Black Crows** | Three consecutive strong down candles — powerful bearish continuation. |
| **Bullish Harami** | A small up candle tucked inside the prior large down candle's body — downside momentum stalling. |
| **Bearish Harami** | A small down candle tucked inside the prior large up candle's body — upside momentum stalling. |

### Divergence Detection <a href="#divergence-detection" id="divergence-detection"></a>

Divergence — when price and a momentum indicator disagree — is one of the most-requested discretionary concepts, and T2A supports all four variants as a first-class condition, checked against **any** oscillator in the indicator toolkit (RSI, MACD, MFI, and more):

| Type | Fires When |
|---|---|
| **Regular Bullish** | Price makes a lower low, but the oscillator makes a higher low — weakening downside momentum, a classic reversal-up signal. |
| **Regular Bearish** | Price makes a higher high, but the oscillator makes a lower high — weakening upside momentum, a classic reversal-down signal. |
| **Hidden Bullish** | Price makes a higher low while the oscillator makes a lower low — a continuation signal in an uptrend. |
| **Hidden Bearish** | Price makes a lower high while the oscillator makes a higher high — a continuation signal in a downtrend. |

Divergence is checked over a recency window of **1–20 bars**, using a swing-pivot strength of **2–10 bars** to define what counts as a turning point. Like structure patterns, divergence is **tri-state**: it never reports a false "no divergence" during warm-up — only "not enough data yet" versus a definite true or false once enough swing pivots exist.

### Tri-State Evaluation on Closed Bars <a href="#tri-state-evaluation-on-closed-bars" id="tri-state-evaluation-on-closed-bars"></a>

Named patterns and divergence checks only ever evaluate against **fully closed candles** — the bar still forming on the chart is never used, so a pattern can't fire off a half-formed candle and "un-happen" a moment later. Until enough history exists to confirm or rule out a pattern, the condition reports **unknown** rather than guessing false — so `NOT` logic can never accidentally fire on missing data.

### Session & Time-of-Day Filters <a href="#session-and-time-of-day-filters" id="session-and-time-of-day-filters"></a>

A strategy can be restricted to specific trading windows — up to **8 per strategy** — each independently day-of-week aware and able to wrap past midnight (e.g. 22:00–04:00). This is how "only trade during the London session" or "never trade on weekends" gets enforced automatically. Named sessions (London, New York, Asia) are converted to explicit time windows automatically; all windows run on UTC.

### Cross-Pair Conditions <a href="#cross-pair-conditions" id="cross-pair-conditions"></a>

A condition can reference market data from a **different pair** than the one being traded — for example, "buy BTC/USDT when ETH/USDT's RSI drops below 30." Cross-pair conditions are feature-gated and rolling out progressively rather than available to every trader at once. Where enabled:

- The referenced pair must be the same market kind (spot or futures) as the strategy's own pair.
- Up to **3 distinct foreign pairs** can be referenced per strategy.
- Indicators, price, and raw candle data can all reference a foreign pair — order-book metrics are single-pair only, since there's no cross-pair order-book data.
