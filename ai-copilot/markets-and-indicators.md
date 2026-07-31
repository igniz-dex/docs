> For the complete documentation index, see [llms.txt](https://igniz.gitbook.io/igniz-docs/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://igniz.gitbook.io/igniz-docs/ai-copilot/markets-and-indicators.md).

# AI Copilot — Markets & Indicators

#### Market Intelligence Suite <a href="#market-intelligence-suite" id="market-intelligence-suite"></a>

Fully available — every market data capability is read-only, so there's no restriction here versus a fully-authorized external agent:

- Trading pair & currency reference data, including supported blockchain networks for deposits/withdrawals.
- Live order book depth, best bid/ask with live spread, and the recent public trade tape.
- 24-hour ticker statistics for any pair or the whole market at once.
- Current funding rates for perpetual contracts.
- Historical candlestick (OHLC) data across **13 selectable timeframes** — 1m, 5m, 10m, 30m, 1h, 2h, 3h, 6h, 12h, 1d, 2d, 3d, and 1w.
- Market screeners — top gainers, top losers, and trending (highest-volume) pairs.

> *Ask Igniz:* "What's the current spread on BTC/USDT?" · "Show me the top gainers today." · "Pull up 1-hour candles for SOL/USDT for the last week."

#### Technical Analysis Engine — 60+ Indicators On Demand <a href="#technical-analysis-engine" id="technical-analysis-engine"></a>

Also fully available in-chat. Ask for any indicator — or several chained together — computed live over any pair and timeframe. Copilot can even build **indicators on top of other indicators** in a single request (e.g., "a 20-period moving average of the RSI").

**The full indicator catalog:**

| Category | Indicators |
|---|---|
| **Trend & Moving Averages** | Simple, Exponential, Double/Triple Exponential, Weighted, Hull, Arnaud Legoux, Kaufman Adaptive, Tillson T3, Smoothed (Wilder's), Endpoint (linear-regression), and MESA Adaptive moving averages · ADX / trend-strength (+DI/−DI) · Vortex Indicator · Parabolic SAR · SuperTrend · Ichimoku Cloud · Elder Ray Index |
| **Momentum** | RSI · Rate of Change (with smoothed variant) · Chande Momentum Oscillator · True Strength Index · TRIX · Price Momentum Oscillator · Awesome Oscillator · Connors RSI · Fisher Transform · Stochastic RSI · Williams %R · Stochastic Oscillator · CCI · Balance of Power · Ultimate Oscillator · Stochastic Momentum Index · Aroon Up/Down/Oscillator |
| **Volatility** | Bollinger Bands (with %B and Z-score) · Rolling Standard Deviation · Ulcer Index · Average True Range · ATR Trailing Stop · Keltner Channel · Donchian Channel · Choppiness Index · Volatility Stop |
| **Volume** | On-Balance Volume · Money Flow Index · Chaikin Money Flow · Force Index · Chaikin Oscillator · VWAP and Rolling VWAP (with volatility bands) · Volume Profile (Point of Control, Value Area) · Volume-Weighted Moving Average · Klinger Volume Oscillator · Percentage Volume Oscillator · Accumulation/Distribution Line |
| **Composite / Advanced** | MACD · Detrended Price Oscillator · Schaff Trend Cycle · ROC with volatility bands · Linear Regression Slope (with R²) · Hurst Exponent (classifies a market as trending, mean-reverting, or random-walk) |
| **Pivots & Structure** | Classic, Fibonacci, Camarilla, Woodie, and Demark pivot-point ladders · Fibonacci retracement & extension levels off the last confirmed swing · Swing structure detection (last confirmed high/low and trend classification) |

Copilot can also simply be asked "what indicators can you calculate?" and it returns the full catalog with plain-language descriptions — its analytical capabilities are always self-documenting. Indicators that need extra history to stabilize (e.g., a 200-period average) have that warm-up handled invisibly, so results are always clean. Where a calculation is an approximation (e.g., Volume Profile from candle data rather than tick-level trades), Copilot says so plainly.

> *Ask Igniz:* "What indicators can you calculate?" · "Show me RSI, MACD, and Bollinger Bands on ETH/USDT, 4-hour." · "Give me a 9-period average of the MACD histogram on BTC."
