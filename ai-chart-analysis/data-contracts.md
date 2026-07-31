> For the complete documentation index, see [llms.txt](llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](ai-chart-analysis/data-contracts.md).

# AI Chart Analysis — Data Contracts

This page is for the curious: it shows the actual shape of what passes between the AI planner and the deterministic engine. You'll never need to write this JSON by hand, but seeing it makes the [Architecture](ai-chart-analysis/architecture.md) split concrete — every field either comes from you, from the AI's interpretation of your intent, or from the engine's own calculation, and the contract makes clear which is which.

### The Analysis Plan <a href="#the-analysis-plan" id="the-analysis-plan"></a>

The plan is what the AI hands to the engine. It's versioned, so the engine always knows exactly how to execute it — and can reject a plan version it no longer understands.

```json
{
  "schema_version": "1.0",
  "request_id": "analysis_124",
  "parent_request_id": "analysis_123",
  "mode": "refinement",
  "intent": "Use the previous swing low for the retracement",
  "scope": {
    "mode": "inherit",
    "selection_source": "ui_passthrough",
    "selected_start_time": null,
    "selected_end_time": null,
    "selected_price_min": null,
    "selected_price_max": null
  },
  "analysis_types": [
    {
      "type": "fibonacci_retracement",
      "priority": "primary",
      "parameters": {
        "anchor_mode": "object_reference",
        "anchor_low_ref": "swing_low_2",
        "anchor_high_ref": "swing_high_1",
        "levels": [0.382, 0.5, 0.618]
      }
    }
  ],
  "retained_objects": ["range_zone_1", "retest_zone_1"],
  "removed_objects": ["fib_1"],
  "display": {
    "detail_level": "standard",
    "maximum_drawings": 6,
    "animate": true,
    "show_summary": true,
    "show_measurements": true,
    "show_alternatives": false
  },
  "preferences_applied": [
    "use_confirmed_swings",
    "prefer_zones",
    "avoid_rsi (inferred)"
  ],
  "assumptions": [
    "\"Previous swing low\" resolved to detected object swing_low_2"
  ]
}
```

A few things worth understanding about this contract:

- **`mode`** is one of `"new"`, `"refinement"`, or `"recipe_run"`. Refinements always carry a `parent_request_id` linking them to the analysis they build on.
- **`anchor_mode: "object_reference"`** is how the planner binds to a structure the engine already detected — like anchoring a new Fibonacci retracement to an existing `swing_low_2` — instead of asking the engine to auto-detect one again. `"auto"` is still available for brand-new analyses.
- **`retained_objects` / `removed_objects`** make refinement diffs explicit, so a request to change one layer never causes unrelated layers to silently recalculate.
- Every analysis type's **parameters** are validated against a schema the engine owns; unknown parameters are rejected outright, never silently ignored.
- Selection coordinates always carry **`selection_source: "ui_passthrough"`**, marking them as captured directly from your on-chart selection — never authored by the AI.

### The Engine Result <a href="#the-engine-result" id="the-engine-result"></a>

The engine's response is what the UI renders directly — nothing about what appears on your chart is inferred from free-form AI text. The example below is illustrative and abridged — a real result also carries an `indicators` array and one drawing object per detected structure — but it shows the fields that matter: discrete confirmation states with named criteria, stable object IDs, exact coordinates, and explicit invalidation rules.

```json
{
  "schema_version": "1.0",
  "request_id": "analysis_124",
  "parent_request_id": "analysis_123",
  "status": "completed",
  "summary_data": {
    "primary_finding": "Bullish breakout with an active retest",
    "supporting_findings": [
      "The former resistance overlaps the 0.5–0.618 retracement zone",
      "Breakout volume was below the selected confirmation threshold"
    ],
    "warnings": [
      "Nearby higher-timeframe resistance limits available room above"
    ]
  },
  "evaluations": [
    {
      "id": "breakout_1",
      "type": "breakout",
      "state": "confirmed",
      "confirmation": {
        "grade": "moderate",
        "criteria": [
          { "name": "close_beyond_range", "passed": true,  "value": true,  "threshold": true },
          { "name": "bars_held_beyond",   "passed": true,  "value": 5,     "threshold": 3 },
          { "name": "volume_ratio",       "passed": false, "value": 1.21,  "threshold": 1.5 },
          { "name": "breakout_candle_atr","passed": true,  "value": 1.34,  "threshold": 1.0 }
        ]
      },
      "invalidation": {
        "rule": "close_back_inside_range",
        "level": 0,
        "consecutive_closes_required": 2
      },
      "metrics": {
        "range_duration_bars": 38
      }
    }
  ],
  "structures": [
    { "id": "swing_low_1", "type": "swing_low", "time": 0, "price": 0, "state": "confirmed" },
    { "id": "swing_low_2", "type": "swing_low", "time": 0, "price": 0, "state": "confirmed" },
    { "id": "swing_high_1", "type": "swing_high", "time": 0, "price": 0, "state": "confirmed" }
  ],
  "drawings": [
    {
      "id": "range_zone_1",
      "type": "rectangle",
      "role": "breakout_range",
      "state": "confirmed",
      "origin": "engine",
      "user_edited": false,
      "coordinates": { "start_time": 0, "end_time": 0, "price_min": 0, "price_max": 0 },
      "label": "Breakout range",
      "editable": true
    }
  ],
  "assumptions": [],
  "alternatives": [],
  "errors": []
}
```

A few contract details that matter directly to how the feature behaves on-screen:

- **Confirmation grades** are `weak`, `moderate`, or `strong` — computed by counting passed criteria against per-type rules. The UI shows the grade, and tapping it reveals the full criteria table (each with its own measured value and threshold). The word "confidence" and 0–1 probability scores never appear anywhere in this contract.
- Every evaluation carries an explicit, machine-readable **`invalidation`** rule, which is what lets the UI transition an object's state live, without calling the AI again (see [Chart Behavior](ai-chart-analysis/chart-behavior.md)).
- **Stable IDs** appear on every structure, drawing, indicator, and evaluation. A rerun in the same session reuses IDs for anything the engine considers "the same structure," and those IDs remain valid `object_reference` targets for your next refinement.
- The UI never places a drawing based on free-form AI text — only on this structured result.

### Schema Versioning and Migration <a href="#schema-versioning" id="schema-versioning"></a>

Plans, results, saved snapshots, and saved recipes all carry a `schema_version`, which keeps the feature stable as it evolves:

- The engine maintains migration adapters for at least the two previous minor schema versions.
- A recipe on a migratable older version runs transparently and is upgraded automatically the next time it's saved.
- If a recipe's version can't be migrated, the rerun fails gracefully with a specific message — *"This recipe uses analysis parameters that no longer exist"* — and offers to rebuild the recipe from its stored intent text.
- Saved snapshots are immutable and always renderable: reopening one depends only on its stored coordinates, never on current engine behavior, so old snapshots keep working exactly as saved (see [Editing & Reuse](ai-chart-analysis/editing-and-reuse.md)).
