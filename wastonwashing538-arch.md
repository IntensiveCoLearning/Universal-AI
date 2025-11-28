---
timezone: UTC+8
---

# Zayne

**GitHub ID:** wastonwashing538-arch

**Telegram:** @Zayne538

## Self-introduction

我是深大链协成员，对Web3和区块链有一定了解，掌握基础的AIGC工具使用，并在使用claude AI 与claude code 进行个人项目搭建，有志于深入DeFi行业，探索去中心化的无限可能

## Notes

<!-- Content_START -->
# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->
### **. ZRC-20 和 ERC-20 的区别（开发者视角）**

ZRC-20 是 ZetaChain 用来表示多链资产的“统一标准”，与普通 ERC-20 相比有以下直观差异：

1.  **部署方式不同**：ERC-20 需要开发者在单条链手动部署；ZRC-20 由 ZetaChain 系统合约自动生成，用来映射外链资产。
    
2.  **跨链能力不同**：ERC-20 无原生跨链能力；ZRC-20 内置跨链交互接口，可在 ZetaChain 上统一处理来自 BTC、ETH、BSC、Cosmos 等不同链的资产。
    
3.  **资产来源不同**：ERC-20 通常是开发者自行发行的代币；ZRC-20 则是外链原生资产在 ZetaChain 上的通用表示（如 BTC → ZRC-20 BTC）。
    

因此，ZRC-20 本质是“多链资产的标准化包装”，使 DApp 可以用一套逻辑操作所有链资产。

### **2\. 通用资产的应用场景举例**

**通用 NFT 通行证 / 跨链身份系统。**

用户可在任意链（例如 ETH、Polygon、BSC、Cosmos）mint 身份 NFT，而 ZetaChain 会将其标准化为同一个 Universal NFT。  
开发者无需为多链重复部署 NFT 合约，所有用户都能在不同链中使用同一份“跨链身份 / 会员卡”。

该机制可用于跨链会员体系、游戏通证、多链活动凭证（POAP）等应用。
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->

我将做一个 **跨链留言板（Cross-Chain Message Board）** 作为我的第一个 Universal App。

用户可以在前端输入一句话，提交后通过 ZetaChain 测试网的 RPC 将该消息写入合约并由前端展示。未来我会扩展成：用户可以从其他链发送消息，通过 Zeta Messaging 做跨链同步。

**Hardhat + ZetaChain CLI + 测试网 RPC（Athens）**

原因是 Hardhat 更适合初学者，官方教程配套，Zeta CLI 能简化跨链流程，而 Universal App 必须在测试网才能正确运行。

Universal App 需要使用 Zeta Messaging 做跨链通信，本地链无法模拟这种跨链架构，因此必须使用 **ZetaChain Athens Testnet** 来验证跨链消息与完整流程。
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->


ZetaChain 上的 Universal App 是一种能够在单一合约中直接调用并处理多条区块链资产与消息的跨链原生应用。 它通过统一的资产包装、跨链消息协议以及完整的开发者工具链，让开发者只写一次代码，就能在所有支持的链上提供完整的 DeFi、NFT、治理等功能，从而大幅降低跨链开发的技术门槛和运营成本。

Gateway的作用：

Gateway是ZetaChain生态中的统一入口接口（Unified Entry Point），部署在ZetaChain主网和每个连接链（如Ethereum、BNB Chain、Polygon、Bitcoin、Solana、TON、Sui等）上。它充当连接链合约/用户与ZetaChain上Universal Apps（通用应用）之间的桥梁，实现无缝跨链交互

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/wastonwashing538-arch/images/2025-11-26-1764172582745-image.png)
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->



要在zetachain上开发有点门槛，从下载各种先决条件第一步就有些吃力，好在能用AI一步一步帮我详解步骤，后面搭建环境也顺利完成，顺便搞定了Qwen

，后面搭建环境

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/wastonwashing538-arch/images/2025-11-24-1763991818351-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/wastonwashing538-arch/images/2025-11-24-1763991706144-image.png)
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
