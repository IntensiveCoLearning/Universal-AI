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
