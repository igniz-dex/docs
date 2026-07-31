> For the complete documentation index, see [llms.txt](https://igniz.gitbook.io/igniz-docs/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://igniz.gitbook.io/igniz-docs/igniz-trade/order-types.md).

# Trading Order Classifications

#### Primary Order Categories <a href="#primary-order-categories" id="primary-order-categories"></a>

Igniz supports comprehensive order type functionality enabling both simple execution strategies and sophisticated algorithmic trading approaches.

**Market Execution**

Market orders execute immediately against available order book liquidity at the best available prices. These orders prioritize execution speed over price certainty, filling against resting limit orders until the specified quantity is satisfied or liquidity is exhausted.

**Limit Execution**

Limit orders specify a maximum purchase price (for buy orders) or minimum sale price (for sell orders). Execution occurs only at the designated limit price or more favorable levels. Unfilled limit orders rest on the order book awaiting counterparty matches.

**Conditional Stop Market**

Stop market orders remain dormant until price action triggers activation. Upon reaching the specified trigger threshold, the order converts to an active market order and executes immediately.

Trigger logic varies by position direction:

* **Long positions:** Trigger price must exceed current mid-market price (protection against upward breakouts)
* **Short positions:** Trigger price must fall below current mid-market price (protection against downward breakouts)

**Conditional Stop Limit**

Stop limit orders combine conditional activation with limit price execution. When price reaches the trigger threshold, the order activates as a standard limit order at the specified limit price rather than executing as a market order.

**Conditional Take Market**

Take market orders activate when price moves favorably to a specified target level. Upon trigger activation, execution proceeds as an immediate market order.

Trigger configuration:

* **Long positions:** Trigger price must be below current mid-market price (capturing downside targets)
* **Short positions:** Trigger price must exceed current mid-market price (capturing upside targets)

**Conditional Take Limit**

Take limit orders activate upon reaching favorable price targets, then execute as limit orders at the designated limit price. This hybrid approach combines conditional logic with controlled execution pricing.

**Range Limit Distribution**

Range orders place multiple limit orders distributed across a specified price range. This order type enables systematic liquidity provision or dollar-cost-averaging strategies without manual order placement at each price level.

#### Order Execution Modifiers <a href="#order-execution-modifiers" id="order-execution-modifiers"></a>

Execution modifiers alter fundamental order behavior, enabling precise control over position management and risk parameters.

**Position Reduction Only**

This modifier restricts orders to reducing existing positions, preventing inadvertent position direction reversals. Orders tagged as reduction-only will not establish new positions in the opposite direction beyond zero exposure.

**Persistent Resting (GTC)**

Good-Till-Cancel orders remain active on the order book indefinitely until complete execution or manual cancellation. This default modifier suits longer-term limit orders anticipating gradual fills across multiple blocks.

**Passive-Only Execution (AON)**

Add-Only-No-Match orders (also called post-only orders) ensure liquidity provision without immediate execution. These orders reject if they would cross the spread and execute as aggressive taker orders. Successfully placed AON orders always rest as maker liquidity, guaranteeing maker fee classification.

**Immediate Execution or Cancellation (IOC)**

Immediate-Or-Cancel orders attempt immediate execution against available book liquidity. Any unfilled quantity automatically cancels rather than resting on the book. IOC orders function as market orders with quantity limits, executing available liquidity without persistent resting.

**Automated Profit Target**

Profit target orders activate when positions reach specified favorable price thresholds. These conditional orders automatically execute as market orders upon trigger activation, systematically capturing gains without manual monitoring.

Configuration permits partial position closure, allowing traders to scale out of positions incrementally. Limit price conversion options enable profit-taking with execution price controls.

**Automated Loss Limitation**

Loss limitation orders trigger when positions reach adverse price thresholds, automatically executing market exit orders to cap downside exposure. These protective orders minimize losses during unfavorable price movements or unexpected volatility events.

Traders configure both trigger price and exit quantity, enabling partial position reduction rather than complete liquidation. Optional limit price parameters provide execution price boundaries for loss exits, though market execution remains the default for maximum exit certainty.

**Combined TP/SL Implementation:**

Both profit target and loss limitation orders can be attached simultaneously to positions, creating automated bracketed exits that capture favorable moves while limiting adverse exposure. The platform manages these conditional orders independently, canceling the unreached condition upon execution of either trigger.
