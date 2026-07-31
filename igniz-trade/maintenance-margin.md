> For the complete documentation index, see [llms.txt](llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](igniz-trade/maintenance-margin.md).

# Maintenance Margin

#### **What Is Maintenance Margin?** <a href="#what-is-maintenance-margin" id="what-is-maintenance-margin"></a>

Maintenance margin is the minimum amount of equity a position must retain in order to stay open. It is distinct from **initial margin**, which is the collateral required to *open* a position at your selected leverage. Initial margin answers "how much collateral do I need to enter this trade?" Maintenance margin answers "how far can my equity fall before this position is at risk of being closed?"

As a position's unrealized loss grows, account equity shrinks toward the maintenance margin requirement. Once equity falls below that threshold, the position becomes eligible for liquidation. See [Liquidation Mechanism](igniz-trade/liquidation-mechanism.md) for the full trigger and settlement process.

> Note: Exact maintenance-margin percentages vary by market and by the leverage tier in effect for that position size, and are shown directly in the trading interface before and after you open a position.

#### **The Maintenance Margin Ratio** <a href="#the-maintenance-margin-ratio" id="the-maintenance-margin-ratio"></a>

The maintenance margin requirement is expressed relative to a position's notional value — the maintenance margin ratio. As a position moves against you:

1. Unrealized losses reduce account equity.
2. Equity is continuously compared against the maintenance margin requirement for the position's current size and leverage tier.
3. As equity approaches the maintenance threshold, the position moves progressively closer to liquidation.
4. If equity breaches the threshold, the platform's liquidation process activates.

Because the maintenance margin requirement scales with leverage — higher maximum leverage corresponds to a lower maintenance margin ratio, and lower maximum leverage corresponds to a higher ratio — the maintenance margin ratio effectively defines how much adverse price movement a position can absorb before liquidation risk begins.

#### **Cross vs. Isolated Margin Context** <a href="#cross-vs-isolated-margin-context" id="cross-vs-isolated-margin-context"></a>

How maintenance margin is evaluated depends on your collateral allocation mode:

* **Unified (cross) collateral mode:** Maintenance margin is evaluated against total account equity — including unrealized profit/loss across all positions sharing that collateral pool — compared to the aggregate maintenance requirement across those positions. Strong performance in one position can offset a shortfall in another.
* **Segregated (isolated) collateral mode:** Maintenance margin is evaluated only against the margin allocated to that specific position. A shortfall in one segregated position does not draw on unified collateral or other segregated positions, and vice versa.

See [Margin](igniz-trade/margin.md) for the full breakdown of collateral allocation modes and how required collateral is calculated.

#### **Staying Ahead of Maintenance Margin** <a href="#staying-ahead-of-maintenance-margin" id="staying-ahead-of-maintenance-margin"></a>

Traders can reduce the risk of breaching maintenance margin by:

* Monitoring the leverage utilization ratio on open positions, since the platform validates leverage only at position initialization.
* Adding margin to a segregated position, or depositing additional collateral to a unified account.
* Partially or fully closing a position before equity approaches the maintenance threshold.
* Using stop-loss orders to exit before the liquidation process is triggered. See [Automated Exit Strategies (TP/SL)](igniz-trade/tp-sl.md).

### Related Pages <a href="#related-pages" id="related-pages"></a>

* [Margin](igniz-trade/margin.md)
* [Leverage](igniz-trade/leverage.md)
* [Liquidation Mechanism](igniz-trade/liquidation-mechanism.md)
