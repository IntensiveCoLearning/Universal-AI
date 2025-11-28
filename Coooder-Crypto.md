---
timezone: UTC+8
---

# Coooder

**GitHub ID:** Coooder-Crypto

**Telegram:** @Coooder_Crypto

## Self-introduction

builder

## Notes

<!-- Content_START -->
# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->
## **ZetaChain 跨链资产标准**

### **ZRC-20 是什么？**

-   ZRC-20 是 **ZetaChain 原生的跨链资产标准**
    
-   能代表并操作任何连接链上的 **原生代币或 ERC-20**
    
-   让开发者 **像在单链上一样使用多链资产**
    

> 为跨链 DeFi、DEX、借贷、资产管理提供统一资产模型

### **资产如何跨链？**

| 操作 | 连接链 | ZetaChain |
| --- | --- | --- |
| 存入（Deposit） | 用户把 token 转给 TSS（或托管合约） | 铸造等量 ZRC-20 给用户 |
| 提取（Withdraw） | 销毁 ZRC-20 | TSS（或托管合约）转回原链 token |

流程特点：

-   协议控制铸造与销毁，无任意增发
    
-   原链资产进入 ZetaChain 后，被**锁定**
    

### **资产同名 ≠ 同一资产**

例如 USDT：

| 资产来源 | 在 ZetaChain 上的表示 |
| --- | --- |
| USDT from Ethereum | USDT.ETH |
| USDT from BSC | USDT.BSC |

虽然名字相同，但被视为不同资产

要跨链必须经历：

**USDT.ETH → Swap → USDT.BSC → Withdraw**

这就是 USDT 跨 ETH 與 BSC 的流程

**可编程性优势**

ZRC-20 支持：

-   跨链转账与清结算
    
-   跨链资产交换
    
-   跨链流动性聚合
    
-   Omnichain DEX、借贷协议
    
-   原生比特币参与 DeFi（无需桥）
    

可直接在 **Universal EVM** 中编写跨链逻辑

### **与普通 ERC-20 的主要区别**

| 特性 | ERC-20 | ZRC-20 |
| --- | --- | --- |
| 作用范围 | 单链 | 跨链原生支持 |
| 是否可跨链提取 | ❌ | ✔ |
| 是否具备链来源属性 | ❌ | ✔（USDC.ETH / USDC.BSC） |
| 铸造权限 | 合约自由发行 | 仅 ZetaChain 协议允许 |
| 支持原生 gas 代币 | ❌ | ✔ 如 ETH、BTC、BNB |
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->

### **📌 ZetaChain**

ZetaChain 致力于打造首个“通用区块链”，实现不同区块链间的原生互联互通，让加密世界像互联网一样开放、可访问、无边界。

**愿景背景**

当前区块链生态割裂，各链封闭、资产与用户被孤立，违背去中心化的初衷。ZetaChain 希望统一不同链，成为所有区块链的“通用入口”。

* * *

### **✨ 核心技术概念**

| 概念 | 作用 |
| --- | --- |
| Universal Blockchain | 原生连接各链，尤其是 BTC、DOGE 等非智能链 |
| Universal PoS | 基于 Cosmos SDK 和 CometBFT，提供快速安全的跨链共识 |
| Universal EVM | 可从任何链调用统一 EVM 环境执行智能合约 |
| Universal Smart Contract | 部署在 ZetaChain，可读写所有连接链上的数据和资产 |

> 开发者在一个链（ZetaChain）部署智能合约

> 就能操作所有链上的原生资产和状态

* * *

### **🧩 解决什么问题**

| 生态 | 提供能力 |
| --- | --- |
| Bitcoin | 为 BTC 引入原生 DeFi 和更多可用场景 |
| DeFi | 聚合多链流动性，统一执行层 |
| NFT/社交/游戏 | 跨链资产管理，同一身份体验各链 |
| 基础设施 | 机构级安全的跨链平台 |
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->


今天整体了解了一下，然后试着部署了环境。刚好最近也在看另一个本地的大模型相关的内容，对照着学习下
<!-- DAILY_CHECKIN_2025-11-25_END -->
<!-- Content_END -->
