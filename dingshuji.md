---
timezone: UTC+8
---

# 喵喵王飞飞

**GitHub ID:** dingshuji

**Telegram:** @xiexingt303

## Self-introduction

认真学习的web3er

## Notes

<!-- Content_START -->
# 2025-12-06
<!-- DAILY_CHECKIN_2025-12-06_START -->
1
<!-- DAILY_CHECKIN_2025-12-06_END -->

# 2025-12-05
<!-- DAILY_CHECKIN_2025-12-05_START -->

[Qwen 代理](https://github.com/QwenLM/Qwen-Agent)是一个基于 Qwen 指令遵循、工具使用、规划和内存能力开发 LLM 应用的框架。它还附带了浏览器助理、代码解释器和自定义助手等示例应用程序。

Qwen-Agent 提供原子组件，如大型语言模型（LLM）和提示符，以及高级组件（如代理）。下面的示例以助手组件为例，演示如何添加自定义工具并快速开发使用工具的代理。
<!-- DAILY_CHECKIN_2025-12-05_END -->

# 2025-12-04
<!-- DAILY_CHECKIN_2025-12-04_START -->


跨链交易（CCTX）可分为两大类：进出交易。  
**入账交易** （连接链→ZetaChain）是在连接链上发起的，并导致在 ZetaChain 上进行交易。一个入账交易由两个交易组成：入站：交易在连接链上被发起并观察。出站：对应的交易在 ZetaChain 上广播并执行。

**出账交易** （ZetaChain→连接链）在 ZetaChain 上发起，并导致在连接链上进行交易。一个出出交易由两个交易组成：入站：交易在 ZetaChain 上被发起并观察到。出站：对应的交易在连接链上广播并执行。
<!-- DAILY_CHECKIN_2025-12-04_END -->

# 2025-12-01
<!-- DAILY_CHECKIN_2025-12-01_START -->



调用通义千问

\- node.js

\- 创建文件夹

\- 初始化 Node.js 项目，生成 package.json 文件：执行 npm init -y

\- 安装 OpenAI SDK：执行命令 npm install openai

\- 新建一个 .js 格式的文件，粘贴开发文档中代码

\- 终端中执行 node 【文件名】：node ai.js
<!-- DAILY_CHECKIN_2025-12-01_END -->

# 2025-11-30
<!-- DAILY_CHECKIN_2025-11-30_START -->




`Swap` 合约是部署在 ZetaChain 上的通用应用。它使用户能够通过一次跨链调用在区块链间进行代币交换。代币以 ZRC-20 形式接收，可选择使用 Uniswap v2 流动性进行交换，并提取回连接链。
<!-- DAILY_CHECKIN_2025-11-30_END -->

# 2025-11-29
<!-- DAILY_CHECKIN_2025-11-29_START -->





部署swap：

\- Node.js（22+）、yarn、git、jq、foundry

\- 下载zetachain包

\- npm install -g zetachain

\- zetachain new --project 文件名hello

\- cd hello

\- yarn

\- forge soldeer update --更新

\- 设置RPC
<!-- DAILY_CHECKIN_2025-11-29_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->






ZRC-20 是集成在 ZetaChain 全链智能合约平台中的代币标准。通过 ZRC-20，开发者可以构建 dApp，在任何连接链上协调本地资产。这使得构建全链 DeFi 协议和 dApps，如全链去中心化交易所（Omnichain DEX）、全链借贷、全链投资组合管理，以及任何涉及多条链上的同质化代币，从一个地方变得极其简单——就像它们都存在于一条链上一样。
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->







\- Hello World Demo工作流：

\- 问题1：使用 CLI + Hardhat / Foundry

\- 区别在于2个框架。Hardhat（ JavaScript/TypeScript ） 或 Foundry（Solidity）

\- 问题2：用本地链还是测试网

\- 本地链（本地私有环境，仅在自己电脑运行）

\- 测试网（公开测试网络，与主网技术特性一致）
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->








\## zeta架构

\- 交互路径：区块链→zetaclient→zetavm→zetacore（ Tendermint 共识和 PoS 验证者网络）→ZetaCore / Blockchain（核心区块链）→签名者签名

\- zetaclient=观察者+签名者

\- Tendermint 共识：拜占庭容错（BFT）共识协议

\- PoS 验证者网络（共识层）：通过质押原生代币参与区块链共识的节点集合
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->









\- 安装zetachain CLI：npm install -g zetachain

\- 检查版本：zetachain --version

\- 检查链接的所有链：zetachain query chains list

\- 在本地启动一个 ZetaChain 测试网节点（本地开发网）：zetachain localnet start
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->










检查安装版本

\- node-v

\- npm-v

\- forge-version（foundry的）

\- 安装foundry

\- 安装git bash；git-v

\- git中安装foundry ：curl -L [https://foundry.paradigm.xyz](https://foundry.paradigm.xyz) | bash

\- 初始化：foundryup

\- 检查foundry的版本：forge --version

\- 安装zetachain CLI：npm install -g zetachain

\- 检查版本：zetachain --version

\- 检查链接的所有链：zetachain query chains list

\- 在本地启动一个 ZetaChain 测试网节点（本地开发网）：zetachain localnet start
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
