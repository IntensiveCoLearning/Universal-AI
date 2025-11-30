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
# 2025-11-30
<!-- DAILY_CHECKIN_2025-11-30_START -->
1.  设计一个通用 DeFi 项目 idea，包括：目标用户、想解决的问题、粗略的跨链 / 通用资产使用方式。
    
2.  Idea名称：链上理财宝
    
3.  描述：全链收益优化协议 - 让您的资产自动在所有区块链上寻找最佳收益机会
    
4.  目标用户：
    
    1.  DeFi散户投资者（60%）
        
        -   持有$1,000 - $50,000资产
            
        -   希望获得稳定收益但不想复杂操作
            
        -   痛点：不懂跨链、Gas 费高、收益率监控困难
            
    2.  DeFi老手/"收益农民"（30%）
        
        -   持有$50,000+资产
            
        -   追求最大化收益
            
        -   痛点：手动跨链费时费力、流动性分散、机会成本高
            
    3.  DAO金库管理者（10%）
        
        -   管理数百万美元资产
            
        -   需要安全稳健的收益策略
            
        -   痛点：多链资产管理复杂、审计困难
            
5.  需要解决的问题：
    
    1.  流动性碎片化（流动性在不同链上变得分散，限制了交易、借贷和质押机会。由于流动性在每条链上被隔离，用户面临受限的市场访问，导致流动性水平降低。）
        
    2.  手动跨链的复杂性和成本（资产被困在不同的区块链上，无法轻松互动。这导致每个池子变得更小，价格随着每次交易波动更大，导致更高的滑点和更少的交易机会。）
        
    3.  收益率追踪困难（各链分别在各自平台上实时展示收益率变化，投资者面临在多个平台监控收益率的问题。）
        
6.  核心功能：
    
    -   一键全链收益优化
        

```
用户操作：
1. 连接钱包
2. 存入USDT/USDC/ETH
3. 选择风险偏好（保守/平衡/激进）
4. 点击"开始赚收益"

后台自动：
✅ 扫描 Ethereum、BSC、Polygon、Optimism、Base 等链的收益率
✅ 自动将资产部署到最高收益的协议
✅ 定期再平衡（每日/每周）
✅ 收益自动复投
```

-   智能路由引擎
    

javascript

```javascript
// 伪代码示例
function findBestYield(asset, amount, riskLevel) {
    // 实时扫描所有链的收益协议
    const opportunities = [
        { chain: "Ethereum", protocol: "Aave", apy: 4.5%, risk: "low" },
        { chain: "BSC", protocol: "Venus", apy: 7.8%, risk: "medium" },
        { chain: "Polygon", protocol: "Aave", apy: 6.2%, risk: "low" },
        { chain: "Base", protocol: "Moonwell", apy: 8.5%, risk: "medium" }
    ];
    
    // 考虑 Gas 费和收益率
    const netAPY = calculateNetReturn(opportunity, amount);
    
    // 根据风险偏好筛选
    return selectBestOption(opportunities, riskLevel);
}
```

-   Universal Token 自动再平衡
    

当某条链的收益率变化时，自动跨链转移资产：

```
场景：用户在 Ethereum Aave 存了 10,000 USDT (4.5% APY)
      
第 3 天：Base Moonwell 的 APY 涨到 9.2%

OmniYield 自动：
1. 从 Ethereum Aave 赎回 USDT
2. 通过 ZetaChain 转移到 Base
3. 存入 Base Moonwell
4. 用户收益立即提升到 9.2%
```
<!-- DAILY_CHECKIN_2025-11-30_END -->

# 2025-11-29
<!-- DAILY_CHECKIN_2025-11-29_START -->

-   在测试网跑通官方跨链Demo（Swap）
    

1.  [参考文档](https://www.zetachain.com/docs/developers/tutorials/swap)
    
2.  [源码](https://github.com/zeta-chain/example-contracts/tree/main/examples/swap)
    
3.  部署Swap合约，并发起跨链请求
    

```
npx hardhat run scripts/deploy.ts --network zeta_testnet
npx hardhat run scripts/swap.ts --network sepolia
```

-   用文字说明从哪里发起的调用？最终在ZetaChain上发生了什么？
    

1.  Demo的跨链请求，是从Ethereum Sepolia发起；
    
2.  在ZetaChain的几个关键步骤：
    

第一步：验证跨链请求是否合法。

第二步：调用在ZetaChain Testnet上部署的Swap合约。

第三步：执行Swap合约。

第四步：将目标token发送到我的ZetaChain Testnet地址上，并产生最终交易记录。
<!-- DAILY_CHECKIN_2025-11-29_END -->

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
