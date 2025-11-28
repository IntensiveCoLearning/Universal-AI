---
timezone: UTC+8
---

# 张为东

**GitHub ID:** siemens84cn

**Telegram:** @siemens84cn

## Self-introduction

程序员，web3爱好者

## Notes

<!-- Content_START -->
# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->
-   从开发者视角，说明ZRC-20 和普通 ERC-20 的直观区别；
    

| 特性 | ERC-20 | ZRC-20 |
| --- | --- | --- |
| 部署位置 | 单一区块链（如以太坊） | ZetaChain（但代表多链资产） |
| 跨链能力 | ❌ 需要外部桥 | ✅ 原生支持 |
| 代表资产 | 本链代币 | 外部链资产（如 ETH.ETH, BTC.BTC） |
| withdraw 函数 | ❌ 没有 | ✅ 提款到原生链 |
| Gas 费处理 | 只需本链 Gas | 自动处理目标链 Gas |

-   举一个「通用资产」可能的应用场景（比如跨链储蓄、通用 NFT 通行证等）；
    

我想到的是可以在GameFI领域使用「通用资产」，使得游戏角色的装备NFT可以在多个游戏和公链间任意的、自由的流转。

跨链游戏资产的应用场景：

1.  玩家在以太坊上mint游戏装备NFT。
    
2.  利用zatachain的技术，可以无缝转移到：
    

\- Polygon（低费用的游戏内交易）

\- BSC（在 BSC 的 NFT 市场出售）

\- Avalanche（参与其他游戏）

从而保持tokenId不变、元数据一致性不变、以及满足玩家对某链的依赖。
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->

-   自己想做的第一个 Universal App 想实现的“打印 / 记录 / 简单逻辑”是什么?
    

结合ZetaChain的特点，在以太坊上发起一个call（比如说，莎士比亚的一段话），通过gateway和zetaChain链的转发，能够在SUI链上接收到这个消息并记录到链上，具体实现逻辑明天设计一下。

-   为后续的 Hello World Demo 决定一种工作流：使用 CLI + Hardhat / Foundry？用本地链还是测试网？
    

我选择使用CLI+Foundry（个人技术栈熟悉），同时选择本地链（也会尝试在测试网上跑通这个Dapp）。
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->


-   Universal App 是什么？
    

它是部署在ZetaChain上的智能合约，作为一个Dapp（去中心化应用），它能够与包括以太坊、比特币、Solana 等在内的多个区块链无缝交互的应用程序。

-   Gateway 大概做什么？
    

它是一个服务接口，能够连接 任一运行在L1链上（以太坊、Solana等）的智能合约与 ZetaChain 上的通用应用程序之间交互的统一入口点，进而支持它们之间的合约调用和代币转移（交易）。

-   画一张简单的架构图：ZetaChain 中心 + Bitcoin / Ethereum / Solana 等外围链 + Gateway。
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->



-   ZetaChain CLI本地环境安装及使用：
    

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/siemens84cn/images/2025-11-25-1764077584427-image.png)

-   测试网 RPC、Faucet、Explorer 的入口参考下表：
    

| RPC | Faucet | Explorer |
| --- | --- | --- |
| https://www.zetachain.com/docs/reference/network/api | https://www.zetachain.com/docs/reference/faucet | https://www.zetachain.com/docs/reference/explorers |

-   Postman完成一次Qwen API的简单请求，截图参考：
    

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/siemens84cn/images/2025-11-25-1764077097175-image.png)
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->




## 开发环境准备步骤

### 1\. 区块链开发环境

-   安装 MetaMask 钱包
    
-   创建/导入钱包地址
    
-   添加 ZetaChain 网络到 MetaMask
    
-   获取测试网代币
    

### 2\. AI 开发环境

-   注册 Qwen 账号
    
-   申请 API Key
    
-   测试 API 调用
    

### 3\. 代码开发环境

-   安装 Node.js（推荐 v18+）
    
-   安装代码编辑器（VS Code & Cursor）
    
-   安装 Git
    
-   准备开发工具：
    
    -   Hardhat / Foundry（智能合约开发）
        
    -   ethers.js / web3.js（区块链交互）
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
