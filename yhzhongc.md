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
# 2025-12-01
<!-- DAILY_CHECKIN_2025-12-01_START -->
今天运行demo，但是没跑通swap项目，没搞懂咋回事，明天再看看，先打个卡
<!-- DAILY_CHECKIN_2025-12-01_END -->

# 2025-11-30
<!-- DAILY_CHECKIN_2025-11-30_START -->

Threshold Signature Scheme，门限签名方案

\### 1. 通俗类比：共管金库的钥匙

想象一下，ZetaChain 的那个“Gateway 金库”里锁着几亿美元的资产。如果只有\*\*一把\*\*私钥能打开它：

\- **传统做法（中心化交易所）：** 这把钥匙由 CEO 保管。如果 CEO 跑路了，或者被绑架了，钱就没了。

\- **TSS 做法（ZetaChain）：**

1\. 这把“钥匙”在生成的瞬间，就被\*\*粉碎\*\*成了 100 个碎片（假设有 100 个验证节点）。

2\. 每个节点只持有 1 个碎片。

3\. **任何单个碎片都无法打开金库**，也无法推算出完整的钥匙是什么。

4\. **规则（门限）：** 想要从金库拿钱，必须凑齐 **2/3 以上** 的碎片（比如 67 个）。

**这就是 TSS：没有单独的管理员，必须大家一起来凑钥匙。**

\### 2. 技术视角：TSS 地址与签名

\#### 什么是 TSS 地址？

在外部链（如 Bitcoin 或 Ethereum）看来，\*\*TSS 地址就是一个极其普通的钱包地址\*\*。

\- **Bitcoin 网络眼中：** “哦，这只是一个普通的地址 bc1q...xyz，不管是谁拥有私钥，只要签名对了我给就转账。”

\- **实际情况：** 这个地址对应的私钥\*\*从来没有在任何一台电脑的内存或硬盘里完整存在过\*\*。它是通过数学算法（MPC，多方计算）虚拟生成并分布存储的。

\#### 什么是 TSS 授权签名？

当 Universal App 决定要从 Gateway 提现 1 BTC 给用户时，流程如下：

1\. **发起提案：** ZetaChain 网络说：“我们要给 Alice 转 1 BTC。”

2\. **独立验证：** 全球各地的验证节点（Nodes）各自检查账本：“这笔交易合法吗？Alice 真的有这么多余额吗？”

3\. **广播碎片：** 节点确认无误后，不会交出自己的碎片，而是利用数学算法进行交互，贡献一次“部分签名”。

4\. **合成签名：** 当凑够了 67% 的节点参与，这些数学数据就会“合成”出一个\*\*最终的、标准的数字签名\*\*。

5\. **上链：** 这个最终签名被发送到 Bitcoin 网络。Bitcoin 网络验证通过，转账完成。

**神奇之处：** Bitcoin 网络根本不知道这是 100 个人商量出来的结果，它只看到一个合法的签名，就像是一个单人用户签的一样。这就是为什么它能兼容不支持智能合约的 Bitcoin

\### 3. 为什么这比多签（Multi-Sig）更厉害？

你可能会问：“这不就是多重签名钱包（Multi-Sig）吗？”

**不是，TSS 比多签更高级、更省钱。**

\- **传统多签 (Multi-Sig)：**

\- 在链上设置：需要在 Bitcoin/Ethereum 上显式地部署一个合约或脚本，规定“需要 A、B、C 三个人的签名”。

\- **缺点：** 所有人都能看到这是个多签钱包；手续费更贵（因为包含了多个人的签名数据）；隐私性差；有些链（如 Solana 或旧版 Bitcoin）对多签支持有限制。

\- **TSS (ZetaChain)：**

\- **链下计算，链上提交：** 所有的“凑碎片”过程都在 ZetaChain 内部网络完成。

\- **最终效果：** 提交给 Ethereum 或 Bitcoin 的只是一串\*\*普通的、单一的签名\*\*。

\- **优点：**

\- **更便宜：** Gas 费和普通转账一样，因为只占用一个签名的空间。

\- **兼容性无敌：** 只要那条链支持数字签名（所有区块链都支持），ZetaChain 就能接入，不需要那条链支持复杂的智能合约。

\### 总结

\- **TSS 地址 =** 一个没有单一主人的“幽灵”地址，由全体节点共同掌管。

\- **TSS 签名 =** 全体节点通过数学魔法，在不暴露各自碎片的情况下，集体签署的一份生效指令。

这也是为什么黑客很难攻击 ZetaChain：他黑掉一两个节点没用，他必须同时攻破全球超过 2/3 的节点，才能凑齐权限挪动资金。

# Universal EVM

ZetaChain 是一个基于 **Cosmos SDK** 和 **CometBFT** 共识引擎构建的 **Proof of Stake (PoS)** 区块链，通过 **Cosmos EVM** 实现了 **完全的 EVM 兼容性**，使得以太坊智能合约无需修改即可在其上本地运行。

1\. 核心架构与角色

ZetaChain 的核心目标是作为\*\*区块链之间的通用连接器\*\*。

• **集线器-辐射式模型 (Hub-and-Spoke)**：ZetaChain 是协调所有跨链活动的\*\*集线器 (Hub)\*\*；外部区块链（包括 EVM 链、Solana、Sui、Ton 和比特币）是连接的\*\*辐射 (Spokes)\*\*。所有跨链消息和交易都通过 ZetaChain，以确保一致性和安全性。

• **验证者角色**：

    ◦ 核心验证者 (Core Validators)：运行 ZetaChain 节点，参与 CometBFT 共识以生产区块并维护状态。

    ◦ 观察者-签名验证者 (Observer-Signer Validators)：运行 ZetaChain 节点和 ZetaClient，监控 ZetaChain 和连接的链以检测跨链事件，投票决定事件的有效性，并使用阈值签名方案 (TSS) 协作签署 出站交易。

2\. 关键模块与合约

ZetaChain 的功能由多个模块和协议合约支持：

• **核心模块**：

    ◦ 跨链模块 (CrossChain Module)：管理跨链交易 (CCTX) 的状态和生命周期，充当跟踪进度的中央账本。

    ◦ 观察者模块 (Observer Module)：处理观察者集合的运作、验证者管理、投票机制和共识策略，并维护授权观察者列表。

    ◦ 可互换模块 (Fungible Module)：管理代表外部链资产的 ZRC-20 代币的部署和流动性。

• **协议合约 (Protocol Contracts)**：

    ◦ Gateway (网关)：是连接链上合约与 ZetaChain 上的通用应用程序交互的统一入口点。连接链上的 Gateway 促进入站交易（存款和合约调用），而 ZetaChain 上的GatewayZEVM促进出站交易（提款和对外部合约的调用）。

3\. 跨链交易 (CCTX) 流程

跨链交易 (CCTX) 是 ZetaChain 的核心功能，分为两类：

• **入站交易 (Connected Chain** → **ZetaChain)**：在连接链上发起，观察者检测事件并投票，多数批准后，相应交易在 ZetaChain 上执行（例如铸造 ZRC-20 或调用合约）。

• **出站交易 (ZetaChain** → **Connected Chain)**：在 ZetaChain 上发起（通过调用 GatewayZEVM），验证者生成出站交易，观察者-签名验证者使用 TSS 协作签名，交易随后在目标连接链上执行。

4\. 资产与费用

• **ZETA 代币**：ZetaChain 的原生代币，作为质押代币和支付交易费用的代币 (denom 是 `azeta`)。

• **ZRC-20 代币**：ZetaChain 上的特殊代币标准，\*\*代表\*\*来自连接区块链的原生 Gas 代币和白名单 ERC-20 代币。这允许开发者将多链上的原生资产作为单个 ZRC-20 代币在 ZetaChain 上进行协调和操作，就像它们都在一条链上一样。

• **费用处理**：

    ◦ 从连接链调用通用应用时，费用以源链的原生 Gas 代币支付。

    ◦ ZetaChain 上的通用应用发起\*\*出站调用或提款\*\*时，需要支付“提款 Gas 费”，费用以目标链的 Gas ZRC-20 代币支付。

5\. 兼容性与安全性

• **地址兼容性**：ZetaChain 既是 Cosmos 链也是 EVM 链，支持 **bech32 Cosmos 地址**（前缀为 `zeta`）和 **Hex EVM 地址**。同一个公钥可以派生出这两种地址，它们代表同一个账户。

• **流动性管理**：为了维护网络稳定性和安全性，ZetaChain 对\*\*入站交易\*\*使用\*\*流动性上限 (Liquidity caps)\*\* (超过上限的交易将被还原)，对\*\*出站交易\*\*使用\*\*速率限制 (Rate limiting)\*\* (确保在指定窗口内总提款量不超过预定限额)。

ZetaChain 通过这种架构，提供了一个单一的平台，用于处理跨链消息、资产转移和合约调用，使开发者能够专注于应用逻辑，而不是多链基础设施的复杂性
<!-- DAILY_CHECKIN_2025-11-30_END -->

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
