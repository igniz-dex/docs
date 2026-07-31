> For the complete documentation index, see [llms.txt](llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](text-to-algo-t2a/live-activation.md).

# Live Activation

Every T2A strategy eventually faces the same moment: turning a validated idea into a real, running order. Whichever path got you there — a [backtest](text-to-algo-t2a/backtesting.md) you're satisfied with, an [optimizer](text-to-algo-t2a/optimization.md) run's winning settings, or a strategy you've proven out in [paper trading](text-to-algo-t2a/paper-trading.md) — going live is the same final step: the strategy's logic is frozen exactly as validated, and it starts running for real, against your real balance, on the real exchange.

#### What changes when you go live <a href="#what-changes-when-you-go-live" id="what-changes-when-you-go-live"></a>

Nothing about a strategy's condition logic changes at launch — entry, take-profit, and stop-loss rules carry over unmodified from whatever validated them. What you *do* choose at the moment of launch (or re-launch) is:

- **Position size** — absolute quantity, quote notional, percent of available balance, percent of total equity (futures), or risk-based sizing derived from your stop distance.
- **Entry expiry** — an optional time after which an entry that hasn't triggered yet is automatically cancelled; a resting limit entry is simply pulled from the book, while a position that has already partially filled keeps being fully managed to completion regardless of expiry.
- **Execution mode** — **Single Order** (one entry, its exits, done), **Continuous Re-Arm** (the strategy resets and waits for the next signal once a cycle closes out flat, with only one cycle open at a time), or **Continuous / Overlapping** (a new entry can start before the prior cycle closes, for pyramiding-style strategies, capped at a safe number of concurrent cycles and restricted to fixed-quantity sizing).
- **Notifications** — four independent, opt-out toggles (all on by default): entry triggered, exit triggered, completed, and failed. Every event is still recorded to the permanent audit trail regardless of notification preference.
- **Leverage & margin mode** (futures) — not part of a saved strategy's own logic at all; a live order simply inherits whatever leverage and margin mode (cross or isolated) you currently have configured for that pair at the moment you launch it, so the same saved strategy behaves predictably no matter how your margin settings have changed since you last ran it.

Because these choices are made at launch rather than baked into the strategy itself, the exact same saved strategy can be run once today with a small size and a short expiry, and run again tomorrow continuously with a completely different size — with no editing required.

#### One-click promotion from Backtesting and Optimization <a href="#one-click-promotion-from-backtesting-and-optimization" id="one-click-promotion-from-backtesting-and-optimization"></a>

You never have to re-describe or re-enter a strategy to go live:

- A completed [backtest](text-to-algo-t2a/backtesting.md) can generate a ready-to-launch live (or paper-trading) order request directly — the same strategy, the same sizing, the same execution settings shown in the report — so you can go live with one click rather than re-entering everything.
- A completed [optimization](text-to-algo-t2a/optimization.md) run lets you take its winning settings straight into a fresh backtest for final confirmation, save them as a reusable strategy, or launch them live directly — without re-entering anything by hand.

#### Paper-to-live is a launch-time choice, not a strategy change <a href="#paper-to-live-is-a-launch-time-choice-not-a-strategy-change" id="paper-to-live-is-a-launch-time-choice-not-a-strategy-change"></a>

[Paper trading](text-to-algo-t2a/paper-trading.md) runs a saved strategy through the exact same execution engine, order lifecycle, and state machine as a real order — the only substitution is a realistic simulator standing in for the exchange. "Paper" versus "live" is decided at the moment you launch, not stored on the strategy itself: a saved strategy carries no memory of ever having been paper-traded. Once you're satisfied with how a strategy behaved against the live market in paper mode, you re-launch the exact same saved strategy for real — with zero changes needed to its logic.

#### The quality-warning gate <a href="#the-quality-warning-gate" id="the-quality-warning-gate"></a>

Promotion from a backtest to a live (or paper) order is deliberately held back in one specific case: if a backtest report carries serious quality warnings — too few trades, a too-short sample period, out-of-sample degradation, parameter instability, and the other flags in the [backtesting quality-flag checklist](text-to-algo-t2a/backtesting.md#the-complete-quality-flag-checklist) — the one-click handoff to a live order is intentionally paused until you have seen and acknowledged those warnings. A strategy that looks good only on paper, in a way its own report already flags as questionable, is never silently waved through to real capital.

#### Live monitoring, cancel-anytime, and the audit trail <a href="#live-monitoring-cancel-anytime-and-the-audit-trail" id="live-monitoring-cancel-anytime-and-the-audit-trail"></a>

Once live, a strategy streams real-time status to you as it happens over a live push connection, with each order tracking a full lifecycle: created → synthesized → entry evaluated → entry triggered → entry fill(s) → fully filled → exit armed → take-profit triggered/filled → stop-loss triggered/filled → completed / cancelled / failed — plus side events such as a child order being created (or failing to be created), an OCO sibling being cancelled, a slippage-guard skip, an automatic expiry cancellation, a trailing-stop watermark update, a manual-close detection, or — for continuous strategies — a new cycle spawning and a whole run being cancelled at once.

You can cancel a single order, or an entire continuous/recurring run, at any time. Every one of these events is permanently recorded (audit log retained 180 days for events, 90 days for detailed logs) and retrievable per-order, so you — or support — can always reconstruct exactly why a strategy did what it did.

#### Safety rails once live <a href="#safety-rails-once-live" id="safety-rails-once-live"></a>

| Control | Default |
|---|---|
| Max open T2A orders per user | 50 |
| Max open T2A orders per pair | 5,000 |
| New-order rate limit | 30 per minute |
| Cancel rate limit | 60 per minute |
| Consecutive slippage-guard skips before forcing a stop through | 3 |
| Consecutive child-order failures before auto-fail | 3 |

A few automatic protections apply specifically to live and paper orders:

- **Pre-flight slippage guard** — every market order checks that the order book is deep enough before firing; if it's too thin, the system waits rather than accepting a bad fill. After 3 misses in a row, though, a stop-loss is always forced through at market anyway, because a stop that never fires is worse than a little slippage.
- **Child-order auto-fail** — if a take-profit or stop-loss leg fails to place 3 times in a row, the strategy stops retrying and marks itself failed rather than silently leaving a position unprotected.
- **Manual-close detection** — if you close a position manually outside of T2A, or a spot balance changes independently, the system notices the mismatch and gracefully retires the strategy instead of acting on stale information.
- **Fail-safe order creation** — if the rate-limiting system itself has an outage, order creation fails safe (blocked): an outage can never be used to spam unlimited orders. Cancelling and generating strategies fail open instead, so a temporary system hiccup can never trap you in a position you're trying to exit, or burn a paid generation quota.

#### See also <a href="#see-also" id="see-also"></a>

- [Backtesting](text-to-algo-t2a/backtesting.md) — validate a strategy against historical data and generate a one-click launch request.
- [Optimization](text-to-algo-t2a/optimization.md) — search for better settings and promote the winner straight to a live order.
- [Paper Trading](text-to-algo-t2a/paper-trading.md) — prove a strategy live against real-time data before committing real capital.
