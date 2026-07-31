> For the complete documentation index, see [llms.txt](https://igniz.gitbook.io/igniz-docs/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://igniz.gitbook.io/igniz-docs/ai-copilot/safety.md).

# AI Copilot — Safety

#### The Trust Model <a href="#the-trust-model" id="the-trust-model"></a>

Copilot's trust model is intentionally simple and reassuring: it's a powerful research analyst, not an unsupervised trader.

#### Instantly Available, Read-Only By Construction <a href="#read-only-by-construction" id="read-only-by-construction"></a>

The moment you open the app and start a conversation, Copilot already has access to a curated set of **45 read-only research and analytics capabilities** — no separate consent screen or authorization step is needed, because the conversation is already running inside your own logged-in, authenticated session.

#### Structurally Unable To Trade Or Move Funds <a href="#structurally-unable-to-trade" id="structurally-unable-to-trade"></a>

Copilot cannot place an order, cancel an order, transfer funds between accounts, or launch a new backtest or optimization run. This isn't a permission toggle that could be misconfigured — those **9 account-changing actions** out of the platform's full 54-capability trading toolset are simply never wired into what the in-app assistant is capable of calling. Copilot is "look, analyze, and advise" by architecture, not by policy — 45 out of 54, always.

#### Full Trading Authority Is A Separate, Explicit Choice <a href="#full-trading-authority" id="full-trading-authority"></a>

For traders who want an AI agent that can actually execute trades or move funds on their behalf, the platform offers a distinct, OAuth-secured integration — **[MCP](https://igniz.gitbook.io/igniz-docs/mcp.md)** — that any authorized agent can connect through, with you granting exactly the permissions you choose, entirely separately from the in-app chat.

This is a deliberate two-tier design:

- **Ask & analyze** — always-on, frictionless, read-only. This is Copilot.
- **Act & trade** — a separate, explicit, revocable grant through MCP.

#### Tool Results Are Sanitized Before The AI Ever Sees Them <a href="#sanitized-tool-results" id="sanitized-tool-results"></a>

Any data Copilot retrieves from an account lookup — a memo field on a withdrawal, a note on a transfer — is cleaned before being handed to the underlying AI model, specifically so that untrusted data returned from a lookup can never be used to smuggle in hidden instructions to the assistant (a prompt-injection defense).

#### Every Lookup Is Visible, In Real Time <a href="#visible-lookups" id="visible-lookups"></a>

Every tool call Copilot makes is shown live in the chat interface, so you always see exactly what it looked up on your behalf — there is no hidden data access. Copilot also runs on the same authenticated session as the rest of the platform — no separate token to manage, no separate login.

#### A Platform-Wide Kill Switch <a href="#kill-switch" id="kill-switch"></a>

Operators can instantly disable Copilot entirely, platform-wide, if ever needed, without affecting any other part of the exchange.

#### Tier-Based Rate Limits And Hardened Uploads <a href="#rate-limits-and-uploads" id="rate-limits-and-uploads"></a>

Messages-per-minute and uploads-per-hour are capped per account tier (see [Access & Limits](https://igniz.gitbook.io/igniz-docs/ai-copilot/access-and-limits.md)), and every uploaded file or image is checked against a strict extension, content-type, and file-signature match before being accepted — so a disguised file can't slip through under a fake extension.

#### The Math <a href="#the-math" id="the-math"></a>

| | |
|---|---|
| Full trading capability catalog (Copilot + MCP combined) | 54 capabilities |
| Read-only capabilities available in Copilot | 45 capabilities |
| Account-changing actions (place order, cancel order, transfer funds, launch a backtest, launch an optimization) — MCP only, never in-app chat | 9 actions |
