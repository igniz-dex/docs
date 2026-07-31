> For the complete documentation index, see [llms.txt](llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](text-to-algo-t2a/nlp-strategy-synthesis/execution-and-automation-settings.md).

# Execution & Automation Settings

A T2A strategy's *logic* — its entry, take-profit, and stop-loss conditions — is saved once and reused. How that logic actually *runs* over time is chosen separately, every time you launch or re-launch a strategy. That separation means the same saved strategy can be run as a single one-off trade today and as a continuously repeating automation tomorrow, with no changes to its underlying rules. For the order and sizing details referenced below, see [Risk, Orders & Position Management](text-to-algo-t2a/nlp-strategy-synthesis/risk-orders-and-position-management.md).

#### Execution modes <a href="#execution-modes" id="execution-modes"></a>

Every launch picks one of three execution modes:

| Mode | Behavior |
|---|---|
| **Single Order** | One entry, its exits, then done. The strategy completes once the position is closed. |
| **Continuous Re-Arm** | After a cycle fully closes out flat, the strategy automatically re-arms and waits for the entry condition to fire again. Only one cycle is ever open at a time — ideal for repeatable, DCA-style setups. |
| **Continuous (Overlapping)** | A new cycle can start as soon as the previous entry has triggered, even before that cycle has closed — enabling pyramiding-style strategies that build into a position across multiple triggers. |

**Overlapping cycles** are capped at a maximum of **3 concurrent cycles** by default, and are restricted to fixed-quantity sizing — **absolute** or **quote notional** only — so that each cycle spends a fixed, known amount and overlapping cycles can never silently compound risk off a shared balance.

#### Cooldown between cycles <a href="#cooldown-between-cycles" id="cooldown-between-cycles"></a>

Both continuous modes support a configurable cooldown between cycles — a minimum pause enforced after one cycle before the next can begin. The floor is **5 seconds**, guaranteeing a strategy can never rapid-fire orders back-to-back with zero pause between them.

#### Notifications <a href="#notifications" id="notifications"></a>

Four independent toggles control what you're notified about, all switched **on by default**:

- Entry triggered
- Exit triggered
- Strategy completed
- Strategy failed

Turning a notification off only affects whether you're pinged — every event is still permanently recorded to the audit trail regardless of your notification preferences (see below).

#### Real-time status & the order lifecycle <a href="#real-time-status-the-order-lifecycle" id="real-time-status-the-order-lifecycle"></a>

Every strategy streams its status to you live, in real time, as it happens. A single order's lifecycle is tracked end-to-end through stages such as: created → synthesized → (validation failed, if applicable) → entry evaluated → entry triggered → entry fill(s) → fully filled → exit armed → take-profit triggered/filled → stop-loss triggered/filled → completed / cancelled / failed.

Alongside these core stages, T2A also surfaces side events as they occur — a child order being created (or failing to be created), an automatic OCO cancellation of a sibling exit, a slippage-guard skip, an automatic expiry cancellation, a trailing-stop watermark update, a manual-close detection, and — for continuous strategies — a new cycle spawning or an entire run being cancelled at once.

#### Permanent audit trail <a href="#permanent-audit-trail" id="permanent-audit-trail"></a>

Every event in a strategy's lifecycle is permanently logged and retrievable per-order, so you (or support) can always reconstruct exactly why a strategy did what it did:

| Record type | Retention |
|---|---|
| Lifecycle events | 180 days |
| Detailed logs | 90 days |

For the platform-wide rate limits and safety rails that govern how frequently you can create, cancel, or edit strategies, see [Limitations & Technical Reference](text-to-algo-t2a/nlp-strategy-synthesis/limitations-and-technical-reference.md).
