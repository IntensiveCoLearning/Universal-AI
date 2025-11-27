---
timezone: UTC+8
---

# mango

**GitHub ID:** mango-ice-cat

**Telegram:** @mangoice

## Self-introduction

code everything

## Notes

<!-- Content_START -->
# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->
# 1，打卡签到
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->

# 1，打卡签到

# 2，Day 3 笔记 — ZetaChain & Universal Blockchain 核心概念

**日期**：2025-11-26  
**目标**：理解通用区块链概念、Universal App、Gateway，并用图示描述架构。

* * *

## 1️⃣ 学习目标回顾

-   理解 **通用区块链 / Universal EVM / Universal App / Omnichain Smart Contract** 的概念
    
-   能够画出 ZetaChain + 多条公链 + Gateway 的结构图
    
-   在笔记中总结核心概念，用自己的话描述
    

* * *

## 2️⃣ 笔记

### 3.1 阅读文档与理解

-   先浏览了 **Universal App 概览**，发现 **Universal App** 就是可以在 **多条公链**上执行的智能合约应用
    
-   文档里提到的 **Omnichain Smart Contract** 核心：
    
    -   一次编写 → 多链执行
        
    -   不需要用户手动跨链桥接资产
        
-   Gateway 的作用是 **连接不同链**，负责消息传递、资产转移的可靠性和安全性
    

> 我的理解：  
> Gateway 就像高速公路的收费站，负责把不同链的数据/资产安全传输到目标链，ZetaChain 就是核心路网。

* * *

### 3.2 用自己的话总结

**Universal App**：

> 可以跨链运行的应用，单份代码可在多条链上调用，无需为每条链单独开发。

**Gateway**：

> 是通用应用和不同区块链之间的桥梁，处理消息、交易、资产的跨链逻辑。

**Universal EVM**：

> 提供统一的 EVM 环境，让 Solidity/Hardhat/Foundry 的合约无需修改即可在多链上执行。

* * *

### 3.3 画图

我在笔记里画了一个简易架构图（ASCII 版）：

```
          +----------------+
          |  ZetaChain     |
          |  Core Layer    |
          +----------------+
             /     |      \
            /      |       \
      Ethereum    Solana   Bitcoin
        |           |        |
      Smart       Smart     Smart
      Contract    Contract  Contract
       (EVM)      (Rust)    (BTC script)
             ^ Gateway ^
```

> -   中心是 ZetaChain，负责跨链消息传递
>     
> -   每条链保留自身的智能合约特性
>     
> -   Gateway 处理跨链调用的安全性和一致性
>     

* * *

### 3.4 思考与笔记

-   ZetaChain 核心价值：**降低跨链开发门槛**，让开发者只关注应用逻辑，不必处理每条链的细节。
    
-   对比传统跨链：以前需要 wrap token / 跨链桥，现在 ZetaChain 直接原生支持跨链资产。
    
-   结合 Day 2 学到的 CLI，我计划 **后续自己动手搭建一个 Hello World Universal App**，先在本地链跑一遍 Swap / Messaging 流程。
    

* * *

### 3.6 今日收获

-   理解了 **Universal App、Gateway、Universal EVM** 的概念
    
-   能够用自己的话描述它们的作用
    
-   画出了简易架构图，帮助理解跨链逻辑
    
-   对后续动手操作有清晰规划
    

* * *

**总结**：  
Day 3 的重点是 **概念理解 + 架构梳理**，我觉得最重要的是把 Gateway 的作用理解透彻，这样后续写跨链合约 / 使用 CLI 才不会迷路。
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->



# 1，今日打卡，加油

# 2，#️⃣ Day 2 学习目标（总结）

-   **完成 ZetaChain 本地/云端开发环境**
    
    -   安装 ZetaChain CLI
        
    -   掌握测试网 RPC / Faucet / Explorer 的入口
        
-   **完成第一次 Qwen API 调用**
    
    -   使用 Postman / curl / Python / Node 调一次 Qwen
        
    -   能看到模型返回（成功 or 报错均算完成）
        
-   **明确自己 的学习目标**
    

# Part 1｜ZetaChain CLI 安装与环境准备

## 1\. 为什么需要 CLI？

ZetaChain CLI 用途：

-   创建项目模板（Universal EVM）
    
-   部署合约
    
-   与 ZetaChain 网络交互（testnet）
    
-   调用 messaging/swap demo
    

是 ZetaChain 开发最核心的工具链之一。

# 2\. 安装 ZetaChain CLI（Windows → WSL2 推荐）

因为我的系统是Windows，所以使用 WSL2 + Ubuntu。

## ✔️ **步骤 1：启用 WSL2**

PowerShell 管理员模式执行：

```
wsl --install
```

重启后自动安装 Ubuntu。

## **步骤 2：安装依赖**

进入 Ubuntu 终端：

```
sudo apt update
sudo apt install -y unzip curl git
```

* * *

## ✔️ **步骤 3：安装 ZetaChain CLI**

安装 ZetaChain CLI：

```
npm install -g zetachain
```

尝试运行：

* * *

## ✔️ **步骤 4：初始化 ZetaChain 项目**

```
zetachain query chains list
```

```
该命令会打印当前连接到 ZetaChain 的所有链
```

# 🟧 Part 2｜ZetaChain 测试网入口记录

* * *

## ✔️ **① RPC 入口（ZetaChain EVM Testnet）**

```
https://www.zetachain.com/docs/reference/network/api
```

* * *

## ✔️ **② Faucet（领测试币）**

```
https://www.zetachain.com/docs/reference/faucet
```

* * *

## ✔️ **③ Explorer**

```
https://www.zetachain.com/docs/reference/explorers
```

* * *

## ✔️ **④ ChainID / Network 信息**

| 项目 | 值 |
| --- | --- |
| 网络名 | ZetaChain Athens Testnet |
| Chain ID | 7001 |
| 货币符号 | ZETA |

* * *

## ✔️ **⑤ 将 RPC 加入 Metamask**

```
网络名称：ZetaChain Athens
RPC：http(s)：https://zetachain-athens-evm.blockpi.network/v1/rpc/public
chainId：7001
symbol：ZETA
```

# 🟦 Part 3｜Qwen API 调通（核心任务）

* * *

* * *

# ✔️ Python 示例（最简单）

```
from openai import OpenAI

client = OpenAI(
    api_key="YOUR_QWEN_API_KEY",
    base_url="https://dashscope.aliyuncs.com/compatible-mode/v1"
)

resp = client.chat.completions.create(
    model="qwen-max",
    messages=[{"role": "user", "content": "用一句话介绍 ZetaChain 是什么？"}]
)

print(resp.choices[0].message)
```

* * *

# 🟩 运行后应看到类似输出：

```
ZetaChain 是一个支持原生跨链消息与资产转移的通用区块链。
```
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->





# 1，签到打卡

# **2，ZetaChain 项目介绍**

## **概述**

ZetaChain 是一个开创性的 Layer 1 区块链平台，被誉为"**第一个通用区块链**"（Universal Blockchain），旨在解决区块链碎片化问题，实现所有链之间的无缝互操作性。

## **核心价值主张**

-   🔗 **解决区块链碎片化**：实现所有链之间的无缝互操作性
    
-   💫 **支持跨链价值转移**：无需桥接或包装代币
    
-   📨 **跨链消息传递**：实现真正的链间通信
    
-   🛠️ **原生全链智能合约**：支持 omnichain smart contracts
    

## **项目发展历程**

| 时间 | 里程碑 |
| --- | --- |
| 2021年7月 | 项目启动 |
| 2024年初 | 主网上线 |
| 当前状态 | 连接10+条链，TVL超3亿美元，用户超500万 |

## **核心技术特点**

### **架构基础**

-   **基于**：Cosmos SDK 和 Tendermint 共识机制
    
-   **安全机制**：Proof of Stake (PoS)
    

### **关键创新**

**🔥 全链智能合约**

-   合约可直接读写多链状态
    
-   支持非智能合约链（如比特币、Dogecoin）的外部管理
    
-   实现原生资产转移
    

**🌐 去中心化连接**

-   每个验证节点配备"观察者"（observers）
    
-   实时监控外部链事件
    
-   确保无单点故障的互操作性
    

**👨‍💻 开发者友好**

-   EVM 兼容，支持 Solidity
    
-   提供 SDK 和工具，简化跨链开发
    

**🛡️ 安全性与效率**

-   使用 Threshold BLS 提升可扩展性
    
-   低费用、高速跨链交换（例如 BTC/EVM/Solana 间）
    

**🎯 生态集成**

-   ZetaHub 作为用户界面，探索 dApps 和网络活动
    
-   支持 DeFi、NFT、治理和 AI 集成
    

## **支持的区块链**

**已支持**：

-   ✅ Ethereum
    
-   ✅ Polygon
    
-   ✅ BSC
    
-   ✅ Solana
    
-   ✅ Avalanche
    
-   ✅ Bitcoin
    
-   ✅ Dogecoin
    

**未来规划**：将持续扩展支持更多区块链

## **ZETA 代币与 Tokenomics**

### **代币基本信息**

-   **代币名称**：ZETA
    
-   **总供应量**：21 亿枚
    
-   **主要用途**：网络治理和激励
    

### **代币用途**

-   💰 支付 gas 费
    
-   🔒 安全 PoS（staking/bonding/slashing）
    
-   🔄 跨链转移/交换/消息传递
    
-   💧 流动性提供
    
-   🔥 代币燃烧
    

### **代币分配**

| 类别 | 比例 | 描述 |
| --- | --- | --- |
| 社区与生态 | 20.8% | 奖励早期贡献者、钱包/社交活动参与者 |
| 验证者/staking | 15% | 网络安全激励 |
| 团队/顾问 | 20% | 锁定 vesting |
| 投资者/基金会 | 25% | 种子/私募轮 |
| 流动性/营销 | 19.2% | 交易所上市、社区活动 |

### **释放计划**

-   主网上线后逐步解锁
    
-   **2025年4月**：有 1500 万美元代币解锁事件
    
-   **奖励程序**：包括主网用户空投，鼓励参与开发和使用
    

## **团队与融资**

### **核心团队**

-   **创始人**：Ankur Nandwani
    
    -   前 Coinbase 员工
        
    -   Basic Attention Token (BAT) 共同创建者
        
-   **团队成员**：包括前 Coinbase/Binance 员工，如 Dan Romero、Sam Rosenblum
    

### **顾问团队**

-   Nathalie McGrath（Coinbase 前 HR 主管）
    
-   Juan Suarez（Coinbase 前法律顾问）
    

### **融资情况**

-   **种子/私募轮**：超 2700 万美元
    
-   **主要投资者**：包括 Polygon 的 JD Kanani、顶级做市商和基金
    
-   **IEO**：2024年在 OKX Jumpstart 进行
    
    -   0.5% 供应量（2.1 亿 ZETA）分配给 BTC/OKB 质押者
        

## **路线图与里程碑**

### **已完成**

-   ✅ 主网上线（2024）
    
-   ✅ 多链连接
    
-   ✅ ZetaHub 启动
    

### **2025年重点**

-   🚀 Threshold BLS 签名提升性能
    
-   🛠️ Grokipedia 生态工具
    
-   ⛓️ 比特币/ETH 深度集成
    
-   🤖 AI Buildathon 黑客松（8月）
    
    -   奖金 9000 美元
        
    -   使用 Gemini AI + ZetaChain 工具
        

### **2026年愿景**

-   🌉 跨链 staking（用 BTC/ETH 质押安全网络）
    
-   🔐 ZK 证明 dApps
    
-   🧠 AI/数据集成
    

### **近期更新**

-   **2025年6月**：网络升级（区块高度 8728280）
    
-   **合作伙伴**：与 Google Cloud 合作黑客松
    
-   **未来发展**：ZetaChain 2.0 提案，支持链抽象
    

## **合作伙伴与生态**

### **关键合作伙伴**

-   🤝 **Google Cloud**：AI 黑客松合作
    
-   🤝 **Alibaba Cloud**：加速亚洲区块链采用
    
-   🤝 **EnsoFi**：流动性层合作
    
-   🤝 **LXDAO**：开源项目支持
    

### **生态系统**

-   **DeFi**：跨链交换等应用
    
-   **NFT**：跨链 NFT 生态
    
-   **游戏**：全链游戏体验
    
-   **社区活动**：韩国社区事件（如 ZUNO Galaxy 活动，抽奖 USDT）
    

### **社区动态**

-   🐦 **X/Twitter**：活跃讨论合规（MiCAR/DFSA）、TVL 增长
    
-   👨‍🏫 **开发者活动**："残酷共学"课程（ZetaChain + Qwen AI）
    
-   📊 **市场表现**：TVL 持续增长，社区活跃度高
    

# **3，**对 ZetaChain 的理解与分析

ZetaChain 的本质：它到底在解决什么问题？ZetaChain 不是又一个“跨链桥”，而是试图终结“跨链桥时代”的基础设施。

ZetaChain 要做的是：把所有链都当成自己的“Layer 2”来管理。  
它通过在自己的 L1 上运行全节点观察者（Observer + Signer），实现了：

-   比特币、Dogecoin 这类非智能合约链也能被原生调用
    
-   无需包装、无需桥、无需第三方托管，直接在 ZetaChain 合约里操作外部链资产
    

一句话总结：  
ZetaChain 想成为“区块链世界的 IPv6”——让所有老链、新链天然互通，不再需要无数个翻译器（桥）。
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
