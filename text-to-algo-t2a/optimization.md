> For the complete documentation index, see [llms.txt](https://igniz.gitbook.io/igniz-docs/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://igniz.gitbook.io/igniz-docs/text-to-algo-t2a/optimization.md).

# Optimization

Once a strategy exists, the natural next question is: *could slightly different settings perform better?* The T2A Optimizer answers that automatically — it searches a strategy's tunable settings (indicator lookbacks, thresholds, trailing-stop distances, and similar numeric knobs) for combinations that perform better historically, running hundreds of backtests behind the scenes.

What sets it apart from a naive "try everything" grid search is what happens *after* the search: a genuine quantitative-finance robustness stack that actively tries to catch and flag results that only look good because the search ran hard enough to stumble onto a lucky number — rather than because the strategy has a real edge.

#### How the search works <a href="#how-the-search-works" id="how-the-search-works"></a>

The optimizer doesn't test every possible combination — it searches intelligently, using proven optimization algorithms:

- **Latin-Hypercube warm-up** — the first batch of trials is spread deliberately evenly across the entire settings space, so the search starts with broad, representative coverage instead of clumping by chance.
- **Tree-structured Parzen Estimator (TPE)** — the default search strategy once warm-up is done. It learns which regions of the settings space tend to perform well versus poorly, and increasingly proposes new combinations likely to perform well, while still exploring less-tested areas.
- **CMA-ES** — an optional, more sophisticated evolutionary search that learns not just *where* good settings cluster but the *shape* of that region (including which settings move together), well suited to smoother, more continuous strategies.
- **Automatic mode** — by default, the optimizer picks the right approach for you, starting broad and narrowing intelligently, with no configuration required.

You can also explicitly pick the search algorithm: **auto** (the default), **tpe**, **cmaes**, or **lhs** (Latin-Hypercube coverage for the entire run, not just the warm-up). A "sobol" option is also accepted for compatibility with the equivalent low-discrepancy technique some traders may be familiar with — it currently runs on the same even-coverage engine as `lhs`.

**Smart early-stopping (ASHA — Asynchronous Successive Halving)** — rather than spending equal time evaluating every candidate, the optimizer runs a three-tier ladder of increasingly thorough looks:

| Tier | How much history is used | How coarse the check is |
|---|---|---|
| First look | 25% of the date range | Coarse (30-minute resolution) |
| Second look | 50% of the date range | Medium (15-minute resolution) |
| Final look | 100% of the date range | Full resolution |

Only the **top third** of candidates at each tier get promoted to the next, more expensive one — so a clearly weak setting is dropped after a cheap 25%-scale look, while a genuinely promising one earns a full, careful evaluation. The practical benefit: the same time budget covers a far larger search, because obviously weak candidates are dropped early instead of eating compute a strong candidate could have used.

#### What it's optimizing for <a href="#what-its-optimizing-for" id="what-its-optimizing-for"></a>

Every candidate setting is scored on a blend of seven factors, not just raw historical profit:

| Factor | What it rewards or penalizes | Default weight |
|---|---|---|
| Probabilistic Sharpe Ratio | Rewards genuine, statistically-credible risk-adjusted return | 1.0 (heaviest factor) |
| Annualized Growth (CAGR) | Rewards strong compounding | 0.5 |
| Max Drawdown | Penalizes deep losses | 0.5 |
| Turnover | Penalizes excessive trading (a proxy for cost/impact) | 0.1 |
| Return Instability | Penalizes lumpy, uneven performance over time | 0.3 |
| Cost Sensitivity | Penalizes strategies where fees/funding eat too much of the edge | 0.3 |
| Generalization Gap | Penalizes a strategy that looks much better on its training data than on data it wasn't tuned against — the classic overfitting fingerprint | 0.5 |

You can re-weight how much each factor matters (for example, prioritizing drawdown protection over raw growth) without needing to understand the statistics behind each one — the defaults above reflect a deliberate "prove the edge is real first, growth and smoothness second" philosophy.

#### The overfitting-protection stack <a href="#the-overfitting-protection-stack" id="the-overfitting-protection-stack"></a>

This is the optimizer's signature capability: a set of genuine quantitative-finance safeguards, run automatically on every job, designed to answer the question every trader should be asking but usually can't: *is this real, or did I just get lucky?*

- **Probabilistic Sharpe Ratio (PSR)** — estimates the probability that a strategy's true edge is actually positive, properly accounting for how much data there is and how lumpy/skewed the returns look — rather than trusting a raw Sharpe number that a short or unusual sample can distort.
- **Deflated Sharpe Ratio (DSR)** — the more skeptical check: given how many different settings the search tried, what result would show up purely by chance? DSR raises the bar accordingly, so a winning result has to clear a higher standard the more exhaustively the search was run — directly protecting against "search long enough and you'll eventually find something that looks good."
- **Probability of Backtest Overfitting (PBO), via Combinatorially-Symmetric Cross-Validation (CSCV)** — the published Bailey, Borwein, López de Prado & Zhu (2017) technique for exactly this problem. The historical timeline is cut into 16 blocks, and every possible symmetric way of using half the blocks as "training" and the other half as "testing" is checked (12,870 combinations) — for each one, does the setting that looked best on the training half still look good on the testing half? A high PBO is a direct warning that the search process itself is likely to have overfit, and that live results might not repeat what the backtest showed. This check runs across a broad, representative sample of **40 distinct configurations spanning both winners and losers** from the whole search — not just the handful of eventual finalists — for an honest, whole-search view of overfitting risk.
- **Confidence intervals via block-bootstrap resampling** — rather than a single headline number, the optimizer redraws the strategy's return history **1,000 times** in randomly-sized, order-preserving chunks (so short-term streakiness and autocorrelation are preserved, not erased the way naive shuffling would) and reports the realistic 95% range this produces for Sharpe ratio and annualized growth, plus the typical and worst-case (95th-percentile) drawdown across those redraws, and the outright probability that the true Sharpe ratio is zero or negative.
- **Parameter-stability (plateau) testing** — the winning settings are nudged in **8 random directions**, each dimension shifted by up to ±10%, and every nudged neighbor is re-scored. If the neighbors' average performance holds up close to the original (high "retention"), and few of them flip from profitable to unprofitable, the result is flagged as sitting on a stable plateau rather than a fragile, razor's-edge spike.
- **Market-regime coverage** — checks that a winning strategy isn't only profitable in one narrow type of market, using the same self-calibrating uptrend/downtrend × high-vol/low-vol classification used in [Backtesting](https://igniz.gitbook.io/igniz-docs/text-to-algo-t2a/backtesting.md) (5-day vs. 20-day price trend, 7-day realized volatility vs. its own median).
- **Cost-shock stress test** — automatically re-runs the candidate with trading fees and slippage all increased by **50%**, to check whether the edge survives materially higher, more realistic trading costs rather than just looking good at today's fee schedule.
- **A locked, never-touched holdout period** — a final slice of history is set aside and never used by any part of the search or selection process, so its result is reported as the closest thing to a genuinely unbiased "how would this really have performed" figure — clearly distinguished on the report from the more optimistic in-sample numbers.

#### The Reality-Check Gate panel <a href="#the-reality-check-gate-panel" id="the-reality-check-gate-panel"></a>

Every finalist is run through seven concrete pass/fail checks before it's presented as trustworthy:

| Gate | Checks that... | Default threshold |
|---|---|---|
| G1 — Statistical edge | The Deflated Sharpe Ratio is real, not noise | Better than a coin-flip (≥ 0.5) after correcting for how many settings were tried |
| G2 — Search overfitting | The search process itself doesn't show signs of overfitting | Probability of Backtest Overfitting ≤ 20% |
| G3 — Sample size | Enough out-of-sample trades exist to trust the statistics | At least 100 out-of-sample trades |
| G4 — Cost resilience | The strategy stays profitable even under stressed trading costs | Must survive the cost-shock stress test |
| G5 — Parameter stability | Performance is stable across nearby parameter values, not a lucky exact spot | At least 70% of a small parameter nudge's performance is retained, and no more than 25% of nearby variants flip the result's sign |
| G6 — Market-condition coverage | The strategy works across more than one type of market | Profitable in at least 2 of the 4 regime buckets, with no single bucket accounting for more than 90% of total profit (only enforced once there are enough trades to judge fairly) |
| G7 — Generalization | Performance on unseen data is consistent with performance on the data it was tuned against | Validation-to-out-of-sample degradation stays under 30% |

A candidate can post an excellent raw historical return and still be marked as **failing** one or more of these gates — by design, the system is built to surface real weaknesses rather than hide them behind an impressive headline number. Every threshold above is a configurable default, not a hard rule.

#### Configuring a run <a href="#configuring-a-run" id="configuring-a-run"></a>

- Start from an existing saved strategy or a brand-new one.
- Choose the historical window to search against — minimum 2 weeks (rejected below that), with a friendly warning under 90 days that results may be statistically thin, and a sensible maximum (about a year by default).
- Set a trial budget: **300 trials by default**, adjustable up to a **1,000-trial hard cap**; trials run in small batches (4 at a time by default) for efficient, orderly processing.
- Optionally set a **wall-clock time budget** — if it expires mid-search, the job finalizes as cancelled rather than silently returning a partial, misleading result.
- Choose how many top candidates to see in the leaderboard: **5 by default, up to 10**.
- **Freeze or narrow specific settings** — lock a setting at its current value, or restrict its search range, rather than letting the optimizer roam freely over everything.
- **Preview the tunable settings for free**, before committing a paid optimization run, to see exactly which numbers are adjustable and their current values and ranges.
- Optionally include an **ensemble** — an automatically blended combination of the top several qualifying results, for a more diversified candidate than any single winner.

Under the hood, every run reserves a **locked 20% out-of-sample tail** of the chosen history that the search itself never optimizes against, a **30% train/validation split** inside the remaining search region, and a further **7% never-gated holdout** carved from the very end of the out-of-sample tail — the one slice of history no gate, search step, or selection decision ever touches, giving the winning candidate's holdout numbers real, unbiased weight.

The optimizer automatically identifies every sensible numeric setting inside a strategy to tune — indicator lookback lengths, comparison thresholds, trailing-stop distances, timing windows, and pattern-strength settings — so you never have to manually enumerate what's tunable. Between 2 and 64 tunable settings are required for a run (too few or too many are rejected up front as not meaningfully optimizable).

#### The complete optimizer toolkit <a href="#the-complete-optimizer-toolkit" id="the-complete-optimizer-toolkit"></a>

| Action | What it does |
|---|---|
| **Preview search space** | See every tunable setting, its current value, and its search range — free, before spending a run. |
| **Submit** | Launch a new optimization run with a chosen trial budget, objective weights, gate thresholds, and search-space overrides. |
| **Status** | Check a running job's progress percentage and current stage. |
| **List** | Browse all of your past and running optimization jobs. |
| **Report** | Pull the full report for a completed job — leaderboard, gate results, Pareto front, holdout metrics. |
| **Trials CSV** | Export every single trial tried, in full. |
| **Best doc** | Get the winning candidate's strategy logic as a ready-to-use package for a fresh backtest, a saved strategy, or a live launch. |
| **Rerun** | Reproduce a past job byte-for-byte from its original stored inputs and seed. |
| **Cancel** | Stop a running job. |
| **Share** | Publish a completed report as a read-only, anonymous public link. |
| **Public report** | View a report via someone else's shared link. |
| **Quota** | Check current usage and concurrency limits. |

#### What you get back <a href="#what-you-get-back" id="what-you-get-back"></a>

- A **ranked leaderboard** of the best-performing, most-credible settings found, each with its full statistical profile and pass/fail status on every reality-check gate.
- A **trade-off frontier** (Pareto front) showing the best available balance between return, drawdown, and trading activity — so you can pick a more conservative or more aggressive variant deliberately, not just accept "the single best number."
- **Never-gated holdout results** for the top pick — the one number set in the whole report that no part of the search or selection process ever touched.
- **Plain-English flags and explanations** automatically attached to the report, calling out anything you should know before trusting the result (for example, if no candidate passed every reality check, or if the winning number carries some inherent selection bias from how finalists were chosen).
- **One-click promotion** — take the winning settings straight into a fresh backtest for final confirmation, save it as a reusable strategy, or launch it live, without re-entering anything by hand. See [Live Activation](https://igniz.gitbook.io/igniz-docs/text-to-algo-t2a/live-activation.md) for how the launch itself works.

#### Built for scale <a href="#built-for-scale" id="built-for-scale"></a>

- Runs as a background job so a large search (hundreds of trials) never ties up your session.
- Sensible per-user concurrency limits keep the system responsive for everyone.
- Job status is tracked durably, so a server interruption never silently loses your in-progress optimization — it's automatically picked back up.

#### Current boundaries & what's next <a href="#current-boundaries-and-whats-next" id="current-boundaries-and-whats-next"></a>

- **Multi-objective search** (optimizing several goals at once and returning a full trade-off frontier natively, rather than the current post-hoc Pareto front) is built and tested internally, and is planned for a future release rather than available today.
- Position size, leverage, and take-profit proportions are intentionally **not** tuned by the optimizer in this release — only the strategy's condition logic (indicators, thresholds, timing) is searched, keeping risk sizing firmly in your control.
- The optimizer does not yet estimate how much capital a strategy can absorb before its own trading starts moving the market (capacity/market-impact analysis) — trading activity is used as a rough proxy today.
- Progress on a running optimization is available by checking status rather than an instant live push — large searches intentionally run on dedicated backend capacity rather than the trader-facing web tier, to keep the rest of the platform responsive.

#### See also <a href="#see-also" id="see-also"></a>

- [Backtesting](https://igniz.gitbook.io/igniz-docs/text-to-algo-t2a/backtesting.md) — the same validation suite and quality-flag checklist used to confirm an optimizer finalist.
- [Paper Trading](https://igniz.gitbook.io/igniz-docs/text-to-algo-t2a/paper-trading.md) — validate an optimizer's winning settings live, against real-time data, with zero real risk.
- [Live Activation](https://igniz.gitbook.io/igniz-docs/text-to-algo-t2a/live-activation.md) — one-click promotion of a winning configuration into a real, running order.
