> For the complete documentation index, see [llms.txt](/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](/ai-chart-analysis/overview.md).

# AI Chart Analysis — Overview

### What AI Chart Analysis Is <a href="#what-it-is" id="what-it-is"></a>

> Describe what you are analyzing, and watch the chart build the analysis.

AI Chart Analysis lets you describe what you want examined on a chart — in plain English — and watch the analysis appear directly on the price action: structure, support and resistance, Fibonacci levels, momentum, volatility, and more, drawn with exact coordinates and backed by named, checkable criteria.

You might type:

```text
Analyze this breakout
```

```text
Map the current market structure
```

```text
Show the latest pullback
```

```text
Draw the relevant Fibonacci retracement
```

```text
Mark key support and resistance
```

```text
Show momentum and the risk map
```

```text
Analyze this chart
```

The feature exists to help you perform chart analysis faster, without first having to build a complete strategy or run a backtest.

### How It Works <a href="#how-it-works" id="how-it-works"></a>

Behind the plain-English box, three things happen in sequence:

1. **Your intent becomes a structured plan.** An AI planner reads your prompt, your chart selection, and your preferences, and converts them into a structured, versioned analysis plan — not free-form text.
2. **A deterministic engine executes the plan.** It reads the OHLCV data, calculates indicators, identifies structures, evaluates conditions against named criteria, and works out exact drawing coordinates. The AI never invents a number, a level, or a coordinate — the engine is the only source of truth for anything that appears on your chart.
3. **The chart is the output.** Drawings, indicators, and structure markers appear directly on the price action, built up step by step so you can see how the analysis was assembled. A short text summary appears alongside the chart — the chart itself carries most of the information, so the text stays brief.

The full mechanics of that pipeline — planning rules, the deterministic engine's responsibilities, and the JSON contracts that connect them — are covered in [Architecture](/ai-chart-analysis/architecture.md) and [Data Contracts](/ai-chart-analysis/data-contracts.md).

### What Analysis Mode Is Not <a href="#what-it-is-not" id="what-it-is-not"></a>

AI Chart Analysis is an **analysis and visualization tool** — not a strategy generator, signal service, or execution system. Specifically, it does not:

- Generate a complete trading strategy
- Backtest or optimize a strategy
- Produce entry and exit signals as instructions to act on
- Execute trades or manage positions
- Guarantee or imply profitability
- Emit numeric probabilities of outcomes
- Replace the strategy builder
- Apply every available indicator to every chart
- Produce long-form chart commentary
- Depend on free-form AI-generated coordinates
- Repaint a drawing once it is confirmed
- Modify your chart permanently without your consent

If you already have a complete trading idea you want to turn into a managed, live-running algorithm, that's a separate feature: see [Text-to-Algo (T2A)](/text-to-algo-t2a.md). AI Chart Analysis and T2A intentionally don't overlap — Analysis Mode helps you *see* the chart more clearly; T2A helps you *act* on a fully-specified idea.

### Compliance and Disclaimers <a href="#compliance" id="compliance"></a>

Because objects like target zones, risk maps, and invalidation levels can resemble investment recommendations in some jurisdictions, AI Chart Analysis is built around a strict compliance posture:

- Everything the feature draws or writes is presented as technical analysis of historical and current price data — **never** as advice to buy or sell.
- A standard disclaimer appears the first time you use Analysis Mode and remains accessible from every analysis summary afterward.
- Words like **"buy," "sell," "enter," "exit,"** and **"you should"** never appear in generated summaries or labels. This restriction is enforced by the analysis engine and the summary templates themselves, not left to chance.

> By using AI Chart Analysis, you acknowledge that its output is technical analysis of price data, not investment advice, and is not a guarantee of future performance.

### What's Covered Elsewhere <a href="#whats-covered-elsewhere" id="whats-covered-elsewhere"></a>

| Page | Covers |
|---|---|
| [User Flow](/ai-chart-analysis/user-flow.md) | The step-by-step flow from opening Analysis Mode to refining and saving a result |
| [Analysis Types](/ai-chart-analysis/analysis-types.md) | Recognized keywords, supported analysis types, and automatic selection |
| [Preferences](/ai-chart-analysis/preferences.md) | Persistent settings, learned preferences, and detail levels |
| [Architecture](/ai-chart-analysis/architecture.md) | How planning and calculation are split between the AI and the deterministic engine |
| [Data Contracts](/ai-chart-analysis/data-contracts.md) | The JSON contracts behind every plan and every engine result |
| [Chart Behavior](/ai-chart-analysis/chart-behavior.md) | What appears on the chart, and the no-repaint drawing state machine |
| [Editing & Reuse](/ai-chart-analysis/editing-and-reuse.md) | Refining, editing, saving, and rerunning an analysis |
| [Reliability & Limits](/ai-chart-analysis/reliability-and-limits.md) | Edge-case handling, latency budgets, and success criteria |

For the exchange's other AI surfaces, see [AI Copilot](/ai-copilot.md) (the in-app conversational assistant) and [Text-to-Algo (T2A)](/text-to-algo-t2a.md) (natural-language strategy creation).
