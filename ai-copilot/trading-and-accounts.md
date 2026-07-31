> For the complete documentation index, see [llms.txt](llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](ai-copilot/trading-and-accounts.md).

# AI Copilot — Trading & Accounts

#### Spot Trading — Visibility <a href="#spot-trading-visibility" id="spot-trading-visibility"></a>

- Check every currently open spot order, for one pair or the whole account.
- Pull up spot order history — everything that's been filled, cancelled, or expired.
- Pull up spot trade history — the actual executions, distinct from order status.

> Order placement and cancellation are intentionally reserved for your own direct action or an authorized agent connected via [MCP](mcp.md) — never the in-app conversation itself.

#### Futures & Perpetuals — Visibility & Risk Monitoring <a href="#futures-and-perpetuals" id="futures-and-perpetuals"></a>

- View every open leveraged position, account-wide or per contract.
- View open futures orders, order history, and trade history.
- Check configured leverage and margin mode (isolated vs. cross) for any contract.
- **A one-call "how much danger am I in?" risk dashboard** — total notional exposure across every open position, total margin at risk, and two purpose-built risk ratios: a **maintenance margin ratio** (how close the account is to a liquidation event) and a **margin utilization ratio** (how much of the account's equity is already committed to open risk). This is one of Copilot's most valuable safety features — it can proactively flag when you're getting close to liquidation.
- Review funding payment history — what's been paid or earned on perpetual positions over time.

> *Ask Igniz:* "What's my current leverage on BTCUSDT?" · "How close am I to liquidation right now?" · "How much funding have I paid this week?"

#### Wallet, Transfers & Fund Tracking — Visibility <a href="#wallet-and-transfers" id="wallet-and-transfers"></a>

- Full spot wallet balances across every currency held — a net-worth snapshot in one call.
- Transfer history between spot, futures, and margin sub-accounts.
- Deposit history — status, amount, transaction hash, and a ready-to-click block-explorer link to verify a deposit independently on-chain.
- Withdrawal history — status, amount, destination, and the currently configured network fee for reference.

> Copilot can help you understand and monitor every fund movement, but cannot itself move money — neither between accounts nor off the platform. That guardrail is universal across the entire assistant, not just the in-app chat: even a fully authorized external agent connected via MCP can never initiate an off-platform withdrawal.

#### Account Profile & Referral Program <a href="#account-and-referrals" id="account-and-referrals"></a>

- Basic account profile lookup, for personalizing a conversation.
- A full referral snapshot — personal referral link, lifetime earnings, referral counts, and how many referrals have gone on to actively trade.
- A detailed, paginated referral earnings ledger, with each commission marked finalized or still pending.

> *Ask Igniz:* "How much have I earned from referrals?" · "How many of my referrals are actually active traders?"
