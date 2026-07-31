> For the complete documentation index, see [llms.txt](https://igniz.gitbook.io/igniz-docs/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://igniz.gitbook.io/igniz-docs/text-to-algo-t2a/nlp-strategy-synthesis/limitations-and-technical-reference.md).

# Limitations & Technical Reference

T2A is built with explicit, published safety rails rather than hidden caps — the table below is the complete reference for every platform limit. Beneath it is an honest list of what T2A doesn't yet support, and the closest way to approximate it today.

#### Exact platform limits <a href="#exact-platform-limits" id="exact-platform-limits"></a>

| Limit | Default |
|---|---|
| Max open T2A orders per user | 50 |
| Max open T2A orders per pair | 5,000 |
| Max saved strategies per user | 100 |
| Max strategy complexity (logic nodes) | 64 |
| Max indicator chaining depth | 3 |
| Max operands per AND/OR group | 8 |
| Max indicator lookback | 200 bars |
| Max take-profit legs | 5 |
| Max time-filter windows | 8 |
| Max leverage | 125x |
| Max slippage tolerance | 100% |
| Max referenced foreign pairs (cross-pair) | 3 |
| Consecutive child-order failures before auto-fail | 3 |
| Consecutive slippage-guard skips before forcing a stop through | 3 |
| AI strategy generations | 3 per day, or 3 per hour (one window applies at a time) |
| New-order rate limit | 30 per minute |
| Cancel rate limit | 60 per minute |
| Save / edit / rename / delete rate limit | 20 per minute |

#### A deliberate safety asymmetry <a href="#a-deliberate-safety-asymmetry" id="a-deliberate-safety-asymmetry"></a>

Two safety choices are intentionally asymmetric:

- **Order creation fails safe (blocked).** If the rate-limiting system itself has an outage, new orders are blocked rather than allowed through — an outage can never be exploited to spam unlimited orders.
- **Cancelling and generating strategies fail open.** The same kind of outage never blocks a cancellation or a new generation attempt — a temporary system hiccup can never trap you in a position you're trying to exit, or waste a paid generation attempt you didn't get to use.

Two further automatic protections sit alongside the rate limits:

- A **pre-flight slippage guard** checks that the order book is deep enough before a market order fires; if it's too thin, the system waits rather than accepting a bad fill. After 3 consecutive misses, however, a stop-loss is always forced through at market anyway — a stop-loss that never fires is worse than a small amount of slippage.
- **Manual-close detection** — if a position is closed manually outside of T2A, or a spot balance changes independently, the system detects the mismatch and gracefully retires the strategy rather than continuing to act on stale information.

#### Known gaps <a href="#known-gaps" id="known-gaps"></a>

T2A is honest about what it can't yet do. If a strategy description names one of the concepts below, T2A either declines with a clear explanation or substitutes the closest available proxy — it will never silently fabricate an unsupported feature.

| Concept | Status | Closest available proxy today |
|---|---|---|
| **Supply / demand zones** (as distinct from order blocks) | Not yet supported | Order-block tap patterns cover the related, narrower case; broader institutional supply/demand zones have no dedicated primitive yet. |
| **Tick-level Volume Profile (VPVR)** | Approximation only | Volume Profile is computed from candle data (spreading each candle's volume across its high-low range), giving a close approximation of point of control and value area — not a tick-by-tick trade tape. |
| **Open interest / long-short ratio** | Not yet exposed | Not currently available as a strategy input; funding rate, mark price, and index price are already supported for futures strategies. |
| **Non-UTC session timezones** | UTC only | Time-of-day filters run on UTC windows; named sessions (London, New York, Asia) are automatically converted to UTC at strategy-creation time, but daylight-saving handling isn't yet automatic. |
| **Multi-pair basket / spread trading** | Partial | A strategy can reference one other pair per condition (cross-pair mode, capped at 3 distinct pairs), letting you compose custom spreads or ratios — but there's no true multi-pair basket weighting yet. |

#### What's next <a href="#whats-next" id="whats-next"></a>

- **Cross-pair strategies** are fully built and working, and are being rolled out progressively rather than switched on for every trader at once.
- **Percentage-of-equity position sizing** on futures is already accepted and validated when a strategy is built, but isn't yet available as a launch option for a live order — see [Risk, Orders & Position Management](https://igniz.gitbook.io/igniz-docs/text-to-algo-t2a/nlp-strategy-synthesis/risk-orders-and-position-management.md).
- **Open Interest** as a strategy input is on the roadmap, alongside the funding rate, mark price, and index price inputs already available today.
- **Daylight-saving-aware session filters** are a documented future improvement over today's UTC-only windows.

For execution modes, notifications, and the audit trail, see [Execution & Automation Settings](https://igniz.gitbook.io/igniz-docs/text-to-algo-t2a/nlp-strategy-synthesis/execution-and-automation-settings.md). For worked examples of common strategy patterns, see [Strategy Examples](https://igniz.gitbook.io/igniz-docs/text-to-algo-t2a/nlp-strategy-synthesis/strategy-examples.md).
