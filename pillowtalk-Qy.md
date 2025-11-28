---
timezone: UTC+8
---

# Qy

**GitHub ID:** pillowtalk-Qy

**Telegram:** @Qy

## Self-introduction

好好学习，天天向上

## Notes

<!-- Content_START -->
# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->
Workshop回放：

【ZetaChain 通用资产与跨链 DeFi 开发导论】 [https://www.bilibili.com/video/BV1zWSgBnEcE/?share\_source=copy\_web&vd\_source=fd6ac63c6fb1f02dcdf46371c30b2168](https://www.bilibili.com/video/BV1zWSgBnEcE/?share_source=copy_web&vd_source=fd6ac63c6fb1f02dcdf46371c30b2168)

1.观看昨天的回放，根据回放复盘前几天的操作内容
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->

摸鱼一天
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->


[ZetaChain](https://www.zetachain.com/docs)是一个与 EVM 兼容的区块链，具有内置的跨链互操作性。

部署在 ZetaChain 上的智能合约可以处理来自任何已连接区块链（例如以太坊、Base、Polygon、BNB、Solana 和比特币）的_入站合约调用。ZetaChain 上的合约也可以向已连接链上的合约发出出站合约调用_。合约调用可以包含任意数据、gas 费用以及支持的 ERC-20 代币等有效载荷。

从连接链转移到 ZetaChain 合约的 Gas 和 ERC-20 代币在 ZetaChain 上以[**ZRC-20**](https://www.zetachain.com/docs/developers/tokens/zrc20)（一种类似 ERC-20 的代币）的形式呈现。例如，当您使用 ETH 发起调用时，ZetaChain 上的合约会收到 ZRC-20 ETH。实际上，ETH 被锁定在 ZetaChain 托管的以太坊服务器上，同时等量的 ZRC-20 ETH 代币被铸造到 ZetaChain 的合约中。任何人都可以无需许可地将 ZRC-20 提取回原生资产。

用于实现这些传入（到 ZetaChain）和传出（从 ZetaChain）通信的机制称为[**网关（Gateway**](https://www.zetachain.com/docs/developers/evm/gateway) [）。每条连接的链都有一个网关，作为与 ZetaChain 上的合约交互的单一入口点。在以太坊和 Polygon 等连接的 EVM 链](https://www.zetachain.com/docs/developers/chains/evm)上存在网关合约， [Solana](https://www.zetachain.com/docs/developers/chains/solana)上存在网关程序，比特币上存在网关地址。ZetaChain[本身](https://www.zetachain.com/docs/developers/chains/zetachain)也有一个网关合约，用于触发传出调用。

例如，[您可以编写一个合约](https://www.zetachain.com/docs/developers/tutorials/swap)，用于处理来自一条链的传入代币，将其兑换为目标代币，然后提取到目标链。要使用此合约执行跨链兑换，用户可以调用一个网关（例如以太坊上的网关），并传入一定数量的 USDC、要调用的 ZetaChain 合约以及包含目标代币地址的数据负载。监视网关的 ZetaChain 验证者会注意到这笔交易，并向 ZetaChain 上的兑换合约发起跨链交易。兑换合约接收 USDC 和负载后，执行兑换操作，并将目标代币提取到目标链（例如比特币）。通过这笔交易，以太坊用户发起了一次跨链合约调用，调用了 ZetaChain 合约，该合约将 ZRC-20 ETH 兑换为 ZRC-20 BTC，并将比特币上的原生 BTC 提取给了接收方。

用户只需在选定的区块链上进行一笔交易，即可在不同区块链之间进行跨链兑换。他们无需担心 gas 费用、资产桥接或钱包切换等问题。当然，兑换只是其中一个例子。NFT 市场、借贷平台、多链治理机制、收益聚合器等等，都可以在 ZetaChain 上构建。

我们将这类合约称为[**通用应用**](https://www.zetachain.com/docs/developers/apps/intro)，因为它们可以跨所有连接的区块链协调复杂的跨链交易。将跨链应用编写成通用应用非常容易，因为大部分逻辑都封装在 ZetaChain 上的单个合约中，该合约可以处理传入的调用并向其他链上的合约发出调用。
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->



1.注册 / 配置好 ZetaChain 开发所需环境

2.注册 Qwen 账号并确认可以进入控制台。

3.加入 ZetaChain 中国开发者社区。

4.ZetaChain是首个能够跨所有区块链生态系统实现原生连接的通用区块链。

5.**通用POS机**

ZetaChain 的权益证明系统基于 Cosmos SDK 和 Comet BFT 构建，可确保高效的跨链操作：

-   5秒终结
    
-   核心验证者和观察者-签名者验证者确保网络和跨链安全
    
-   优先考虑所有连接链安全性的激励机制
    

6.**通用电子投票机**

通用 EVM 是一个可以从任何区块链调用的执行环境，允许开发人员：

-   管理和交互跨连接链的原生资产、合约和钱包
    
-   在熟悉的 EVM 环境中创建具有内置跨链功能的应用程序
    

7.**通用智能合约**

通用智能合约原生部署在 ZetaChain 上，可以读写任何连接的链。它通过以下方式简化跨链开发：

-   协调复杂的多链行动
    
-   获取不同网络的流动性
    
-   提供统一的跨链用户体验
    
-   必要时通过添加特定于链的合约来扩展功能
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
