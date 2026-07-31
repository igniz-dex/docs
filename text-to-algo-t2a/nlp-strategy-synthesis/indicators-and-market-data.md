> For the complete documentation index, see [llms.txt](llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](text-to-algo-t2a/nlp-strategy-synthesis/indicators-and-market-data.md).

# Indicators & Market Data

T2A strategy conditions draw on a technical analysis toolkit of **69 indicators** across six categories, plus a set of non-indicator market-data inputs — live price, raw candle data, order-book microstructure, and more. This page is the complete catalog.

### Composability <a href="#composability" id="composability"></a>

A few rules apply across every indicator:

- **Timeframe** — every indicator is computed independently on any of **10 timeframes**: `1m`, `5m`, `15m`, `30m`, `1h`, `2h`, `4h`, `6h`, `12h`, `1d`. A single strategy can freely mix timeframes — for example, a fast 5-minute trigger gated by a slower 1-hour trend filter.
- **Lookback** — each indicator reads a configurable lookback of **0–200 bars**, so conditions like "yesterday's close" or "RSI three bars ago" don't need a dedicated primitive.
- **Chaining** — an indicator's input source defaults to the candle close, but can be set to any raw candle field, or to **another indicator's output** — indicators can be chained on top of each other up to **3 levels deep** (e.g. *RSI computed on VWAP*, or *Bollinger Bands computed on an EMA*).
- **Chainable vs. OHLC-only** — indicators marked **Chainable** below produce a single value series and can be nested inside another indicator as its source. Indicators marked **OHLC-only** need full candle data and can only sit as the outermost node of a chain — though they can still be nested *inside* a chainable parent (e.g. `EMA(ATR(14))` is valid).
- **Outputs** — an indicator with more than one named output (like MACD's line, signal, and histogram) lets a condition pick exactly which one to read.

### Trend & Moving Averages (18) <a href="#trend-and-moving-averages" id="trend-and-moving-averages"></a>

| Indicator | What It Measures | Outputs | Kind |
|---|---|---|---|
| **SMA** (Simple Moving Average) | Arithmetic mean of the last N bars — the standard baseline trend line. | sma | Chainable |
| **EMA** (Exponential Moving Average) | Weighted average favoring recent prices — reacts faster than SMA. | ema | Chainable |
| **DEMA** (Double EMA) | Reduces the lag of a standard EMA via a two-pass smoothing technique. | dema | Chainable |
| **TEMA** (Triple EMA) | Even less lag than DEMA, via a three-pass smoothing technique. | tema | Chainable |
| **WMA** (Weighted Moving Average) | Linear weighting — the most recent bar counts the most. | wma | Chainable |
| **HMA** (Hull Moving Average) | A very smooth moving average with minimal lag, using weighted differences. | hma | Chainable |
| **ALMA** (Arnaud Legoux Moving Average) | Gaussian-weighted moving average — very smooth with low lag. | alma | Chainable |
| **KAMA** (Kaufman's Adaptive Moving Average) | Automatically speeds up in trending markets and slows down in choppy ones. | kama, efficiency ratio | Chainable |
| **T3** (Tillson's T3) | A triple-smoothed, volume-aware moving average — extremely smooth with low lag. | t3 | Chainable |
| **SMMA** (Smoothed / Wilder's Moving Average) | A slower-reacting EMA variant — the same smoothing method RSI and ADX use internally. | smma | Chainable |
| **EPMA** (Endpoint Moving Average) | Projects a linear-regression best-fit line forward to the latest bar. | epma | Chainable |
| **MAMA** (MESA Adaptive Moving Average) | An advanced adaptive moving average (with a paired "follow" line) that speeds up and slows down with the market's cycle. | mama, fama | Chainable |
| **ADX** (Average Directional Index) | Measures how strongly a market is trending, regardless of direction (above 25 = trending). | adx, +DI, −DI | OHLC-only |
| **Vortex** | Compares positive vs. negative directional movement to gauge trend direction. | positive, negative | OHLC-only |
| **Parabolic SAR** | A trend-following stop level that flips sides at market extremes — a classic trailing-stop tool. | sar, reversal flag | OHLC-only |
| **SuperTrend** | A widely-used volatility-based trend overlay, popular for signaling entries. | superTrend, upper band, lower band | OHLC-only |
| **Ichimoku Cloud** | A complete multi-line trend system (Tenkan, Kijun, cloud span A/B, lagging span) showing trend, momentum, and support/resistance at a glance. | tenkan, kijun, span A, span B, lagging span | OHLC-only |
| **Elder Ray** | Splits price action into "bull power" and "bear power" to show which side is in control. | bull power, bear power | OHLC-only |

### Momentum (17) <a href="#momentum" id="momentum"></a>

| Indicator | What It Measures | Outputs | Kind |
|---|---|---|---|
| **RSI** (Relative Strength Index) | The classic 0–100 momentum gauge — above 70 is overbought, below 30 is oversold. | rsi | Chainable |
| **ROC** (Rate of Change) | Simple percent change over N bars. | roc, momentum | Chainable |
| **CMO** (Chande Momentum Oscillator) | A momentum gauge on a −100…+100 scale. | cmo | Chainable |
| **TSI** (True Strength Index) | A double-smoothed momentum oscillator with its own signal line. | tsi, signal | Chainable |
| **TRIX** | A triple-smoothed rate-of-change indicator that flags trend reversals. | trix, signal | Chainable |
| **PMO** (Price Momentum Oscillator) | A smoother alternative to MACD. | pmo, signal | Chainable |
| **Awesome Oscillator** | Compares a fast and slow moving average of the mid-price to gauge market momentum shifts. | oscillator | Chainable |
| **Connors RSI** | A composite of RSI, a "streak" RSI, and a percentile rank — a more responsive short-term momentum read. | connorsRsi, rsi, streak, percent rank | Chainable |
| **Fisher Transform** | Reshapes price into a bell-curve-like distribution so sharp turning points stand out clearly. | fisher, trigger | Chainable |
| **Stochastic RSI** | The Stochastic formula applied to RSI itself — a faster-firing signal than plain RSI. | stochRsi, signal | Chainable |
| **Williams %R** | A −100…0 momentum gauge; above −20 is overbought, below −80 is oversold. | williamsR | OHLC-only |
| **Stochastic Oscillator** | Classic %K/%D crossover momentum indicator — above 80 overbought, below 20 oversold. | k, d, j | OHLC-only |
| **CCI** (Commodity Channel Index) | Measures how far price has strayed from its recent average; beyond ±100 signals a strong move. | cci | OHLC-only |
| **Balance of Power** | Gauges whether buyers or sellers are in control, on a −1…+1 scale. | bop | OHLC-only |
| **Ultimate Oscillator** | Blends momentum across three different timeframes into one signal. | ultimate | OHLC-only |
| **SMI** (Stochastic Momentum Index) | A refined, double-smoothed version of the Stochastic Oscillator. | smi, signal | OHLC-only |
| **Aroon** | Measures how long it's been since the recent high and low, to gauge trend strength and direction. | aroon up, aroon down, oscillator | OHLC-only |

### Composite / Trend-Cycle (6) <a href="#composite-trend-cycle" id="composite-trend-cycle"></a>

| Indicator | What It Measures | Outputs | Kind |
|---|---|---|---|
| **MACD** | The classic Moving Average Convergence Divergence — trend and momentum in one, with a signal line and histogram. | macd, signal, histogram | Chainable |
| **DPO** (Detrended Price Oscillator) | Strips the trend out of price to highlight underlying cycles. | dpo | Chainable |
| **STC** (Schaff Trend Cycle) | Combines MACD and Stochastic logic for a sharper trend-turn signal than MACD alone. | stc | Chainable |
| **ROC with Bands** | Rate of change plus a volatility envelope around it. | roc, upper band, lower band | Chainable |
| **Linear Regression Slope** | The steepness of the best-fit trend line over N bars, plus how well price actually fits that line. | slope, r-squared | Chainable |
| **Hurst Exponent** | Tells you whether a market is currently trending, mean-reverting, or behaving randomly. | hurst exponent | Chainable |

### Volatility (9) <a href="#volatility" id="volatility"></a>

| Indicator | What It Measures | Outputs | Kind |
|---|---|---|---|
| **Bollinger Bands** | A moving average with bands set by recent volatility — also reports %B and Z-score for "how stretched is price right now." | sma, upper band, lower band, %B, Z-score | Chainable |
| **Standard Deviation** | Rolling statistical volatility of price. | stdDev, mean | Chainable |
| **Ulcer Index** | Measures the depth and duration of drawdowns — a "how much pain" volatility gauge. | ulcer index | Chainable |
| **ATR** (Average True Range) | The industry-standard volatility measure in price units — commonly used to size stops. | atr | OHLC-only |
| **ATR Stop** | A ready-made trailing stop level derived directly from ATR. | atr stop | OHLC-only |
| **Keltner Channel** | An EMA-based volatility envelope, generally smoother/less choppy than Bollinger Bands. | centerline, upper band, lower band | OHLC-only |
| **Donchian Channel** | The highest-high/lowest-low envelope over N bars — the classic breakout indicator. | centerline, upper band, lower band | OHLC-only |
| **Choppiness Index** | A 0–100 read on whether the market is trending or just chopping sideways. | chop | OHLC-only |
| **Volatility Stop** | A Wilder-style trailing stop that also flags exactly when a reversal happens. | stop level, reversal flag | OHLC-only |

### Volume (12) <a href="#volume" id="volume"></a>

| Indicator | What It Measures | Outputs | Kind |
|---|---|---|---|
| **OBV** (On-Balance Volume) | Running cumulative volume, added or subtracted based on price direction. | obv | OHLC-only |
| **Money Flow Index** | A volume-weighted version of RSI — often nicknamed "volume RSI." | mfi | OHLC-only |
| **Chaikin Money Flow** | An accumulation/distribution oscillator on a −1…+1 scale. | cmf | OHLC-only |
| **Force Index** | Combines price change and volume into a single momentum reading. | force index | OHLC-only |
| **Chaikin Oscillator** | MACD logic applied to the Accumulation/Distribution Line. | oscillator | OHLC-only |
| **VWAP** (Volume Weighted Average Price) | The running average price, weighted by volume, from the start of the data. | vwap | OHLC-only |
| **Rolling VWAP** | A moving-window version of VWAP with volume-weighted bands around it — unlike plain VWAP, it never "resets from the beginning" and stays relevant on any lookback window. | vwap, upper band, lower band | OHLC-only |
| **Volume Profile** | Identifies the price level with the heaviest trading activity (point of control) and the value area around it, approximated from candle volume (not a tick-by-tick trade tape). | point of control, value area high/low | OHLC-only |
| **VWMA** (Volume Weighted Moving Average) | A moving average weighted by volume instead of just time. | vwma | OHLC-only |
| **Klinger Volume Oscillator** | Tracks longer-term volume flow trends, with its own signal line. | oscillator, signal | OHLC-only |
| **PVO** (Percentage Volume Oscillator) | MACD logic applied to volume instead of price. | pvo, signal, histogram | OHLC-only |
| **ADL** (Accumulation/Distribution Line) | A running cumulative money-flow indicator. | adl | OHLC-only |

### Pivots & Structure Levels (7) <a href="#pivots-and-structure-levels" id="pivots-and-structure-levels"></a>

These are price *levels* to compare against, rather than oscillators. Pivot ladders are computed from the prior window of candles (a daily timeframe with a 1-window lookback gives classic daily pivots); swing-derived levels confirm only once enough bars have passed on each side of a turning point.

| Indicator | What It Measures | Outputs |
|---|---|---|
| **Classic Pivot Points** | The traditional floor-trader pivot with three resistance and three support levels, from the prior period's high/low/close. | pivot, R1–R3, S1–S3 |
| **Fibonacci Pivots** | Pivot ladder spaced using Fibonacci ratios (0.382 / 0.618 / 1.0) of the prior period's range. | pivot, R1–R3, S1–S3 |
| **Camarilla Pivots** | A tighter, intraday-focused reversal ladder (four levels each side) anchored to the prior close. | pivot, R1–R4, S1–S4 |
| **Woodie Pivots** | A pivot variant that gives extra weight to the prior close. | pivot, R1–R2, S1–S2 |
| **DeMark Pivots** | A pivot calculation that adapts based on whether the prior period closed above or below its open. | pivot, R1, S1 |
| **Fibonacci Retracement & Extension** | Retracement levels (23.6% / 38.2% / 50% / 61.8% / 78.6%) of the most recent confirmed price swing, plus extension targets (127.2% / 161.8% / 261.8%) projecting beyond it for measured-move profit targets. | retracement levels, extension levels, swing high/low, direction |
| **Swing Structure** | The last confirmed swing high and swing low, plus the current trend read (higher-highs/higher-lows, lower-highs/lower-lows, or mixed) — the foundation for structure-aware entries and stops. | last swing high, last swing low, trend |

### Beyond Indicators: Market Data Inputs <a href="#beyond-indicators-market-data-inputs" id="beyond-indicators-market-data-inputs"></a>

A strategy's conditions can also reference market data directly, without going through a named indicator:

#### Live Price <a href="#live-price" id="live-price"></a>

The live last-trade price for the pair. It carries its own short bar history, so it's a valid input for crossover comparisons.

#### Raw Candle Fields <a href="#raw-candle-fields" id="raw-candle-fields"></a>

Any of the raw OHLCV candle fields — **open, high, low, close, volume** — plus five derived midpoints: `hl2` ((high+low)/2), `hlc3` ((high+low+close)/3), `oc2` ((open+close)/2), `ohl3` ((open+high+low)/3), and `ohlc4` ((open+high+low+close)/4). Available on any of the 10 timeframes with any lookback — e.g. "today's close above yesterday's high."

#### Order-Book Microstructure <a href="#order-book-microstructure" id="order-book-microstructure"></a>

Live snapshot metrics from the order book, for strategies that want to react to liquidity conditions rather than just price:

| Metric | What It Computes |
|---|---|
| **Spread** | Best ask minus best bid. |
| **Mid-price** | Midpoint between best bid and best ask. |
| **Weighted mid-price** | A volume-weighted midpoint across the top N levels each side (depth 1–50) — more stable than the raw mid-price. |
| **Imbalance** | Ratio of total bid quantity to total ask quantity over the top N levels each side (depth 1–50) — a read on buy vs. sell pressure. |
| **Bid depth** | Total resting bid quantity at or above a chosen price. |
| **Ask depth** | Total resting ask quantity at or below a chosen price. |

Order-book metrics have no bar history, so they can't be used in a crossover comparison and can't be nested inside an indicator.

#### Arithmetic <a href="#arithmetic" id="arithmetic"></a>

Simple arithmetic — add, subtract, multiply, divide — between any two market-data values, for derived expressions with no dedicated primitive of their own (e.g. spread as a percent of mid-price, or the raw gap between two moving averages).

#### Percent From My Entry <a href="#percent-from-my-entry" id="percent-from-my-entry"></a>

A side-aware target price expressed as a percentage move from wherever the entry actually filled — used for take-profit and stop-loss legs. It's resolved automatically once the entry has a fill, and the sign is applied correctly whether the position is long or short.

#### Futures-Only Scalars <a href="#futures-only-scalars" id="futures-only-scalars"></a>

On futures pairs only, a strategy can reference live scalar values: **funding rate**, **mark price**, and **index price** — enabling funding-aware entries and exits, like avoiding holding a short into deeply negative funding.
