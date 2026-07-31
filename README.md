> For the complete documentation index, see [llms.txt](https://igniz.gitbook.io/igniz-docs/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://igniz.gitbook.io/igniz-docs.md).

# Igniz Overview

### **What is Igniz?**

Igniz is an AI-native decentralized exchange platform built on Web3 infrastructure, delivering a fully trust-less, non-custodial trading ecosystem with a native AI layer covering strategy automation, market intelligence, and risk monitoring. The platform seamlessly integrates multiple Layer 1 and Layer 2 blockchain protocols with multi-chain deposit support, enabling users to deposit and trade assets from any supported blockchain network directly within a unified interface while maintaining complete ownership and control of their assets.

### **AI Layer**

Igniz is built around a native AI layer that operates in direct continuity with the trading engine, covering strategy automation, execution, market intelligence, and capital protection.

#### **NLP Strategy Automation**

Translates plain-language trading instructions into fully executable, rule-based strategies spanning the complete lifecycle from synthesis and optimization to backtesting and live deployment. Traders describe their strategy in natural language; the engine converts it into a structured trading script, fine-tunes parameters across the full configuration space, validates it against historical data, and deploys it to the live execution engine with a single action.

#### **Text-to-Algo (T2A)**

T2A is the execution primitive underlying NLP Strategy Automation and a standalone order type in its own right, operating separately from standard Limit and Market orders. A T2A order encapsulates a complete strategy: an entry condition, up to five Take-Profit legs, and a condition-based or trailing Stop-Loss. Once confirmed, the T2A engine monitors live market data continuously and places child orders the moment conditions are met, running autonomously 24/7 across Futures markets.

#### **Market Intelligence Agent**

A conversational AI interface purpose-built for active crypto traders. It operates on multi-dimensional market data including price action patterns, volatility regime shifts, and volume anomalies to surface actionable trading insights in real time. The Agent draws on an enterprise-grade multi-model infrastructure integrating leading frontier models including OpenAI GPT, Anthropic Claude, Google Gemini, Grok, Perplexity AI, and more.

#### **Risk Guard**

A continuous background surveillance system that monitors for high-risk market conditions and behavioural risk patterns simultaneously. It detects flash crashes, extreme slippage, network congestion, and pump-and-dump patterns, issuing structured alerts before adverse events materialize into losses.

### **Technical Overview**

Igniz is architected as a multi-layer decentralized trading platform optimized from the ground up to address the fundamental challenges of blockchain scalability, transaction costs, and user experience. At the infrastructure level, a native AI layer runs in direct continuity with the trading engine, handling strategy execution, market analysis, and risk surveillance without routing through external systems or intermediaries.

Igniz scales horizontally across multiple blockchain networks, supporting both Layer 1 solutions (Ethereum, BSC, Solana, Avalanche, Fantom, Cardano, Cosmos) and Layer 2 scaling solutions (ZK rollups, sidechains). The platform features multi-chain deposit infrastructure, allowing users to deposit and trade assets from any supported blockchain network directly within the interface. The platform leverages Inter-Blockchain Communication (IBC) protocols and cross-chain bridges to enable seamless asset migration across chains. Future development includes deployment of custom Cosmos SDK-based blockchains and ZK-STARKs rollups to further enhance throughput and scalability.

The platform features decentralized liquidity pools and complete privacy with no KYC requirements. Users need only connect a Web3-compatible wallet to begin trading immediately with no registration, volume caps, or withdrawal limits.
