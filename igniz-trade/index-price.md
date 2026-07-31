> For the complete documentation index, see [llms.txt](https://igniz.gitbook.io/igniz-docs/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://igniz.gitbook.io/igniz-docs/igniz-trade/index-price.md).

# Index Price

#### Overview <a href="#overview" id="overview"></a>

Igniz employs multiple independent pricing mechanisms that aggregate order book data and external market feeds to establish manipulation-resistant reference prices. This multi-source approach ensures accurate valuation across all trading operations while mitigating single-point-of-failure risks.

The **Index Price** (also referred to as the **Reference Price**) is the external, market-wide fair value of an asset, derived exclusively from independent centralized exchange data. It is the anchor against which funding is calculated and against which the platform's [Mark Price](https://igniz.gitbook.io/igniz-docs/igniz-trade/mark-price.md) is measured.

#### Reference Price Feed <a href="#reference-price-feed" id="reference-price-feed"></a>

The **Reference Price** serves as the primary input for funding rate calculations. This metric aggregates weighted median prices from multiple centralized exchange sources, providing complete independence from Igniz's internal order book data.

Reference price updates occur through validator consensus approximately every 3.5 seconds, ensuring timely price discovery while maintaining decentralization and tamper resistance. Because this feed relies exclusively on external market data, it remains immune to localized manipulation attempts on the Igniz platform.

#### Update Frequency <a href="#update-frequency" id="update-frequency"></a>

Both reference and mark prices refresh synchronously with validator oracle publications, maintaining approximately 3.5-second update intervals. This frequency balances real-time responsiveness with computational efficiency across the distributed validator network.

#### Manipulation Resistance <a href="#manipulation-resistance" id="manipulation-resistance"></a>

The index price is designed to be resistant to manipulation by construction:

* **External Reference Independence:** Funding rates derive entirely from off-platform data, so activity on the Igniz order book cannot influence the index price or the funding it drives.
* **Multi-Source Aggregation:** Prices are aggregated across multiple independent centralized exchanges rather than any single venue.
* **Median-Based Calculation:** A weighted median ensures that an outlier price on any one source cannot skew the final valuation.

Because the index price consumes only external market data, it provides a trustworthy, independent benchmark for funding and for the fair-value checks that protect margin and liquidation logic.

#### Relationship to Mark Price <a href="#relationship-to-mark-price" id="relationship-to-mark-price"></a>

The index price is one of the inputs to the platform's **Mark Price** — the canonical fair value used for margin, liquidations, and conditional order execution. For how the mark price combines the index price with Igniz's own order book and an external perpetuals aggregate, see [Mark Price](https://igniz.gitbook.io/igniz-docs/igniz-trade/mark-price.md).
