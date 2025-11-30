---
timezone: UTC+8
---

# Tam

**GitHub ID:** TAnNbR

**Telegram:** @Tam_069

## Self-introduction

研三、dapp开发

## Notes

<!-- Content_START -->
# 2025-11-30
<!-- DAILY_CHECKIN_2025-11-30_START -->
# 通用 DeFi 项目构想 — “跨链储蓄 + 资产池 / 流动性池”（Cross-Chain Savings & Pool）

## 一、项目名称（暂拟）

**OmniPool** —— 一个跨链、多资产、一站式储蓄与流动性池

* * *

## 二、目标用户

-   拥有多链资产（例如 ETH, BNB, USDC, Stablecoin, 乃至原生 BTC / EVM native gas token 等）的加密资产持有者。
    
-   希望将不同链上的资产统一管理 / 聚合 / 积累收益 / 分散风险的人。
    
-   想避免频繁跨链操作、桥接、multiple-wallet／multiple-chain 管理复杂性的用户。
    
-   想在一个统一界面下查看、多链资产组合／收益／抵押／稳定币／流动性池等 — 对跨链友好、对新手友好的用户。
    

* * *

## 三、想解决的问题 / 用户痛点

1.  **资产分散** — 多链、多资产导致用户需要持有多个钱包、管理多个地址与不同链 gas／资产。
    
2.  **流动性分散、不易组合** — 各链上的资产和流动性分散，无法方便地组合为更大池子，获取更高收益或更稳定收益。
    
3.  **跨链操作复杂、门槛高** — 桥接、wrap／unwrap、token mapping、手续费、跨链风险 — 对普通用户不友好。
    
4.  **资产管理与收益策略复杂** — 如果把资产分散在多个链上，要手动管理收益、汇率、风险。
    
5.  **缺乏统一、多资产 + 多链 + 易用 + 安全的储蓄 / 流动性解决方案**。
    

* * *

## 四、项目核心理念 & 跨链 / 通用资产 使用方式

OmniPool 的核心是使用 ZetaChain 的跨链基础设施 + ZRC-20（通用资产标准） + 通用合约 (Universal App) + Gateway，打造一个多链资产统一池 (multi-chain asset pool)。

主要方式／机制如下：

-   **资产接入 (Deposit)**
    
    -   用户在任意支持链 (EVM 链, 未来还可能包括非 EVM 链) 存入资产 (native gas token, ERC-20, stablecoin, 或其他已映射资产)。
        
    -   使用 ZetaChain 的 Gateway／Connected 合约，将该资产跨链到 ZetaChain。
        
    -   在 ZetaChain 上，资产被映射成对应的 ZRC-20 代币 (协议内统一资产表示)。
        
-   **池子管理 & 收益 / 流动性逻辑**
    
    -   所有资产都在 ZetaChain 上统一管理 (Universal App 合约)，不论资产原来是哪条链 / 哪种 token。
        
    -   可以设计多种策略：例如混合资产池 (multi-asset pool)、稳定币池 (stable asset pool)、收益 /借贷 /利息池 (借贷 + 利息 + 稳定收益)、流动性池 (liquidity pool)、跨链套利池 (跨链 swap + 跨链流动性) 等。
        
    -   利用统一资产 + 合约逻辑，简化管理、收益分配、计息、估值、风险控制等。
        
-   **提现 / 资产释放 (Withdraw / Redeem)**
    
    -   用户可以请求把资产从 ZetaChain 池子中赎回 (burn ZRC-20) → 通过 Gateway 跨链释放资产 → 回到任意目标链 + 指定链 + 钱包地址。
        
    -   这样用户可以自由选择资产所在链，资产迁移／提取一键完成。
        
-   **跨链组合 / 灵活交互**
    
    -   资产池 + 流动性 + 借贷 + 稳定币 + 多链资产混合。
        
    -   例如：你存入 ETH (Ethereum)、BNB (BNB Chain)、USDC (某条链) → 全部变成池中资产 → 池子按某种算法组合 / 抵押 / 分散 / 计息 → 赎回时可选择任意链 + 资产 + 数量。
        
    -   对用户来说，仅需一次 deposit，就能享受跨链、多资产组合的便利。
        

* * *

## 五、项目价值 / 优势

-   **用户体验极大简化** — 用户不需要管理多个钱包，不需要自己做桥、不需要手动 wrap／unwrap、多链资产合并、手续费管理、跨链 gas 等复杂操作。
    
-   **统一资产、统一逻辑** — 后端逻辑统一在 ZetaChain + Universal App + ZRC-20，易于维护、易于扩展。
    
-   **增强流动性 & 资本效率** — 多链资产合并到一个池子，资产聚合带来更高流动性、更好的收益 / 更高资本利用率。
    
-   **跨链兼容、未来扩展空间大** — 随着 ZetaChain 支持的链越来越多 (EVM + 非 EVM)，资产来源 & 提现目标更灵活。
    
-   **适合长期储蓄 / 稳定收益 / 多链资产持仓者** — 对希望将资产“放入池子、一键管理、多链灵活变动”的用户尤其合适。
    

* * *

## 六、技术 / 架构概要 (高层)

-   使用 ZetaChain 的 **Universal EVM + Gateway** 构建合约
    
-   资产标准采用 ZRC-20 (或未来 Universal Token / Universal NFT)
    
-   Pool 合约部署在 ZetaChain
    
-   Deposit / Withdraw / Redeem / Asset-mapping / Asset-release / 权益分配 / 收益计算 / 资产估值等逻辑全部在 ZetaChain 上执行
    
-   支持多链接入 / 多 token / multi-asset / multi-chain withdraw
    

* * *

## 七、面临的挑战 / 需要注意的问题

-   资产流动性池设计需要谨慎 (池子资产结构、抵押比例、风险分散、定价机制)
    
-   跨链提现 / 释放机制需要 robust, 考虑费率、gas token、跨链稳定性
    
-   安全性 — 跨链、跨资产操作复杂，需要逻辑严谨 + 安全审计
    
-   用户体验 — 虽然简化，但仍需要用户了解跨链基本概念 (source chain, destination chain, gas fee, token mapping 等)
    

* * *

## 八、小结

OmniPool 这个构想利用 ZetaChain 的跨链能力 + ZRC-20 通用资产标准 + Universal App + Gateway，将传统分散、多链、多资产持有模式简化为一个统一、易用、高流动性的池子／储蓄 / 流动性 /资产管理服务。它可以成为多链用户的 “单一资产管理入口 + 多链流动性池 + 跨链储蓄 / 流动性 / 抵押 / 释放” 的解决方案。

如果你觉得这个 idea 有潜力，我可以帮你把它 **写成白皮书 / 设计文档 / 合约草图 / 部署脚本**，方便你进一步评估与实现。
<!-- DAILY_CHECKIN_2025-11-30_END -->

# 2025-11-29
<!-- DAILY_CHECKIN_2025-11-29_START -->

## 我的调用 & ZetaChain 上发生的事情

### 我是从哪里发起的调用？

我从一个支持链（例如 Base / 以太坊 测试网）发起调用。调用方式是通过 ZetaChain 的 Gateway，也就是用户在源链使用 `deposit-and-call`，把资产 + 目标资产／接收者信息提交给 ZetaChain。

### 最终在 ZetaChain 上发生了什么？

1.  源链资产（比如 ETH / ERC-20 / native gas token）被 Gateway 接收并锁定。
    
2.  ZetaChain 将该资产映射为对应的 ZRC-20 代币 — 也就是在 ZetaChain 内部创建一个统一表示该资产的 ZRC-20。
    
3.  Swap 合约 (部署在 ZetaChain) 接收到跨链调用（`onCall(...)`），并解析调用参数 (目标 ZRC-20 token 地址、接收者地址、是否提现标识等) 。
    
4.  合约使用 ZetaChain 内流动性池 (例如 Uniswap v2 路由) 将输入资产兑换为目标 ZRC-20 资产。
    
5.  如果需要提现 (withdraw)，合约调用 Gateway 的 `withdraw(...)`，销毁 (burn) 得到的 ZRC-20 资产，并触发跨链提现机制 — 最终目标链上释放真实资产给接收者。
    
6.  用户在目标链收到目标资产。整个流程对用户相当于 “一次交易 + 跨链 swap + 提现 / 转账”。
<!-- DAILY_CHECKIN_2025-11-29_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->


# ZRC-20 与 ERC-20 的区别及通用资产应用示例

## 一、ZRC-20 与 ERC-20 的主要区别（开发者视角）

ZRC-20 是 ZetaChain 提供的跨链资产标准，它与传统 ERC-20 的核心区别在于多链支持和协议层跨链能力。以下为两者的主要区别。

### 1\. 链的适用范围

\- **ERC-20**：仅适用于单条 EVM 链，如 Ethereum、BNB Chain 和 Polygon。

\- **ZRC-20**：由 ZetaChain 管理，可代表任意连接链的资产，包括 EVM 链和非 EVM 链。

### 2\. 跨链能力

\- **ERC-20**：不具备跨链能力，需要借助外部桥接方案，常涉及 wrapped tokens。

\- **ZRC-20**：原生支持跨链操作，通过“锁定与铸造 / 销毁与释放”模型在协议层处理跨链转移。

### 3\. 资产类型

\- **ERC-20**：仅代表所属链上的代币。

\- **ZRC-20**：可以代表多个链的原生 gas 代币或 ERC-20 代币，使多链资产在 ZetaChain 上表现为统一接口。

### 4\. 多链应用开发模式

\- **ERC-20**：开发多链应用时需要多链部署并维护跨链消息传递逻辑。

\- **ZRC-20**：开发者可将核心逻辑集中部署在 ZetaChain 上，通过 Gateway 接收来自其他链的调用，减少复杂度。

### 5\. 资产提现机制

\- **ERC-20**：无跨链提现机制。

\- **ZRC-20**：支持将 ZRC-20 销毁并在原链或目标链释放真实资产。

总体而言，ERC-20 是单链代币标准，而 ZRC-20 是多链资产抽象层，旨在提供统一、原生的跨链代币体验。

\---

## 二、通用资产的应用场景示例

以下为一个基于 ZRC-20 的典型通用资产应用场景。

### 跨链储蓄或多链资产池

此类应用允许用户从任意支持链存入资产，由 ZetaChain 负责在链间进行抽象和管理。

主要流程如下：

1\. 用户在任意链存入资产，例如 ETH 或 BNB。ZetaChain 在源链锁定资产，并在 ZetaChain 上铸造等值的 ZRC-20 资产。

2\. 储蓄池或资产池合约部署在 ZetaChain 上，负责资产管理、计息或其他策略执行。

3\. 用户需要取款时，销毁对应的 ZRC-20，ZetaChain 在目标链释放对应资产。

这一模式实现了统一的多链资产管理，适用于跨链储蓄、跨链稳定币、跨链流动性池、跨链投资组合等场景。开发者无需编写复杂的跨链桥逻辑，也无需在多链重复部署多套策略。

\---

如需进一步扩展，我可以继续为你起草基于 ZRC-20 的完整应用设计文档、合约结构或通用应用模板。
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->



# 我的第一个 ZetaChain Universal App 设想

## 背景与目标

基于 ZetaChain 的 Universal EVM + 多链互操作能力，我想先做一个 “最简单的跨链事件日志 / 记录” 应用 —— 用来测试跨链调用、跨链消息 + 资产转移 + 跨链合约回调。

目标是：\*\*用户在任意连接链（Ethereum / Bitcoin / Solana …）触发一次调用 → Universal App 在 ZetaChain 上接收 → 记录日志 + optionally 发回一个简单回执 / 状态。\*\*

这个应用主要不是 DeFi，也不是复杂金融逻辑 — 而是一个 **跨链“打印 / 记录 /回执”** 工具，用来验证 ZetaChain 的跨链基础设施，也可作为后续复杂应用（跨链 swap / vault / RWA …）的基础。

\---

## 功能 / 业务流程（What／Why）

\- **跨链调用 & 日志记录**：

任意连接链用户 → 调用 Gateway → deposit token 或 native coin + 携带 message → Universal App 的 `onCall(...)` 被触发。

Universal App 读取 context（sender 链、sender 地址、token / amount、message 数据等），将这些信息以事件或 on–chain 存储的形式记录到 ZetaChain。

\- **回执 / 状态返回（可选）**：

Universal App 可生成一个回执 — 例如一个字符串、事件 ID、时间戳、调用链 + 合约 + sender 信息等。

如果用户希望，也可以通过 Gateway 将这个回执或者某种 token/asset 发回原链。

\- **跨链调用兼容 / 测试多链**：

\- 用户可以从 Ethereum 发起调用，也可以从 Bitcoin / Solana /其他支持链调用同一个合约。

\- 测试不同链资产 deposit → ZetaChain 接收 → 记录 → (可选) 再 withdraw / 回调。

\- 利用 ZetaChain 的 “通用 + 多链 + gas 抽象 + ZRC-20 机制”，验证跨链基础设施是否稳定。

\- **轻量 & 安全**：

\- 合约逻辑非常简单，只负责记录和回执，不做复杂金融运算。

\- 方便调试、开发测试，不易出大问题。

\---
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->




# \# ZetaChain — Universal App 与 Gateway 说明

## \## 1. Universal App 是什么

\- **Universal App** 是部署在 ZetaChain 上的一种智能合约，它运行在 ZetaChain 的 **Universal EVM**。

\- 与普通合约不同，Universal App **可以接受来自任意连接链（如 Ethereum、Bitcoin、Solana 等）的合约调用、消息和代币转移**。

\- 同样，它也可以从 ZetaChain 向连接链发起合约调用或代币转账。

\- 通过这个机制，一个用户签名交易就能触发跨链多步操作。例如：比特币用户发送 BTC → ZetaChain 上的 Universal App 收到 → 转换 / 处理 → 再将资产发到 Ethereum 或 BNB 链。

\- Universal App 支持 “Gas 抽象”（gas abstraction）：

\- 来自连接链（source chain）的调用／转账时，只需要支付源链的 gas。

\- 如果 Universal App 要发起跨链调用或 withdraw，它需要 ZRC-20 gas 代币来支付目标链的 gas。

\- 因为 ZetaChain 的 Universal EVM 与标准 EVM 兼容，开发者可以继续用 Solidity + 现有工具（Hardhat、Foundry 等）开发，只需稍作修改即可获得跨链能力。

\- 总之，Universal App 是 ZetaChain 提供的一种 **跨链智能合约**，可以作为 “中心枢纽（hub）” 来协调多个链之间的资产和调用。

\---

## \## 2. Gateway 大概做什么

\- **Gateway** 是连接 ZetaChain 与各连接链（connected chains）之间的统一 “入口 / 桥” 接口。

\- 每条连接链（例如 Ethereum、Solana、Bitcoin）都有自己的 Gateway：

\- 对于 EVM 链，是一个智能合约。

\- 对于非 EVM 链（如 Bitcoin），是对应的 Gateway 程序 / 地址，由 ZetaChain 的 observer-signer validators 管理。

\- Gateway 的功能包括：

\- **接收（deposit）来自连接链的代币或 native gas**，然后发起调用到 Universal App。

\- **允许连接链用户调用 Universal App**，即使他们只在自己的链上操作，也能触发 ZetaChain 上的逻辑。

\- 在 ZetaChain 一端（Universal EVM 侧），Gateway 还负责：

\- 将 ZRC-20 代币 “withdraw” 回连接链（变回原生资产或 ERC-20）。

\- 发起对连接链的跨链合约调用`withdrawAndCall`）——也就是 Universal App 在 ZetaChain 上可以发起对目标链合同的调用并附带资金。

\- **错误／回退处理（revert）**：如果跨链调用失败（目标链上的调用发生 revert），Gateway 支持灵活的退款机制 — 可以把资产退回到用户地址或调用某个指定合约。

\- Gateway 本质上简化了开发者和最终用户在跨链场景下的交互逻辑：

\- 开发者只需要通过 Gateway 统一 API 与 Universal App 交互，不必为每条链写不同逻辑。

\- 用户从自己熟悉的链（钱包所在链）出发，就能无缝调用跨链应用，无需频繁切换网络。

\---

## \## 3. ZetaChain 架构与设计背景（简要）

\- ZetaChain 采用 **hub-and-spoke 模型**：

\- **Hub** 是 ZetaChain 本身，负责跨链协调和资产中转。

\- **Spokes** 是外部连接链（Ethereum、Solana、Bitcoin 等）。

\- 验证者（Validators）分为两类：

\- **Core Validators**：负责 ZetaChain 区块生产与共识。

\- **Observer-Signer Validators**：负责观察链上外部事件（如用户 deposit）并签名跨链交易。

\- ZetaChain 提供模块化架构（Cosmos SDK 模式），包括 CrossChain 模块、Fungible（ZRC-20）模块等。

\- 跨链交易的生命周期由 CrossChain 模块管理，从接收、验证、签名到最终执行或回退。

\---

## \## 4. 结论

\- **Universal App** 是 ZetaChain 提供的 “跨链兼容合约” 模式：一个合约可以同时服务多个链，处理跨链资产与调用。

\- **Gateway** 是连接各个链与 Universal App 的桥：它负责代币转入 (deposit)、跨链 call、Withdraw，以及错误回退处理。

\- 结合 ZetaChain 的 hub-and-spoke 架构与验证者系统，这套设计使构建跨链应用变得更简单、安全、可扩展。

\---
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->





# ZetaChain 开发精要文档

整理内容来自：

\- ZRC-20 标准

\- Universal Token / NFT

\- 跨链 Swap 教程

\- example-contracts 示例仓库

目标：

1\. 了解 ZRC-20 / Universal Token / Universal NFT

2\. 理解跨链 Swap / DeFi 基础流程

3\. 跑通官方 Demo

\---

## 1\. 核心资产标准

### 1.1 ZRC-20

ZetaChain 上的 ERC-20 增强版，支持跨链资产交互。

**特性：**

\- ERC-20 完全兼容

\- 支持跨链消息 & 跨链转账

\- 内置跨链 Gas 支付（gasToken）

\- 用于构建跨链 DeFi / Swap

\> 简单理解：跨链能力的 ERC-20。

\---

### 1.2 Universal Token

外链真实资产在 ZetaChain 上的统一表示。

**特点：**

\- 与 ERC-20 类似

\- 一个 Token 可被多个链共享

\- 用于跨链支付 / DeFi / 统一资产管理

\---

### 1.3 Universal NFT

一个 NFT 的跨链镜像。

\- 一次 mint，多链使用

\- 用于跨链游戏、身份、资产流通

\---

## 2\. 跨链 Swap / DeFi 的基本原理

跨链交互核心流程：

1\. 用户在链 A 调用 swap

2\. A 链合约通过 `zetaSend()` 发送跨链指令

3\. ZetaChain 中继器转发消息

4\. 链 B 合约收到 `onZetaMessage()` 回调

5\. 链 B 执行 swap / 转账

6\. 用户在链 B 收到资产

**关键函数：**

\- `zetaSend()`：发跨链消息

\- `onZetaMessage()`：链 B 回调

\- `zetaRevert()`：跨链失败回滚

\> 所有跨链 Swap 示例均基于这套机制。

\---

## 3\. 官方 Demo 实操指南（最短路径）

所有示例均来自 `example-contracts` 仓库。

\---

### 3.1 克隆仓库

```
git clone https://github.com/zeta-chain/example-contracts
```

```
cd example-contracts
```

```
npm install
```
<!-- DAILY_CHECKIN_2025-11-25_END -->
<!-- Content_END -->
