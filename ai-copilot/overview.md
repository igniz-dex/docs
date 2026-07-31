> For the complete documentation index, see [llms.txt](llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](ai-copilot/overview.md).

# AI Copilot — Overview

#### What Igniz AI Copilot Is <a href="#what-it-is" id="what-it-is"></a>

Igniz AI Copilot is the exchange's in-app conversational co-pilot — available the moment you open the app, wired directly into the exchange's own live order books, wallets, and analytics engines. It can see your live account, read the market in real time, run professional-grade technical analysis, coach you on your own trading behavior, and help you validate strategy ideas before risking real money.

Copilot is not a generic chatbot bolted onto the side of the product. Every answer it gives is grounded in real, live account data rather than guesswork — nothing is simulated or approximated unless it says so explicitly.

#### A Fast, Disciplined Research Desk <a href="#research-desk" id="research-desk"></a>

Talk to Copilot the way you'd talk to a very fast, very disciplined research desk. It answers using the exact same live data and math the exchange itself relies on.

> *Ask Igniz:* "What's my open exposure on BTC right now?" · "Show me RSI and MACD on ETH/USDT, 1-hour chart." · "How has my trading been this month — am I disciplined with stop-losses?" · "Pull up the results of that backtest I ran yesterday."

#### What's Covered Elsewhere <a href="#whats-covered-elsewhere" id="whats-covered-elsewhere"></a>

#### Design Principles <a href="#design-principles" id="design-principles"></a>

Everything Copilot does follows from a small set of principles:

1. **Grounded in real data.** Every number Copilot reports comes from the same live systems and formulas that power the exchange itself.
2. **Read-only by architecture, not by policy.** The account-changing actions in the underlying capability catalog are simply never wired into Copilot's toolset — there is no misconfiguration to reason about.
3. **A separate, explicit path to full trading authority.** Traders who want an AI agent to actually trade for them get there through a distinct, auditable, revocable authorization — never through the always-on in-app chat.
4. **Auditable and visible.** Every lookup Copilot performs is shown live in the conversation.
5. **Speaks trader language.** Human-readable pair symbols and currency tickers throughout, never internal account plumbing.
6. **Honest by default.** Copilot is built to say "I don't have enough data for that" rather than presenting a misleading or fabricated figure.

This page introduces Copilot. The rest of the section covers the conversational experience itself, the safety model, and the complete tour of what you can ask it to do:

| Page | Covers |
|---|---|
| [Chat](ai-copilot/chat.md) | Model choice, visible reasoning, transparent tool use, multimodal input, memory, streaming |
| [Safety](ai-copilot/safety.md) | The trust model, read-only architecture, and how full trading authority is granted separately |
| [Trading & Accounts](ai-copilot/trading-and-accounts.md) | Spot, futures, wallet, and referral visibility |
| [Markets & Indicators](ai-copilot/markets-and-indicators.md) | Market intelligence and the 60+ indicator technical analysis engine |
| [Performance & Strategy](ai-copilot/performance-and-strategy.md) | The AI Performance Coach and Strategy Lab research tools |
| [Access & Limits](ai-copilot/access-and-limits.md) | Tier-based upload caps and rate limits |

For traders who want an AI agent that can actually execute trades or move funds on their behalf, see [MCP](mcp.md) — a separate, explicitly-authorized integration covered in full on the [Safety](ai-copilot/safety.md) page.
