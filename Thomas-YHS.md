---
timezone: UTC+8
---

# Thomas

**GitHub ID:** Thomas-YHS

**Telegram:** @Thomas_YHS

## Self-introduction

LXDAO成员，智能合约开发者，AI Agent开发者，参与了 ZetaChain 中文文档翻译，希望这次共学黑客松可以学习进步。

## Notes

<!-- Content_START -->
# 2025-12-01
<!-- DAILY_CHECKIN_2025-12-01_START -->
打卡一下
<!-- DAILY_CHECKIN_2025-12-01_END -->

# 2025-11-29
<!-- DAILY_CHECKIN_2025-11-29_START -->

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Thomas-YHS/images/2025-11-29-1764429918263-image.png)
<!-- DAILY_CHECKIN_2025-11-29_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->


今天整理了笔记，之后看了昨天workspace的回放，先打个卡，之后今天有了一个AI金库的想法，先打个卡免得被淘汰，正在想可行性。

# ZetaChain 介绍

## Universal Blockchain

ZetaChain 是一个 **跨链互操作性（Interoperability）平台**，它的目标是成为 **“Universal Blockchain（通用区块链）”** —— 一个能够**原生连接所有区块链**、让资产和数据在任何链之间自由流动的基础设施。

> **ZetaChain 是一个能够让所有区块链互相通信、互相调用智能合约、互相转账的“跨链操作系统”。**它让 **Bitcoin → Ethereum、Solana → BNB Chain、Sui → Polygon** 等跨链交互像在同一条链上一样简单。

## Universal PoS

ZetaChain 采用了PoS 的质押模式，基于Cosmos SDK + CometBFT 来构建的公链。

> Universal EVM allows smart contracts to read/write to other chains.

## Universal EVM

> Universal EVM allows smart contracts to read/write to other chains.

-   一个智能合约控制多个链的钱包
    
-   在 ZetaChain 上写一个合约 = 控制所有链上的合约/资产
    
-   开发者用 EVM 语言（Solidity）就能写多链应用
    

## Universal Smart Contract

-   跨链 orchestrate（编排）复杂动作
    
-   一次交易十链联动
    
-   从任意链读写数据
    
-   访问任何链的原生资产
    
-   不需要跨链桥
    

我觉得 ZetaChain 在 Defi 方面厉害的点是不需要跨链桥就可以完成跨链操作，如果大面积使用的话会减少跨链 Defi 产品的成本，增强资金的流动性。

# 为什么选用 ZetaChain ？

## Chain Orchestrate（跨链协调能力）

> One of the standout features of ZetaChain is its ability to **orchestrate complex transactions** across multiple blockchains from a single smart contract. Universal apps built on ZetaChain can manage incoming token transfers and contract calls from different blockchains and initiate outgoing transactions to connected networks—all within a unified framework.

跨链编排能力可以允许你只维护有限的代码，就可以实现复杂的跨链操作。你不需要再为不同的代码仓库发愁，只需要管ZetaChain的一个代码仓库即可。

## **Forward Compatibility（自动兼容未来所有链）**

> When you deploy an app on ZetaChain, it automatically connects to all networks in the ZetaChain ecosystem. As new blockchains are added, your universal app gains compatibility with these networks without any additional effort or modifications to your contract's source code. This forward compatibility is a significant advantage, as it future-proofs your application. You don't have to anticipate which blockchains might become popular or widely adopted in the future; your app will be ready to interact with them as soon as they are integrated into ZetaChain. This not only saves development time but also ensures that your app remains relevant and accessible to users across different blockchain networks.

当未来有新链发布，你想要接入新链，不需要更改代码自动接入新链。

## Transfer Native Tokens

> For example, **users can send native USDC from Ethereum directly to Polygon, or swap native BTC from the Bitcoin network for PEPE tokens on Ethereum.** This cross-chain token transfer functionality enhances liquidity and provides a seamless experience, eliminating the need for intermediaries or complex token wrapping mechanisms.
> 
> To facilitate these transfers, universal apps have access to unified liquidity pools on ZetaChain. These pools aggregate liquidity from all connected chains, containing tokens that have been transferred to ZetaChain from their respective networks.
> 
> Adding liquidity to these unified pools involves transferring tokens to the custody of ZetaChain validators on the connected chains. For instance, ZetaChain validators hold custody of ETH, USDC, and other tokens on Ethereum. This custody mechanism is secured through robust consensus protocols and validator incentives, ensuring the safety of the assets.
> 
> This system allows users to send tokens to and from universal apps, as well as directly between connected chains, without the need for centralized exchanges or third-party bridges. It streamlines the user experience and reduces the risks associated with cross-chain transactions, such as double-spending or transaction failures.
> 
> By using native tokens, users benefit from the inherent properties of those assets, including their security features, consensus mechanisms, and existing network effects. This enhances trust and reliability in cross-chain operations conducted through ZetaChain-powered universal apps.

支持从不同的链转移原生代币到 ZetaChain ，增加了资金安全性，降低了跨链桥的风险。同时 ZetaChain 维护跨链流动池，并由验证者管理原生资产的托管，安全级别类似于 Tendermint+BFT 的验证者模型。
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->



今天先打个卡，刚到家，准备一会写一个ZRC-20合约，本地部署一下
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->




今天主要看了ZetaChain的文档，搭建了ZetaChain cli 测试网节点
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->





打卡第一天
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
