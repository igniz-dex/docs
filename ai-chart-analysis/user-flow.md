> For the complete documentation index, see [llms.txt](llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](ai-chart-analysis/user-flow.md).

# AI Chart Analysis — User Flow

### From Prompt to Annotated Chart <a href="#from-prompt-to-annotated-chart" id="from-prompt-to-annotated-chart"></a>

A single analysis pass moves through the same sequence every time:

1. **Open Analysis Mode** on any chart.
2. **Optionally select something** — a candle, multiple candles, a price range, a swing high and low, an existing drawing, or just the currently visible area. Selection is optional; if you skip it, the AI reads the visible chart.
3. **Enter your instruction** — a natural-language sentence, a recognized keyword, or both together.
4. **The AI identifies the requested analysis type(s).** If you didn't name one, it selects the most applicable common analysis for the current chart and your preferences (see [Analysis Types](ai-chart-analysis/analysis-types.md)).
5. **The AI returns a structured, versioned analysis plan** — never raw numbers, never chart coordinates (see [Data Contracts](ai-chart-analysis/data-contracts.md)).
6. **The deterministic engine executes the plan** against OHLCV and available market data.
7. **The engine returns**, all with stable object IDs:
   - Evaluations with discrete confirmation states
   - Measurements and indicator values
   - Market structures
   - Drawing objects with exact chart coordinates
   - Warnings and invalidation conditions
8. **The chart animates the result into place**, layer by layer, so you can see how the analysis was built (see [Chart Behavior](ai-chart-analysis/chart-behavior.md)).
9. **A brief summary appears** beside or below the chart.
10. **You refine, edit, remove, lock, save, or rerun** the analysis as needed (see [Editing & Reuse](ai-chart-analysis/editing-and-reuse.md)).

### The Closed Loop <a href="#the-closed-loop" id="the-closed-loop"></a>

The flow above isn't a one-way pipeline that starts over on every request. The engine's results — object IDs, detected structures, and their states — are retained as your **active analysis context** and fed back to the AI planner on every refinement. That's what makes a follow-up instruction like *"use the previous swing low"* work: the planner can see the structures the engine already found and bind directly to one of them by ID, instead of guessing at coordinates from scratch.

```text
        Your Prompt + Chart Context + Preferences
                          ↓
        ┌───────────────────────────────────────┐
        │             AI Analysis Planner        │◄──────────────┐
        └───────────────────────────────────────┘               │
                          ↓                                      │
          Structured, Versioned JSON Analysis Plan               │
                          ↓                                      │
        ┌───────────────────────────────────────┐               │
        │      Deterministic Analysis Engine     │               │
        └───────────────────────────────────────┘               │
                          ↓                                      │
   Evaluations + Measurements + Indicators + Drawings            │
        (all with stable object IDs and states)                  │
                          ↓                                      │
             OHLCV Chart Visualization Layer                     │
                          ↓                                      │
        Brief Summary + Editable Chart Analysis                  │
                          ↓                                      │
     Active Analysis Context (IDs, states, your edits) ──────────┘
              fed back on every refinement
```

Every refinement is planned against this active context, not against a blank chart — so "remove the Fibonacci," "use the earlier swing low," or "keep the range and drop everything else" all resolve against structures the engine already identified, rather than requiring you to re-describe the whole analysis from the beginning.

### Next Steps <a href="#next-steps" id="next-steps"></a>

- See [Analysis Types](ai-chart-analysis/analysis-types.md) for what you can ask for and how automatic selection works.
- See [Editing & Reuse](ai-chart-analysis/editing-and-reuse.md) for the full refinement, save, and rerun workflow.
