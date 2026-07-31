> For the complete documentation index, see [llms.txt](/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](/ai-chart-analysis/preferences.md).

# AI Chart Analysis — Preferences

### Core Persistent Preferences <a href="#core-preferences" id="core-preferences"></a>

The preference surface is deliberately small — only a handful of settings are exposed directly, so configuration never becomes a chore before you can start analyzing:

- **Default analysis detail level** — Minimal, Standard, or Detailed
- **Maximum number of automatic drawings**
- **Lines versus zones** — whether structures render as single lines or shaded zones by default
- **Wick-based versus close-based levels** — whether support, resistance, and range boundaries default to candle wicks or closing prices

Everything else is either inferred from how you use the feature, or set per-analysis with a temporary instruction.

### Detail Levels <a href="#detail-levels" id="detail-levels"></a>

**Minimal**
- Primary structure only
- Three or four drawings at most
- A one-line summary

**Standard** *(recommended default)*
- Primary structure
- Supporting context
- Relevant measurements
- The most important warning, if any

**Detailed**
- Secondary structures
- Additional indicators
- Alternative interpretations
- More measurements
- An expanded explanation

You can drop back to a lighter view at any time:

```text
Simplify this.
```

This removes secondary information while keeping the primary analysis, and it's handled instantly — no round trip to the AI planner is needed (see [Chart Behavior](/ai-chart-analysis/chart-behavior.md)).

### Inferred Preferences <a href="#inferred-preferences" id="inferred-preferences"></a>

Beyond the four core settings, Analysis Mode learns your stylistic preferences from your corrections rather than asking you to configure them up front. For example:

- Repeatedly removing an indicator (say, "remove RSI" three times) → the planner stops including it by default and tells you: *"I'll stop adding RSI by default. Undo?"*
- Repeatedly switching a Fibonacci anchor to an earlier swing → your swing-sensitivity preference is adjusted.
- Repeatedly saying "simplify" → your default detail level is lowered, after you confirm.

This learning follows a few firm rules:

- An inference only happens after **at least three consistent corrections** — a single edit is never over-interpreted as a standing preference.
- Every inferred change is surfaced to you with a **one-tap undo**.
- All inferred preferences are visible and editable in a single **"Learned preferences"** list — nothing is learned silently.
- You can **freeze learning entirely** if you'd rather set everything explicitly.

### Session Context <a href="#session-context" id="session-context"></a>

Every time you send a prompt or a refinement, the AI planner receives the full context needed to interpret it correctly:

- Current symbol, asset class, and timeframe
- Visible time range and chart scale
- Current session (regular / pre-market / post-market)
- Your selected candles or region, as UI-captured coordinates
- Existing user drawings and existing AI drawings, by object ID
- Active indicators
- Your **active analysis context** — the object IDs, types, states, and key values from the most recent engine result
- Previous commands in the same analysis session

This is what lets short, contextual instructions like "use the earlier swing low" or "remove Fibonacci" work without you having to restate the whole analysis.

### Temporary Instructions <a href="#temporary-instructions" id="temporary-instructions"></a>

You can override any preference for a single analysis without changing your defaults:

```text
Ignore pre-market data
```

```text
Use closing prices only
```

```text
Use the earlier swing low
```

```text
Do not show RSI
```

```text
Only analyze today's session
```

```text
Keep this analysis minimal
```

```text
Include daily resistance
```

Temporary instructions apply only to the analysis you're running — they're never saved as a preference unless you explicitly say so.
