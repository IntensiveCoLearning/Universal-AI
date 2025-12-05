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
# 2025-12-05
<!-- DAILY_CHECKIN_2025-12-05_START -->
# Day 12 端到端 Demo 串联

### 一、核心理解

今天的目标是将前面几天学过的所有模块**串成一条完整通路**：自然语言 → Qwen-Agent → 后端脚本 →（模拟或真实）ZetaChain 调用

核心流程可以总结为：

```
User Input → Agent（解析意图） → 生成链操作参数
                 ↓
            Backend Script（Hardhat / CLI）
                 ↓
       执行链交互（本地或测试网）
                 ↓
              返回执行结果
```

* * *

### 二、今日重点模块回顾

1\. Qwen-Agent 部分

负责理解自然语言，并输出结构化参数，例如：

-   操作类型（读取余额 / 发起交易 / 写入留言）
    
-   使用的链 / 网络
    
-   必要参数（金额、目标地址、内容等）
    

Agent 的作用是：  
**把“自然语言需求”翻译为“可执行的链指令”。**

* * *

2\. 后端链操作部分（ZetaChain Demo）

我选择用 Hardhat 或 CLI（任一之前跑通过的 Demo）执行实际链指令。

例如：

-   读取合约状态
    
-   模拟发送交易
    
-   或者打印“即将执行”的操作（若暂不调用真实链）
    

后端的目标是：  
**根据 Agent 的输出执行链层逻辑。**

* * *

### 三、我完成的端到端最小 Demo（MVP）

我实现的最小 Demo 包含三步：

1.  **输入一句自然语言**  
    例如：  
    “帮我查询一下 Universal Affirmation 合约里的留言数量。”
    
2.  **Agent 输出结构化参数**  
    如：
    
    ```
    {
      "action": "readContract",
      "contract": "affirmation",
      "method": "getCount"
    }
    ```
    
3.  **后端脚本执行链上操作**  
    在终端打印执行结果（例子）：
    
    ```
    当前留言总数为：18
    ```
<!-- DAILY_CHECKIN_2025-12-05_END -->

# 2025-12-03
<!-- DAILY_CHECKIN_2025-12-03_START -->

# Day 9 学习笔记

## Qwen-Agent 入门 & 简单 Tool

### 一、核心理解

Qwen-Agent 的框架由四部分组成：

1.  **LLM（大模型）**  
    负责语言理解与生成，是 Agent 的“大脑”。
    
2.  **Agent（主体逻辑）**  
    管理对话、调用工具、决定下一步行动。
    
3.  **Tools（工具）**  
    可以由开发者定义，用于完成模型无法直接执行的操作，如：
    
    -   访问 API
        
    -   计算
        
    -   查询数据库
        
    -   文件操作
        
4.  **Memory（记忆）**  
    用于让 Agent 记住历史对话/上下文，使多轮交互更自然。
    

一句话总结：

> Qwen-Agent = LLM + Tool + Memory 的组合式智能体框架。

* * *

### 二、我理解的 Qwen-Agent 工作流程

一个典型流程如下：

```
用户 → Agent → (判断是否需要 Tool)
                 ↳ 调用 Tool（如果需要）
                 ↳ 返回结果给 Agent
Agent → 根据结果生成最终回复 → 返回给用户
```

核心思想：

-   LLM 决定行动
    
-   Tool 执行具体任务
    
-   两者结合完成“推理 + 操作”
    

* * *

### 三、我实际运行的官方示例

我成功跑通了一个 Qwen-Agent 官方示例，包括：

-   初始化模型（qwen-turbo / qwen-plus 均可）
    
-   创建 Agent 实例
    
-   运行一段简单对话
    
-   观察 Agent 是否会自动调用内置工具
    

这一步确保了环境准备与 API 调用链路正常。

* * *

### 四、我自定义的简单 Tool

我编写了一个最简单的示例 Tool——**字符串大写转换**：

```
def to_uppercase(text: str) -> str:
    return text.upper()
```

并将其挂载到 Agent：

```
tools = [
    {
        "name": "to_uppercase",
        "description": "Convert text to uppercase.",
        "function": to_uppercase
    }
]
```

然后测试：

**输入：**  
“请把 hello world 转成大写。”

**Agent 过程：**

-   LLM 判断需要调用 Tool
    
-   自动调用 `to_uppercase("hello world")`
    
-   获得结果 `"HELLO WORLD"`
    

**输出：**  
“HELLO WORLD”

这验证了：

-   Tool 注册成功
    
-   Agent 能理解工具用途
<!-- DAILY_CHECKIN_2025-12-03_END -->

# 2025-12-01
<!-- DAILY_CHECKIN_2025-12-01_START -->



# Day 8 Qwen AI 基础 & API 调用

### 一、核心理解

Qwen API 的本质是：  
**通过一个 HTTP 请求，把提示词发给模型 → 得到生成的文本结果。**

调用流程非常简单，包含三要素：

1.  **模型名**（如 qwen-turbo / qwen-plus / qwen-max）
    
2.  **请求体（messages）**
    
3.  **Authorization: Bearer <API\_KEY>**
    

* * *

### 二、我使用的模型 & 参数

**模型选择：**`qwen-turbo`

选择原因：

-   速度快
    
-   成本最低
    
-   适合写脚本、调试、提供短文本生成
    
-   完全足够用于 Web3 Hackathon 的辅助生成（介绍文案、提示词、合约注释等）
    

**关键参数：**

```
{
  "model": "qwen-turbo",
  "input": {
    "messages": [
      {
        "role": "user",
        "content": "Introduce ZetaChain."
      }
    ]
  }
}
```

必要参数只有两个：

-   `model`：指定模型
    
-   `input.messages`：输入提示词（同 ChatGPT API 格式）
    

其他如 token 限制、采样等都可以先不管。

* * *

### 三、我写的最小可运行脚本（Node.js）

```
import fetch from "node-fetch";

const API_KEY = "YOUR_API_KEY";

async function callQwen() {
  const res = await fetch(
    "https://dashscope.aliyuncs.com/api/v1/services/aigc/text-generation/generation",
    {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
        Authorization: `Bearer ${API_KEY}`,
      },
      body: JSON.stringify({
        model: "qwen-turbo",
        input: {
          messages: [
            { role: "user", content: "请用简单语言介绍 ZetaChain。" }
          ]
        }
      })
    }
  );

  const data = await res.json();
  console.log(data.output.text);
}

callQwen();
```
<!-- DAILY_CHECKIN_2025-12-01_END -->

# 2025-11-30
<!-- DAILY_CHECKIN_2025-11-30_START -->




# Day 7 学习笔记

## 通用 DeFi 赛道预热 & Idea 收敛

### 一、核心理解

ZetaChain 的价值在于：  
它不是在做“多链 DeFi”，而是在做 **跨链原生 DeFi（Universal DeFi）**。

差异非常关键：

-   **多链 DeFi**：每条链部署一份合约，再用跨链桥拼起来
    
-   **Universal DeFi**：所有跨链资产被 ZetaChain 统一成 ZRC-20，通过一套逻辑处理所有链
    

因此 Universal DeFi 的特点是：

-   单一合约即可操作来自 BTC / ETH / BNB / SOL 的资产
    
-   不需要桥、包装资产或重复部署
    
-   逻辑统一、风险统一、体验统一
    

从第 3–5 天累计的内容来看，核心基石包括：

-   ZRC-20（统一资产表示层）
    
-   Universal Token / NFT（跨链身份/资产）
    
-   Swap 模块（跨链兑换）
    
-   Messaging（跨链信息流）
    

这些共同构成 Universal DeFi 的基础。

* * *

### 二、我理解的几类可行的 Universal DeFi 模式

1.  **跨链聚合（Cross-chain Aggregation）**  
    聚合多条链上的资产和收益渠道，通过 ZetaChain 在统一合约中操作。
    
2.  **跨链储蓄/收益管理（Cross-chain Yield）**  
    用户把钱存在 ZetaChain，但收益来源于多个链的策略。
    
3.  **跨链身份/抵押（Universal Collateral）**  
    某条链的资产可作为其他链借贷的抵押品。
    
4.  **跨链 NFT 权益（Universal NFT）**  
    一个 NFT = 多链通行证。
    

* * *

### 三、以下是我基于 Universal DeFi 的两个方向。

* * *

## Idea 1：Universal Yield Box（全链收益聚合盒子）

**目标用户：**  
想获取收益但不想手动跨链/管理资产的普通用户。

**要解决的问题：**  
传统收益产品散落在不同链上，用户需要跨链、换币、切钱包，非常复杂。

**核心思路：**

-   用户在任何链存入资产（BTC/ETH/SOL 等）
    
-   ZetaChain 将其资产表示成 ZRC-20
    
-   合约接入多链收益渠道（ETH LST、SOL LST、BNB LST 等）
    
-   最终收益统一计入 ZetaChain
    
-   用户随时一键提取到任意链
    

**跨链/资产使用方式：**

-   存入：任意链 → Gateway → ZRC-20
    
-   策略：在 ZetaChain 内部统一调度
    
-   提取：ZRC-20 → 用户想去的链
    

* * *

## Idea 2：Universal Collateral Vault（全链抵押金库）

**目标用户：**  
需要借贷但资产分散在多条链上的用户。

**要解决的问题：**  
BTC/ETH/SOL/BNB 在不同链上，无法作为统一抵押品。

**核心思路：**

-   用户把不同链的资产存入 ZetaChain
    
-   合约将所有跨链资产统一计价（通过 ZRC-20 + oracle）
    
-   用户在一个地方借贷
    
-   不需要跨链桥，不需要包装资产
    

**跨链使用方式：**

-   用户资产 → Gateway → ZRC-20
    
-   合约统一计价 & 抵押管理
    
-   借款以 ZRC-20 或某条链的 token 提供
<!-- DAILY_CHECKIN_2025-11-30_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->





# Day 5 Universal DeFi & 全链资产基础（概念向）

### 一、核心概念理解

1\. 什么是 ZRC-20

ZRC-20 是 ZetaChain 上的“通用资产标准”，它的作用不是取代 ERC-20，而是**统一表示来自不同链的资产**。

直观理解：

-   ERC-20：只属于以太坊生态
    
-   ZRC-20：属于 **多链世界的统一资产形态**
    

通过 ZRC-20，不同链上的资产（BTC、ETH、BNB 等）可以在 ZetaChain 上用**一致的接口和规则**被操作。

* * *

2\. Universal Token / Universal NFT

**Universal Token：**

-   可以代表来自任意链的资产
    
-   在 ZetaChain 上被统一建模
    
-   可以被一个合约同时读取、转移、组合
    

**Universal NFT：**

-   不局限于某条链
    
-   拥有“跨链身份”
    
-   可作为多个链共同承认的凭证 / 资产
    

本质是：  
**跨链资产被抽象成“统一对象”进行处理。**

* * *

### 二、多链资产在 ZetaChain 上如何被统一表示

传统世界：

```
BTC -> Ethereum 不兼容
ETH -> Solana 不互认
```

ZetaChain 通过 ZRC-20：

```
Bitcoin 资产 ┐
Ethereum 资产 ├ → ZRC-20 统一表示 → Universal App 处理
Solana 资产  ┘
```

也就是说：

-   不同来源资产在 ZetaChain 上被“翻译”为一种统一标准
    
-   开发者只需操作 ZRC-20，不再管资产来自哪条链
    

这使得：真正的跨链 DeFi / NFT / 应用成为可能。

* * *

### 三、ZRC-20 与普通 ERC-20 的直观区别（开发者视角）

| 对比点 | ERC-20 | ZRC-20 |
| --- | --- | --- |
| 所属链 | 只在以太坊 | 属于 ZetaChain（跨链） |
| 资产范围 | 单链资产 | 可对应多链资产 |
| 目标 | 同链转账 & DeFi | 跨链统一资产 |
| 适用场景 | DApp / DeFi | Universal App |
| 视角 | Blockchain-native | Omnichain-native |

一句话总结：

> ERC-20 是“某一国家的货币”，ZRC-20 是“全球通行货币”。

* * *

### 四、一个「通用资产」的应用场景举例

场景：跨链储蓄 / 通用 NFT 凭证

例子 1：跨链储蓄池

-   用户可以存入 BTC / ETH / SOL
    
-   这些资产在 ZetaChain 上被转为 ZRC-20
    
-   在同一个合约中进行质押、计息、借贷
    
-   本质：多链资产 → 同一资金池
    

例子 2：Universal NFT 通行证

-   用户在任何一条链 mint NFT
    
-   该 NFT 在 ZetaChain 上被识别为“通用身份”
    
-   可用于多链会员、权限、身份认证
    
-   本质：一个 NFT，全球通用
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->






# Day 4 学习笔记

## Universal App + Hello World 心智模型

### 一、核心理解

**Universal App 的最小结构包含三层：**

1.  **合约层（ZetaChain 上）**
    
    -   存储数据、执行逻辑
        
    -   提供写入/读取函数（例如 `writeMessage()` / `getMessages()`）
        
2.  **RPC/脚本层（Hardhat / ethers.js）**
    
    -   负责向 ZetaChain 发送交易、读取链上数据
        
3.  **前端层（简单 UI）**
    
    -   输入 → 发送到链上
        
    -   再从链上读取 → 展示结果
        

**整体链路：**  
前端 → RPC → Universal 合约（ZetaChain）→ RPC → 前端展示

* * *

### 二、我设计的第一个 Universal App

主题：**Universal Affirmation Log（全链显化留言板）**

**功能：**

-   用户从任意链写入一条“自我肯定语句”
    
-   合约记录：文本 + 地址 + 来源链 + 时间
    
-   前端展示所有链上的显化语句
    

**合约最小逻辑：**

-   `writeAffirmation(string text)`
    
-   `getAffirmations()`
    

这是一个最简单、最适合作为 Hello World 的“打印 + 记录”应用。

* * *

### 三、我的开发工作流选择

**工具链：ZetaChain CLI（内置 Hardhat）**

理由：已成功配置、本身面向 Universal App、入门成本最低。

**网络选择：**

-   开发阶段：本地 Hardhat 网络（轻量、易调试）
    
-   最终验证：ZetaChain 测试网（已有 RPC 和测试币）
    

这套流程兼顾：轻量、本地资源占用低、可直接部署到真实链。
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->







# Day 3 Universal App&EVM

### 一、今日学习目标

-   什么是 Universal Blockchain（通用区块链）
    
-   什么是 Universal EVM / Universal App
    
-   什么是 Omnichain Smart Contract
    
-   ZetaChain 在整个多链世界中的位置
    
-   Gateway 在其中的作用
    

## **二、核心概念理解**

### 1\. 什么是 Universal Blockchain（通用区块链）

传统的区块链中每一条链的生态是封闭的

比如：ETH 上的资产不能直接被 Solana 使用；BTC 资产无法直接参与以太坊的 DeFi；每条链都需要单独写合约、部署、维护

而 Universal Blockchain 的目标就是打破链与链之间的隔离，让资产、数据、操作可以跨链流动。

ZetaChain 就扮演这个“连接者”的角色，使得开发者可以：

-   不用关心用户在什么链上
    
-   不用为每条链写一套代码
    
-   用一套逻辑，操控多条链
    

### 2\. 什么是 Universal App（通用应用）

传统 DApp 的问题是它们只跑在某一条链上，比如：

-   Uniswap → 只能在以太坊 / L2
    
-   Magic Eden → 只服务于 Solana 生态
    

而 Universal App 是：一种可以同时与多条链交互的应用，不被单一链限制。

在 ZetaChain 上，一个 Universal App 可以：

-   接收 BTC
    
-   操作 ETH 合约
    
-   与 Solana 交互
    
-   在同一个程序逻辑里完成跨链行为
    

更重要的一点是**开发者只需要编写一次，而不是为每条链重复开发。**

因此我理解：Universal App = 一种跨链原生应用

### 3\. 什么是 Universal EVM

EVM（Ethereum Virtual Machine）本来只为以太坊服务。

而 Universal EVM 是：由 ZetaChain 提供的、能“理解并控制多条链”的虚拟执行环境。

开发者写的合约，其实是在 Universal EVM 中运行，而这个 EVM 会负责与：

-   Ethereum
    
-   Bitcoin
    
-   Solana
    
-   Sui
    
-   Ton 等
    

进行交互。

### 4\. 什么是 Omnichain Smart Contract

Omnichain Smart Contract 指的是一个可跨多条链生效的智能合约。一般的智能合约只在一条链上有效，而 Omnichain 合约可以在 ZetaChain 上部署一次，然后它可以通过 Gateway 与多条链发生交互。

### 5\. Gateway 是干嘛的？

Gateway 是一个非常关键的组件，它负责：

-   连接 ZetaChain 与外部链
    
-   接收和发送跨链信息
    
-   保证跨链操作的安全与一致性
    

## **三、为什么在 ZetaChain 上开发是有价值的？**

我总结出三个主要原因：

1.  **技术维度：** 它解决的是一个真实、长期存在的问题：链与链之间不互通
    
2.  **成本维度：** 开发者只需要维护一套代码，避免多链开发的地狱
    
3.  **未来趋势：** 世界不会只剩一条链，多链共存是必然趋势 ZetaChain 天生符合未来结构
    

### 四、 架构图

![screenshot-1764126499493.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/skelitalynn/images/2025-11-26-1764126516887-screenshot-1764126499493.png)
<!-- DAILY_CHECKIN_2025-11-26_END -->

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
