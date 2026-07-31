> For the complete documentation index, see [llms.txt](llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](ai-chart-analysis/reliability-and-limits.md).

# AI Chart Analysis — Reliability & Limits

### How Edge Cases Are Handled <a href="#edge-cases" id="edge-cases"></a>

AI Chart Analysis is built to be honest about its limits rather than force an answer. Here's how it behaves in the situations that come up most often:

**No clear structure.** If nothing qualifies, you'll see something like *"No clear breakout or pullback structure was found in the selected area. The chart is currently behaving as a broad range."* The system may still draw the detected range if that's useful on its own.

**Multiple valid interpretations.** The most likely interpretation is used, with an alternative optionally surfaced:

```text
Primary interpretation: Short-term bullish impulse
Alternative: Broader session impulse
```

Selecting the alternative swaps it in as a refinement, reusing object IDs wherever the structures overlap.

**Unsupported request.** The system explains the limitation briefly and offers the closest supported analysis instead. Error responses carry machine-readable codes, so the UI can always offer the right fallback action.

**Invalid or insufficient data.** The system never draws an unsupported object — it states specifically what's missing, for example: *"Only 40 bars of history are available; swing analysis needs at least 60 on this timeframe."*

**Insufficient history or illiquid symbols.** Every structure type has its own minimum bar count; below it, that analysis type is skipped with a stated reason. On illiquid symbols, volume-based confirmations are reported as "not assessable" rather than failed, and excluded from the confirmation grade rather than counted against it.

**Data gaps, halts, and session boundaries.** Gaps — weekends, halts, listing gaps — never generate phantom structures; range and trend detection treats them as boundaries, not as moves. Pre-/post-market data is excluded by default for equities and included for 24-hour markets; whichever session setting is in effect is always stated in the summary, and can be overridden per analysis.

**Symbol or timeframe change mid-analysis.** Switching symbol or timeframe archives the active analysis (offered to you as a snapshot) and clears the chart. An analysis is never silently re-projected onto a different symbol or resolution. Rerunning a recipe on the new context is offered as a one-tap action.

**Live data movement during analysis.** If price moves materially between when the plan was created and when the engine executes it, the engine evaluates on the data as of execution and flags any assumption that no longer holds — for example, "the retest completed while analyzing."

**Plan validation failures.** If the AI's plan is invalid or fails parameter validation, the system retries planning once with the validation errors included. On a second failure, it reports *"I couldn't build a valid analysis plan for that request"* along with the closest supported alternative. An invalid plan is never partially executed.

**Engine timeout or partial failure.** Each analysis type in a plan executes independently — if one fails or times out, the completed layers still render, the failed layer is listed with a retry action, and the result is honestly reported with `status: "partial"`.

**Drawing budget exceeded.** If a plan's layers would exceed the maximum-drawings setting, the engine keeps objects by priority (primary first), reports what was omitted, and you can raise the limit for that analysis with a single refinement.

**Rerun on changed or unavailable instruments.** Rerunning a recipe on a delisted or data-unavailable symbol fails with a specific message rather than rendering an empty analysis. Rerunning on an asset class that conflicts with the recipe's assumptions (for example, session-based rules on a 24-hour market) applies the closest valid interpretation and states the adaptation made.

**Multi-timeframe context.** This is always explicit, never implicit: the higher timeframe defaults to the standard aggregation ladder (5m→1h, 1h→1D, 1D→1W) and is always named in the layer list. Higher-timeframe levels are calculated on **closed higher-timeframe bars only**, so they never repaint intra-bar on the lower timeframe you're viewing. If higher-timeframe data is unavailable, that layer is skipped with a stated reason while the rest of the analysis proceeds.

### Latency Budget <a href="#latency-budget" id="latency-budget"></a>

Speed is treated as a product requirement, not an afterthought:

| Interaction | Target |
|---|---|
| Local refinements (hide, remove, lock, simplify, undo) | ≤ 150 ms, no network round-trip |
| New analysis — prompt to first drawing on chart | ≤ 3 s at p50, ≤ 6 s at p95 |
| Planner-based refinement | ≤ 2.5 s at p50 |
| Live invalidation transitions | ≤ 250 ms after the triggering bar close |

The first detected structure renders as soon as its layer completes — the system never waits for the entire result before it starts drawing, which is why the construction sequence in [Chart Behavior](ai-chart-analysis/chart-behavior.md) matters for perceived speed as much as total time does. If planning takes longer than expected, you'll see progress states ("interpreting request… detecting structure…") rather than a blank spinner; if the total run exceeds twice the p95 budget, it's cancelled with a retry option.

### Success Criteria <a href="#success-criteria" id="success-criteria"></a>

The team holds this feature to measurable standards, not just qualitative goals:

- **Plan validity rate**: at least 99% of planner outputs pass schema and parameter validation on the first attempt.
- **Anchor correction rate**: at most 15% of analyses see you edit or replace an AI-selected anchor or structure — the primary signal for "the AI picked the right structure."
- **Refinement resolution rate**: at least 95% of object-reference refinements (like "the previous swing low") bind to the structure you actually meant.
- **Repaint complaints**: zero confirmed-object coordinate changes in production — this is a hard, alarmed invariant, not a target to approach.
- **Latency**: the budgets above, met at both p50 and p95.
- **Recipe durability**: at least 99% of saved recipes rerun successfully across schema releases.
- **Offline evaluation set**: a curated library of charts with hand-labeled structures, against which the engine's detection accuracy and the planner's selection relevance are regression-tested on every release.

### Final Product Definition <a href="#final-product-definition" id="final-product-definition"></a>

AI Chart Analysis is a natural-language interface for operating deterministic chart-analysis tools, closed into a loop by a persistent, refinable analysis state. Four parties share the work:

- **You** provide intent, an optional visual selection, corrections and refinements, and a small set of explicit preferences.
- **The AI** provides analysis selection, structured and versioned planning, tool orchestration, object-level refinement, and display prioritization.
- **The deterministic engine** provides calculations, evaluations with named criteria, indicators, structures, exact drawing coordinates, stable object IDs, and explicit confirmation and invalidation rules.
- **The chart UI** provides live construction under a strict no-repaint state machine, brief findings, editing and control, and versioned saving and reuse.

> The trader describes what they want to analyze, and the chart constructs the analysis in front of them — and every refinement builds on what is already there.
