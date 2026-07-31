> For the complete documentation index, see [llms.txt](/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](/igniz-trade/adl.md).

# Auto-Deleveraging (ADL)

#### **What Is Auto-Deleveraging?** <a href="#what-is-auto-deleveraging" id="what-is-auto-deleveraging"></a>

Auto-Deleveraging (ADL) is a backstop mechanism that closes out opposing positions when a liquidation cannot otherwise be completed. It exists as a final safety net beneath Igniz's primary liquidation flow, invoked only when neither the order book nor the Insurance Vault can fully absorb a liquidation.

Igniz's standard liquidation process runs in two stages — see [Liquidation Mechanism](/igniz-trade/liquidation-mechanism.md) for the full detail:

1. **Order book liquidation.** The platform submits a market order for the distressed position to the decentralized order book, letting other participants absorb it at prevailing prices.
2. **Insurance Vault liquidation.** If order book liquidity is insufficient and equity deteriorates further, the position (or account, in unified collateral mode) transfers to the Insurance Vault.

ADL sits beyond both of these stages: it is the mechanism that engages if a position cannot be closed at the bankruptcy price and the Insurance Vault's own backstop capacity is insufficient to absorb it. Rather than leaving the loss unresolved, the platform closes out one or more opposing positions to bring the failed liquidation to a stable resolution and preserve platform solvency.

> Note: ADL is a backstop of last resort. Igniz's dual-stage liquidation design (order book, then Insurance Vault) is intended to resolve the large majority of liquidations before ADL would ever be needed.

#### **When ADL Can Occur** <a href="#when-adl-can-occur" id="when-adl-can-occur"></a>

ADL becomes relevant when all of the following are true for a given liquidation:

* The position cannot be closed via the order book at an acceptable price.
* The Insurance Vault's available backstop capacity is insufficient to absorb the shortfall.
* A bankruptcy-price close of the position would otherwise leave the platform holding an unresolved loss.

In this scenario, the platform deleverages one or more counterparty positions on the opposite side of the market to close out the gap.

> Note: The precise conditions and thresholds that trigger ADL are not fixed public constants — they depend on real-time order book depth, Insurance Vault balance, and market conditions at the time of the liquidation event, and are not published as a static figure.

#### **How Counterparties Are Selected** <a href="#how-counterparties-are-selected" id="how-counterparties-are-selected"></a>

When ADL engages, the platform identifies opposing positions to deleverage rather than selecting counterparties at random. Ranking for deleveraging typically weighs factors such as:

* **Profitability** — positions carrying larger unrealized gains are more likely to be prioritized, since they have the greatest capacity to absorb a deleveraging event without themselves becoming distressed.
* **Leverage** — positions using higher leverage are typically weighted more heavily in the ranking, reflecting their outsized exposure relative to posted margin.

> Note: Igniz does not publish an exact ADL ranking formula. Describe this as directional prioritization (favoring the most profitable and most highly leveraged opposing positions) rather than a fixed scoring model.

#### **Notification and Impact for Affected Traders** <a href="#notification-and-impact-for-affected-traders" id="notification-and-impact-for-affected-traders"></a>

If your position is selected for auto-deleveraging:

* Your position is closed, in full or in part, at the relevant settlement price for the event.
* You are notified through the platform's alerting channels that an ADL event affected your position.
* Any remaining, unaffected portion of your position (if only partially deleveraged) continues to operate normally, subject to the same margin and liquidation rules as before.

Traders who want to reduce ADL exposure can do so indirectly by avoiding sustained, highly leveraged positions on the side of the market where liquidations are concentrated, and by using take-profit/stop-loss automation to manage positions proactively. See [Maintenance Margin](/igniz-trade/maintenance-margin.md) and [Automated Exit Strategies (TP/SL)](/igniz-trade/tp-sl.md).

### Related Pages <a href="#related-pages" id="related-pages"></a>

* [Liquidation Mechanism](/igniz-trade/liquidation-mechanism.md)
* [Maintenance Margin](/igniz-trade/maintenance-margin.md)
* [Risks](/risks.md)
