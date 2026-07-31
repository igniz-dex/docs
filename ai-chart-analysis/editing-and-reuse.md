> For the complete documentation index, see [llms.txt](/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](/ai-chart-analysis/editing-and-reuse.md).

# AI Chart Analysis — Editing & Reuse

### Refinement Commands <a href="#refinement-commands" id="refinement-commands"></a>

Once an analysis is on the chart, you refine it the same way you started it — in plain English:

```text
Use the previous swing low
```

```text
Remove Fibonacci
```

```text
Show the daily resistance
```

```text
Use closing prices
```

```text
Only keep confirmed levels
```

```text
Make this analysis simpler
```

```text
Show the bearish interpretation
```

```text
Analyze only today's session
```

```text
Keep the range and remove everything else
```

```text
Recalculate using the broader impulse
```

### The Refinement Loop <a href="#the-refinement-loop" id="the-refinement-loop"></a>

Every refinement is planned against your **active analysis context** — the previous engine result's object IDs, types, states, and any edits you've made (see [User Flow](/ai-chart-analysis/user-flow.md)). That context is what makes "the previous swing low" resolvable at all: the planner can see `swing_low_1`, `swing_low_2`, and so on, and binds your instruction to the right one by ID rather than guessing.

Refinement plans carry `mode: "refinement"`, a `parent_request_id`, and explicit `retained_objects` / `removed_objects` diffs (see [Data Contracts](/ai-chart-analysis/data-contracts.md)). The engine recalculates only what the diff touches — a refinement updates your current analysis, it never spins up a disconnected new one.

### Local Refinements (No AI Round-Trip) <a href="#local-refinements" id="local-refinements"></a>

Some commands are simple and unambiguous enough that the UI and engine execute them directly, with no AI call — which is also why they're instant:

- Remove / hide / show a named layer ("remove Fibonacci," or tapping a layer toggle)
- Lock / unlock a drawing
- Simplify (drops every secondary-priority object)
- Undo / redo
- Clear analysis

These complete in **150 milliseconds or less** (see [Reliability & Limits](/ai-chart-analysis/reliability-and-limits.md)). Anything that actually requires interpretation — "use the broader impulse," "show the bearish interpretation" — goes through the AI planner instead.

### User Edit Persistence <a href="#user-edit-persistence" id="user-edit-persistence"></a>

Your manual edits are treated as durable, not as a suggestion the system can overwrite on the next pass:

1. When you edit an object's anchors, it's flagged `user_edited: true` and behaves like a locked object for automatic changes.
2. On any later refinement or rerun, **your edited objects are preserved by default** — the engine recalculates everything else around them.
3. If a refinement explicitly targets an object you've edited ("recalculate the fib"), the system asks one inline question — *"Recalculate and discard your manual anchors?"* — with a keep/discard choice. This is one of the few places the feature will interrupt you with a prompt.
4. **"Restore AI-selected anchors"** is always available per object, reverting it back to the engine's own anchors.
5. Contradictory refinements in the same session (say, "remove RSI" then later "add RSI") simply apply in order — the latest instruction wins, and no preference is inferred from a pair of instructions that contradict each other.

### Saving and Reuse <a href="#saving-and-reuse" id="saving-and-reuse"></a>

There are two distinct ways to keep an analysis for later, and they behave very differently:

**Save Snapshot** captures the current chart analysis exactly as it stands — symbol, timeframe, scope, prompt and refinement history, the plan(s) with their schema version, engine results, drawings, indicators, states, and any edits you made. Reopening a snapshot renders those exact stored coordinates; **it is never recalculated.**

**Save Recipe** captures a reusable *method* instead of a one-time result:

```text
Breakout Analysis (recipe, schema 1.0)

Always include:
- Range boundaries
- Breakout level
- Relative volume
- Retest zone

Include when relevant:
- Fibonacci retracement
- Higher-timeframe resistance
- Failed breakout warning

Maximum drawings: 6
```

A saved recipe can be run later on the current chart, a different symbol, a different timeframe, a watchlist item, or a selected chart region.

**Rerun** always uses current market data while keeping the saved recipe and its preferences intact. It produces a fresh execution (`mode: "recipe_run"`) — not a replay of old coordinates. Schema versioning ensures recipes keep working across releases; see [Data Contracts](/ai-chart-analysis/data-contracts.md) for how migration is handled.

### User Controls <a href="#user-controls" id="user-controls"></a>

The full control surface available at any point in an analysis:

- Analyze
- Stop analysis
- Undo / Redo
- Clear analysis
- Simplify
- Explain
- Save snapshot / Save recipe / Rerun
- Lock drawings
- Hide individual layers
- Edit drawing anchors
- Restore AI-selected anchors
- Freeze preference learning

AI-created drawings are always visually distinguishable from your own drawings, and any AI drawing you've edited is marked as such.

### Trust and Transparency <a href="#trust-and-transparency" id="trust-and-transparency"></a>

Every AI-generated drawing is explainable. Selecting one reveals:

- What the object represents
- Why it was added
- Which data points were used
- Which parameters were applied
- Its state (developing / provisional / confirmed / invalidated) and its full confirmation criteria table
- Exactly what would invalidate it — the published rule
- Whether you've edited it

The system is always explicit about what kind of thing you're looking at, distinguishing between:

- Measured fact
- Rule-based evaluation, with its criteria
- AI-selected interpretation
- Your own preference — whether explicit or inferred
- An alternative interpretation

Every plan and result is auditable: the pass-through selection source, the plan diff, and the engine version are all stored alongside each saved snapshot.
