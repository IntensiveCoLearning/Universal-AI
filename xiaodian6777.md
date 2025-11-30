---
timezone: UTC+8
---

# Lansyue

**GitHub ID:** xiaodian6777

**Telegram:** @Lansyue

## Self-introduction

大二计算机学生，对web3非常热爱，有一年多交易经验，主要做的defi

## Notes

<!-- Content_START -->
# 2025-11-30
<!-- DAILY_CHECKIN_2025-11-30_START -->
第六天主要是动手把一个官方 Universal DeFi Demo 跑通，亲身体会“调用一次全链 DeFi 动作”的完整流程。reddit+1​

## 当天学习目标

能从 ZetaChain 官方示例里，实际跑起来一个基础跨链 Demo（推荐 Swap 或 Messaging）。[x](https://x.com/search?q=okav.com+%7C+AI-%E5%AE%8B%E7%A5%96%E5%84%BF-%E8%AF%BE%E9%97%B4%E6%93%8D.zwf&lang=el)​

亲自走一遍从发起调用到 ZetaChain 链上执行动作的闭环，建立对 “全链 DeFi 调用一次就联动多链” 的直观感受。[x](https://x.com/search?q=okav.com+%7C+AI-%E5%AE%8B%E7%A5%96%E5%84%BF-%E8%AF%BE%E9%97%B4%E6%93%8D.zwf&lang=el)​

## 查阅和使用的资料

Tutorials：重点看 Swap / Messaging / Calls to-from EVM 相关文档，理解跨链调用流程、关键参数和执行路径。[x](https://x.com/search?q=okav.com+%7C+AI-%E5%AE%8B%E7%A5%96%E5%84%BF-%E8%AF%BE%E9%97%B4%E6%93%8D.zwf&lang=el)​

Example code 仓库：[https://github.com/zeta-chain/example-contracts，拉取示例合约和脚本，作为本地或测试网实战的基础代码。x​](https://github.com/zeta-chain/example-contracts，拉取示例合约和脚本，作为本地或测试网实战的基础代码。x​)

## 动手过程与关键记录

选择了一个官方 Demo（如 Swap 或 Messaging），按照文档完成部署与调用，可在本地或测试网环境中操作。[x](https://x.com/search?q=okav.com+%7C+AI-%E5%AE%8B%E7%A5%96%E5%84%BF-%E8%AF%BE%E9%97%B4%E6%93%8D.zwf&lang=el)​

过程中重点记录：使用的链 / 网络配置、关键命令、重要环境变量和配置项（RPC、私钥/钱包、合约地址等），方便后续复用和排错。​

## 自己的理解与复盘

本次调用是从哪里发起的？例如：本地脚本、CLI、前端页面或合约函数，如何向 ZetaChain 发送请求。​

在 ZetaChain 上实际发生了什么？例如：跨链消息如何路由、ZRC-20 或通用资产如何在链上被更新，最终在 Explorer 上能看到的交易和事件。​

## 与前几天的衔接

通过第六天的 Demo，前面关于 Universal App、ZRC-20、Gateway 与多链架构的概念开始“落地成真实交易”，为后续通用 DeFi 赛道以及 AI × ZetaChain 的整合打下实践基础。​

这次实战也帮助明确后面可以复用哪些官方 Demo 模板，作为自己黑客松项目的构建起点。​
<!-- DAILY_CHECKIN_2025-11-30_END -->

# 2025-11-29
<!-- DAILY_CHECKIN_2025-11-29_START -->

理解第 5 天的核心，就是搞清楚「ZRC-20 / 通用资产」在 ZetaChain 里的角色，以及它和多链资产统一表示的关系。

## Day 5 核心概念理解

ZRC-20 可以理解为「ZetaChain 上对多链资产的统一包装标准」，开发体验类似 ERC-20，但背后资产可能来自不同公链。​

普通 ERC-20 只存在于单条链上（比如只在 Ethereum），而 ZRC-20 代表的是某个跨链资产在 ZetaChain 上的映射与记账方式。​

## ZRC-20 vs ERC-20（开发者视角）

地址与部署环境：ERC-20 部署在单条 EVM 链上，ZRC-20 部署在 ZetaChain 的 Universal EVM 上，通过跨链机制对应外部链上的真实资产。​

跨链能力：ERC-20 自身不懂跨链，需要额外桥或协议；ZRC-20 从设计上就是为跨链场景服务的，天然对接 ZetaChain 的跨链消息和资产管理。​

资产统一抽象：对 DApp 来说，ZRC-20 像是一个「多链资产统一接口」，减少直接处理各条链细节的复杂度。​

## 通用资产的应用场景想法

跨链储蓄 / 收益聚合：用户把 BTC、ETH、稳定币等充值到对应的 ZRC-20，前端只看到统一「资产余额」，策略层在多链上自动寻找最佳收益。​

通用 NFT 通行证：某个 NFT 在不同链有不同版本（或跨链流转），但在 ZetaChain 上有一个统一的「通用 NFT 身份」，用它做跨链会员、门票、积分等权限控制。​

## 自己的理解与后续打算（示例可改成你自己的）

对自己来说，第 5 天是从「跨链 = 桥 + 多份资产」转向「跨链 = 一个通用资产视图」的心智转折点，后面做 DeFi 逻辑时可以只面向 ZRC-20 接口。​

后续在设计 Universal DeFi idea 时，可以优先考虑：前端和业务逻辑只操作 ZRC-20 / 通用 NFT，把具体在哪条链上执行收益、Swap、质押都交给底层策略和 ZetaChain 完成。​
<!-- DAILY_CHECKIN_2025-11-29_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->


## 一、今天学了什么

继续翻了一遍 ZetaChain Developers 里 EVM 和 Tutorials 相关部分，大概知道 Universal App 就是部署在 ZetaChain 的合约，但可以同时跟多条外部链交互，用一次调用完成跨链逻辑。

现在对「合约 + 前端 + RPC」这三块的关系更清晰了：合约负责全链逻辑，前端只是把参数组织好发请求，RPC/节点负责把请求送到 ZetaChain，再由 ZetaChain 帮我触达外链。

## 二、我的第一个 Universal App 想做什么

想做一个「跨链状态留言板」的 Hello World：用户在任意支持的链上“留言”，ZetaChain 统一在链上记录一条结构化消息（比如链名、时间、内容），相当于一个全链可见的 on-chain log。

这个留言板一开始只做最简单的“记录一条文本 + 链信息”，后面再考虑加上跨链奖励、积分之类的玩法。

## 三、为 Hello World 选的工作流

工具链：准备用 ZetaChain CLI 搭配 Hardhat 来走完整流程，一方面能复用以太坊生态经验，另一方面方便后面接测试网和部署脚本。

网络选择：优先直接在 ZetaChain 测试网上跑，而不是本地链，这样一开始就按照真实跨链场景来思考，避免之后从本地到测试网再重构一次思路。

这两天不会强行开写代码，主要先把合约大致接口、事件设计、前端交互流程在脑子里走通，给 Day 5–6 的实战做铺垫。
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->



## 关键概念理解

通用区块链（Universal Blockchain）：指像 ZetaChain 这种天然为多链互通设计的底层网络，它不是只服务某一条链，而是把多条公链当成一个整体来协同管理和调用。通过这种设计，开发者可以在一条链上写逻辑，但同时操作多条链上的资产和状态。​

Universal EVM：可以理解为“带多链能力的 EVM 执行环境”，合约在 ZetaChain 的 EVM 上运行，却可以感知并操作外部链（Bitcoin、Ethereum 等）上的资产与消息，形成一种“跨多链的统一执行层”。​

Omnichain Smart Contract / Universal App：运行在 ZetaChain 上、但具备多链读写能力的智能合约或应用，用户只看到一个入口，背后却在协调多条链完成跨链转账、交易或消息通信，减少了用户自己跨链、桥接的复杂度。​

## Universal App的角色

Universal App 就是部署在 ZetaChain 上、通过链上原生跨链能力，把多条公链当成一个整体来用的全链应用，用户只需要和一个入口交互，就能完成多链上的一系列资产和合约操作

## Gateway 的角色

Gateway 的核心作用，是充当 ZetaChain 与外部公链之间的“桥梁”和“翻译层”，负责接收来自不同链的交易或消息，并把它们安全、标准化地传入 ZetaChain。反过来，当 ZetaChain 决定在某条外部链上执行动作时，也需要通过 Gateway 进行落地和验证。​

可以把 Gateway 想象成一个多链总闸口：一方面监听各条链上的事件和资金流入，另一方面根据 ZetaChain 合约的指令，在目标链发起对应操作，从而实现真正的跨链调用闭环。
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->




# 1.zetachain安装

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/xiaodian6777/images/2025-11-25-1764086089910-image.png)

# **2.**下面是ZetaChain测试网的三个主要入口：

**1\. RPC（节点接入，开发连接）入口：**

-   BlockPI公共RPC:  
    `https://zetachain-athens-evm.blockpi.network/v1/rpc/public`  
    这是EVM兼容的主RPC，用于钱包、Dapp连接和链上数据交互。如果需要其他RPC也可以参考Blast API：  
    `https://zetachain-testnet.public.blastapi.io`
    

**2\. Faucet（水龙头，领测试币）入口：**

-   官方Faucet页面可领ZETA测试币：  
    [**ZetaChain Docs Faucet**](https://www.zetachain.com/docs/reference/faucet/)  
    页面内有多个Faucet列表，任选一个即可（如Google Cloud Web3/FaucetMe等）。
    

**3\. Explorer（区块浏览器，查链上信息）入口：**

-   官方ZetaChain Explorer：  
    [**https://explorer.zetachain.com/**](https://explorer.zetachain.com/)
    
-   也可用第三方：  
    [**https://staking-explorer.com/explorer/zetachain**](https://staking-explorer.com/explorer/zetachain)
    

# 3.Qwen API核心终端代码

curl -X POST "[https://dashscope.aliyuncs.com/api/v1/services/aigc/text-generation/generation](https://dashscope.aliyuncs.com/api/v1/services/aigc/text-generation/generation)" \\

\-H "Authorization: Bearer <YOUR\_API\_KEY>" \\

\-H "Content-Type: application/json" \\

\-d '{

"model": "qwen-turbo",

"input": {

"prompt": "帮我简单介绍一下ZetaChain"

}

}'
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->





![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/xiaodian6777/images/2025-11-24-1763999677280-image.png)

sop笔记：[https://kcn4whyfn2kc.feishu.cn/docx/Mw1Gderu3okBRbxhfgacSNeenLb?from=from\_copylink](https://kcn4whyfn2kc.feishu.cn/docx/Mw1Gderu3okBRbxhfgacSNeenLb?from=from_copylink)

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/xiaodian6777/images/2025-11-24-1763999857623-image.png)
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
