---
timezone: UTC+8
---

# xiongruijie

**GitHub ID:** Xiongruijie

**Telegram:** @xxxiongjerry

## Self-introduction

一个偷偷学习会飞飞飞的打工人

## Notes

<!-- Content_START -->
# 2025-11-29
<!-- DAILY_CHECKIN_2025-11-29_START -->
EVM 是 Ethereum Virtual Machine 的缩写，中文常叫“以太坊虚拟机”。

一句话理解：

EVM 就是运行在以太坊网络上的“世界计算机”，它负责执行所有智能合约、记录状态变化，并保证每台节点算出来的结果完全一致。

🔍 拆开看：

| 关键词 | 含义 |

| ------------------- | ------------------------------------- |

| **Ethereum** | 指以太坊这条公链。 |

| **Virtual Machine** | 虚拟机，相当于一台用软件模拟出来的“裸机”，有独立的 CPU、内存、堆栈。 |

| **去中心化** | 全球几千个节点各自跑一份 EVM，自动保持同步，谁也别想单方面改结果。 |

🧠 把 EVM 想成一台“透明收银机”

你把一段代码（智能合约）放进去，它按规则自动算账。

每一步操作都要付汽油费（Gas），防止死循环和垃圾交易。

所有节点都独立跑一遍，结果一样才写进区块，保证可验证、不可篡改。

✅ 为什么大家天天喊“EVM 兼容”？

因为 EVM 已经成为“智能合约操作系统”的事实标准。

链只要实现同样的指令集和状态格式，就能直接跑以太坊上的合约和工具（MetaMask、Solidity、Hardhat 等），开发者零成本迁移。

所以我们看到 BSC、Polygon、Arbitrum、Avalanche C 链、ZetaChain 等都自称“EVM 兼容”。

🛠️ 技术细节一句话带过（可跳过）

栈式虚拟机，字长 256 位，支持 140+ 条 opcode。

每个合约有独立存储（Storage）、内存（Memory）、调用栈（Stack）。

交易触发后，EVM 计算状态转移 → 消耗 Gas → 更新世界状态 → 被打包进区块。

📌 总结

EVM 就是以太坊的“执行引擎”，也是目前区块链行业最主流的“智能合约运行环境”。

懂 EVM，就懂 90% 公链的兼容性逻辑。

公链（Layer-1）

通用定义：独立跑节点、自己出块的底层网络。

ZetaChain 视角：它是一条基于 Cosmos SDK + Tendermint 的 全链互操作 Layer-1，不仅管自己，还顺带把比特币、以太坊、BSC 等 40+ 链的账本同步到同一套共识里，所以叫“Universal L1”。

EVM / zEVM

通用定义：以太坊虚拟机，负责执行 Solidity 合约。

ZetaChain 视角：内置 Universal EVM（zEVM），合约里可直接调用比特币、狗狗币等“非智能链”资产，开发者一套代码就能多链复用，不用分别部署。

Gas

通用定义：给矿工/验证者的小费，按计算步骤计费。

ZetaChain 视角：无论跨哪条链，统一用 ZETA 代币当汽油；用户侧感受不到“链”的存在，钱包只扣一次 ZETA 就把跨链手续费全包干。

跨链桥

通用定义：把资产 A 从 X 链锁仓、再到 Y 链铸造映射资产。

ZetaChain 视角：官方把自己叫“不是桥”，而是自带跨链能力的城市——资产直接以 ZRC-20 形式活在 ZetaChain 本体，不走外部桥，减少一层合约风险。

ZRC-20

通用定义：ZetaChain 版的 ERC-20 标准。

用途：任何外来代币（BTC、BNB、DOGE...）锁进来都会 1:1 变成 ZRC-20，在 zEVM 里可组合、可 DeFi，提回时再销毁，保证总量刚性锚定。

智能合约

通用定义：链上自动执行的程序。

ZetaChain 视角：合约可以直接“遥控”外部链——例如一个在 zEVM 上的 DEX，可以帮用户把原生 BTC 换成 ETH，合约内部单 tx 完成跨链结算，无需中间商。

质押（Staking）

通用定义：把代币抵押给节点，换出块奖励。

ZetaChain 视角：ZETA 是委托权益证明（DPoS）的核心抵押品，节点质押越多，越有机会出块；质押者可拿 双重收益：出块奖励 + 跨链手续费分成。

链抽象（Chain Abstraction）

通用定义：把“链”的存在藏起来，让用户只关注资产和应用。

ZetaChain 做法：钱包里只显示“你有 1 枚 BTC”——不提醒这条 BTC 此刻是在比特币链还是 ZetaChain 上，转账/DeFi 一键完成，体验接近 Web2 支付。

全链应用（Universal Apps）

通用定义：一套前端+合约，同时服务多链用户。

ZetaChain 实例：官方统计已部署 5,500 个 Universal 合约，用户从任意链接入，流动性在同一块全球状态层里聚合，不再碎片化。

代币经济（Tokenomics）

ZETA 角色三合一：

① Gas 费（链上油费）

② 跨链燃料（多链手续费）

③ 质押保安（节点抵押）

单向锚定机制：A 链销毁 → B 链铸造，总供给恒定，避免多链同时通胀。

Faucet

人话：免费领测试币的水龙头。

场景：在 ZetaChain 上跑 Demo 前，先去官网 Faucet 领 1 个测试 ZETA，否则 Gas 都付不起。

RPC

人话：区块链的“数据线”，钱包/代码通过它读写链上数据。

场景：把 MetaMask 的 RPC 改成 [https://api.athens2.zetachain.com，才能连到](https://api.athens2.zetachain.com，才能连到) ZetaChain 测试网。

Explorer

人话：链上浏览器，用来查交易哈希、合约、区块高度。

场景：交易卡住？把 tx hash 粘进 ZetaScan（ZetaChain Explorer）一看就知道哪一步报错。

Universal EVM / zEVM

人话：ZetaChain 内置的“超级以太坊”，合约里可直接操纵 BTC、DOGE 等非智能链资产。

场景：一份 Solidity 代码里既能调 UniSwap 路由，也能把原生比特币转给别人——不用额外桥。

Gateway

人话：ZetaChain 的“海关”，负责把外部链的消息/资产登记到本链状态。

场景：你在以太坊主网锁了 100 USDC，Gateway 验证事件后在 ZetaChain 上给你 mint 100 ZRC-20 USDC。

Connected Chains

人话：被 ZetaChain 官方接入、支持跨链互操作的公链列表（BTC、ETH、BNB、Solana…）。

场景：只有 Connected 的链才能享受“单 tx 跨链调用”，否则得自己搭桥。

ZRC-20

人话：ZetaChain 版的 ERC-20，外部资产映射后的统一格式。

场景：把 BTC 锁进来 → 拿到 zBTC（ZRC-20），直接进 ZetaChain 上的流动性池子挖矿。

Omnichain Smart Contract

人话：一份合约就能指挥多条链的资产和状态，无需逐链部署。

场景：在 zEVM 里写个“全链 DEX”，用户从任何链下单，流动性却集中在一个合约里。

Universal App

人话：基于 Omnichain 合约做的 dApp，用户端看不到“链”选项，只剩“资产”。

场景：用户输入“把 0.1 BTC 换成 USDC”，Universal App 自动帮你走 BTC→ZRC-20→Swap→转回 ETH 全链路。

Messaging（Cross-Chain Message）

人话：链 A 的事件证明被送到链 B，触发 B 上的合约逻辑。

场景：在以太坊上 burn 掉 50 个 ZRC-20 USDC，Gateway 把消息转给 ZetaChain，ZetaChain 再解锁 50 真实 USDC 到目标地址。

Localnet

人话：你自己电脑里跑的“私有 ZetaChain”，出块秒级，随便刷币。

场景：写合约时先在 Localnet 把逻辑跑通，再部署到测试网，省得烧测试币。

Testnet（Athens-3）

人话：官方公开的“公测网”，水龙头领币零成本，但全球节点共同维护。

场景：课程 Demo 要求“跑通 Swap”，默认就是部署在 Athens-3 测试网。

API Key

人话：调用中心化服务（如 Qwen AI）的“密码”，没有就返回 401。

场景：去阿里云 ModelScope 创建 Qwen 应用，拷贝一串 sk-\*\*\*，贴进代码才能请求大模型。

Agent（Qwen-Agent）

人话：让大模型不仅能聊天，还能调用外部函数（发推、查价格、调合约）的框架。

场景：你给 Agent 注册一个 swapTool，用户说“帮我用 10 USDC 买 DAI”，Agent 自动填参数 → 调 ZetaChain 合约 → 返回 tx hash。

Tool（Function Calling）

人话：Agent 可调用的“外挂函数”，用来把自然语言转成链上操作。

场景：定义 depositToAave(token, amount) 工具，Agent 识别意图后，把用户输入解析成链上 calldata。

Intent Recognition

人话：AI 把“人话”翻译成结构化参数的过程。

场景：用户说“我想存点 USDC 赚利息”，Agent 识别出 action=deposit, token=USDC, amount=MAX, chain=ZetaChain。

Structured DeFi Params

人话：AI 输出给合约的“标准 JSON”，防止手工拼参数出错。

场景：Agent 返回 {tokenIn: "USDC", tokenOut: "DAI", amount: "100000000", chain: "zeta"}，前端直接喂给 Universal Swap 合约。

OpenAI-Compatible API

人话：Qwen 提供跟 OpenAI 一模一样的接口格式，原来调 GPT 的代码改一行 base\_url 就能切到 Qwen。

场景：课程示例用 openai.NodeJS 包，把 baseURL 换成 [https://dashscope.aliyuncs.com/compatible-mode/v1](https://dashscope.aliyuncs.com/compatible-mode/v1) 即可。

Gas Limit / Gas Price

人话：你愿意为这笔交易出的“最大计算量”和“单价”。

场景：ZetaChain 主网 Gas Price 约 0.0001 ZETA，Limit 300 万基本够部署合约；设太低会“Out of Gas”失败。

白名单资产

人话：AI Agent 只允许操作预设的代币合约，防止用户被钓鱼。

场景：在 Agent 的 swapTool 里写死 \[USDC, DAI, WETH, zBTC\]，其他币一律拒绝，减少风险。
<!-- DAILY_CHECKIN_2025-11-29_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->

2025年11月27日22点49分
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->


今天有点麻烦，完成币安账号创建，一直都没创建币安账号，以及水龙头绑定，搭建起来zetachain的基本运行环境。

照着马铃薯大神的笔记做了一遍，主要还是国内环境这些梯子工具不太熟悉。
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->



刚下班，打个卡，现在开始学，待会儿补齐笔记。。。。。
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->




day1

主要了解Zetachain的主要概念，去除链间的隔离，目标是让任何区块链（包括比特币、狗狗币等无智能合约链）都能在同一套开发环境里直接通信、调用资产和消息，而无需传统桥接或封装资产。

哈哈哈哈哈感觉看英文文档有点吃力。
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
