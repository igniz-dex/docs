> For the complete documentation index, see [llms.txt](https://igniz.gitbook.io/igniz-docs/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://igniz.gitbook.io/igniz-docs/ai-chart-analysis/architecture.md).

# AI Chart Analysis — Architecture

### Division of Labor <a href="#division-of-labor" id="division-of-labor"></a>

AI Chart Analysis is built on a deliberate split between two components with very different jobs:

- **The AI plans.** It interprets your intent and turns it into a structured request. It never touches OHLCV data directly and never does the math itself.
- **The deterministic engine calculates.** It is the only component that reads market data, runs indicator math, evaluates conditions, and produces the numbers, coordinates, and states that end up on your chart.

This split exists so that anything you see drawn on a chart — a support level, a Fibonacci ratio, a confirmation grade — traces back to a deterministic calculation, never to something an AI model guessed at. It's the same reasoning that keeps repainting out of the drawing state machine (see [Chart Behavior](https://igniz.gitbook.io/igniz-docs/ai-chart-analysis/chart-behavior.md)).

### How Selection and Wording Are Reconciled <a href="#selection-rules" id="selection-rules"></a>

Your prompt and your chart selection are usually in agreement, but the system has clear rules for every case:

| Situation | What happens |
|---|---|
| **Explicit request** — you name the analysis type | The AI uses it directly. *"Draw a Fibonacci retracement on the latest bullish impulse"* selects Fibonacci retracement, anchored to the latest bullish impulse. |
| **Combined request** — you ask for multiple compatible analyses | All of them are selected together. *"Analyze this breakout, show the retest zone, and mark the next resistance"* selects breakout structure, retest analysis, and resistance mapping. |
| **Unspecified request** — no structure or type is named | The AI inspects the chart, identifies the most structurally relevant condition, selects a limited set of useful analyses, and prefers your core and inferred preferences — while avoiding unnecessary indicators and clutter. |
| **Ambiguous request** — the AI has to interpret rather than ask | The AI makes a reasonable, reversible assumption instead of blocking you. *"Analyze the move"* might resolve to the latest confirmed impulse, with the assumption stated in the output. Clarification is only requested when the possible interpretations would produce materially different results. |
| **Conflicting selection and prompt** — your visual selection and your wording disagree (e.g., you select a bearish region but write "analyze this bullish breakout") | Your **selection wins for scope**, your **text wins for analysis type**. The assumption is stated in the output, and you can flip it with one refinement. |

### What the AI Should Do <a href="#what-the-ai-should-do" id="what-the-ai-should-do"></a>

- Interpret your intent and identify the requested analysis type(s)
- Select applicable default analyses when none are specified
- Apply your core and inferred preferences
- Determine the intended chart scope
- Determine which deterministic tools are required
- Define analysis parameters and the dependencies between analyses
- Reference existing engine objects by ID when refining (e.g., anchor to `swing_low_2` rather than an unresolved "auto")
- Set display priority and limit chart clutter
- Provide a brief explanation template
- Return a valid plan matching the required, versioned schema

### What the AI Must Never Do <a href="#what-the-ai-must-never-do" id="what-the-ai-must-never-do"></a>

- Perform final numerical calculations
- Invent OHLCV values or indicator values
- Produce unsupported price levels
- Directly generate chart coordinates — selection coordinates are UI pass-through only, never AI-authored
- Emit numeric probabilities or confidence scores
- Claim a setup is profitable, or present untested analysis as a validated edge
- Use recommendation vocabulary — "buy," "sell," "enter," "exit," "should" (see [Overview](https://igniz.gitbook.io/igniz-docs/ai-chart-analysis/overview.md))
- Execute trades or create orders

### What the Deterministic Engine Does <a href="#what-the-engine-does" id="what-the-engine-does"></a>

The engine is the source of truth for every chart value and drawing coordinate. On every request it:

- Validates the analysis plan against its schema version
- Resolves the selected chart scope
- Detects requested structures and assigns each a **stable object ID** that persists across refinements and live updates
- Calculates indicators, identifies swing points, calculates support/resistance and Fibonacci levels
- Evaluates breakout, pullback, retest, reversal, momentum, volume, and volatility conditions
- Produces **discrete confirmation states with named criteria** — never bare probabilities
- Calculates exact time and price coordinates
- Returns drawing objects, measured findings, and warnings — each invalidation condition carrying an explicit trigger rule
- Rejects unsupported or invalid requests with machine-readable error codes
- Produces repeatable results from the same inputs and the same schema version

The exact shape of what passes between the AI and the engine — and what the engine hands back to the chart — is covered next in [Data Contracts](https://igniz.gitbook.io/igniz-docs/ai-chart-analysis/data-contracts.md).
