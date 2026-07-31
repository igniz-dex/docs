> For the complete documentation index, see [llms.txt](llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](text-to-algo-t2a/nlp-strategy-synthesis/risk-orders-and-position-management.md).

# Risk, Orders & Position Management

Every T2A strategy is more than an entry signal — it's a complete order plan. This page covers how a strategy enters the market, how it takes profit, how it protects against loss, and how much it trades. For how a strategy runs over time once it's live, see [Execution & Automation Settings](text-to-algo-t2a/nlp-strategy-synthesis/execution-and-automation-settings.md).

#### Entry orders <a href="#entry-orders" id="entry-orders"></a>

An entry can be placed as either:

- **Market** — fills immediately at the best available price. This is the default when no price is mentioned.
- **Limit** — fills only at a specified price or better. Naming a concrete level in plain English (for example, "buy at 64,000" or "sell when it hits 70k") automatically makes the entry a limit order at that price.

An entry can also be marked **reduce-only**, meaning it can only ever shrink an existing position, never open or flip one — useful for strategies designed purely to scale out of a position rather than add to it. All exit orders (take-profit and stop-loss legs) are automatically reduce-only on futures, so they can never accidentally increase exposure.

#### Take-profit: the laddered exit <a href="#take-profit-the-laddered-exit" id="take-profit-the-laddered-exit"></a>

Take-profit isn't limited to a single target. A strategy can define up to **5 take-profit legs**, each with its own trigger condition and its own share of the position:

- Each leg claims a **fraction** of the open position (greater than 0%, up to 100%).
- The fractions across all legs must add up to 100% or less — you're never asked to give away more than the position you hold.
- Whichever leg actually fires last automatically closes the **exact remaining quantity** — never leaving a tiny, unclosed "dust" position behind.
- The moment any take-profit leg (or the stop-loss) fires, every other resting exit order for that position is **automatically cancelled** (a one-cancels-other, or OCO, relationship) — there's never a stray order left resting after the position is already closed.
- If a take-profit leg only partially fills, the stop-loss protecting the remaining position is **automatically resized** to match what's actually left open.

A common pattern: take 50% off at +1%, another 30% at +2%, and let a trailing stop manage the final 20% — see [Strategy Examples](text-to-algo-t2a/nlp-strategy-synthesis/strategy-examples.md) for more worked patterns like this.

#### Stop-loss <a href="#stop-loss" id="stop-loss"></a>

A strategy's stop-loss can take one of two forms:

- **Condition-based** — any market condition you can describe (price falling below a moving average, RSI climbing back above 70, a multi-part "and/or" rule) triggers the stop when it becomes true.
- **Trailing** — the stop tracks the best price reached since entry (the highest price for a long, the lowest for a short) and fires once price retraces a chosen distance from that peak, expressed either as a percentage or a fixed amount.

Unlike take-profit, a stop-loss is never partial — whichever kind is used, it always closes **100% of the remaining position** in one action. There's no such thing as a partial stop-out in T2A.

#### Entry expiry <a href="#entry-expiry" id="entry-expiry"></a>

Every launch can optionally carry an **expiry** on the entry phase — a time after which an entry that hasn't triggered yet is automatically cancelled. If the entry is a resting limit order, it's simply pulled from the order book. If the position has already partially filled by the time expiry arrives, the strategy keeps managing that position through to completion regardless — expiry only ever affects an entry that hasn't happened yet. Because expiry is chosen at launch rather than baked into the saved strategy, the same saved strategy can be run for an hour today and run continuously tomorrow.

#### Position sizing <a href="#position-sizing" id="position-sizing"></a>

How much a strategy trades is chosen independently of its logic, at launch (or re-launch) time. Five sizing methods are available:

| Sizing kind | How it works |
|---|---|
| **Absolute** | A fixed quantity of the asset you're trading (the base currency), fixed at the moment you launch. |
| **Quote notional** | A fixed spend in the market's quote currency — e.g. "$20" or "500 USDT." The exact quantity is worked out right when the entry actually triggers, using the price at that moment, so you don't need to know the price in advance. |
| **Balance %** | A percentage of your available balance, calculated at the moment the entry actually triggers — not when the strategy was launched. |
| **Equity %** *(futures only)* | A percentage of your total account equity (margin balance plus any unrealized profit or loss), calculated at the moment the entry triggers. |
| **Risk %** | Sizes the position so that if the stop-loss is hit, you lose exactly the chosen percentage of your balance — the quantity is derived automatically from the distance between your entry price and your stop price. |

**Risk %** requires a stop-loss with a distance that can be pinned down in advance — either a condition-based stop with an explicit price, or a stop expressed as a percentage move from the entry fill. A stop-loss without a fixed, calculable distance (a trailing stop, or an open-ended condition like "RSI crosses back above 70" with no attached price) can't be used to derive a risk-based size, since there's no single distance to divide by.

> **Roadmap:** Equity % sizing is fully validated and accepted when building a strategy, but isn't yet available as a sizing choice when actually launching a live order. This is next on the roadmap.

#### Leverage & margin mode <a href="#leverage-margin-mode" id="leverage-margin-mode"></a>

Futures strategies support leverage up to **125x**. Leverage and margin mode (cross or isolated) are deliberately *not* part of a saved strategy's own logic — instead, a futures order simply inherits whatever leverage and margin mode you currently have configured for that pair at the moment you launch it. This means the same saved strategy behaves predictably and consistently no matter how your margin settings have changed since you last ran it.

#### Safety rails <a href="#safety-rails" id="safety-rails"></a>

| Rail | Limit |
|---|---|
| Maximum take-profit legs | 5 |
| Maximum leverage | 125x |
| Maximum slippage tolerance on market orders | 100% |
| Risk % sizing | Requires a stop-loss with a resolvable, fixed distance |
| Equity % sizing | Futures only; accepted at build time, not yet available at live launch |

For the complete set of platform-wide limits, see [Limitations & Technical Reference](text-to-algo-t2a/nlp-strategy-synthesis/limitations-and-technical-reference.md).
