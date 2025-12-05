---
timezone: UTC+8
---

# Blue

**GitHub ID:** B1u-e

**Telegram:** @bblue626

## Self-introduction

Web3 全沾开发者、社区运营

## Notes

<!-- Content_START -->
# 2025-12-05
<!-- DAILY_CHECKIN_2025-12-05_START -->
安全性与审计

-   **公开审计与透明度**：ZetaChain 在其 GitHub / 官方仓库中公开了多份审计报告与安全相关文档（例如 Hashlock 审计、Hacken 报告等），并在社区展示修复进展。虽然有审计，但跨链写入外链交易本身增加了额外攻击面（oracle/observer/TSS 协调、外链节点故障、重放与前端攻击等）。建议重点审阅最新的审计执行记录与修复日志。
    

团队与资金面

-   **团队构成**：资料显示项目由一批有交易所/区块链背景的成员领导（公开记录有 Ankur Nandwani、Charlie Pyle、Charlie McCowan 等核心成员），团队曾获得数轮风投资金（公开报道有 ~2700万美元等），且有机构/交易所背书。团队背景是技术与运营相对强的加分项，但也要实时核验团队治理与人员变动。
<!-- DAILY_CHECKIN_2025-12-05_END -->

# 2025-12-04
<!-- DAILY_CHECKIN_2025-12-04_START -->

5) 安全性与审计（Audits）

-   **公开审计与透明度**：ZetaChain 在其 GitHub / 官方仓库中公开了多份审计报告与安全相关文档（例如 Hashlock 审计、Hacken 报告等），并在社区展示修复进展。虽然有审计，但跨链写入外链交易本身增加了额外攻击面（oracle/observer/TSS 协调、外链节点故障、重放与前端攻击等）。建议重点审阅最新的审计执行记录与修复日志。
    

6) 团队与资金面

-   **团队构成**：资料显示项目由一批有交易所/区块链背景的成员领导（公开记录有 Ankur Nandwani、Charlie Pyle、Charlie McCowan 等核心成员），团队曾获得数轮风投资金（公开报道有 ~2700万美元等），且有机构/交易所背书。团队背景是技术与运营相对强的加分项，但也要实时核验团队治理与人员变动。
<!-- DAILY_CHECKIN_2025-12-04_END -->

# 2025-12-02
<!-- DAILY_CHECKIN_2025-12-02_START -->


**代币经济（Tokenomics & Utility）**

-   **ZETA 的主要用途**：作为链内 gas（支付内外链事务写入）、用于协议池兑换外链气体资产、用于质押/安全与治理等。链上也实现了类似 EIP-1559 的燃烧机制（部分交易会销毁 ZETA）。
    
-   **总量与分配（概要）**：公开资料显示 ZETA 总供应量约在 ~2.1B（需核验最新合约/文档），早期有锁仓与分期解锁安排，项目通常会有团队/基金会/社区/生态预留等分配。对投资人最重要的是关注代币释放曲线与流动性池的锁定情况（token unlock 风险）。
    

**生态与链上数据（当前规模与动态）**

-   **Mainnet Beta 与早期数据**：官方2024 年启动 Mainnet Beta，并在上线初期报告了大量用户与交易（如首周数十万至百万级用户与数百万级交易）。这表明早期市场推广与活动推力强。
    
-   **TVL / 经济体量**：第三方数据（DeFiLlama / CoinGecko 等）显示 ZetaChain 的 TVL 处于较低级别（以美元计数百万级别，生态仍在早期阶段）。具体数字（例如 DeFiLlama 报 $~9.8M）会波动，合约活跃度和 DEX 体量相对较小。注意这意味着“生态深度/流动性”仍薄弱。
    
-   **合作与集成**：官方博客与公告显示项目在积极扩展合作伙伴（比如与云厂商、一些生态项目与交易平台的合作），并推动开发者工具（ZetaHub、开发文档、Omnichain SDK）。
<!-- DAILY_CHECKIN_2025-12-02_END -->

# 2025-12-01
<!-- DAILY_CHECKIN_2025-12-01_START -->



1) 项目概述（What）

ZetaChain 定位为“Universal Blockchain / Omnichain L1”，目标是在 L1 层为开发者提供原生跨链合约和原生访问外部链资产的能力（不仅仅是消息传输或桥接）。它宣称可以直接读写包括以太坊、比特币、Solana 等链上的状态，目的是让 dApp 开发者能写一次合约并在多链/跨链环境中原生运作。

2) 核心技术与架构（How）

共识与链设计：采用 Tendermint-style 的 PoS 架构（快速最终性路径），并针对跨链互动进行了节点角色划分（validator、observer、TSS signer）。该设计帮助链在维护自身共识的同时，持续同步外部链状态。

阈值签名（TSS / GG20）：为实现“链上对外发起交易”或“原生管理外链资产”，ZetaChain 使用阈签（leaderless GG20）机制来做分布式密钥生成与签名，避免单点私钥泄露，从而以去中心化方式签发外链交易。

Omnichain Smart Contracts / Omnichain Accounts：Zeta 推出“Omnichain Accounts”与“Omnichain Smart Contracts”概念，合约可以在 Zeta 发起/控制并写回或读取其他链的状态（例如在链 A mint，在链 B 可见），从而支持真正的跨链状态一致性与统一应用体验。

跨链消息与价值转移：与 LayerZero（消息层）不同，Zeta 将“消息+资产操作”放入 L1 原生逻辑，目标是减少中间桥接/包装代币需求并降低 UX 复杂度。
<!-- DAILY_CHECKIN_2025-12-01_END -->

# 2025-11-30
<!-- DAILY_CHECKIN_2025-11-30_START -->




30好今天，卡卡卡卡打卡
<!-- DAILY_CHECKIN_2025-11-30_END -->

# 2025-11-29
<!-- DAILY_CHECKIN_2025-11-29_START -->





今天水一下打卡，在外出活动，手机水水水水水水
<!-- DAILY_CHECKIN_2025-11-29_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->






**Zetachain 投研报告快速结论（TL;DR）**

-   ZetaChain 自称为“Universal / Omnichain”Layer-1，目标是在链层原生支持跨链资产与跨链合约（包括比特币这类非智能合约链），代表着“把跨链能力内建到 L1”的路线。
    
-   项目已于 2024 年启动 Mainnet Beta，并在短期内产生显著的链上活动（用户与交易量报道）。Zeta 作为链内燃料/治理/互操作性工具币，承担跨链写入成本等关键角色。
    
-   当前生态体量仍相对较小（TVL 与链上经济活动处于早期），但已有多轮审计与公开安全报告，技术上采取了 Tendermint 共识 + TSS（阈签）等方案来保证外链签名安全。
    
-   主要风险：强竞争（LayerZero、Axelar、CCIP 等）、token 价格与流动性风险、与外链交互带来的复杂攻击面与监管不确定性。
    
-   投资/跟踪建议：关注主网 1.0 里程碑、外链原生资产上链稳定性、关键审计修复情况、代币解锁/通胀日程与 TVL / 收费流水变化。
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->







## **1️⃣ 通用区块链（Universal Blockchain）**

指一种不局限于单一功能或单一场景、能够 **承载多种应用类型** 的综合型区块链。  
它通常具备：

-   高扩展性
    
-   多链互通能力
    
-   强适配性（支持不同编程语言/虚拟机）
    

其愿景是成为**所有应用的底层基础设施**，而不是只服务某个垂直领域（如 DeFi 或游戏）。

```
ZetaChain 通用应用是一个部署在 ZetaChain 上的智能合约，可以从任何连接的链上被调用。在代币交换场景下，它解决了用户需要与跨链桥或中心化交易所进行复杂交互的问题，允许用户通过单笔交易完成跨链、跨资产的兑换。
```

## **2️⃣ Universal EVM（通用 EVM）**

EVM（Ethereum Virtual Machine）是以太坊的智能合约执行环境。  
“Universal EVM”强调：

-   不仅在以太坊
    
-   而是在**任何链上都能兼容、运行 EVM 合约**
    
-   实现跨链一致的开发体验与合约行为
    

简单理解：

> **EVM 一次开发 → 多链通用 → 行为一致、部署无缝。**

像 Polygon、BNB Chain、Avalanche、Scroll 都属于 EVM 兼容链，但 Universal EVM 更进一步，会强调 **跨链状态共享或代码统一执行**。

## **3️⃣ Omnichain Smart Contract（全链智能合约）**

传统智能合约都只运行在一条链上，彼此之间不共享状态。  
“Omnichain Smart Contract”指：

-   一组合约在多条链上运行
    
-   共享逻辑 & 状态
    
-   链间可互相通信
    
-   用户在任何链上操作，都能更新全局状态
    

例如：

-   在链 A 上 mint
    
-   在链 B 上立刻可见资产
    
-   全链统一的余额、身份、权限、逻辑
    

这也是 LayerZero、ZetaChain 等项目推动的核心方向。

## **4️⃣ Universal App（通用型应用 / 全链应用）**

构建在 Omnichain 或 Universal EVM 之上，让应用：

-   一次部署
    
-   多链可用
    
-   用户无需区分链
    
-   所有链共享一个应用状态
    

它解决的是：

> 当用户在不同链上进入同一应用时，应获得相同的身份、资产、体验。

比如：

-   一个 NFT 市场：无论用户在 BNB Chain、Polygon、Ethereum，都能看到同一订单簿
    
-   一个跨链 DEX：多链交易流动性共享
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->








第一天打卡，大概看了一下学习资料和配置了一下环境
<!-- DAILY_CHECKIN_2025-11-26_END -->
<!-- Content_END -->
