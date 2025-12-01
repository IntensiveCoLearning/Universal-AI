---
timezone: UTC+8
---

# Plato

**GitHub ID:** Plato333

**Telegram:** @Plato3

## Self-introduction

web3 building

## Notes

<!-- Content_START -->
# 2025-12-01
<!-- DAILY_CHECKIN_2025-12-01_START -->
鸽
<!-- DAILY_CHECKIN_2025-12-01_END -->

# 2025-11-30
<!-- DAILY_CHECKIN_2025-11-30_START -->

准备下周考试了鸽🤣🤣
<!-- DAILY_CHECKIN_2025-11-30_END -->

# 2025-11-29
<!-- DAILY_CHECKIN_2025-11-29_START -->


再鸽一天hhh
<!-- DAILY_CHECKIN_2025-11-29_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->



**ZRC-20 的核心特性**

1.  **跨链映射与流通**
    
    外部链的资产可通过 ZetaChain 的**Gateway（网关）合约**存入并铸造成对应的 ZRC-20 代币；反之，ZRC-20 代币可销毁并兑换回原链的原生资产，实现 “原生资产 ↔ ZRC-20 代币” 的双向映射，且跨链过程无需第三方桥接，由 ZetaChain 原生支持。
    
2.  **ERC-20 兼容**
    
    ZRC-20 在接口、语法上与 ERC-20 高度兼容（如 `transfer`、`approve`、`totalSupply` 等核心方法），以太坊开发者可直接复用 ERC-20 的开发经验，无需重新学习全新的接口规范。
    
3.  **支持非 EVM 链资产**
    
    不仅兼容以太坊等 EVM 链的资产，还能将比特币、狗狗币等**非 EVM 链的原生资产**映射为 ZRC-20 代币，这是普通 ERC-20 无法实现的核心能力。
    
4.  **统一的全链资产层**
    
    所有跨链资产在 ZetaChain 上都以 ZRC-20 形式存在，开发者开发 Universal App（全链应用）时，只需对接 ZRC-20 标准，即可处理多链资产，无需适配不同链的资产协议。
    

**ZRC-20 的工作原理（以 BTC 映射为例）**

1.  **铸币（Mint）**：用户将 BTC 转入 ZetaChain 为比特币设计的网关地址，ZetaChain 确认到账后，在链上铸造等量的 **ZRC-20 版本 BTC** 并发送给用户。
    
2.  **跨链转账 / 使用**：用户可在 ZetaChain 或其他接入 ZetaChain 的公链上，使用 ZRC-20 BTC 进行交易、DeFi 借贷、调用全链应用等操作。
    
3.  **销毁（Burn）**：用户若需取回原生 BTC，销毁持有的 ZRC-20 BTC，ZetaChain 验证后，通过网关将原生 BTC 划转至用户指定的比特币地址。
    

**ZRC-20 的应用场景**

1.  **全链 DeFi**：基于 ZRC-20 实现跨链 Swap（兑换）、借贷、流动性挖矿，比如用 ZRC-20 BTC 在以太坊上直接兑换 ERC-20 USDT，无需跨链桥。
    
2.  **Universal App 开发**：作为全链应用的资产载体，比如跨链支付应用中，用户可通过 ZRC-20 代币在不同链间完成无缝转账。
    
3.  **多链资产管理**：将不同链的资产统一映射为 ZRC-20 后，在 ZetaChain 上实现一站式的资产管理与操作。
    

总结：

ZRC-20 是**ZetaChain 区块链网络中定义跨链代币的核心标准**，衍生自以太坊的 ERC-20 代币标准，但针对 ZetaChain 的**全链互操作性**进行了关键扩展，是 ZetaChain 实现多链资产互通的基础协议。

ERC-20 是以太坊单链的代币标准，仅能在以太坊生态内流通；而 ZRC-20 是 ZetaChain 为解决 “跨链资产孤岛” 问题设计的标准，它将比特币（BTC）、以太坊（ETH）、Solana（SOL）等任意公链的原生资产 / 代币，**映射为 ZetaChain 上的标准化代币**，并赋予这些代币跨链流转的能力。
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->




今天忙的事太多了，鸽一天hh
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->





测试网RPC是指向测试链的HTTP/WebSocket 接口用于进行链上操作，Faucet是水龙头，Explorer区块链浏览器如Etherscan。

今天继续学习zetachain通用AI文档，在本地部署了CIL，终端调用千问的api
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->






今天有点忙，主要学习了下官方文档
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
