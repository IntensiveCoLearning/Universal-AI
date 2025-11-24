---
timezone: UTC+8
---

# 小猪

**GitHub ID:** Xboxpig

**Telegram:** @not_neptunia

## Self-introduction

初学者

## Notes

<!-- Content_START -->
# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->
\## 什么是ZetaChain?

一个通用区块链, 能够原生访问任何区块链网络,并且跨网络进行发起交易,换购数字货币

!\[\[CleanShot 2025-11-24 at 18.26.46@2x.png\]\]

这张图是一个非常好的示例，它展示了 **ZetaChain (ZetaChain)** 如何实现\*\*跨链互操作性（Omnichain Interoperability）\*\*，并让用户在一个交易中完成从一条链到另一条链的资产转移和兑换（Swap）。

\### Q:为什么 6 ETH 变成 6 ZRC-20 ETH？

A: ZetaChain 的核心功能是充当一个\*\*通用应用层（Universal App）\*\*，连接所有外部区块链（如 Ethereum、BNB Chain）。它不能直接处理原生以太坊上的 ETH，而是使用一种特殊的代币标准来代表（映射）外部链上的资产：\*\*ZRC-20 标准\*\*

\- **ZRC-20 标准:** ZRC-20 是 ZetaChain 自己定义的代币标准，专门用于在 ZetaChain 内部表示和操作外部链上的原生资产（例如 ETH、BNB、BTC 等）和 ERC-20 代币。

\- **映射机制:** 当 $6 \\text{ ETH}$ 被\*\*锁定\*\*在以太坊的网关合约中后，ZetaChain 会在内部 **铸造（Mint）** 相应数量的 **ZRC-20 ETH**。

\- **ZRC-20 ETH** 的作用就是 $6 \\text{ ETH}$ 在 ZetaChain 上的\*\*代理/凭证\*\*。

\- **锁定 $6 \\text{ ETH}$** 保证了 **ZRC-20 ETH** 拥有 $1:1$ 的价值支撑，从而保持价格稳定

\### Q: 变成6 ZRC-20 ETH之后的操作是什么?

\#### A:

1\. ZetaChain univ app 内的操作

\- **onCall:** $6 \\text{ ZRC-20 ETH}$ 被发送到 ZetaChain 上的应用合约中，同时接收到用户最初的指令 **"I want BNB"**。

\- **Swap:** 应用合约（可能是去中心化交易所 DEX）根据指令，使用 $6 \\text{ ZRC-20 ETH}$ 兑换成 **$1 \\text{ ZRC-20 BNB}$** (这是一个假设的兑换比例)。

2\. 提取和调用 BNB Chain (Withdraw and Call)

\- **提取 (Withdraw):** 交易完成兑换后，\*\*$1 \\text{ ZRC-20 BNB}$\*\* 被发送到 ZetaChain 上的 **Gateway** 合约，并指示将其提取到 BNB Chain。

\- **关键点:** $1 \\text{ ZRC-20 BNB}$ 在 ZetaChain 上被\*\*销毁（Burn）\*\*。

3\. **释放原生资产:** ZetaChain 的机制会指示 BNB Chain 上的 **Gateway** 合约，\*\*释放（Release）\*\* 相应数量的\*\*原生 BNB\*\*。

4\. \*\*最终接收:\*\* **$1 \\text{ BNB}$** 被发送到用户指定的 BNB Chain 地址，可能同时触发 BNB Chain 上的某个\*\*合约调用 (Call)\*\*。

\### Q:在这期间 ETH 网络支付给 univapp 的资产是什么? univ 支付给 bnb 网络的资产又是什么? univapp 得到了什么

\#### A:

1\. ETH 网络支付给 ZetaChain Universal App 的资产是什么？

\- ETH 网络（或更准确地说，用户通过 ETH 网络的交易）\*\*直接支付\*\*给 Universal App 的资产是 **ZRC-20 ETH**。

\> \*ETH 网络锁定, Zeta 网络相应生成了自己等数量的代币 zrc20 ETH 用于等额. -xboxpig

2\. Universal App 支付给 BNB Chain 网络的资产是什么？

\- Universal App 将其兑换得来的资产发送回 ZetaChain 的 Gateway 合约，最终促使 BNB Chain 释放\*\*原生 $\\text{BNB}$\*\*。

3\. Universal App 得到了什么？

\- Universal App（通常是一个 DEX）在这次交易中扮演了\*\*服务提供者\*\*的角色，它通常得到以下几项：

\- A. **手续费收入（核心收入）**

这是 Universal App 的主要收入来源。

\- 付出! 兑换费用 (Swap Fee): 在 $6 \\text{ ZRC-20 ETH}$ 兑换成 $1 \\text{ ZRC-20 BNB}$ 的过程中，DEX 会从兑换的总额中抽取一小部分作为\*\*交易手续费\*\*（例如 $0.2\\%$ 或 $0.3\\%$）。

\- B. **用于兑换的资产（交易流转）**

\- Universal App **收到了 $6 \\text{ ZRC-20 ETH}$** 并将其添加到其流动性池中。

\- Universal App **支付了 $1 \\text{ ZRC-20 BNB}$**（从其流动性池中取出）。

\- C. **Gas 费补偿**

\- 付出! 虽然 $Universal \\text{ App}$ 本身是在 ZetaChain 上运行，但整个跨链交易的 Gas 费用由用户在初始 $\\text{ETH}$ 交易中支付，用于覆盖 $\\text{ETH}$ 网络的 $\\text{Gas}$ 费、ZetaChain 的 $\\text{Gas}$ 费以及 $\\text{BNB}$ Chain 的 $\\text{Gas}$ 费。ZetaChain 会负责将这部分费用分发给各个网络的验证者。

\> **注意:** Universal App 的主要目的是实现资产的\*\*兑换\*\*和\*\*流转\*\*，而不是\*\*持有\*\*大量资产。它的持续运营依赖于用户支付的\*\*交易手续费\*\*。

**简而言之：** Universal App 得到了为用户提供\*\*流动性服务\*\*所收取的\*\*交易手续费\*\*。

\### Q: 这么说 univ app 应当在各个链上有大量的资产 才可以提供跨链流动性 因为所谓的 ZRC-20 ETH/BNB 其实不被各个链所认可和接受的

\#### A:

1\. ERC-20 ETH/BNB 其实不被各个链所认可和接受 是\*\*完全正确的\*\*。ZRC-20 代币只存在于 ZetaChain 内部，外部链（如 Ethereum 或 BNB Chain）对此一无所知

2\. **“Universal App 应当在各个链上有大量的资产才可以提供跨链流动性”** 这一推论，在 ZetaChain 的架构中 说的有些问题

\- ZetaChain(去中心化的验证者集群) 通过部署在各个链上的 **$\\text{Gateway}$ 合约**来保管和管理这些链上的\*\*原生资产\*\*储备。

\- $\\text{Universal}$ $\\text{App}$ 只处理这些原生资产在 $\\text{ZetaChain}$ 上的\*\*数字凭证（ZRC-20）\*\*。

\### 碎碎念

\>\*我怎么觉得这就像一个去中心化的 链上 Hook 呢... - xboxpig
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
