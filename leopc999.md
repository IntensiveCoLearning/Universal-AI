---
timezone: UTC+8
---

# Leo

**GitHub ID:** leopc999

**Telegram:** @elegant_5T

## Self-introduction

A Web3 enthusiast interested in AI and DeFi.

## Notes

<!-- Content_START -->
# 2025-12-05
<!-- DAILY_CHECKIN_2025-12-05_START -->
今天观看了 Workshop 的回放视频
<!-- DAILY_CHECKIN_2025-12-05_END -->

# 2025-12-03
<!-- DAILY_CHECKIN_2025-12-03_START -->

继续学习资料，形成总结笔记
<!-- DAILY_CHECKIN_2025-12-03_END -->

# 2025-12-01
<!-- DAILY_CHECKIN_2025-12-01_START -->


ZetaChain + Universal EVM + Gateway + 多链 (EVM / 非 EVM / Bitcoin…) 之间的调用 / 资产 / 消息流动关系总结如下：

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/leopc999/images/2025-12-01-1764600215564-image.png)
<!-- DAILY_CHECKIN_2025-12-01_END -->

# 2025-11-30
<!-- DAILY_CHECKIN_2025-11-30_START -->



## 🌐 ZetaChain 的定位 — “通用区块链 (Universal Blockchain)”

-   ZetaChain 是一个 **Layer-1 公链 (L1)**，但区别于传统单链，它的目标是成为 “通用 (universal) 区块链”，即一个能**连接、抽象 (abstract)、桥接** 多条不同区块链 (包括 EVM 链、非 EVM 链，甚至比特币这样的传统链) 的中心枢纽。
    
-   它基于 Cosmos SDK + Tendermint/CometBFT 共识机制构建。
    
-   核心特点是 **链无关 (chain-agnostic/interoperable)** —— 意味着开发者可以不必针对每条链分别构建 dApp，而是通过 ZetaChain 构建一次，就能面向所有连接链 (connected chains)。
    
-   对开发者 / 用户来说，好处是统一、简化的跨链用户体验 (无需手动切换网络、无需复杂桥接逻辑)；对资产来说，有可能实现统一流动性 (unified liquidity) 而不是碎片化流动性。
    

简单来说，ZetaChain 的愿景是 “一条链连接所有链 (and beyond)” —— 为整个 Web3 构建一个跨链基础设施。

* * *

## 🧰 Universal EVM (通用 EVM)

-   “Universal EVM” 就是 ZetaChain 上的智能合约执行层 (execution environment)。它兼容 EVM (因此你可以用 Solidity / Vyper / 以太坊生态的开发工具) 编写合约。
    
-   与普通 EVM 的区别是：Universal EVM 内建 “多链互操作 (omnichain interoperability)” 特性。也就是它不是仅在单链 (ZetaChain) 内执行逻辑，而可以**调用、管理、跨链 (跨不同区块链) 的资产和消息交互**。
    
-   对开发者意味着：你可以用熟悉的 EVM 工具 / 语言 (Solidity 等) 来构建 “跨链 / 通用 (universal)” 应用 (Universal Apps)，门槛较低。
    

因此，Universal EVM 是 ZetaChain 的基础 — 它把 EVM 的易用性 + 跨链能力结合起来。

* * *

## 📱 Universal App / Omnichain Smart Contract

-   “Universal App” (通用应用) 是部署在 Universal EVM 上的智能合约 (或 dApp)。与传统 dApp 不同，Universal App **原生 (native) 支持所有连接链 (connected chains)** — 无论是 EVM 链 (Ethereum, BNB, Polygon 等)、非 EVM 链 (比如比特币, Solana) 都可以与之交互。
    
-   “Omnichain Smart Contract” 也是对这种跨链合约/应用模型的称呼 — 合约逻辑一次写，就可以在多个链之间调用、转移资产、跨链发送消息/交易。
    
-   功能包括：接收来自任意连接链 (call, message, token transfer)、触发向任意链的合约调用 + 资产转移 (token transfer) 等。这样就可以构建复杂的、多链、多阶段 (multi-leg / multi-chain) 应用逻辑。
    
-   对最终用户 (或其他链) 来说，这意味着他们可以 “不切换网络 (no network switching)” 就与跨链 dApp 交互 — 用户体验 (UX) 更统一、更无缝。
    

总结：Universal App / Omnichain Smart Contract 是 ZetaChain 的关键 — 它让 “一份代码 / 一套合约” 就能服务整个多链生态。

* * *

## 🛣️ Gateway / Cross-Chain / Chain Abstraction Framework (CAF) —— 如何实现跨链

这些术语紧密相关，是 ZetaChain 实现跨链功能的关键机制 / 组件。

| 名称 | 含义 / 功能 |
| --- | --- |
| Chain Abstraction Framework (CAF) | ZetaChain 的核心架构 —— 通过 CAF，ZetaChain 的网络 (节点) 能够观察 (observe) 并签名 (signer) 不同链 (connected chains) 的交易，读写这些链的资产或状态。也就是说 CAF 将底层的区块链差异「抽象化 (abstract)」，为上层 Universal EVM + Universal App 提供统一接口。 |
| Gateway | Gateway 是一个接口 (contract / contract-set / API)，充当 “单一入口 (single entry point)” —— 使得来自任意连接链 (connected chain) 的交互 (token deposit, calls, token withdrawals, cross-chain calls) 都可以通过它发起并被路由到 ZetaChain 上的 Universal App，或从 ZetaChain 发回其他链。实际上，在每条连接链上 (如 EVM 链) 都会部署一个 Gateway 合约 (或等效机制)，作为该链 ↔ ZetaChain 的桥 / 门。 |
| Cross-Chain (跨链) / Cross-Chain Transactions (CCTX) | 使用 ZetaChain + Gateway + Universal EVM/App 实现的跨链转账、跨链合约调用、资产 / 数据 /消息传递。开发者可以通过例如 depositAndCall 这样的方法，从某条源链 (source chain) 存入资产/消息，并触发在 ZetaChain 上或目的链 (destination chain) 的逻辑执行 / 资产转移。这种跨链交易 (CCTX) 支持 EVM 链间，也支持更异构 (heterogeneous) 链 — 例如比特币、Solana、TON、Sui 等 (取决是否已连接) 。 |

> 简而言之：CAF 提供跨链基础设施 & 节点能力，Gateway 提供统一接口 / 通道 (entry point)，Cross-Chain Transaction 则是开发者 / 用户最终使用跨链能力的方式。

* * *

## 🎯 开发者 & 系统使用者视角 — 意义与优势

对于开发者或任何想构建 / 使用跨链 dApp 的人来说，ZetaChain 提供了：

-   **统一开发体验**：借助 Universal EVM + Universal App + EVM 兼容性，你可以用熟悉的 Solidity / EVM 工具链 (Hardhat / Foundry / etc.) 来开发跨链应用。
    
-   **跨链资产 + 流动性 + 状态管理**：资产 (token)、资产转移 (token transfer)、合约调用 (contract calls)、消息 (message) 都能跨链 — 减少流动性碎片化、状态孤岛 (state silo) 。
    
-   **更好的用户体验 (UX)**：最终用户不需要频繁切换网络 / 链，也不需要使用传统桥 (bridge) + 包装 (wrapped token) 手段，就能访问多链资产 & 应用。
    
-   **更灵活 & 强大的应用场景**：跨链 swap, 跨链 token transfer, 跨链合约调用, 跨链消息 — 为 DeFi、cross-chain NFT / token、跨链身份 (identity)、multi-chain games 等场景打开了可能性。
    

* * *

## 🧩 总结

-   把 ZetaChain 看作 “多链 + 跨链 + EVM 兼容 + 协调 (or 橋接)” 的基础设施；不是简单的另一条 EVM 链，而是一个连接、抽象、统一多链生态的通用平台。
    
-   Universal EVM 是它的核心 — EVM 的熟悉工具 + 跨链功能的组合，让开发门槛低。
    
-   Universal App / Omnichain Smart Contract = “一次部署，多链通用”。适合构建跨链 dApp。
    
-   Gateway + Chain Abstraction Framework (CAF) + Cross-Chain Transaction 构成整个跨链体系 — Gateway 是入口 / 桥 / 路由，CAF 是链间连接逻辑 / 节点基础设施，Cross-Chain Transaction 是你实际发起的跨链交互。
    
-   对当前的 ERC4626 Vault / DeCC /跨链背景来说，ZetaChain 的这些机制可能非常便利 — 它能让你在一个平台上管理多链资产、跨链合约与资产流动，而不必为每条链都写不同逻辑。
<!-- DAILY_CHECKIN_2025-11-30_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->




继续阅读相关文档
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->





参加了第一次 Workshop，之后还需要看回放视频来巩固
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->






了解了 ZetaChain 的通用 App 的概念，正在学习 Gateway 的含义。
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->







今天配置了开发环境，学习了一些基础知识。
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->








第一天学习，简单阅读了 ZetaChain 的说明文档
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
