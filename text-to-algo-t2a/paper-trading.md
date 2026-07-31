> For the complete documentation index, see [llms.txt](https://igniz.gitbook.io/igniz-docs/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://igniz.gitbook.io/igniz-docs/text-to-algo-t2a/paper-trading.md).

# Paper Trading

Paper trading lets you run any T2A strategy **live, against real, real-time market data**, with every fill, fee, funding payment, and liquidation fully simulated — no real orders reach the exchange, and no real wallet is ever touched. It's the natural third step in the T2A workflow:

1. **Generate & Backtest** — describe a strategy in plain English, then check how it would have performed against up to two years of real historical data. See [Backtesting](https://igniz.gitbook.io/igniz-docs/text-to-algo-t2a/backtesting.md).
2. **Paper Trade** — run the strategy for real, right now, watching the live market, with a virtual balance standing in for real capital.
3. **Go Live** — flip the same strategy to real execution once it has proven itself, with zero changes to its logic. See [Live Activation](https://igniz.gitbook.io/igniz-docs/text-to-algo-t2a/live-activation.md).

Where backtesting answers *"how would this have done historically?"*, paper trading answers a different and equally important question: *"is this strategy's logic sound, and does it behave the way I expect, against the market as it is happening right now?"* It catches things a historical replay can't — a strategy that never quite triggers because its condition is subtly too strict, a sizing rule that behaves unexpectedly on the pair's actual current volatility, an entry price that looked fine on paper but the strategy would never actually reach.

The defining guarantee, and the entire point of the feature: **a paper strategy runs through the exact same execution engine, the exact same order lifecycle, and the exact same state machine as a real one.** Entries arm, trigger, and fill; take-profits and stop-losses race each other and cancel their sibling; continuous strategies re-arm for their next cycle — all of it identical to a live order, with one substitution: a realistic simulator stands in for the exchange.

#### Every trade simulated with real-world friction <a href="#every-trade-simulated-with-real-world-friction" id="every-trade-simulated-with-real-world-friction"></a>

A common failure of "paper trading" features elsewhere is that they assume perfect fills at the displayed price, which teaches you nothing about how your strategy will actually behave once real orders start interacting with a real, finite order book. This paper trading engine simulates execution the way a trade would actually happen:

- **Market orders walk the real, live order book.** A simulated market order doesn't fill at a single quoted price — it consumes the pair's actual visible depth level by level, exactly as a real market order would, and pays a realistic price for size that exceeds what's thinly available at the top of the book. A small order in a deep book fills close to the mid-price; a larger order, or one on a thin pair, pays for the size it's actually asking for.
- **A small, deliberate execution delay** models the real-world round trip between deciding to trade and the order actually reaching the exchange — a market can move in that window, and the simulator reflects that instead of pretending decisions execute instantaneously.
- **A limit order isn't guaranteed a fill just because the price was touched.** It fills only once the market has genuinely traded through the level, and — for a price that's merely being tested rather than broken through — the simulator conservatively estimates your position in the order queue at that price level, so a resting order isn't credited a fill faster than a real one waiting in line would have gotten one.
- **Real fees, not estimates.** Every simulated fill is charged your actual maker or taker fee rate — whatever tier you're really on, including any real fee discount or exemption — so the paper P&L reflects exactly what a live account would have paid.
- **A hard floor against unrealistically good fills.** Regardless of how favorable the book looks, a simulated fill is never better than a small guaranteed minimum cost against you — protecting against the classic paper-trading trap of a strategy that looks profitable purely because its simulated fills were too generous.

#### Full futures realism: funding & liquidation <a href="#full-futures-realism-funding-and-liquidation" id="full-futures-realism-funding-and-liquidation"></a>

Futures paper positions are not just spot fills with leverage bolted on — the simulator models the two things that make leveraged trading fundamentally different:

- **Funding payments are simulated on the real, live funding rate and schedule for the pair.** A paper futures position that stays open across a funding interval pays or receives funding exactly as a real position would, and every settlement is recorded on the order's audit trail.
- **Liquidation risk is real, checked continuously.** The simulator tracks the paper position's margin against the pair's actual tiered maintenance-margin schedule on every price update — not once a bar, once a minute. If a paper position would have been liquidated on the real exchange, it is force-closed in the simulation too, at a realistic (adverse) closing price with a liquidation fee applied, and the order's history clearly shows that a liquidation — not a normal exit — is what ended the run. This is the single most important thing separating this feature from a naive fill simulator: over-leveraging a paper strategy has real, visible, disqualifying consequences, exactly as it would with real money.

Both **spot and futures (perpetual)** markets are fully supported, across every pair the exchange lists.

#### A virtual budget, not a shared sandbox balance <a href="#a-virtual-budget-not-a-shared-sandbox-balance" id="a-virtual-budget-not-a-shared-sandbox-balance"></a>

Each paper run gets its **own independent virtual budget**, chosen by you at launch time — there is no single shared "demo wallet." This means:

- You can run several paper strategies at once, each with its own capital, and compare them head-to-head without one strategy's activity affecting another's available balance.
- The budget is denominated in the pair's quote currency (e.g. USDT for a `BTC/USDT` strategy) and is spent, replenished by profitable exits, and drawn down by fees/funding/losses exactly as a real wallet would be for that one run.
- A continuous, self-re-arming strategy shares its budget across its own cycles (the same way a real continuous run shares one real wallet across its cycles) — so sizing rules like "risk 2% of my balance per trade" behave the same way they will once the strategy goes live.
- If a paper run's budget is fully spent or a position becomes unaffordable to size, the run ends cleanly with the same "insufficient balance" outcome a real order would show — there's no invisible overdraft.

If the executor restarts for any reason, a paper run's simulated balance and open position are faithfully reconstructed from its recorded history — a paper strategy is exactly as durable and crash-safe as a real one.

#### It's not a separate feature — it's a mode <a href="#its-not-a-separate-feature-its-a-mode" id="its-not-a-separate-feature-its-a-mode"></a>

Nothing about how a strategy is described, generated, previewed, saved, edited, or managed changes for paper trading. You:

- Generate or pick a saved strategy exactly as always.
- Choose "paper" and a virtual budget at the moment you launch it (or launch for real — identical strategy, identical logic, your choice at that moment).
- See the run in the same order list and detail view as every other T2A order, clearly marked as simulated, with the same live status updates, the same trigger/fill notifications, the same cancel-anytime control, and the same complete audit history.
- Can re-launch the exact same saved strategy for real the moment you're satisfied — with zero changes needed to the strategy itself.

A saved strategy has no memory of having been paper-traded; "paper" is a property of one launch, not a property of the strategy.

#### What's deliberately conservative in v1 <a href="#whats-deliberately-conservative-in-v1" id="whats-deliberately-conservative-in-v1"></a>

In the interest of never showing you a more flattering result than reality would produce, a few choices favor caution over completeness:

| Behavior | Why |
|---|---|
| A resting limit order fills its **full** size at once, never in partial pieces | Modeling true partial-fill queue dynamics without a real trade-by-trade tape would require guessing at a level of precision that could just as easily mislead as help; a conservative full-or-nothing model with a cautious queue estimate is preferred over a falsely precise one. |
| The queue-position estimate for a resting order is intentionally cautious | It assumes your order clears the queue *more slowly* than it optimistically might, so a strategy that "works" in paper trading under this assumption isn't depending on unrealistically fast fills to look good. |
| "Percentage of total equity" sizing is not yet available for paper runs | This mirrors a current limitation of live futures execution itself — paper trading intentionally matches what live trading can do today, rather than simulating a capability live trading doesn't have yet. |
| A funding settlement that occurred while the trading engine was briefly offline is not retroactively charged on restart | An extremely rare edge case (funding intervals are hourly-or-longer); flagged here for completeness rather than silently glossed over. |

None of these affect the core guarantee: a paper strategy's fills, fees, funding, and liquidations are priced realistically, and a strategy that performs well in paper trading is not doing so because the simulation was lenient.

#### Why this matters <a href="#why-this-matters" id="why-this-matters"></a>

Backtesting proves a strategy *would have* worked. Paper trading proves it *does* work, right now, against a live market, with real execution costs — without a single dollar of real risk. It closes the gap between "the historical numbers look good" and "I'm confident enough to run this for real," which is exactly the gap that turns a promising-looking strategy into either a funded live strategy or a strategy sent back to the drawing board — the way it should be, before any real capital is on the line.

#### See also <a href="#see-also" id="see-also"></a>

- [Backtesting](https://igniz.gitbook.io/igniz-docs/text-to-algo-t2a/backtesting.md) — validate a strategy against historical data before paper trading it.
- [Optimization](https://igniz.gitbook.io/igniz-docs/text-to-algo-t2a/optimization.md) — search for better settings before proving them out on paper.
- [Live Activation](https://igniz.gitbook.io/igniz-docs/text-to-algo-t2a/live-activation.md) — re-launch a paper-proven strategy for real, with zero logic changes.
