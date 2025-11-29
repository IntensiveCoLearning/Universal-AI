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
