> For the complete documentation index, see [llms.txt](/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](/ai-chart-analysis/analysis-types.md).

# AI Chart Analysis — Analysis Types

### Recognized Analysis Keywords <a href="#recognized-keywords" id="recognized-keywords"></a>

Analysis Mode recognizes a set of common analysis keywords and maps each one to a supported analysis type. You can use a keyword on its own or inside a longer sentence — both work identically.

| Keyword | Mapped analysis type |
|---|---|
| Market structure | `market_structure` |
| Trend | `trend` |
| Range / Consolidation | `range` |
| Breakout | `breakout` |
| Failed breakout | `failed_breakout` |
| Pullback | `pullback` |
| Retest | `retest` |
| Reversal | `reversal` |
| Support / Resistance | `support_resistance` |
| Swing analysis | `swing_analysis` |
| Fibonacci retracement | `fibonacci_retracement` |
| Fibonacci extension | `fibonacci_extension` |
| Momentum | `momentum` |
| Volume | `volume` |
| Volatility | `volatility` |
| Risk map | `risk_map` |
| Multi-timeframe context | `multi_timeframe` |

Every keyword in this table maps to a fully supported analysis type — there are no recognized-but-unsupported keywords. If you type something the system doesn't recognize, it routes to the unsupported-request handling described in [Reliability & Limits](/ai-chart-analysis/reliability-and-limits.md).

Examples:

```text
Breakout
```

```text
Analyze the latest pullback.
```

```text
Show market structure, momentum, and nearby resistance.
```

### Supported Scope <a href="#supported-scope" id="supported-scope"></a>

All keyword-mapped analysis types are fully supported:

- Market structure
- Trend
- Support and resistance
- Range and consolidation
- Breakout
- Failed breakout
- Pullback
- Retest
- Reversal
- Swing analysis
- Fibonacci retracement
- Fibonacci extension
- Momentum
- Volume confirmation
- Volatility (including ATR-based extension)
- Risk map — invalidation levels, reaction zones, and measured-risk distances; these are analytical objects only, never trade instructions
- Multi-timeframe context

### When You Don't Specify an Analysis Type <a href="#automatic-selection" id="automatic-selection"></a>

You don't have to name a specific analysis type. If you leave it unspecified, the AI selects one or more common and applicable analyses on your behalf, based on the chart and your preferences.

```text
Analyze this chart.
```

A typical automatic selection for a request like that:

- Current market structure
- Active trend
- Key support and resistance
- The most relevant breakout, range, or pullback
- A nearby invalidation area

Automatic selection is deliberately conservative: it prioritizes relevance and chart clarity, and it never applies every available analysis type at once. By default it applies:

1. Current market structure
2. The most relevant support and resistance
3. The most relevant active setup (breakout, range, pullback, or reversal)
4. No more than one supporting indicator
5. No more than six drawings

You can always narrow or broaden the result afterward with a refinement — see [Editing & Reuse](/ai-chart-analysis/editing-and-reuse.md). Your default detail level and drawing limit also shape automatic selection — see [Preferences](/ai-chart-analysis/preferences.md).
