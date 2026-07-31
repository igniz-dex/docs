> For the complete documentation index, see [llms.txt](llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](text-to-algo-t2a/nlp-strategy-synthesis.md).

# NLP Strategy Synthesis

This is the part of T2A that turns what you type into what actually runs: a plain-English description of an entry, a take-profit, and a stop-loss becomes a precise, validated strategy — with every step designed so nothing gets lost, misread, or silently guessed in translation.

### The Synthesis Pipeline <a href="#the-synthesis-pipeline" id="the-synthesis-pipeline"></a>

1. **You describe your intent** — separate free-text fields for the entry condition, the take-profit logic, and the stop-loss logic, plus a few simple structured choices (pair, side, entry order type, optional limit price, expiry, notification preferences).
2. **An AI model drafts the strategy** from your description.
3. **The draft is strictly validated** against the exact same rulebook the live trading engine enforces — every condition, indicator, and threshold has to check out.
4. **You get a plain-English confirmation screen** that describes exactly what will run.
5. **You can preview it for free** — a dry run against the current market, at no cost to your capital or your generation quota.
6. **Guardrails apply before anything is spent** — oversized requests are rejected up front, and anything you type is sanitized so it can never be mistaken for a system instruction.
7. **Nothing is trusted twice** — the strategy is independently re-checked at preview time, launch time, and again if it's saved or edited.

### Multi-Vendor AI Models <a href="#multi-vendor-ai-models" id="multi-vendor-ai-models"></a>

T2A isn't locked to a single AI provider. Depending on the request, it can draw on OpenAI, Anthropic, Google, or an in-house-hosted model — and the platform can add new models to its catalog over time without changing how the feature works for you.

### Self-Correction <a href="#self-correction" id="self-correction"></a>

If the AI's first attempt at a strategy doesn't pass validation, the specific reason is fed back to it automatically, and it gets **up to three chances** to self-correct before you ever see an error. Most requests resolve cleanly well before that limit is reached.

If a description names something T2A genuinely can't express, the system is built to be honest about it — it will either map your request onto the closest supported concept or return a clear explanation of what couldn't be understood, rather than quietly running the wrong strategy.

### The Confirmation Screen <a href="#the-confirmation-screen" id="the-confirmation-screen"></a>

The summary you see before approving a strategy isn't written freeform by the AI — it's rendered directly from the final, validated strategy logic itself. What you read is a guaranteed, word-for-word description of what will actually run, not a hopeful paraphrase of what the AI thinks it built.

### Free Preview / Dry Run <a href="#free-preview-dry-run" id="free-preview-dry-run"></a>

Before committing any capital, you can preview a strategy against the current market: whether each condition would already be true right now, and how close the market is to triggering it. Preview is free — it draws on the same light rate limit as generation, not the limit that governs actually placing an order.

### Guardrails & Re-Validation <a href="#guardrails-re-validation" id="guardrails-re-validation"></a>

- Oversized or malformed requests are rejected before any AI cost is spent.
- Free-text fields are sanitized so nothing you type can be mistaken for a system instruction to the AI.
- A strategy is independently re-validated at preview time, at launch time, and again whenever it's saved or edited — so a stale or tampered draft can never slip through.

### Rails & Limits <a href="#rails-limits" id="rails-limits"></a>

| Control | Limit |
|---|---|
| AI self-correction attempts | Up to 3 per generation |
| Free-text field length (entry / take-profit / stop-loss) | Up to 2,000 characters each |
| AI strategy generations | 3 per day or 3 per hour (one window applies at a time) |
| Preview / dry run | Shares the light generation rate limit — not the order-creation limit |

### Building a Strategy <a href="#building-a-strategy" id="building-a-strategy"></a>

- [Strategy Overview](text-to-algo-t2a/nlp-strategy-synthesis/strategy-overview.md) — the entry / take-profit / stop-loss mental model.
- [Indicators & Market Data](text-to-algo-t2a/nlp-strategy-synthesis/indicators-and-market-data.md) — the full 69-indicator toolkit and every other market-data input.
- [Conditions & Trading Signals](text-to-algo-t2a/nlp-strategy-synthesis/conditions-and-trading-signals.md) — comparators, boolean logic, chart patterns, divergence, and session filters.
- [Risk, Orders & Position Management](text-to-algo-t2a/nlp-strategy-synthesis/risk-orders-and-position-management.md) — order types, the take-profit ladder, stop-loss, and position sizing.
- [Execution & Automation Settings](text-to-algo-t2a/nlp-strategy-synthesis/execution-and-automation-settings.md) — execution modes, cooldowns, notifications, and monitoring.
- [Strategy Examples](text-to-algo-t2a/nlp-strategy-synthesis/strategy-examples.md) — ready-to-build patterns mapping trader concepts to primitives.
- [Limitations & Technical Reference](text-to-algo-t2a/nlp-strategy-synthesis/limitations-and-technical-reference.md) — the exact numeric limits, known gaps, and roadmap.
