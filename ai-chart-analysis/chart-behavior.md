> For the complete documentation index, see [llms.txt](https://igniz.gitbook.io/igniz-docs/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://igniz.gitbook.io/igniz-docs/ai-chart-analysis/chart-behavior.md).

# AI Chart Analysis — Chart Behavior

### The Chart Is the Primary Output <a href="#primary-output" id="primary-output"></a>

The main output of an analysis is the chart itself, not the text next to it. Depending on what you asked for, the chart can show:

- Support and resistance lines and zones
- Trendlines and channels
- Range boxes
- Swing markers
- Breakout, failed-breakout, and reversal markers
- Retest zones
- Fibonacci retracements and extensions
- Volume and volatility indicators
- Momentum indicators
- Moving averages, VWAP
- Structure labels
- Invalidation levels
- Reaction zones
- Risk-map zones
- Measurement labels
- Higher-timeframe level overlays

### Brief Summary <a href="#brief-summary" id="brief-summary"></a>

Text stays short by design. A typical summary reads like this:

> Bullish breakout with an active retest. The 0.5–0.618 retracement overlaps former resistance, but volume confirmation is weak (moderate confirmation).

By default it contains:

- The primary interpretation
- One or two important supporting findings
- One warning, when relevant
- The confirmation grade of the primary evaluation

Summaries never contain recommendation vocabulary — see [Overview](https://igniz.gitbook.io/igniz-docs/ai-chart-analysis/overview.md).

### Analysis Layer List <a href="#analysis-layer-list" id="analysis-layer-list"></a>

Every drawing and indicator that's currently applied is listed so you always know what's on your chart:

```text
Current analysis

✓ Breakout range          confirmed
✓ Retest zone             provisional
✓ Fibonacci retracement   confirmed (edited)
✓ Relative volume
✓ Daily resistance
```

Each layer supports:

- **Show** or **Hide**
- **Edit**
- **Lock**
- **Remove**
- **Explain**
- **Save**

### Progressive Construction <a href="#progressive-construction" id="progressive-construction"></a>

An analysis doesn't snap onto the chart all at once — it builds up in view, so you can follow how it was assembled:

1. Highlight the selected or detected structure.
2. Draw the primary range or trend.
3. Add the main measurement.
4. Add supporting indicator or context.
5. Add warnings or invalidation areas.
6. Display the brief summary.

### Drawing States <a href="#drawing-states" id="drawing-states"></a>

Every object on the chart is always in one of six states:

| State | Meaning |
|---|---|
| **Developing** | The structure depends on the current, unclosed bar and can still change. |
| **Provisional** | Complete on closed bars, but hasn't yet met its confirmation criteria. |
| **Confirmed** | All confirmation criteria are met on closed data. |
| **Invalidated** | The object's published invalidation rule has triggered. |
| **Locked** | You froze the object — no automatic changes of any kind apply to it. |
| **User-edited** | You changed its anchors or coordinates; this overlays whichever other state applies. |

### The No-Repaint Policy <a href="#no-repaint-policy" id="no-repaint-policy"></a>

Repainting — a drawing silently moving after the fact — is the most common trust complaint about automated technical analysis tools. AI Chart Analysis is built around explicit commitments against it:

1. **Developing → Provisional** happens only on bar close. Developing objects can move freely intra-bar and are rendered in a distinct "live" style (dashed, for example) so it's always clear they're not final yet.
2. **Provisional → Confirmed** happens only once every named confirmation criterion passes on closed data — for a breakout, that means close beyond the range, a minimum number of bars held, and a volume ratio, each evaluated independently.
3. **Confirmed objects never move.** A confirmed object's price and time anchors are frozen for good. New information either creates a *new* object or transitions the existing one to Invalidated — a confirmed drawing is never silently relocated.
4. **Confirmed → Invalidated** happens only through the object's published invalidation rule (for example, two consecutive closes back inside the range). Invalidated objects are greyed out rather than deleted, until you remove them or start a new analysis.
5. **Provisional objects can be removed or updated**, but only on bar close, and the change is animated so you see it happen rather than finding the chart different than you left it.
6. **Locked and User-edited objects are never modified automatically.** If live data would invalidate a locked object, the UI badges it ("invalidation condition met") without altering it.
7. **Swing points confirm after a configurable number of subsequent closed bars** (3 by default) — before that, they're Provisional and clearly marked as such.

### Live Invalidation Without Re-Planning <a href="#live-invalidation" id="live-invalidation"></a>

Because every evaluation ships with a machine-readable invalidation rule (see [Data Contracts](https://igniz.gitbook.io/igniz-docs/ai-chart-analysis/data-contracts.md)), the engine can transition a Confirmed object straight to Invalidated as live data arrives — with no AI call required. A brief system note appends to the summary automatically:

> *"Breakout invalidated: two closes back inside the range."*
