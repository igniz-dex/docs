> For the complete documentation index, see [llms.txt](https://igniz.gitbook.io/igniz-docs/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://igniz.gitbook.io/igniz-docs/text-to-algo-t2a/backtesting.md).

# Backtesting

Before risking real capital, you can run any T2A strategy against real historical market data and get back an honest, statistically-scrutinized report of how it would have performed — including whether the result looks like a genuine edge or just a lucky stretch.

The defining design principle behind backtesting is that **the engine reuses the exact same decision-making logic as live trading.** The same condition evaluation, the same market-data snapshots, the same position-sizing math, and the same P&L bookkeeping that manage a live order are used, unmodified, inside the backtest. Only two things are swapped out: historical data replaces the live market feed, and a realistic fill simulator replaces real order routing. In practice, this means **what you backtest is what you'll get live** — a strategy's logic never behaves differently between simulation and reality; only realistic trading frictions differ, and those are fully disclosed on every report.

#### Historical data coverage <a href="#historical-data-coverage" id="historical-data-coverage"></a>

- **One-minute candle resolution**, for both **spot and futures (perpetual)** markets — every higher timeframe your strategy needs (5m, 15m, 1h, 4h, and beyond) is derived automatically from this base resolution, exactly as it would be live.
- **Futures-specific data**: historical mark price, index price, and actual funding-rate settlement history are all replayed alongside price.
- **Up to two years of history per run**, with automatic detection of exactly how much history exists for a pair — a backtest never silently under-runs; if the requested range isn't fully available, the report clearly discloses the clamped range rather than guessing.
- Data streams in efficiently so multi-year runs don't require enormous memory or time to prepare.

#### Realistic order & fill simulation <a href="#realistic-order-and-fill-simulation" id="realistic-order-and-fill-simulation"></a>

Every simulated fill goes through a dedicated fill model designed to avoid the classic backtesting trap of unrealistically perfect fills:

- **No look-ahead, ever.** A decision is only made using information available at the close of a candle; any resulting order can only fill on the very next candle — never inside the same bar the decision was made on.
- **Market orders** fill at the next candle's open, with realistic slippage and half-spread cost applied against you — never in your favor.
- **Limit orders** fill at their own price once the market actually trades through that level (not merely touches it), modeling real queue-priority behavior rather than assuming an instant, guaranteed fill.
- **Maker and taker fees are charged correctly** depending on whether an order added or took liquidity, using your real fee tier.
- **Conservative ordering when multiple exits could fire on the same candle** — if it's ambiguous whether a stop-loss or a take-profit would have hit first, the simulation always resolves it in your disfavor, never in your favor.
- **Automatic position downsizing on insufficient funds**, mirroring exactly how the live system behaves when an account can't afford the full requested size.
- **Optional stress dials** — an artificial entry delay, and multipliers on fees/slippage — let you see how sensitive a strategy is to worse-than-normal execution conditions.

Every report also shows and lets you override a default assumption card:

| Assumption | Default |
|---|---|
| Slippage | 5 basis points against you |
| Extra spread cost proxy | 0 basis points (configurable) |
| "Conservative touch" (limit fills require price to trade *through*, not just touch, a level) | On |
| Artificial entry delay | 0 bars |
| Funding simulation | On |
| Extra liquidation fee | 0% (configurable, on top of the normal taker fee) |
| Maker / taker fees | Pulled automatically from your real, current fee tier |

#### Futures simulation: funding & liquidation <a href="#futures-simulation-funding-and-liquidation" id="futures-simulation-funding-and-liquidation"></a>

- **Funding payments** are simulated on the real historical funding schedule and rate for the pair — longs pay shorts (or vice versa) exactly as they would have live.
- **Liquidation risk is fully modeled**, using the pair's real tiered margin-requirement schedule, checked continuously through the simulation.
- **Both isolated and cross margin modes are supported** — isolated checks each position independently; cross checks the whole account's combined risk, and a breach liquidates every open position at once, exactly mirroring live behavior.

#### Deterministic results & rerun <a href="#deterministic-results-and-rerun" id="deterministic-results-and-rerun"></a>

Every backtest is fully reproducible: the same strategy, the same date range, and the same random seed always produce byte-for-byte identical results. There is no hidden randomness or timing dependency in the simulation.

This matters in practice: a shared report, a support investigation, or your own re-run of a test will always show the exact same trades and the exact same numbers — never "it looked different when I ran it again." A one-click **rerun** reproduces any past backtest exactly from its original saved inputs, which is also how you (or an auditor) can independently verify a result wasn't a fluke of that particular run.

#### Performance & risk analytics <a href="#performance-and-risk-analytics" id="performance-and-risk-analytics"></a>

Every completed backtest returns a full, institutional-style performance report:

**Headline returns** — Net Profit, Net Return %, Annualized Return %.

**Risk** — Max Drawdown %, longest drawdown duration, recovery time, Annualized Volatility, Sharpe Ratio, Sortino Ratio, Calmar Ratio, Value-at-Risk (95%) and Conditional VaR / Expected Shortfall, Market Exposure %, Portfolio Turnover.

**Trade statistics** — trade count, win rate, profit factor, average winning/losing trade, profit concentration (are gains spread out, or riding on a few lucky trades?), average holding period, worst single-trade loss.

**Costs** — total fees paid, total funding paid, cost as a share of gross profit.

**Benchmark comparison** — an automatic buy-and-hold comparison for the same pair over the same period, and the strategy's excess return over that benchmark, so every report answers "did this actually beat just holding?"

When a statistic can't be reliably estimated (too little data, no variance), the report shows it as unavailable rather than a misleading zero — never manufacturing false confidence.

#### A full validation suite, not just a single report <a href="#a-full-validation-suite-not-just-a-single-report" id="a-full-validation-suite-not-just-a-single-report"></a>

Beyond a single-run report, backtesting includes a genuine, institutional-grade robustness suite, selectable at three depths:

- **Quick** — the base report plus a locked out-of-sample split.
- **Standard** (the default) — adds walk-forward analysis, stress testing, and market-regime analysis.
- **Deep** — adds full parameter-sensitivity perturbation testing on top of everything in Standard.

**Out-of-sample split** — history is split chronologically (70% / 30% by default, adjustable), and performance on the untouched, later portion is checked against the earlier portion — directly answering "did this only work on the data it was tuned against?" A result is flagged if the return sign flips between the two halves, or if the profit factor collapses to less than 60% of its in-sample value.

**Walk-forward analysis** — history is broken into several consecutive windows (5 by default), and consistency across each window is reported individually — how many of the windows were profitable, and how much the results varied — showing whether performance holds up across different market periods rather than depending on one lucky stretch.

**Parameter sensitivity testing** (Deep) — the strategy's own numeric settings (thresholds, trailing distances, indicator lookbacks, take-profit/stop percentages) are nudged by ±10% and ±20% and re-tested, producing a **0–100 stability score**. A strategy is automatically flagged **unstable** if a small ±10% nudge flips the result's sign or wipes out more than half of a positive return.

**Stress testing** — the strategy is automatically re-run with fees at 0×/2×/3× normal, slippage at 2×/4× normal, an added one-bar entry delay, and against several randomized start dates — reporting plainly whether the strategy "survives costs" even at the harshest setting, and whether its results are sensitive to the exact day it happened to start on.

**Market-regime analysis** — every closed trade is automatically bucketed into one of four market conditions, with trade count, total profit, and win rate reported for each, so you can see immediately whether a strategy only works in one type of market. The regime for any given day is read directly off the market itself, not guessed: **trend** is "up" when the 5-day average price is at or above the 20-day average (and "down" otherwise), and **volatility** is "high" whenever the trailing 7-day realized volatility is above its own historical median for the run (and "low" otherwise) — giving a clean, self-calibrating uptrend/downtrend × high-vol/low-vol grid with no arbitrary hardcoded thresholds.

**Multi-asset portfolio backtesting** — up to **five** different pairs/strategies can be combined and tested together as a single portfolio (each on its own capital slice, weights summing to 100% or less, remainder held as cash), complete with a **daily-return correlation matrix** showing how much true diversification the combination actually provides.

#### The complete quality-flag checklist <a href="#the-complete-quality-flag-checklist" id="the-complete-quality-flag-checklist"></a>

Every run is automatically screened for the following conditions, in plain English, so nothing questionable is left for you to notice on your own:

| Flag | What it's warning about |
|---|---|
| Zero fees assumed | The run didn't include trading fees — real results would be lower. |
| Zero slippage assumed | The run didn't include any slippage — real fills are rarely this clean. |
| Too few trades | Fewer than 30 trades occurred — not enough to trust the statistics. |
| Sample period too short | Less than 90 days of history was tested. |
| Profit concentration | A handful of trades account for most (or all) of the profit. |
| Out-of-sample degradation | Performance meaningfully worsened on the untouched portion of history — a classic overfitting sign. |
| Parameter instability | Small nudges to the strategy's own settings destroy the result. |
| High turnover | The strategy trades unusually often relative to its capital. |
| High leverage | The run used more than 10x leverage. |
| Liquidated in simulation | The simulated account got liquidated at least once during the run. |
| Order-book conditions unknown in history | Some of the strategy's conditions depend on live order-book data that isn't available historically, so they never triggered in this simulation. |
| Date range clamped | The requested date range exceeded the history actually available and was automatically shortened. |
| Warm-up range clamped | Not enough lead-in data existed for indicators to fully warm up over the full requested range. |
| Data gaps detected | Gaps were found in the historical price data for this period. |
| Ended with an open position | The strategy still held a position when the backtest window ended. |
| Account wiped out | The simulated account balance was fully wiped out during the run. |
| Unusually many tunable parameters | The strategy has more tunable numeric settings than usual, which raises overfitting risk if it's later optimized. |
| No exit rules defined | The strategy has neither a take-profit nor a stop-loss. |
| Fails under stressed costs | The strategy stops being profitable once fees/slippage are stress-tested higher. |

Every report also carries standard risk disclosures (hypothetical results, not investment advice, past performance doesn't guarantee future results) alongside plain-English narrated explanations of anything unusual the run turned up.

#### From backtest to live, seamlessly <a href="#from-backtest-to-live-seamlessly" id="from-backtest-to-live-seamlessly"></a>

A completed backtest can generate a ready-to-launch live (or paper-trading) order request — the same strategy, the same sizing, the same execution settings — so if you like what you see, you can go live with one click rather than re-entering everything. If a report carries serious quality warnings, that handoff is intentionally held back until you've seen and acknowledged them. See [Live Activation](https://igniz.gitbook.io/igniz-docs/text-to-algo-t2a/live-activation.md) for the full handoff flow.

#### The complete backtesting toolkit <a href="#the-complete-backtesting-toolkit" id="the-complete-backtesting-toolkit"></a>

| Action | What it does |
|---|---|
| **Submit** | Launch a new backtest (single-pair or up to a 5-leg portfolio) against a chosen date range, capital, sizing, execution mode, and validation depth. |
| **Status** | Poll (or receive a live push of) a running job's progress percentage and current stage. |
| **List** | Browse all of your past and running backtests. |
| **Report** | Pull the full report for a completed job — metrics, validation suite results, quality flags, equity curve. |
| **Trades CSV** | Export the complete, untruncated trade-by-trade detail. |
| **Rerun** | Reproduce a past job byte-for-byte from its original stored inputs and seed. |
| **Cancel** | Stop a running job (honored at the next processing checkpoint). |
| **Share** | Publish a completed report as a read-only, anonymous public link. |
| **Public report** | View a report via someone else's shared link. |
| **Quota** | Check current daily usage and concurrency limits. |

#### Built for scale <a href="#built-for-scale" id="built-for-scale"></a>

| Control | Behavior |
|---|---|
| Max history per run | Up to 2 years, auto-detected and clamped to what's actually stored |
| Max simulated bars per run | ~1.1 million (about 2 years of 1-minute data) |
| Concurrent backtests per user | A small, configurable number running at once |
| Queued backtests per user | A small, configurable number waiting in line |
| Daily backtest quota | A sensible default per user (admin-adjustable per user/tier) |
| Report size | Equity/benchmark curves are downsampled to a manageable point count (always keeping the exact first/last points); the displayed trade list is capped, with the full list always available via CSV |

Backtests run as background jobs with live progress updates, so long historical ranges never block or time out your session. Portfolio legs are simulated one at a time (not in parallel) specifically to keep memory bounded, regardless of how many legs or how much history is requested. The system automatically recovers and re-queues any job if a server process is interrupted mid-run — your request is never silently lost.

#### Current boundaries <a href="#current-boundaries" id="current-boundaries"></a>

- Simulation runs on candle data, not a full historical order book — strategies that can *only* ever trigger from live order-book depth aren't eligible for backtesting (this is checked and reported up front, not discovered after the fact).
- Only full fills are simulated in this release; partial fills are on the roadmap.
- Spot "sell" strategies are simulated as reducing/re-entering a holding, not as true short-selling — appropriate for spot markets, and clearly labeled as such in every report.
- Cross-pair strategies aren't yet eligible for backtesting (they are supported live, in progressive rollout).

#### See also <a href="#see-also" id="see-also"></a>

- [Optimization](https://igniz.gitbook.io/igniz-docs/text-to-algo-t2a/optimization.md) — search a strategy's settings for better-performing combinations, with the same overfitting scrutiny.
- [Paper Trading](https://igniz.gitbook.io/igniz-docs/text-to-algo-t2a/paper-trading.md) — validate a backtested strategy live, against real-time data, with zero real risk.
- [Live Activation](https://igniz.gitbook.io/igniz-docs/text-to-algo-t2a/live-activation.md) — turn a validated backtest into a real, running order.
