---
timezone: UTC+8
---

# yhzhongc

**GitHub ID:** yhzhongc

**Telegram:** @yhzhongc

## Self-introduction

学习

## Notes

<!-- Content_START -->
# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->
今天下班太晚了，先打卡，明天笔记补两天的内容
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->

\### 什么是Universal App?

Universal App 是部署在 ZetaChain 区块链上的智能合约。

**它与普通合约的区别：**

\- **普通合约（如在 Ethereum 上）：** 只能与自己这条链上的资产和数据交互。

\- **Universal App（在 ZetaChain 上）：** 是“全链（Omnichain）”连接的。它可以：

\- **接收**来自任何连接链（如 Ethereum, BNB, Bitcoin, Solana 等）的代币和数据调用。

\- **发送**代币和指令去任何连接链。

**一句话总结：**

它就像一个\*\*中央指挥官\*\*。用户只需要在任何一条链上发起一个请求，Universal App 就能在 ZetaChain 上处理逻辑，然后指挥其他链上的资产进行流转。

\---

\### 核心技术机制：它是如何工作的？

\#### A. Universal EVM（通用以太坊虚拟机）

ZetaChain 运行的是一个兼容 EVM（以太坊虚拟机）的环境，但它做了扩展。

\- **意味着什么？** 如果你会写 Solidity（以太坊的编程语言），你就会写 ZetaChain 的应用。你现有的代码可以直接部署。

\- **全链能力：** 通过引入特殊的接口（UniversalContract），你的合约就获得了“监听”和“控制”其他链的能力。

\#### B. Gateway Contracts（网关合约）

这是连接 ZetaChain 和外部世界（如 Bitcoin, Ethereum）的桥梁。

\- **在外部链上：** 每条连接的链（比如 Ethereum）上都有一个 ZetaChain 的“网关合约”。

\- **交互流程：** 用户不需要直接去连接 ZetaChain，他们只需要向自己所在链（比如 Ethereum）的“网关合约”发送资产或数据。网关合约会把这些信息打包，传送到 ZetaChain 上的 Universal App。

\#### C. onCall 函数（核心代码逻辑）

文档给出了一个代码示例，其中最重要的是 onCall 函数。这是 Universal App 的“入口”。

当有人在外部链调用网关时，ZetaChain 上的合约会自动触发这个函数：

Solidity

\`\`\`

function onCall(

MessageContext calldata context, // 上下文（谁调用的，从哪条链来的）

address zrc20, // 传入的代币类型（在 ZetaChain 上的映射地址）

uint256 amount, // 代币数量

bytes calldata message // 附加信息（比如“我要买NFT”或“转账给Bob”）

) external virtual override { ... }

\`\`\`

\- **深入理解：** 你可以在这个函数里写任何逻辑，比如“如果收到的是 BTC，就把它换成 ETH，然后发给 Alice”。

\---

\### ZRC-20 标准：解决跨链资产的碎片化

这是 ZetaChain 最具创新性的地方之一。

\- **传统跨链（桥）：** 通常会生成“包装代币”（Wrapped Token），比如在以太坊上的 WBTC。这会导致资产碎片化，风险也高。

\- **ZetaChain 的做法 (ZRC-20)：**

\- 当外部资产（如 Ethereum 上的 ETH 或 Bitcoin 上的 BTC）进入 ZetaChain 系统时，它们会被映射为 **ZRC-20 代币**。

\- **关键点：** ZRC-20 代币不仅兼容 ERC-20 标准（可以像普通代币一样交易、借贷），而且它们\*\*拥有“提现”回原生链的能力\*\*。

\- 如果你把 ZRC-20 的 ETH 转入一个特殊的燃烧地址或调用提现功能，ZetaChain 会自动在 Ethereum 主网上把真正的 ETH 转给目标地址。

\---

\### 两个杀手级特性

\#### A. 比特币智能合约支持 (Bitcoin Support)

Bitcoin 本身不支持复杂的智能合约。但是，通过 ZetaChain：

\- 用户可以在 Bitcoin 网络上发送 BTC 到网关。

\- ZetaChain 上的 Universal App 接收到 BTC（作为 ZRC-20 BTC），执行复杂的逻辑（比如 DeFi 借贷、Swap）。

\- 结果可以再转回 Bitcoin 网络，或者变成 ETH 发到以太坊网络。

\- **意义：** 这实际上赋予了 Bitcoin 智能合约的能力，而不需要改变 Bitcoin 协议本身。

\#### B. Gas 抽象 (Gas Abstraction)

这是跨链开发中最头疼的问题：通常用户在 A 链操作 B 链，需要同时拥有 A 链和 B 链的 Gas 代币（手续费）。

\- **ZetaChain 的方案：** 用户只需要支付一种代币（起点的代币）。

\- **后台逻辑：** Universal App 可以自动把用户传入的一小部分代币（比如 USDC），在 ZetaChain 内部兑换成目标链需要的 Gas 代币（比如 BNB），然后帮用户支付目标链的手续费。

\- **用户体验：** 用户只签一次名，付一种币，剩下的全自动完成。
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->


1.  注册了Qwen账号，尝试了Qwen官方的在线调试请求，申请了api key，返回Access Denied
    
2.  按照Zetachain官方文档开始学习Zetachain CLI，卡在了forge build上。
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->



1.  了解本次共学的计划
    
2.  本地安装了zetachain并运行
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
