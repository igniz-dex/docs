> For the complete documentation index, see [llms.txt](llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](quick-start-guide/connect-mobile-via-qr-code.md).

# Connect Mobile via QR Code

If you keep your wallet on your phone rather than in a browser extension, you can connect it to Igniz on desktop using a WalletConnect-style QR code — no extension required on the desktop side.

#### **Prerequisites** <a href="#prerequisites" id="prerequisites"></a>

* A mobile wallet app that supports WalletConnect (most major mobile wallets do).
* The desktop web browser you use to access Igniz open and ready.
* Your mobile device and its camera available to scan a QR code.

#### **Step-by-Step: Connecting via QR Code** <a href="#step-by-step-connecting-via-qr-code" id="step-by-step-connecting-via-qr-code"></a>

1. On desktop, open Igniz and select the **Connect** (or **Connect Wallet**) button.
2. From the list of wallet options, choose the **WalletConnect** / mobile option rather than a browser-extension wallet.
3. A QR code appears on screen. Keep this window open — it will remain active while you complete the next steps on your phone.
4. On your mobile device, open your wallet app and locate its scan or WalletConnect feature (this is usually a scan icon or a "WalletConnect" entry in the app's connection menu).
5. Use your phone's camera within the wallet app to scan the QR code displayed on your desktop screen.
6. Your wallet app will display a connection request from Igniz, including the site and the permissions being requested. Review the request and approve it on your phone.
7. Once approved, the desktop session updates automatically to show your wallet as connected, and you can proceed to trade.

> Note: After connecting, you may still need to complete a one-time signature to enable gas-free order placement, as described in [How to Start Trading](quick-start-guide/how-to-start-trading.md).

#### **Tips** <a href="#tips" id="tips"></a>

* Keep both the desktop screen (showing the QR code) and your mobile wallet app open at the same time — the pairing happens between the two live sessions.
* Only approve connection requests you initiated yourself. If your wallet prompts you to approve a request you did not expect, decline it.
* The QR code is time-limited. If it expires before you finish scanning, refresh the connect screen on desktop to generate a new one.

#### **Troubleshooting** <a href="#troubleshooting" id="troubleshooting"></a>

If scanning does not result in a connection, try refreshing the QR code on desktop, confirming your mobile wallet app is fully updated, and checking that your phone has an active internet connection. If problems persist, disconnect any existing session on both devices and restart the connection flow from step 1.

### Related Pages <a href="#related-pages" id="related-pages"></a>

* [How to Start Trading](quick-start-guide/how-to-start-trading.md)
* [Wallet Backup](quick-start-guide/wallet-backup.md)
