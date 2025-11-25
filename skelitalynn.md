---
timezone: UTC+8
---

# Skelita

**GitHub ID:** skelitalynn

**Telegram:** @+86 183 2062 9925

## Self-introduction

我是scut软件工程大三的学生，目前的技术栈是java、python、solidity，主要是后端开发，也正在学习web3的相关知识，目标是成长为全栈的web3工程师，谢谢！

## Notes

<!-- Content_START -->
# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->
### Day 2：环境与工具实操（ZetaChain + Qwen）

**1\. 今日目标**

-   在本地配置 ZetaChain CLI 并成功运行
    
-   掌握基本区块链开发工具使用方式
    
-   完成一次 AI 接口（Qwen）的真实调用
    
-   搭建 Web3 + AI 的基本技术通路
    

* * *

**2\. ZetaChain CLI 本地部署**

在本地目录执行：

-   `git clone https://github.com/zeta-chain/cli.git`
    
-   `cd cli`
    
-   `npm install --legacy-peer-deps`
    

虽然过程中出现 deprecated 和 bigint 警告信息，但均不影响使用。

通过执行：

```
npx tsx src/index.ts --help
```

成功看到 CLI 的完整命令列表，包括：

-   new
    
-   faucet
    
-   query
    
-   chains
    
-   tokens
    
-   balances
    
-   contracts
    
-   ask...
    

说明：**ZetaChain 官方 CLI 工具已经在本地成功运行。**

* * *

**3\. 成功使用 ZetaChain 查询网络信息**

通过命令：

```
npx tsx src/index.ts query
```

成功进入 query 模块，并能够访问：

-   chains
    
-   fees
    
-   tokens
    
-   contracts 等链上数据入口
    

这意味着： 当前设备已具备与 ZetaChain 测试网交互的能力。

* * *

**4\. Qwen API 连通性实测成功**

通过 Postman 请求：

-   Method: POST
    
-   URL: `https://dashscope.aliyuncs.com/api/v1/services/aigc/text-generation/generation`
    
-   Headers：
    
    -   Content-Type: application/json
        
    -   Authorization: Bearer <我的API Key>
        
-   Body：
    

```
{
  "model": "qwen-turbo",
  "input": {
    "messages": [
      {
        "role": "user",
        "content": "Hello, this is my first Qwen API request."
      }
    ]
  }
}
```

返回结果：

-   HTTP 200 OK
    
-   成功接收到 AI 生成文本响应
    

![image-20251125113439425](file:///C:/Users/86183/AppData/Roaming/Typora/typora-user-images/image-20251125113439425.png?lastModify=1764051923)

| Faucet | Chain / Asset |
| --- | --- |
| Google Cloud Web3 | ZetaChain ZETA |
| FaucetMe | ZetaChain ZETA |
| Optimism | Ethereum, Base |
| Chainlink | Ethereum, Base, Avalanche, Arbitrum, Polygon |
| Circle | USDC |
| Solana | Solana |
| Polygon | Polygon |
| Binance Smart Chain | BSC |
| mempool.space | Bitcoin Testnet 4 |
| testnet4.dev | Bitcoin Testnet 4 |
| triangleplatform.com | Bitcoin Testnet 4 |
| Signet | Bitcoin Signet |

# Explorers

| Name | Mainnet | Testnet | Description |
| --- | --- | --- | --- |
| Blockscout | Mainnet | Testnet | EVM block explorer that lets you search transactions, addresses, and tokens, verify and interact with smart contracts, track cross-chain activity, and access data through HTTP and GraphQL APIs. |
| ExploreMe | Mainnet | Testnet | EVM and Cosmos explorer with blocks, transactions, top accounts, contracts, tokens/NFTs, validators, proposals, params/uptime/API. |
| Mintscan | Mainnet | — | Cosmos explorer with blocks, transactions, accounts, validators & staking, governance proposals, and params. |
| Nodejumper | Mainnet | — | Node operator dashboard with chain indicators and APIs, and utilities. |
| Ping.pub | Mainnet | Testnet | Cosmos explorer with blocks, transactions, accounts, validators, and governance proposals. |
| Polkachu | Mainnet | Testnet | Node operator dashboard with RPC/API endpoints, snapshots, state-sync, upgrade watcher, tools. |
| Explorers.guru | Mainnet | Testnet | Cosmos explorer with blocks, transactions, accounts, validators, and governance proposals. |
| Staking Explorer | — | Testnet | Cosmos explorer with validators, delegations, rewards/APR, uptime, and validator analytics. |
| Liveraven Explorer | — | Testnet | Cosmos explorer with blocks, transactions, accounts, validators, and governance proposals. |
| NodeStake | Mainnet | — | Cosmos explorer with blocks, transactions, accounts, validators, and governance proposals. |
| ITRocket | — | Testnet | Cosmos explorer with blocks, transactions, accounts, validators, and governance proposals. |

连接测试：状态码200
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->

# Day 1 ZetaChain & Qwen 启动与环境准备

## **一、今日目标**

今天的核心任务并不是进行具体开发，而是完成一次冷启动式的认知建立与环境搭建，为后续的 Web3 + AI 项目开发打下基础。重点包括：

NaN.  建立对 ZetaChain 和 Qwen 的基本认知框架
      
NaN.  确认可以访问 ZetaChain 官方文档和 Developers 入口
      
NaN.  注册并成功进入 Qwen 控制台
      
NaN.  配置好 Web3 和开发所需的基础工具环境
      

## **二、我对 ZetaChain 的初步理解**

通过浏览 ZetaChain 官方文档，我对其核心定位有了清晰认识：

**ZetaChain 是一个支持跨链（Omnichain / Cross-chain）的区块链平台，目的是让开发者可以构建跨多个区块链运行的通用应用（Universal App）。**

它解决的是当前区块链世界中「不同链彼此割裂」的问题，提供一种方式，使得一个应用可以与多个链进行交互，而不局限在单一生态中。

我成功完成了以下动作：

-   成功访问 ZetaChain Docs 首页
    
-   成功访问 Developers 页面
    
-   浏览文档结构，对其信息架构有整体感知
    
-   明确了未来开发方向会围绕 Universal App / Omnichain / Smart Contract 等关键词展开
    

## **三、我对 Qwen 的初步理解**

我将 Qwen 视为整个项目中的「AI 大脑」。

通过浏览其文档，我明确了几个关键点：

-   Qwen 是一个提供大模型能力的平台
    
-   它可以通过 API 形式接入应用中
    
-   未来可以承担：文本生成、智能回复、内容构建等核心AI功能
    

## **四、环境与工具准备**

为了保证后续可以顺利进行开发，我在本地也完成了基础工具确认：

-   安装并配置好 Web3 钱包 MetaMask
    
-   确认Node.js 可用（node -v 正常）
    
-   确认Git 可用（git --version 正常）
    
-   浏览器及基础开发环境已就绪
    

目前本机已经满足：

NaN.  Web3 环境
      
NaN.  AI 接入口
      
NaN.  基础开发工具
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
