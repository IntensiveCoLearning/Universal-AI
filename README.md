# 通用 AI · Universal AI

## 介绍

「通用 AI 残酷共学」由 ZetaChain 与阿里云发起，LXDAO 联合主办，面向有基础的开发者，集中训练「通用区块链 × 大模型」的实战能力。课程从环境配置与基础概念出发，带你掌握 ZetaChain 的通用区块链架构、ZRC-20 与跨链 DeFi 开发，以及 Qwen / Qwen-Agent 的 API 调用与意图解析。最终目标是让每位参与者都能完成一条从「自然语言指令 → AI Agent 解析 → 调用 ZetaChain 通用合约 → 完成真实 DeFi 操作」的端到端 Demo，为黑客松「通用 DeFi」与「通用 AI 应用」赛道提供可直接复用的项目基础。

更多活动详情请参见：<https://x.com/ZetaChain_CH/status/1990601068620009846>
## 关键词

Universal AI, 通用 AI, Qwen, ZetaChain, EVM, Smart Contract, 跨链 DeFi, Web3 开发
## 面向人群

- 已具备一定编程基础的开发者（熟悉 JavaScript/TypeScript、Node.js 或 Python 等任一语言）。
- 对 Web3 / 区块链有基础认知，了解 EVM、智能合约、钱包与 RPC 等概念者优先。
- 想要系统了解「通用区块链（ZetaChain）」与「大模型（通义千问 Qwen）」如何结合，落地跨链 DeFi、AI Agent 等实际应用的开发者。
- 即将参加或计划参加本次黑客松「通用 DeFi」与「通用 AI 应用」赛道，希望通过高强度共学快速补齐知识、完成从 0→1 Demo 的团队与个人。
- 相关领域的创业者，优秀的人才和团队将会最高可获来自 ZetaChain $200,000 生态资助以及诸多权益
## 报名时间

- 报名开始时间：2025-11-17
- 报名结束时间：2025-11-23
## 共学时间

- 共学开始时间：2025-11-24
- 共学结束时间：2025-12-07
## 发起人

- 姓名：Bruce
- GitHub ID：brucexu-eth
- Telegram：brucexu_eth
- Email：brucex2710@gmail.com
## 发起组织

  [ZetaChain](http://zetachain.com/) <img alt="organization-logo" height="60px" width="60px" src="https://cdn.lxdao.io/010474d9-3890-4498-bd74-5d83696e7c64.png" />

  [Alibaba Cloud](https://www.alibabacloud.com/) <img alt="organization-logo" height="60px" width="60px" src="https://cdn.lxdao.io/eb59bf19-313b-421c-955a-6f2b17f0a899.png" />

  [LXDAO](https://lxdao.io/) <img alt="organization-logo" height="60px" width="60px" src="https://cdn.lxdao.io/bafkreiay6vxsvv3ksxr75lzzt3iqy3zja3o2epuxh47ivs24p2xs3awexm.png" />


## 社群

Telegram：https://t.me/zetachain_asia
## 学习资料/课程安排

欢迎来到由 ZetaChain 与阿里云共同发起由 LXDAO 联合主办的 ”通用 AI“ 残酷共学。我们推荐参考 14 天的学习计划：<https://lxdao.notion.site/AI-14-2addceffe40b80a28a5dd0fbdd3494d2?source=copy_link>

下面是更完整的学习资料：

### 模块 0：环境与工具准备（ZetaChain + Qwen）

**目标**

- 大家都能本地 / 云端跑通：
  - ZetaChain 开发环境（CLI / Localnet / Testnet）。
  - Qwen 的基础 API 调用。

**关键知识点**

- ZetaChain 账户、RPC、Faucet、Explorer、CLI、本地开发环境。
- Qwen 账号、API Key、基础请求（Chat / Completion）。

**相关文档**

- ZetaChain 总文档 https://www.zetachain.com/docs/
- ZetaChain Developers（教程、示例）https://www.zetachain.com/docs/developers
- ZetaChain CLI https://github.com/zeta-chain/cli
- ZetaChain Node / RPC / Faucet / Explorer / 测试币获取 http://zetachain.com/docs/reference/
- Qwen 总文档（Quickstart / Key Concepts） https://qwen.readthedocs.io/zh-cn/latest/
- Qwen API 参考（含 OpenAI 兼容方式） https://www.alibabacloud.com/help/zh/model-studio/qwen-api-reference

---

### 模块 1：ZetaChain & Universal Blockchain 基础

**目标**

- 理解 “通用区块链 / Universal EVM / Omnichain Smart Contract / Universal App” 等核心概念。
- 能简单画出：ZetaChain + 多条公链 + Gateway 的架构图，并且解释相关工作流程。

**关键知识点**

- Universal App / Universal EVM 的概念。
- Gateway 的角色与跨链消息大致流程。
- Connected Chains（例如 Bitcoin、Ethereum、Solana 等）的意义。

**推荐文档**

- Universal Apps 概览 https://www.zetachain.com/docs/start/app
- 开发者总览（Universal EVM / Gateway / Cross-Chain） https://www.zetachain.com/docs/developers
- 为什么在 ZetaChain 上面开发？https://www.zetachain.com/docs/start/build
- ZetaChain 架构设计 https://www.zetachain.com/docs/developers/architecture/overview

---

### 模块 2：Universal DeFi & 全链资产（对应“通用 DeFi”赛道）

**目标**

- 了解 ZRC-20、Universal Token / NFT 等资产标准。
- 能基于官方示例理解一个跨链 Swap / 简单 DeFi 协议。
- 根据 Workshop 运行并且跑起来官方提供的 Demo，进行交互和测试。

**关键知识点**

- ZRC-20：多链资产在 ZetaChain 上的统一表示。
- 通用资产：Universal Token / Universal NFT 的基本使用场景。
- Swap / Messaging 教程中的跨链调用流程。

**推荐文档**

- ZRC-20 标准 https://www.zetachain.com/docs/developers/evm/zrc20/
- Universal 资产文档 https://www.zetachain.com/docs/developers/standards/overview
- Tutorials（Swap / Messaging / Calls to-from EVM）https://www.zetachain.com/docs/developers/tutorials/swap
- Example code repo：https://github.com/zeta-chain/example-contracts

---

### 模块 3：Qwen AI 基础 & API 调用

**目标**

- 会用自己熟悉的语言（建议 Node.js / Python）调用 Qwen API。
- 了解常用模型、参数设置和基本费用概念。

**关键知识点**

- 不同 Qwen 模型系列（如通用对话、代码、Agent 等）的用途。
- OpenAI 兼容接口调用方式。
- 基本错误处理、重试逻辑。

**推荐文档**

- Qwen API 平台 [qwen.ai](http://qwen.ai/)
- Qwen 总文档（入门、示例代码） https://qwen.readthedocs.io/en/latest/
- Qwen API 参考（含 OpenAI 兼容示例） https://www.alibabacloud.com/help/en/model-studio/qwen-api-reference

---

### 模块 4：Qwen-Agent & Web3 AI Agent（对应“通用 AI 应用”赛道）

**目标**

- 会用 Qwen-Agent 搭建一个简单 Agent，并挂载自定义 Tool。
- 能设计 “意图识别 → 调用链上合约”的工具接口，为 ZetaChain DeFi / 钱包操作服务。
- 能让 Agent 输出结构化 DeFi 参数（例如 swap 所需参数）。

**关键知识点**

- Qwen-Agent 组件：LLM 配置、Agent、Tools、Memory。
- 如何定义一个 DeFi 工具（如 `swap(tokenIn, tokenOut, amount, chain)`），并让 Agent 自动调用。
- 安全限制：额度限制、白名单资产、人工确认。

**推荐文档**

- Qwen-Agent 文档 https://qwen.readthedocs.io/en/v2.5/framework/qwen_agent.html
- Qwen-Agent GitHub（示例代码） https://github.com/QwenLM/Qwen-Agent

---

### 模块 5：ZetaChain × Qwen 综合实践（AI 控制通用 DeFi）

**目标**

- 能把“通义千问 Agent + ZetaChain 通用合约”连起来，完成一条端到端路径
  - 用户用自然语言下单 → Qwen Agent 解析 → 调用后端工具 → 通过 Gateway 与 Universal App 交互 → 完成 DeFi 操作。

**关键知识点**

- 前端：简单输入框 / Chat UI（提供模板即可）。
- 后端：封装 Qwen 调用和链上调用的服务（Node.js / Python）。
- 合约：基于 ZRC-20 / Swap / Messaging 实现一个最小可用 DeFi 合约。

**参考资料**

- ZetaChain Developers（Swap 示例、Messaging 示例） https://www.zetachain.com/docs/developers
- Qwen-Agent 自定义工具相关部分 https://qwen.readthedocs.io/en/v2.5/framework/qwen_agent.html
- 前端 Example code：TODO

**最终挑战 demo 要求（可作为黑客松基础模版）：**

- 支持至少 2 条链之间的简单 Swap 或存款操作。
- 有清晰的日志与提示信息，便于 Debug。

---

参加共学营的朋友请务必加入 ZetaChain 中国开发者社区，如有加群问题，可以添加微信：arainqinqin（备注：通用 AI）：

![Image](https://cdn.lxdao.io/7b27148f-9afb-4419-9402-ac4ffe7bd28e.jpg)


## 共学激励

对于每天认真学习并且坚持用高质量笔记打卡的学员，你将有机会获得：

- 优秀实习机会，加入 ZetaChain 大使计划
- Qwen 的 API Token Credit
- 受邀加入第二阶段的休闲黑客松，更高概率获奖
- 更高概率被 ZetaChain 官方团队发现，并且获得最高 20 万美元的创业资助


## 更多信息

报名和打卡流程如下：

- [如何报名残酷共学](https://www.notion.so/lxdao/28fdceffe40b81b4ae63c22c91c4aab2)
- [如何进行每日打卡](https://www.notion.so/lxdao/28fdceffe40b81fa8eedee04c770ddde)


## 报名和打卡规则

- 报名：https://www.notion.so/lxdao/232dceffe40b8030993ad26f2eb6bed2

- 打卡：https://www.notion.so/lxdao/232dceffe40b80508330c5ee936d4dab

## 残酷共学打卡记录表

✅ = Done ⭕️ = Missed ❌ = Failed

<!-- START_COMMIT_TABLE -->
| Name | 11.24 | 11.25 | 11.26 | 11.27 | 11.28 | 11.29 | 11.30 | 12.01 | 12.02 | 12.03 | 12.04 | 12.05 | 12.06 | 12.07 |
| ------------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| [machiwachi](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/machiwachi.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [brucexu-eth](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/brucexu-eth.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ⭕️ | ⭕️ | ✅ | ✅ | ✅ | ✅ |   | |
| [SuBCweb3](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/SuBCweb3.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ⭕️ | ⭕️ | ✅ |   | | |
| [Igor777-Li](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Igor777-Li.md) | ✅ | ⭕️ | ⭕️ | ❌ | | | | | | | | | | |
| [ququzone](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/ququzone.md) | ✅ | ⭕️ | ⭕️ | ❌ | | | | | | | | | | |
| [Plato333](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Plato333.md) | ✅ | ✅ | ✅ | ✅ | ⭕️ | ✅ | ✅ | ✅ | ✅ | ⭕️ | ⭕️ |   | | |
| [Darkells](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Darkells.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |   | | |
| [wyeeeh](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/wyeeeh.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [jeffierw](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/jeffierw.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |   | | |
| [B1u-e](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/B1u-e.md) | ⭕️ | ⭕️ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ⭕️ | ✅ | ✅ | | |
| [xiaodian6777](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/xiaodian6777.md) | ✅ | ✅ | ⭕️ | ✅ | ✅ | ✅ | ✅ | ✅ | ⭕️ | ✅ | ✅ |   | | |
| [tumiao233](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/tumiao233.md) | ⭕️ | ✅ | ✅ | ⭕️ | ✅ | ❌ | | | | | | | | |
| [000fan000](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/000fan000.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [Coooder-Crypto](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Coooder-Crypto.md) | ⭕️ | ✅ | ⭕️ | ✅ | ✅ | ❌ | | | | | | | | |
| [younggogogo](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/younggogogo.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | | |
| [TPB003](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/TPB003.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [pillowtalk-Qy](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/pillowtalk-Qy.md) | ✅ | ✅ | ✅ | ⭕️ | ✅ | ✅ | ✅ | ✅ | ⭕️ | ✅ | ✅ | ✅ | | |
| [Hu-Wentao](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Hu-Wentao.md) | ✅ | ✅ | ✅ | ⭕️ | ✅ | ✅ | ✅ | ✅ | ✅ | ⭕️ | ✅ |   | | |
| [Thomas-YHS](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Thomas-YHS.md) | ✅ | ⭕️ | ✅ | ✅ | ✅ | ✅ | ⭕️ | ✅ | ✅ | ⭕️ | ✅ | ✅ | | |
| [Appler-R](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Appler-R.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ⭕️ | ✅ | ✅ | | |
| [15998431520](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/15998431520.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | | |
| [Max-wht](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Max-wht.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | | |
| [lilexi-bot](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/lilexi-bot.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [linghuccc](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/linghuccc.md) | ✅ | ✅ | ⭕️ | ⭕️ | ❌ | | | | | | | | | |
| [wukong988](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/wukong988.md) | ✅ | ✅ | ✅ | ⭕️ | ✅ | ✅ | ✅ | ✅ | ⭕️ | ⭕️ | ❌ | | | |
| [siemens84cn](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/siemens84cn.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | | |
| [hanschurer](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/hanschurer.md) | ⭕️ | ✅ | ⭕️ | ❌ | | | | | | | | | | |
| [BrKDDD](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/BrKDDD.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ⭕️ | ⭕️ | ✅ | ✅ | ✅ | ✅ |   | | |
| [binova3](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/binova3.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [Raphael-LEI](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Raphael-LEI.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [gitgdut](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/gitgdut.md) | ✅ | ⭕️ | ⭕️ | ❌ | | | | | | | | | | |
| [Ylim314](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Ylim314.md) | ✅ | ✅ | ✅ | ⭕️ | ✅ | ✅ | ✅ | ✅ | ✅ | ⭕️ | ✅ | ✅ | | |
| [1664730539](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/1664730539.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ⭕️ | ⭕️ | ✅ | ✅ | ✅ | ✅ |   | | |
| [lisaite](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/lisaite.md) | ✅ | ✅ | ⭕️ | ⭕️ | ❌ | | | | | | | | | |
| [beyondz0110](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/beyondz0110.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [ChaoyuWang04](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/ChaoyuWang04.md) | ✅ | ✅ | ✅ | ⭕️ | ⭕️ | ✅ | ✅ | ✅ | ⭕️ | ⭕️ | ✅ |   | | |
| [CirCleZhouZhou](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/CirCleZhouZhou.md) | ✅ | ✅ | ⭕️ | ⭕️ | ❌ | | | | | | | | | |
| [Aster-amellus](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Aster-amellus.md) | ⭕️ | ✅ | ✅ | ⭕️ | ❌ | | | | | | | | | |
| [LambertAlpha](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/LambertAlpha.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [ja564](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/ja564.md) | ⭕️ | ✅ | ✅ | ✅ | ⭕️ | ❌ | | | | | | | | |
| [RSXLX](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/RSXLX.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ⭕️ | ✅ | ⭕️ | ✅ | ✅ | ⭕️ | ✅ | | |
| [fylcr](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/fylcr.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | | |
| [Teddy070509](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Teddy070509.md) | ✅ | ⭕️ | ⭕️ | ❌ | | | | | | | | | | |
| [INEEDYOUHELP](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/INEEDYOUHELP.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [jiayunlong228](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/jiayunlong228.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [alexsupa597](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/alexsupa597.md) | ⭕️ | ✅ | ⭕️ | ❌ | | | | | | | | | | |
| [kuove](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/kuove.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ⭕️ | ⭕️ | ✅ | ✅ | ✅ | ✅ |   | | |
| [baicaiyihao](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/baicaiyihao.md) | ⭕️ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | | |
| [catcatcash](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/catcatcash.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [veithly](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/veithly.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [coolzeta](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/coolzeta.md) | ✅ | ✅ | ⭕️ | ✅ | ⭕️ | ❌ | | | | | | | | |
| [bassfuture](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/bassfuture.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [debugzhao](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/debugzhao.md) | ✅ | ⭕️ | ✅ | ✅ | ⭕️ | ❌ | | | | | | | | |
| [Readering1](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Readering1.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [anqi42196-crypto](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/anqi42196-crypto.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [muxin-web3](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/muxin-web3.md) | ✅ | ⭕️ | ⭕️ | ❌ | | | | | | | | | | |
| [12270164](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/12270164.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [chesenas](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/chesenas.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [immortals1278](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/immortals1278.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | | |
| [BiscuitCoder](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/BiscuitCoder.md) | ✅ | ⭕️ | ⭕️ | ✅ | ✅ | ❌ | | | | | | | | |
| [wastonwashing538-arch](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/wastonwashing538-arch.md) | ✅ | ⭕️ | ✅ | ✅ | ✅ | ⭕️ | ❌ | | | | | | | |
| [EastWind0](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/EastWind0.md) | ✅ | ⭕️ | ⭕️ | ✅ | ❌ | | | | | | | | | |
| [skcvan](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/skcvan.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ⭕️ | ⭕️ | ⭕️ | ⭕️ | ❌ | | | | |
| [zylg-create](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/zylg-create.md) | ✅ | ✅ | ✅ | ⭕️ | ⭕️ | ✅ | ✅ | ✅ | ✅ | ⭕️ | ✅ |   | | |
| [dingshuji](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/dingshuji.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ⭕️ | ⭕️ | ✅ | ✅ | | |
| [weleuther900-arch](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/weleuther900-arch.md) | ✅ | ⭕️ | ⭕️ | ❌ | | | | | | | | | | |
| [JunZ-Leo](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/JunZ-Leo.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ⭕️ | ✅ | ✅ | ✅ | ✅ | ✅ |   | | |
| [fca2025774696-art](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/fca2025774696-art.md) | ✅ | ✅ | ✅ | ⭕️ | ⭕️ | ❌ | | | | | | | | |
| [Frederick2313072](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Frederick2313072.md) | ⭕️ | ✅ | ⭕️ | ❌ | | | | | | | | | | |
| [JintolChan](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/JintolChan.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | | |
| [everest-an](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/everest-an.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [MoMNiToT](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/MoMNiToT.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ⭕️ | ✅ | ✅ | ✅ | ✅ | ⭕️ | ✅ | | |
| [Quiz-web177](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Quiz-web177.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [Jizhixing-Kieran](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Jizhixing-Kieran.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [leopc999](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/leopc999.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ⭕️ | ✅ | ✅ | ⭕️ | ✅ | ⭕️ | ✅ | | |
| [slucifersz](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/slucifersz.md) | ✅ | ✅ | ⭕️ | ✅ | ⭕️ | ❌ | | | | | | | | |
| [zane3412](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/zane3412.md) | ✅ | ✅ | ✅ | ✅ | ⭕️ | ⭕️ | ✅ | ⭕️ | ⭕️ | ❌ | | | | |
| [amandakelake](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/amandakelake.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [siphonelee](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/siphonelee.md) | ✅ | ⭕️ | ⭕️ | ❌ | | | | | | | | | | |
| [zg870](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/zg870.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |   | | |
| [Sifotd](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Sifotd.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [HUA-cyber](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/HUA-cyber.md) | ✅ | ✅ | ✅ | ⭕️ | ✅ | ✅ | ✅ | ✅ | ⭕️ | ⭕️ | ❌ | | | |
| [tian-Sun](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/tian-Sun.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [spryo9](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/spryo9.md) | ✅ | ✅ | ✅ | ✅ | ⭕️ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |   | | |
| [git149](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/git149.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ⭕️ | ✅ | ⭕️ | ✅ | ⭕️ |   | | |
| [cwj526](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/cwj526.md) | ✅ | ⭕️ | ⭕️ | ❌ | | | | | | | | | | |
| [Zheng-Yu7463](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Zheng-Yu7463.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ⭕️ | ✅ | ✅ | ✅ | | |
| [ShawnX-F](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/ShawnX-F.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | | |
| [JoyWQ](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/JoyWQ.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ⭕️ | ⭕️ | ✅ | ✅ | ✅ | ✅ |   | | |
| [JoenHonwang](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/JoenHonwang.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [cheng-chun-yuan](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/cheng-chun-yuan.md) | ✅ | ✅ | ✅ | ⭕️ | ✅ | ✅ | ✅ | ✅ | ⭕️ | ✅ | ⭕️ | ✅ | | |
| [diandian42](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/diandian42.md) | ✅ | ✅ | ✅ | ⭕️ | ✅ | ✅ | ⭕️ | ✅ | ✅ | ⭕️ | ✅ |   | | |
| [linmu115](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/linmu115.md) | ✅ | ✅ | ✅ | ⭕️ | ✅ | ⭕️ | ❌ | | | | | | | |
| [StarryNights9527](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/StarryNights9527.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [gegej1](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/gegej1.md) | ⭕️ | ✅ | ⭕️ | ❌ | | | | | | | | | | |
| [fDu-Xia](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/fDu-Xia.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [Airjiannan05](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Airjiannan05.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | | |
| [Kongweiyi928](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Kongweiyi928.md) | ✅ | ✅ | ✅ | ✅ | ⭕️ | ✅ | ✅ | ⭕️ | ✅ | ⭕️ | ❌ | | | |
| [BlackGou](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/BlackGou.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [skelitalynn](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/skelitalynn.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ⭕️ | ✅ | ✅ | ⭕️ | ✅ | ⭕️ | ✅ | | |
| [PeaSea93](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/PeaSea93.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [LWT-111](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/LWT-111.md) | ✅ | ⭕️ | ✅ | ✅ | ⭕️ | ❌ | | | | | | | | |
| [bentong-chain](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/bentong-chain.md) | ✅ | ⭕️ | ⭕️ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | | |
| [SJW1111011](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/SJW1111011.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [LouisCanBe](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/LouisCanBe.md) | ✅ | ✅ | ✅ | ✅ | ⭕️ | ✅ | ⭕️ | ✅ | ✅ | ⭕️ | ⭕️ |   | | |
| [jvbaoge1](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/jvbaoge1.md) | ✅ | ✅ | ✅ | ✅ | ⭕️ | ✅ | ✅ | ⭕️ | ✅ | ⭕️ | ✅ | ✅ | | |
| [ccc21800](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/ccc21800.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [mango-ice-cat](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/mango-ice-cat.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | | |
| [Free-riding](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Free-riding.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | | |
| [StellaWang5209](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/StellaWang5209.md) | ✅ | ✅ | ✅ | ⭕️ | ⭕️ | ❌ | | | | | | | | |
| [fkkloorgennting-prog](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/fkkloorgennting-prog.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ⭕️ | ✅ | ✅ | ✅ | ✅ | ✅ | | |
| [Kgzsgod](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Kgzsgod.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | | |
| [willietheking](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/willietheking.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [lpppy](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/lpppy.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [tan9799](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/tan9799.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | | |
| [Muramasa-Lynn](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Muramasa-Lynn.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [wkarry450-max](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/wkarry450-max.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [Valerie-HXH](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Valerie-HXH.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [lkunyang721-design](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/lkunyang721-design.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [rtxmythically](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/rtxmythically.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [timerring](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/timerring.md) | ✅ | ⭕️ | ✅ | ✅ | ✅ | ⭕️ | ❌ | | | | | | | |
| [big-dudu-mosty](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/big-dudu-mosty.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [libaiwudufuyou](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/libaiwudufuyou.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [Jerrykingone](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Jerrykingone.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [TEDGD](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/TEDGD.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [Xiongruijie](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Xiongruijie.md) | ✅ | ✅ | ✅ | ✅ | ⭕️ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | | |
| [rayjun](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/rayjun.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [Jack-OuCJ](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Jack-OuCJ.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [NoTks0808](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/NoTks0808.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | | |
| [XiaoHai67890](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/XiaoHai67890.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | | |
| [jorgenlou](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/jorgenlou.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [qky190525](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/qky190525.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | | |
| [gooo36923-web](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/gooo36923-web.md) | ✅ | ⭕️ | ⭕️ | ❌ | | | | | | | | | | |
| [kelvinqin7](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/kelvinqin7.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [guow7394](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/guow7394.md) | ✅ | ✅ | ⭕️ | ✅ | ✅ | ✅ | ✅ | ✅ | ⭕️ | ⭕️ | ❌ | | | |
| [june-in-exile](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/june-in-exile.md) | ⭕️ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ⭕️ | ✅ | | |
| [mersault18](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/mersault18.md) | ✅ | ✅ | ✅ | ⭕️ | ✅ | ✅ | ✅ | ✅ | ✅ | ⭕️ | ✅ | ✅ | | |
| [linqiwei666](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/linqiwei666.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [changfeng-01](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/changfeng-01.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [jackysense](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/jackysense.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ⭕️ | ⭕️ | ⭕️ | ✅ | ⭕️ | ❌ | | | |
| [xihale](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/xihale.md) | ✅ | ⭕️ | ⭕️ | ❌ | | | | | | | | | | |
| [Mikey202580](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Mikey202580.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [sheep3gold](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/sheep3gold.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [Echo-qxt](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Echo-qxt.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [lyzhellook](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/lyzhellook.md) | ✅ | ⭕️ | ⭕️ | ❌ | | | | | | | | | | |
| [xuzqyy](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/xuzqyy.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ⭕️ | ✅ | ✅ | ⭕️ | ✅ | ⭕️ | ✅ | | |
| [zlp528](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/zlp528.md) | ✅ | ✅ | ⭕️ | ✅ | ⭕️ | ✅ | ✅ | ⭕️ | ✅ | ⭕️ | ✅ | ✅ | | |
| [Tuscl-1](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Tuscl-1.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [dersteppenwolfruowen-316](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/dersteppenwolfruowen-316.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [ARZER-TW](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/ARZER-TW.md) | ⭕️ | ✅ | ⭕️ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |   | | |
| [Toby1009](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Toby1009.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | | |
| [Modestide](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Modestide.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [kikyou1009](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/kikyou1009.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [ads12306](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/ads12306.md) | ⭕️ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |   | | |
| [qwe638853](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/qwe638853.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | | |
| [Oblivionis214](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Oblivionis214.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [c4yCC](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/c4yCC.md) | ✅ | ✅ | ✅ | ✅ | ⭕️ | ⭕️ | ❌ | | | | | | | |
| [coolifewindy](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/coolifewindy.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [aced-lab](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/aced-lab.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ⭕️ | ⭕️ | ⭕️ | ⭕️ | ❌ | | | | |
| [3550625279-ux](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/3550625279-ux.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [wodeche](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/wodeche.md) | ⭕️ | ✅ | ⭕️ | ❌ | | | | | | | | | | |
| [Rousseau-DeworkHub](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Rousseau-DeworkHub.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [MapleRhythm](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/MapleRhythm.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [MRzzz-cyber](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/MRzzz-cyber.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [yedeyu](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/yedeyu.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |   | | |
| [yoyos12138](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/yoyos12138.md) | ✅ | ✅ | ⭕️ | ⭕️ | ❌ | | | | | | | | | |
| [leran-133](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/leran-133.md) | ✅ | ⭕️ | ⭕️ | ❌ | | | | | | | | | | |
| [YXZ252426](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/YXZ252426.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [SkySommer](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/SkySommer.md) | ✅ | ✅ | ⭕️ | ✅ | ✅ | ✅ | ✅ | ✅ | ⭕️ | ✅ | ⭕️ | ✅ | | |
| [xk666-lab](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/xk666-lab.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [JACKhuamei](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/JACKhuamei.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [Augety1017](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Augety1017.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ⭕️ | ✅ | ✅ | ⭕️ | ⭕️ | ✅ | | |
| [zhishi66621](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/zhishi66621.md) | ✅ | ✅ | ✅ | ⭕️ | ⭕️ | ❌ | | | | | | | | |
| [Xboxpig](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Xboxpig.md) | ✅ | ✅ | ⭕️ | ✅ | ✅ | ✅ | ✅ | ⭕️ | ✅ | ⭕️ | ❌ | | | |
| [TAnNbR](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/TAnNbR.md) | ⭕️ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ⭕️ | ⭕️ | ❌ | | | | |
| [neppiggy](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/neppiggy.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [yhzhongc](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/yhzhongc.md) | ✅ | ✅ | ✅ | ✅ | ⭕️ | ⭕️ | ✅ | ✅ | ⭕️ | ✅ | ⭕️ | ✅ | | |
| [madison42](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/madison42.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [Y4NG666](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Y4NG666.md) | ✅ | ⭕️ | ⭕️ | ❌ | | | | | | | | | | |
| [BrucerTao](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/BrucerTao.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [zjx91](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/zjx91.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [SharKPep](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/SharKPep.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [piscispsa](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/piscispsa.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [jiangjiax](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/jiangjiax.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [Micteach](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Micteach.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [TigerOpt](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/TigerOpt.md) | ✅ | ✅ | ✅ | ✅ | ⭕️ | ⭕️ | ❌ | | | | | | | |
| [MartinYeung5](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/MartinYeung5.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [uneer143](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/uneer143.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [xzzlks](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/xzzlks.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | | |
| [Lumi-Zheng](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Lumi-Zheng.md) | ✅ | ⭕️ | ⭕️ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ⭕️ | ✅ |   | | |
| [asdspal](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/asdspal.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [Ramlisese](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Ramlisese.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [QIN7QIN7](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/QIN7QIN7.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [den1305](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/den1305.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | | |
| [mascotty](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/mascotty.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [dasjfalskfjaf](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/dasjfalskfjaf.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [g15230103179-bot](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/g15230103179-bot.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [jmwasky-creative](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/jmwasky-creative.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | | |
| [621-hykd](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/621-hykd.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | | |
| [Kunsoul](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Kunsoul.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [0x-IHRR](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/0x-IHRR.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [nocb](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/nocb.md) | ✅ | ⭕️ | ⭕️ | ✅ | ❌ | | | | | | | | | |
| [chenyllove001](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/chenyllove001.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [taiaiJiaZhi](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/taiaiJiaZhi.md) | ✅ | ⭕️ | ⭕️ | ✅ | ❌ | | | | | | | | | |
| [wind1994ai](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/wind1994ai.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [8LokyYang8](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/8LokyYang8.md) | ✅ | ⭕️ | ⭕️ | ❌ | | | | | | | | | | |
| [kaka666111](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/kaka666111.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [Nikoheihei](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Nikoheihei.md) | ⭕️ | ✅ | ✅ | ⭕️ | ❌ | | | | | | | | | |
| [KevinYZH](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/KevinYZH.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [hxzxa](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/hxzxa.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [jake2020520](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/jake2020520.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ⭕️ | ⭕️ | ⭕️ | ⭕️ | ❌ | | | | |
| [Whiteoleand](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Whiteoleand.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [70549019i-code](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/70549019i-code.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [dreamR-cmd](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/dreamR-cmd.md) | ✅ | ✅ | ⭕️ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | | |
| [lynn-000-cccc](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/lynn-000-cccc.md) | ✅ | ✅ | ⭕️ | ✅ | ⭕️ | ❌ | | | | | | | | |
| [Professor-dev-1](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Professor-dev-1.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [yrd241](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/yrd241.md) | ✅ | ✅ | ⭕️ | ✅ | ✅ | ⭕️ | ✅ | ✅ | ⭕️ | ⭕️ | ✅ |   | | |
| [EternalSaint1van](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/EternalSaint1van.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [jack111-222-333](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/jack111-222-333.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [Eeeeye](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Eeeeye.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | | |
| [StariverKang](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/StariverKang.md) | ✅ | ⭕️ | ⭕️ | ✅ | ❌ | | | | | | | | | |
| [shihshaoni](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/shihshaoni.md) | ⭕️ | ⭕️ | ❌ | | | | | | | | | | | |
| [poerty0509](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/poerty0509.md) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ⭕️ | ✅ | ✅ | ⭕️ | ⭕️ |   | | |
| [Rikki-anonlove](https://github.com/IntensiveCoLearning/Universal-AI/blob/main/Rikki-anonlove.md) | ⭕️ | ✅ | ⭕️ | ❌ | | | | | | | | | | |
<!-- END_COMMIT_TABLE -->

















































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































<!-- STATISTICALDATA_START -->
## 统计数据

- 总参与人数: 0
- 完成人数: 0
- 完成用户: 
- 全勤用户: 
- 淘汰人数: 0
- 淘汰率: 0.00%
<!-- STATISTICALDATA_END -->


## 请假规则

每周请假 2 次


