---
timezone: UTC+8
---

# Steph

**GitHub ID:** kuove

**Telegram:** @stephkuove

## Self-introduction

对web3感兴趣的开发者

## Notes

<!-- Content_START -->
# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->
| 特性 | ERC-20 | ZRC-20 |
| --- | --- | --- |
| 应用平台 | 主要在 以太坊（Ethereum） 及其兼容网络（EVM 链）。 | 在 ZetaChain 上运行。 |
| 资产范围 | 只能代表和管理部署在同一条链上的资产。 | 可以管理和操作所有 ZetaChain 连接的外部链上的原生资产（如 ETH, BNB, BTC）。 |
| 跨链能力 | 不具备原生跨链能力。需要通过**桥接（Bridging）/包裹（Wrapping）**机制实现跨链。 | 具备原生全链（Omnichain）能力。它代表了外部链上被锁定在 ZetaChain 托管地址中的原生资产。 |
| 交易燃料 | 交易费用（Gas）通常使用该链的原生代币（例如，在以太坊上使用 ETH）。 | 可直接在 ZetaChain 智能合约中接收和使用外部链的原生 Gas 资产（通过 ZRC-20 形式）。 |
| 底层实现 | 基础的代币功能（transfer, approve, balanceOf）。 | 是 ERC-20 的扩展，包含了额外的逻辑和接口，使其能够与 ZetaChain 的网关（Gateway）协议交互，实现资产的存入和取出。 |

**通用 NFT 通行证 (Universal NFT Passport)**

**概念**

通用 NFT 通行证（或称为全链 NFT）允许一个 NFT 在**一个区块链**上被铸造和拥有，但其**实用性、访问权限或状态**可以被**任何其他连接的区块链**上的应用验证和使用。

**工作原理**

1.  **NFT 铸造:** ◦ 一个 NFT（如一个“VIP 会员卡”）在 **Ethereum** 链上被铸造。
    
2.  **状态追踪 (ZetaChain 监听):** ◦ ZetaChain 持续监听 Ethereum 上的 NFT 合约，跟踪该 NFT 的当前**所有者地址**。
    
3.  **通用 NFT 通行证应用:** ◦ 一个应用（例如一个游戏）部署在 **Polygon** 上，但它需要验证用户是否拥有这个 Ethereum 上的 NFT。 ◦ 传统的做法是让用户桥接 NFT（创建一个包裹版本），这会产生新的合约和碎片化风险。 ◦ 在 ZetaChain 方案中，**Polygon 上的应用**向 **ZetaChain** 发送一个查询请求：“地址 $X$ 是否拥有这个 Ethereum 上的 VIP NFT？”
    
4.  **跨链验证:** ◦ ZetaChain 利用其状态观测能力和全链智能合约，直接确认地址 $X$ 是 Ethereum 上的 NFT 所有者。 ◦ ZetaChain 将**验证结果**（`True` 或 `False`）安全地发送回 Polygon 上的应用。
    
5.  **授予权限:** ◦ Polygon 上的游戏应用根据 ZetaChain 的验证结果，授予用户游戏内的 VIP 特权。
    

**应用价值**

• **消除碎片化:** NFT 的本体始终保存在其铸造的原生链上（例如 Ethereum），无需在其他链上创建包裹或镜像版本，保证了 NFT 的单一真实来源。 • **全链实用性:** NFT 的所有权可以解锁任何连接链上的实用功能，如访问权限、折扣或投票权。 • **跨链身份:** NFT 可以充当用户的**全链身份凭证**，允许用户在任何支持 ZetaChain 的链上应用中使用其身份，而无需将资产移动到目标链。
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->

![wechat_2025-11-27_220450_264.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/kuove/images/2025-11-27-1764252340702-wechat_2025-11-27_220450_264.png)

使用测试链实现hello信息传递
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->


Universal apps在24号的笔记中总结过，

-   可以接收已连接的链上的合约调用
    
-   可以触发合约调用，进行代币在不同链上的转换
    
-   完全兼容 EVM 链（如以太坊、BNB 和 Polygon）、比特币、Solana、TON 和 Sui。
    
-   原生 Gas 和支持的 ERC-20 代币以 ZRC-20 代币的形式呈现。ZRC-20 代币无需许可即可提取回其原始链。
    

[**Gateway**](https://www.zetachain.com/docs/developers/evm/gateway)

适配不同的链的网关，与Universal apps进行连接交互

The implementation of the gateway depends on the connected chain:

-   EVM chains: a gateway smart contract
    
-   Solana: a gateway program
    
-   Bitcoin: a TSS MPC gateway address managed by a network of observer-signer validators
    

[https://embed.figma.com/design/mYXNTORUuvGVaQ01SF7h9Y/Gateway-for-Universal-Apps?node-id=0-1&p=f&embed-host=notion&footer=false&theme=system](https://embed.figma.com/design/mYXNTORUuvGVaQ01SF7h9Y/Gateway-for-Universal-Apps?node-id=0-1&p=f&embed-host=notion&footer=false&theme=system)

支持以下特性（在不同的链上）

-   将原生 Gas 代币存入 ZetaChain 上的Universal apps或账户
    
-   将支持的 ERC-20 代币（包括 ZETA 代币）存入 ZetaChain 上的Universal apps或账户
    
-   存入原生 gas 代币并向通用Universal apps发出合约调用（可传递任意数据）
    
-   存入受支持的 ERC-20 代币，并向Universal apps发出合约调用（可传递任意数据）。
    
-   向Universal apps发出合约调用（传递任意数据）
    

在ZetaChain上的特性（从Universal apps向其他链上调用合约或者提取代币）

-   将 ZRC-20 代币作为原生 gas 或 ERC-20 代币提取到已连接的链上
    
-   将 ZETA 代币提取到连接的链
    
-   将代币提取到已连接的链上并进行合约调用
    
-   在连接链上进行合约调用
    

目前只支持单链调用，同时支持回滚，调用失败时可以取消当前的操作
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->



1.  注册qwen，获取api，能在本地通过node调用
    
2.  阅读[**Getting Started**](https://www.zetachain.com/docs/developers/tutorials/intro)
    
3.  [Faucet](https://www.zetachain.com/docs/reference/faucet)： [这个用过](https://zetachain.faucetme.pro/)
    
4.  [RPC](https://www.zetachain.com/docs/reference/network/api)
    
5.  [Explorer](https://www.zetachain.com/docs/reference/explorers)
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->




-   Universal apps:
    
    -   can receive contract calls and tokens from users and contracts on connected chains
        
    -   can trigger contract calls and token transfers to connected chains
        
    -   can automatically handle gas for cross-chain transactions
        
    -   are fully compatible with EVM chains (like Ethereum, BNB, and Polygon), Bitcoin, Solana, TON, Sui. Support for Cosmos (through IBC) and other chains is coming soon.
        
-   Native gas and supported ERC-20 tokens are represented as ZRC-20 tokens. A ZRC-20 token can be permissionlessly withdrawn back to its original chain.
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
