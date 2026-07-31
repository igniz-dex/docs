> For the complete documentation index, see [llms.txt](/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](/igniz-trade/fund-safety.md).

# Fund Safety

Igniz is built as a fully non-custodial, no-KYC trading platform. This page explains the mechanisms that keep your funds under your control at every step, and what you are responsible for as a self-custody user.

### Non-Custodial by Design <a href="#non-custodial-by-design" id="non-custodial-by-design"></a>

Igniz never takes custody of your assets. Trading is enabled by connecting a Web3-compatible wallet — there is no registration, no identity verification, and no account onboarding step where you hand assets to a platform-controlled custodian. You retain ownership and control of your assets at all times; Igniz's role is to provide the trading engine, order matching, and settlement logic that your wallet interacts with directly.

Key implications of this model:

* **No KYC required.** Trading access is gated only by a wallet connection, not by identity verification.
* **Signature-based order placement.** Once you enable trading, orders are authorized by cryptographic signatures from your own wallet rather than by a custodial account login.
* **Trustless withdrawals.** Withdrawals execute directly through smart contract logic rather than through a manual, platform-approved payout process. See [Deposit and Withdrawal](/deposit-and-withdrawal.md) for the step-by-step flow.

### Execution-Integrity Safeguards <a href="#execution-integrity-safeguards" id="execution-integrity-safeguards"></a>

Alongside the non-custodial model, Igniz applies safeguards designed to protect the integrity of order execution itself:

* **Server-side validation.** Orders and account actions are validated before being accepted into the matching flow, reducing the risk of malformed or invalid requests reaching the order book.
* **Deterministic pre-confirmation.** Where the platform surfaces figures such as estimated liquidation price ahead of a trade, these are computed deterministically from your inputs so you can review expected outcomes before confirming.
* **Duplicate-submission protection.** Safeguards are in place to prevent the same order or transaction request from being processed more than once, reducing the risk of accidental double-execution.

> Note: These safeguards protect the integrity of the trading and settlement process; they do not substitute for the self-custody responsibilities described below, and they do not eliminate market risk. See [Risks](/risks.md) for a fuller discussion of trading and platform risk.

### Your Self-Custody Responsibility <a href="#your-self-custody-responsibility" id="your-self-custody-responsibility"></a>

Because Igniz is non-custodial, the security of your funds ultimately depends on the security of your wallet credentials — Igniz has no ability to recover funds on your behalf if your keys are lost or compromised.

* If you connect an external wallet (MetaMask, WalletConnect, OKX Wallet, etc.), protect your recovery phrase and never share it with anyone. See [Wallet Backup](/quick-start-guide/wallet-backup.md).
* Anyone who obtains your recovery phrase or private key has full control over the associated assets. Store these credentials offline and never enter them on untrusted sites or applications.

### Related Pages <a href="#related-pages" id="related-pages"></a>

* [Deposit and Withdrawal](/deposit-and-withdrawal.md)
* [Wallet Backup](/quick-start-guide/wallet-backup.md)
* [Risks](/risks.md)
