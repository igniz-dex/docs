> For the complete documentation index, see [llms.txt](https://igniz.gitbook.io/igniz-docs/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://igniz.gitbook.io/igniz-docs/deposit-and-withdrawal.md).

# Deposit and Withdrawal

Igniz is built on multi-chain deposit infrastructure, letting you deposit and trade assets from any supported blockchain network directly within a single unified interface — without manually bridging assets yourself beforehand. Because Igniz is fully non-custodial, every deposit and withdrawal settles trustlessly between your own wallet and the platform's smart contracts; Igniz never takes custody of your funds.

### Multi-Chain Deposits <a href="#multi-chain-deposits" id="multi-chain-deposits"></a>

Igniz scales horizontally across multiple Layer 1 networks (Ethereum, BSC, Solana, Avalanche, Fantom, Cardano, Cosmos) and Layer 2 scaling solutions (ZK rollups, sidechains). To make this multi-chain footprint feel like one platform, Igniz leverages Inter-Blockchain Communication (IBC) protocols and cross-chain bridges so that assets can migrate across chains seamlessly.

In practice, this means:

* You can deposit collateral from whichever supported chain your assets already live on — there is no requirement to pre-bridge to a single "home" chain first.
* Cross-chain settlement is handled automatically by the platform once you authorize a deposit; you do not need to manually route assets through a separate bridging interface.
* Your trading balance is usable across supported markets regardless of which chain the underlying deposit originated from.

#### **How to Deposit** <a href="#how-to-deposit" id="how-to-deposit"></a>

1. Connect a Web3-compatible wallet to Igniz (see [How to Start Trading](https://igniz.gitbook.io/igniz-docs/quick-start-guide/how-to-start-trading.md) if you haven't connected yet).
2. Open the deposit interface via the "Deposit" button.
3. Select the source blockchain network and the asset you want to deposit. Stablecoins (USDC recommended) are the most common trading collateral.
4. Specify the deposit amount and authorize the transaction from your wallet.
5. Wait for the transaction to confirm on-chain. Cross-chain settlement is handled automatically, and your balance becomes available for trading once confirmed.

> Note: Confirmation times depend on the source network's own block times and are not set by Igniz.

#### **How to Withdraw** <a href="#how-to-withdraw" id="how-to-withdraw"></a>

Withdrawals on Igniz are trustless: your request is executed directly by smart contract, and Igniz imposes no withdrawal limits.

1. Open the withdrawal interface via the "Withdraw" button.
2. Choose the destination blockchain network and the asset you wish to withdraw.
3. Enter the recipient address and the withdrawal amount.
4. Authorize the withdrawal from your wallet. The smart contract executes the transfer directly — there is no custodial approval step and no platform-imposed withdrawal ceiling.
5. Assets arrive at your specified address on the selected chain once the on-chain transaction confirms.

### Fees <a href="#fees" id="fees"></a>

Igniz does not charge fees for deposits or withdrawals. The only cost involved is the network gas fee charged by the underlying blockchain for processing your on-chain transaction — Igniz does not add any platform fee on top of it. See the [Fee Structure Overview](https://igniz.gitbook.io/igniz-docs/igniz-trade/fee-structure.md) for how this compares to trading fees.

> Note: Gas costs vary by network and current network congestion; they are set by the blockchain, not by Igniz.

### Related Pages <a href="#related-pages" id="related-pages"></a>

* [Fee Structure Overview](https://igniz.gitbook.io/igniz-docs/igniz-trade/fee-structure.md)
* [Fund Safety](https://igniz.gitbook.io/igniz-docs/igniz-trade/fund-safety.md)
* [How to Start Trading](https://igniz.gitbook.io/igniz-docs/quick-start-guide/how-to-start-trading.md)
