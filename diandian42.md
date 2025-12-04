---
timezone: UTC+8
---

# diandian

**GitHub ID:** diandian42

**Telegram:** @diandian

## Self-introduction

学习AI和web3的新人

## Notes

<!-- Content_START -->
# 2025-12-04
<!-- DAILY_CHECKIN_2025-12-04_START -->
# **day8**

## **一、AI + 区块链核心定位（千问 3Engine 角色）**

1.  核心身份：**意图识别层**，充当自然语言与链上操作的 “翻译官”，将用户非结构化需求（如 “转 10 USDC 到 BSC”）转化为标准化执行指令。
    
2.  核心逻辑：通过定制角色（Role）+ 系统提示（SP）引导 AI 输出 JSON 格式结果，由分发器解析意图并调用对应功能模块，自动化复杂操作流程。
    
3.  核心价值：降低区块链使用门槛，无需手动切换网络、填写参数，简化链上交互。
    

## **二、技术实现核心要点**

### **1\. 核心调用流程**

用户自然语言指令 → 千问 3 API 处理（Prompt 含角色 / 参数 /temperature）→ JSON 格式化返回 → 分发器解析意图 → 执行前校验（网络 / 地址 / 余额）→ 链上操作（MetaMask 签名）。

### **2\. 技术栈选型**

-   前端：React + Tailwind CSS，集成 MetaMask 实现钱包连接与交易签名；
    
-   区块链：基于 ZetaChain 实现跨链，核心依赖 Gateway 合约（资产锁定 / 铸造）；
    
-   数据存储（拓展方向）：MongoDB 用于持久化交易记录与历史数据。
    

## **三、核心功能实操逻辑**

### **1\. 链内转账**

用户发起指令 → AI 解析 Intent → 分发器调用转账函数 → MetaMask 签名 → 链上执行，全程自动化校验。

### **2\. 跨链转账（以 ZetaChain→Polygon/BSC 为例）**

-   两步核心：原链（ZetaChain）用户操作锁定资产 → 验证器确认 → 目标链（BSC）自动铸造对应 ZRC-20 资产；
    
-   核心依赖：ZetaChain 跨链机制，通过 Gateway 合约与验证器保障资产安全与状态同步。
    

## **四、项目拓展与优化方向**

1.  功能拓展：支持更多 Token、公链，新增提现（withdraw）、兑换（swap）等 DeFi 意图；
    
2.  体验优化：前端异步状态提示、交易状态轮询、多语言界面、一键添加链主网；
    
3.  技术完善：分发器新增意图分支、异常处理（滑点保护 / 余额不足提示）、请求重试机制。
    

## **五、关键技术依赖与参考**

-   钱包交互：MetaMask API（添加链、签名交易）；
    
-   区块链：ZetaChain 文档（ZRC-20 标准、跨链教程、Explorer API）；
    
-   AI 层：千问 3 模型调优（固定 temperature 保障输出标准化）。
<!-- DAILY_CHECKIN_2025-12-04_END -->

# 2025-12-02
<!-- DAILY_CHECKIN_2025-12-02_START -->

## **一、ZetaChain 通用 DeFi 核心模式梳理**

基于 ZetaChain 「原生跨链能力 + 通用 EVM+Gateway 统一入口」的技术优势，通用 DeFi 模式的核心是**打破单链资产割裂，实现多链要素的无缝组合与自动化交互**，以下为 5 类高潜力模式：

### **1\. 跨链流动性聚合与 DEX**

**核心逻辑**

通过 ZetaChain 连接 Ethereum、BSC、Solana 等链的流动性池，为用户提供「一站式跨链兑换」体验 —— 无需切换钱包 / 网络，单次操作即可完成多链资产交换（如 BTC→ETH→USDC）。

**ZetaChain 赋能点**

-   **Gateway 统一接口**：抽象多链协议差异，开发者无需适配各链 DEX 接口，通过单一 API 调用多链流动性；
    
-   **ZRC-20 资产映射**：将 BTC、SOL 等非 EVM 资产映射为 ZRC-20，实现与 EVM 链资产的直接兑换；
    
-   **原子跨链交易**：依托 ZetaChain 共识层保障跨链兑换的原子性，避免 “单边到账” 风险。
    

**应用案例参考**

-   通用 DEX 聚合器：整合 Uniswap（Ethereum）、PancakeSwap（BSC）、Raydium（Solana）的流动性，为用户筛选最优兑换路径；
    
-   跨链限价单：用户设置 “在 Base 链用 10 USDC 买 ETH”，系统自动监测多链价格，触发时通过 Gateway 完成跨链交易。
    

### **2\. 多链抵押借贷协议**

**核心逻辑**

允许用户用「任意链的资产」作为抵押，在「任意链借出资产」（如用 Bitcoin 抵押，在 Polygon 借出 USDC），实现跨链资产的价值互通。

**ZetaChain 赋能点**

-   **跨链抵押品管理**：通过 Gateway 锁定源链资产（如 BTC 存入 Bitcoin 链 TSS 地址），同步在 ZetaChain 生成对应 ZRC-20 抵押凭证；
    
-   **多链清算自动化**：当抵押率不足时，系统可跨链处置抵押品（如将 BSC 上的抵押 BNB 直接兑换为 USDC 偿还债务）；
    
-   **无需跨链桥**：原生支持非 EVM 资产（如 BTC、SOL）作为抵押品，避免传统跨链桥的资产托管风险。
    

**应用案例参考**

-   通用抵押借贷平台：用户存入 BTC（Bitcoin 链），可选择在 Ethereum 借出 ETH、在 BSC 借出 BUSD，利息统一以 ZRC-20 代币结算。
    

### **3\. AI 驱动的跨链收益聚合器**

**核心逻辑**

结合 AI 策略与 ZetaChain 跨链能力，自动为用户在多链中寻找高收益机会（如流动性挖矿、Staking、套利），并完成跨链资金调度与复投。

**ZetaChain 赋能点**

-   **跨链资金一键调度**：通过 `withdrawAndCall` 接口，单次交易即可完成 “从 Ethereum 提取 USDC→ZetaChain 兑换为 BNB→BSC 参与挖矿” 全流程；
    
-   **多链收益数据统一接入**：Gateway 简化多链协议（如 Aave、Curve）的接口调用，AI 可实时抓取多链收益数据；
    
-   **风险对冲跨链执行**：AI 识别某链风险时，可快速将资产跨链转移至低风险链（如从 Arbitrum 转移至 Ethereum 主网）。
    

**应用案例参考**

-   Amana（官方生态项目）：用户存入 USDC 后，AI 自动分析多链 APY，跨链参与最优挖矿池，收益定期自动复投。
    

### **4\. 跨链稳定币与套利系统**

**核心逻辑**

基于 ZetaChain 多链资产价格一致性，发行「全链稳定币」（抵押多链资产），或构建「跨链无风险套利」工具，捕捉多链资产价差。

**ZetaChain 赋能点**

-   **多链抵押品背书**：稳定币可同时抵押 Ethereum 的 ETH、BSC 的 BNB、Solana 的 SOL，抵押物跨链托管，抗单链风险能力更强；
    
-   **实时跨链价格同步**：ZetaChain 作为多链信息枢纽，为套利系统提供实时、统一的多链价格数据；
    
-   **套利交易低成本执行**：跨链 Gas 费通过 ZRC-20 支付，且无需多次跨链操作，套利成本比传统桥接低 30%+。
    

### **5\. 通用跨链保险协议**

**核心逻辑**

为用户的多链资产（如 NFT、DeFi 仓位、跨链转账）提供「一站式保险服务」，覆盖跨链交易失败、单链协议漏洞、资产托管风险等场景。

**ZetaChain 赋能点**

-   **跨链风险统一承保**：无需在各链单独购买保险，一次投保即可覆盖多链资产风险；
    
-   **理赔跨链自动执行**：当触发理赔条件（如跨链转账失败），Gateway 自动将理赔金跨链发放至用户指定链的地址；
    
-   **通用 NFT 保险适配**：支持为 ZetaChain 通用 NFT（跨链流通）投保，解决传统 NFT 保险仅覆盖单链的问题。
    

## **二、黑客松方向提案（通用 DeFi + AI）**

结合 ZetaChain 技术优势与 Qwen AI 能力，聚焦「降低用户门槛 + 提升收益效率」，提出 2 个高落地性方向：

### **方向 1：AI 驱动的多链资产智能管家（目标赛道：通用 DeFi）**

**1\. 目标用户**

-   普通 DeFi 用户：不懂多链操作、没时间筛选收益机会；
    
-   中小机构：需要跨链资产配置与风险对冲，但缺乏技术团队。
    

**2\. 解决的核心问题**

-   多链操作复杂：用户需切换钱包、使用跨链桥、手动跟踪多链收益，门槛高；
    
-   收益效率低：难以实时捕捉多链动态收益机会（如某链突发高 APY 活动）；
    
-   风险不可控：单链黑天鹅事件（如协议漏洞）可能导致资产归零，缺乏跨链风险对冲工具。
    

**3\. 核心功能**

| 功能模块 | 实现逻辑 |
| --- | --- |
| AI 需求意图解析 | 用户输入自然语言（如 “用 1000 USDC 做 3 个月稳健投资，风险低”），Qwen AI 提取关键参数（金额、周期、风险偏好），生成结构化投资策略； |
| 多链资产自动配置 | 基于策略，系统通过 ZetaChain Gateway 跨链调度资产：- 稳健部分：跨链存入多链借贷协议（如 Aave 多链池）；- 收益部分：跨链参与高 APY 挖矿（如 BSC 机枪池）； |
| 实时风险监控与调整 | AI 实时监控多链协议风险（如某链 APY 骤降、协议漏洞预警），自动触发跨链资产转移（如从风险链转移至安全链）； |
| 跨链收益可视化 | 前端展示多链资产分布、实时收益、历史收益曲线，支持一键赎回至用户指定链。 |

**4\. 技术栈**

-   **ZetaChain 层**：Gateway（跨链调用）、ZRC-20（资产映射）、Universal 合约（多链资产管理）；
    
-   **AI 层**：Qwen 大模型（意图解析、风险分析）、Qwen-Agent（工具调用，触发跨链交易）；
    
-   **开发工具**：Foundry（合约开发）、Hardhat（测试）、ZetaChain CLI（Localnet 调试）。
    

**5\. 创新点**

-   **“自然语言→跨链执行” 全流程自动化**：用户无需理解任何 DeFi 术语或跨链概念，仅需输入需求；
    
-   **多链风险对冲 AI 模型**：基于 ZetaChain 多链数据，AI 动态调整资产跨链分布，抗单链风险；
    
-   **零门槛跨链体验**：无需安装多链钱包、无需手动跨链，所有操作通过单一界面完成。
    

### **方向 2：跨链 Meme-Fi AI 孵化平台（目标赛道：通用 DeFi + 通用 AI 应用）**

**1\. 目标用户**

-   Meme 爱好者：想发行 Meme 代币，但缺乏技术能力与跨链流通渠道；
    
-   小开发者：想快速验证 Meme-Fi 想法，但难以解决多链流动性与社区运营问题。
    

**2\. 解决的核心问题**

-   Meme 代币跨链流通难：传统 Meme 代币仅在单链流通，难以触达多链用户；
    
-   技术门槛高：发行代币、开启流动性、跨链部署需专业开发能力；
    
-   社区运营效率低：难以实时捕捉多链社区情绪，内容创作与传播成本高。
    

**3\. 核心功能**

| 功能模块 | 实现逻辑 |
| --- | --- |
| AI 驱动 Meme 代币生成 | 用户输入 Meme 主题（如 “猫币”），Qwen AI 自动生成：- 代币名称、符号、Logo；- 通用 Token 合约（基于 ZetaChain 通用标准，支持跨链流通）；- 白皮书与社区传播文案； |
| 跨链流动性一键开启 | 用户支付少量 USDC 后，系统通过 ZetaChain Gateway：- 在 Ethereum、BSC、Polygon 自动创建流动性池；- 生成跨链交易链接，用户可分享至多链社区； |
| AI 社区情绪与运营 | Qwen AI 实时爬取多链社区（Twitter、Discord、中文社群）情绪，生成：- 动态运营策略（如某链社区热度高，推送跨链挖矿活动）；- 多语言社区内容（自动翻译为英文、中文、韩语）； |
| 跨链收益自动分发 | Meme 代币交易手续费按比例跨链分发至持有者地址（支持用户指定接收链），AI 自动计算并执行分发。 |

**4\. 技术栈**

-   **ZetaChain 层**：通用 Token 标准（无需许可跨链流通）、Gateway（跨链流动性部署）、Universal 合约（手续费跨链分发）；
    
-   **AI 层**：Qwen 大模型（内容生成、情绪分析）、Qwen-Agent（社区运营工具调用）；
    
-   **前端层**：React + Ethers.js，连接 MetaMask 即可操作（无需多链钱包）。
    

**5\. 创新点**

-   **Meme-Fi 全流程 AI 自动化**：从代币生成到跨链运营，开发者无需写一行代码；
    
-   **跨链 Meme 流量聚合**：打破单链 Meme 圈层，快速触达多链用户，提升代币流动性；
    
-   **情绪驱动的动态运营**：AI 实时调整运营策略，解决 Meme 项目 “生命周期短” 的痛点。
    

## **三、方向落地优势与适配性**

1.  **技术适配性**：均基于 ZetaChain 原生跨链能力（Gateway、ZRC-20、Universal 合约），无需额外开发跨链桥，落地成本低；
    
2.  **AI 融合深度**：Qwen AI 不仅是 “辅助工具”，而是核心流程的驱动者（如意图解析、策略生成、情绪分析），符合黑客松 “通用 AI 应用” 赛道要求；
    
3.  **生态契合度**：两个方向均属于 ZetaChain 官方重点扶持的 “通用 DeFi” 领域，且解决真实用户痛点（降低门槛、提升效率），易获得生态资助。
    

可优先推进「AI 多链资产智能管家」，其用户基数更大、需求更刚性，且可复用前期学习的 ZRC-20 余额查询、跨链调用逻辑，开发周期更短。
<!-- DAILY_CHECKIN_2025-12-02_END -->

# 2025-12-01
<!-- DAILY_CHECKIN_2025-12-01_START -->


### **1\. ZRC-20 标准：跨链资产的 “通用接口”**

**（1）定义与核心定位**

ZRC-20 是 ZetaChain 专为跨链资产设计的代币标准，**兼容 ERC-20 但扩展了跨链能力**，是连接不同链原生资产与 ZetaChain 全链生态的桥梁（参考[ZRC-20 官方文档](https://www.zetachain.com/docs/developers/evm/zrc20/)）。

**（2）核心特性**

-   **跨链映射**：将以太坊、BNB 链等链的原生资产（如 ETH、USDC）映射为 ZetaChain 上的 ZRC-20 代币，实现 “一币跨多链”；
    
-   **无需信任的转换**：通过 ZetaChain 的 Gateway 合约实现原生资产与 ZRC-20 的双向兑换，无需中心化中介；
    
-   **兼容 ERC-20**：保留 ERC-20 的核心接口（`transfer`/`approve`/`balanceOf`），开发者可无缝迁移 ERC-20 合约逻辑；
    
-   **Gas 费支付**：支持用 ZRC-20 代币支付跨链交易 Gas 费，简化用户操作。
    

**（3）与 ERC-20 的关键差异**

| 特性 | ERC-20 | ZRC-20 |
| --- | --- | --- |
| 适用范围 | 单链（如以太坊） | 全链（多链互通） |
| 跨链能力 | 无原生跨链支持 | 原生支持跨链转账 / 交互 |
| 资产映射 | 仅单链内资产 | 多链原生资产映射 |

**（4）实践关联（结合前期项目）**

在 “call” 项目中，跨链传递的 USDC 实际是**ZRC-20 版本的 USDC.ETH**（Localnet 预部署地址：`0x1fA02b2d6A771842690194Cf62D91bdd92BfE28d`），通过 ZRC-20 接口实现了从以太坊到 BNB 链的跨链资产转移。

### **2\. Universal 资产：全链资产的 “统一表示层”**

**（1）定义与设计理念**

Universal 资产（Universal Token/NFT）是 ZetaChain 提出的全链资产标准，**打破单链资产的边界**，让资产在任意链上都能以统一形式存在和交互。

**（2）核心价值**

-   **统一身份**：一个 Universal Token 可同时在以太坊、BNB 链、Solana 等链上被识别和操作，无需重复发行；
    
-   **原生跨链交互**：资产持有者可直接在任意链上调用合约、参与 DeFi，无需通过桥接器跨链；
    
-   **简化开发**：开发者无需为不同链适配不同资产标准，基于 Universal 标准即可实现全链资产逻辑。
    

**（3）实现机制**

-   **ZetaChain 作为 “中枢”**：所有 Universal 资产的状态由 ZetaChain 维护，确保多链数据一致性；
    
-   **Gateway 合约作为 “入口”**：各链的 Gateway 合约负责将本地资产操作同步到 ZetaChain，再广播到目标链；
    
-   **轻客户端验证**：通过 ZetaChain 的轻客户端在各链验证跨链资产操作的合法性，保证安全性。
    

### **3\. 多链资产在 ZetaChain 的统一表示逻辑**

ZetaChain 通过 “**映射 + 统一状态层**” 实现多链资产的统一管理：

1.  **资产映射**：将各链原生资产（如 ETH、BTC）通过 Gateway 映射为 ZRC-20/Universal Token；
    
2.  **状态统一**：所有资产的余额、交易记录等状态由 ZetaChain 维护，确保多链数据一致；
    
3.  **透明交互**：用户 / 合约在任意链上操作资产时，底层通过 ZetaChain 的跨链消息机制同步状态，上层感知不到链的差异。
<!-- DAILY_CHECKIN_2025-12-01_END -->

# 2025-11-29
<!-- DAILY_CHECKIN_2025-11-29_START -->



# **day5**

## **下载WSL**

-   这个其实就是在Windows系统下面整一个Linux的子系统，我们可以在里面挂载自己的项目
    

## **下载Foundry**

-   这个就比较折磨了，我用命令行下载了好多次，但都差一点，最后干脆下载压缩包，移动到wsl中（\\wsl$\\Ubuntu这个在文件管理器中打开wsl的目录）
    

> 之后就按照官网的步骤运行，一下是我答应的数据：
> 
> **1\. Ethereum (11155112) 链（模拟以太坊测试网）**
> 
> ```
> ┌───────────────┬────────────────────────────────────────────┐
> │ Contract      │ Address                                    │
> ├───────────────┼────────────────────────────────────────────┤
> │ erc20Custody  │ 0xa85233C63b9Ee964Add6F2cffe00Fd84eb32338f │
> ├───────────────┼────────────────────────────────────────────┤
> │ gateway       │ 0x09635F643e140090A9A8Dcd712eD6285858ceBef │
> ├───────────────┼────────────────────────────────────────────┤
> │ zetaConnector │ 0x67d269191c92Caf3cD7723F116c85e6E9bf55933 │
> ├───────────────┼────────────────────────────────────────────┤
> │ zetaToken     │ 0x7a2088a1bFc9d81c55368AE168C2C02570cB814F │
> ├───────────────┼────────────────────────────────────────────┤
> │ USDC.ETH      │ 0x1fA02b2d6A771842690194Cf62D91bdd92BfE28d │
> └───────────────┴────────────────────────────────────────────┘
> ```
> 
> -   `gateway`：跨链网关合约（部署 `Connected.sol` 时需传入这个地址作为构造函数参数）；
>     
> -   `zetaToken`：Zeta 原生代币合约（跨链 Gas 费支付用）；
>     
> -   `USDC.ETH`：测试用的 USDC 代币合约（模拟跨链资产）。
>     
> 
> **2\. ZetaChain (31337) 链（Zeta 主链模拟环境）**
> 
> ```
> ┌───────────────────┬────────────────────────────────────────────┐
> │ Contract          │ Address                                    │
> ├───────────────────┼────────────────────────────────────────────┤
> │ gateway           │ 0xB7f8BC63BbcaD18155201308C8f3540b07f84F5e │
> ├───────────────────┼────────────────────────────────────────────┤
> │ uniswapV2Factory  │ 0xe7f1725E7734CE288F8367e1Bb143E90bb3F0512 │
> ├───────────────────┼────────────────────────────────────────────┤
> │ uniswapV2Router02 │ 0x9fE46736679d2D9a65F0992F2272dE9f3c7fa6e0 │
> ├───────────────────┼────────────────────────────────────────────┤
> │ zetaToken         │ 0x5FbDB2315678afecb367f032d93F642f64180aa3 │
> ├───────────────────┼────────────────────────────────────────────┤
> │ ZRC-20 ETH.ETH    │ 0x2ca7d64A7EFE2D62A725E2B35Cf7230D6677FfEe │
> └───────────────────┴────────────────────────────────────────────┘
> ```
> 
> -   `gateway`：ZetaChain 侧的跨链网关（`Universal.sol` 需调用这个合约实现跨链）；
>     
> -   `uniswap` 合约：模拟去中心化交易，用于测试资产兑换；
>     
> -   `ZRC-20 ETH.ETH`：ETH 映射的 ZRC-20 代币（跨链资产载体）。
>     
> 
> **3\. BNB (98) 链（模拟 BSC 测试网）**
> 
> ```
> ┌───────────────┬────────────────────────────────────────────┐
> │ Contract      │ Address                                    │
> ├───────────────┼────────────────────────────────────────────┤
> │ gateway       │ 0x0E801D84Fa97b50751Dbf25036d067dCf18858bF │
> ├───────────────┼────────────────────────────────────────────┤
> │ zetaToken     │ 0x99bbA657f2BbC93c02D617f8bA121cB8Fc104Acf │
> └───────────────┴────────────────────────────────────────────┘
> ```
> 
> -   模拟 BSC 链环境，用于测试跨链到 BNB 链的场景。
>     
> 
> **4.测试账户信息（核心！部署 / 调用合约用）**
> 
> ```
> [LOCALNET] EVM default wallet address: 0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266
> [LOCALNET] EVM default wallet private key: 0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80
> [LOCALNET] Default wallet mnemonic: test test test test test test test test test test test junk
> ```
> 
> -   **地址**：测试用的默认账户（已预充测试币），部署合约时会作为部署者；
>     
> -   **私钥**：部署 / 调用合约的核心凭证（`forge create`/`cast send` 需用这个私钥签名交易）；
>     
> -   **助记词**：备用，可导入钱包（如 MetaMask）连接 Localnet 交互。
>
<!-- DAILY_CHECKIN_2025-11-29_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->




### 1\. 核心认知：Universal App 合约 = “跨链翻译官 + 全链协调器”

它不是传统单链合约的 “升级版”，而是专为 ZetaChain 跨链生态设计的 “多链交互中枢”—— 用一套 Solidity 代码，就能和 Bitcoin、Ethereum、Solana 等所有链通信，核心作用是：

-   统一协议：把不同链的 “语言”（如 EVM 的合约调用、Bitcoin 的 UTXO 转账、Solana 的 Program 交互）转换成 ZetaChain 能识别的统一格式；
    
-   协调流程：跨链交易的 “发起→验证→执行→反馈” 全流程，都由这一套合约调度，不用为每条链写单独的适配逻辑；
    
-   资产映射：自动对接 ZRC-20/Universal Token 标准，让多链资产在合约中能统一计算、流转（如 BTC 和 ETH 直接在合约中完成兑换逻辑）。
    

### 2\. 直观类比（帮你快速代入）

传统单链合约 → 只能服务 “一个城市” 的居民，只懂当地语言；

Universal App 合约 → 能服务 “全球所有城市” 的居民，自带所有语言翻译功能，还能协调不同城市间的物资（资产）、信息（数据）流转，且全程透明可追溯。

### 3\. 关键特点（和单链合约的核心区别）

-   无需链适配：一套代码跑通所有链，不用为 Ethereum 写一个版本、Solana 写另一个版本；
    
-   原生跨链能力：合约能主动发起跨链调用（如在 Ethereum 链触发合约，自动调用 Solana 链的记录功能），不用依赖第三方桥；
    
-   全链状态可见：合约能读取多链的实时数据（如同时获取 Ethereum 的 ETH 价格、Bitcoin 的 BTC 价格），支撑跨链逻辑决策。
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->





### 1\. Universal App（通用应用）是什么？

在 ZetaChain 语境下，**Universal App（通用应用）** 不是传统的手机 / 电脑应用，而是能**跨所有区块链（EVM 链、比特币、Solana、TON 等）原生运行的去中心化应用**，无需为不同链单独开发适配版本。

核心特点：

-   **全链统一逻辑**：基于 ZetaChain 的 Universal EVM（通用 EVM），开发者用 Solidity 写一套合约，就能同时在以太坊、BNB Chain、比特币、Solana 等链上生效，不用针对不同链的虚拟机（如 EVM、Solana VM、Bitcoin Script）改写代码。
    
-   **原生跨链交互**：应用能直接读取 / 操作多链资产（比如比特币 + 以太坊的流动性池）、触发跨链交易（比如用比特币还以太坊贷款），无需通过第三方桥接，体验像单链应用一样流畅。
    
-   **无链感知**：用户使用时不用关心 “当前在哪个链”，比如在比特币链发起的交易，能直接触发 ZetaChain 上的智能合约逻辑，对用户透明。
    

典型场景：

-   跨链 DeFi：支持用比特币参与以太坊的 Uniswap 交易，或用 Solana 代币做 BNB Chain 借贷的抵押品；
    
-   全链 NFT：NFT 能在以太坊、Solana、TON 间自由转移，且所有链上都能验证其所有权；
    
-   跨链支付：用比特币直接给以太坊地址转账，实时到账且无需兑换。
    

### 2\. Gateway 大概做什么？

Gateway（网关）是 ZetaChain 实现**跨链交互的 “统一翻译官” 和 “入口 / 出口”**，连接外部链（如以太坊、比特币）与 ZetaChain 核心网络，解决不同链的协议差异问题，核心作用分三类：

（1）“入链” 处理：接收外部链的交易并传递给 ZetaChain

当用户在外部链（比如以太坊）发起跨链操作（如存款、调用合约）时：

-   用户将资产 / 交易发送到对应链的 Gateway 合约（如以太坊的 GatewayEVM）；
    
-   Gateway 捕获交易事件，由 ZetaChain 的 Observer 节点验证后，将交易信息（资产金额、目标地址、指令）传递到 ZetaChain 核心网络；
    
-   触发后续逻辑（如 mint ZRC-20 代币、执行智能合约）。
    

比如之前的用例中，你向 GatewayEVM 转 ETH，Gateway 就把这笔存款的信息传给 ZetaChain，让 Fungible 模块 mint 对应的 ZRC-20 代币。

（2）“出链” 处理：将 ZetaChain 的指令下发到外部链

当 ZetaChain 需向外部链执行操作（如给比特币地址转币、调用以太坊合约）时：

-   ZetaChain 生成跨链指令，通过 Gateway 转发到目标链的 Gateway 合约 / 模块；
    
-   Gateway 负责将指令转换成目标链能识别的格式（比如比特币的 Script、以太坊的合约调用），并由 TSS 节点签名确保安全；
    
-   在目标链上执行最终操作（如转账、触发合约）。
    

（3）资产与逻辑映射：统一多链交互规则

-   资产映射：将外部链的原生资产（如 BTC、SOL）与 ZetaChain 上的 ZRC-20 代币绑定，让多链资产能在 ZetaChain 上统一管理；
    
-   逻辑适配：把不同链的交易逻辑（如比特币的 UTXO 模型、以太坊的账户模型）转换成 ZetaChain 能处理的统一格式，避免协议冲突。
    

简单说，Gateway 就像 ZetaChain 与外部链之间的 “海关”，负责检查、翻译、传递跨链交易，让原本互不兼容的链能顺畅互通。
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->






# **ZetaChain 跨链调用项目 - 使用分析报告**

## **📋 项目简介**

这是一个 ZetaChain 跨链调用示例项目，演示了如何在 EVM 链、Solana 和 Sui 之间进行跨链操作，包括跨链调用、资产转移和错误处理。

## **🏗️ 项目结构**

```
call/
├── contracts/          # Solidity 智能合约
│   ├── Connected.sol   # 外部链合约（EVM）
│   └── Universal.sol   # ZetaChain 中心合约
├── commands/           # CLI 命令行工具
│   ├── connected/      # Connected 合约操作
│   ├── universal/      # Universal 合约操作
│   └── deploy.ts       # 部署脚本
├── solana/             # Solana 链代码
├── sui/                # Sui 链代码
└── test/               # 测试用例
```

## **🚀 快速开始**

### **1\. 环境准备**

```
# 安装依赖
yarn install
​
# 编译合约
npx hardhat compile
```

### **2\. 部署合约**

**部署 Universal 合约（ZetaChain）**

```
npx tsx commands/index.ts deploy \
  --name Universal \
  --rpc https://zetachain-athens-evm.blockpi.network/v1/rpc/public \
  --private-key YOUR_PRIVATE_KEY \
  --gateway 0x6c533f7fe93fae114d0954697069df33c9b74fd7
```

**部署 Connected 合约（外部 EVM 链）**

```
npx tsx commands/index.ts deploy \
  --name Connected \
  --rpc YOUR_EVM_RPC_URL \
  --private-key YOUR_PRIVATE_KEY \
  --gateway GATEWAY_ADDRESS
```

## **💡 核心功能使用**

### **一、Connected 合约操作（外部链 → ZetaChain）**

**1\. 跨链调用（Call）**

从外部 EVM 链调用 ZetaChain 上的 Universal 合约：

```
npx tsx commands/index.ts connected call \
  --contract CONNECTED_CONTRACT_ADDRESS \
  --receiver UNIVERSAL_CONTRACT_ADDRESS \
  --types '["string"]' \
  --values '["Hello ZetaChain"]' \
  --rpc YOUR_EVM_RPC_URL \
  --private-key YOUR_PRIVATE_KEY
```

**功能说明：**

-   从外部链发起跨链调用
    
-   调用 ZetaChain 上的 Universal 合约的 `onCall()` 函数
    
-   不涉及资产转移，仅传递消息
    

**2\. 存款（Deposit）**

将原生代币（如 ETH）存入 ZetaChain：

```
npx tsx commands/index.ts connected deposit \
  --contract CONNECTED_CONTRACT_ADDRESS \
  --receiver UNIVERSAL_CONTRACT_ADDRESS \
  --amount 0.1 \
  --rpc YOUR_EVM_RPC_URL \
  --private-key YOUR_PRIVATE_KEY
```

**功能说明：**

-   将原生代币转换为 ZRC20 代币
    
-   代币会出现在 ZetaChain 上的 Universal 合约地址
    
-   支持原生代币和 ERC20 代币
    

**3\. 存款并调用（Deposit and Call）**

存款的同时执行跨链调用：

```
npx tsx commands/index.ts connected deposit-and-call \
  --contract CONNECTED_CONTRACT_ADDRESS \
  --receiver UNIVERSAL_CONTRACT_ADDRESS \
  --amount 0.1 \
  --types '["string"]' \
  --values '["Hello"]' \
  --rpc YOUR_EVM_RPC_URL \
  --private-key YOUR_PRIVATE_KEY
```

**功能说明：**

-   一次性完成存款和调用操作
    
-   适合需要同时转移资产和执行逻辑的场景
    

### **二、Universal 合约操作（ZetaChain → 外部链）**

**1\. 跨链调用（Call）**

从 ZetaChain 调用外部链上的 Connected 合约：

```
npx tsx commands/index.ts universal call \
  --contract UNIVERSAL_CONTRACT_ADDRESS \
  --receiver CONNECTED_CONTRACT_ADDRESS \
  --zrc20 ZRC20_TOKEN_ADDRESS \
  --types '["string"]' \
  --values '["Hello EVM"]' \
  --function "hello(string)" \
  --call-options-gas-limit 500000 \
  --rpc ZETACHAIN_RPC_URL \
  --private-key YOUR_PRIVATE_KEY
```

**参数说明：**

-   `--zrc20`: 用于支付 gas 费的 ZRC20 代币地址
    
-   `--function`: 可选，指定要调用的函数（任意调用模式）
    
-   `--call-options-is-arbitrary-call`: 启用任意函数调用
    

**2\. 提款（Withdraw）**

从 ZetaChain 提款到外部链：

```
npx tsx commands/index.ts universal withdraw \
  --contract UNIVERSAL_CONTRACT_ADDRESS \
  --receiver CONNECTED_CONTRACT_ADDRESS \
  --amount 0.1 \
  --zrc20 ZRC20_TOKEN_ADDRESS \
  --rpc ZETACHAIN_RPC_URL \
  --private-key YOUR_PRIVATE_KEY
```

**功能说明：**

-   将 ZRC20 代币转换回原生代币
    
-   代币会发送到目标链上的指定地址
    
-   自动处理 gas 费（可能使用不同的代币）
    

**3\. 提款并调用（Withdraw and Call）**

提款的同时执行跨链调用：

```
npx tsx commands/index.ts universal withdraw-and-call \
  --contract UNIVERSAL_CONTRACT_ADDRESS \
  --receiver CONNECTED_CONTRACT_ADDRESS \
  --amount 0.1 \
  --zrc20 ZRC20_TOKEN_ADDRESS \
  --types '["string"]' \
  --values '["Hello"]' \
  --call-options-gas-limit 500000 \
  --rpc ZETACHAIN_RPC_URL \
  --private-key YOUR_PRIVATE_KEY
```

## **⚙️ 高级选项**

### **错误处理配置**

所有命令都支持错误处理选项：

```
--call-on-revert              # 是否在回滚时调用 onRevert
--revert-address ADDRESS      # 回滚处理合约地址
--abort-address ADDRESS       # 中止处理合约地址
--revert-message "message"    # 自定义回滚消息
--on-revert-gas-limit 500000  # 回滚调用的 gas 限制
```

### **示例：带错误处理的调用**

```
npx tsx commands/index.ts connected call \
  --contract CONNECTED_ADDRESS \
  --receiver UNIVERSAL_ADDRESS \
  --types '["string"]' \
  --values '["Test"]' \
  --call-on-revert \
  --revert-address CONNECTED_ADDRESS \
  --revert-message "Custom error message" \
  --rpc RPC_URL \
  --private-key PRIVATE_KEY
```

## **🔄 典型使用流程**

### **场景 1：跨链消息传递**

```
1. 部署 Connected 合约到外部链（如 Ethereum）
2. 部署 Universal 合约到 ZetaChain
3. 从外部链调用：connected call → Universal.onCall()
4. 从 ZetaChain 调用：universal call → Connected.onCall()
```

### **场景 2：跨链资产转移**

```
1. 存款：connected deposit → 资产转换为 ZRC20
2. 在 ZetaChain 上使用资产
3. 提款：universal withdraw → ZRC20 转换回原生代币
```

### **场景 3：跨链 DeFi 操作**

```
1. 存款并调用：connected deposit-and-call
   → 资产转移到 ZetaChain + 执行 DeFi 操作
2. 提款并调用：universal withdraw-and-call
   → 资产转回外部链 + 执行最终操作
```

## **🧪 运行测试**

```
# 使用 Foundry 运行测试
forge test --match-path "test/CallTest.t.sol" -vvv

# 测试覆盖的功能：
# - EVM ↔ ZetaChain 双向调用
# - 原生代币和 ERC20 存款
# - 提款功能
# - 错误处理和回滚
```

## **📝 注意事项**

NaN.  **Gas 费处理**
      
      -   Universal 合约操作需要 ZRC20 代币支付 gas 费
          
      -   系统会自动计算并处理 gas 费
          
NaN.  **地址格式**
      
      -   EVM 链：使用 0x 开头的地址
          
      -   Solana：使用 base58 编码的地址
          
      -   Sui：使用 0x 开头的地址（Move 格式）
          
NaN.  **消息编码**
      
      -   使用 ABI 编码传递参数
          
      -   支持任意函数调用（通过 `--function` 参数）
          
NaN.  **错误处理**
      
      -   配置 `onRevert` 处理调用失败
          
      -   配置 `onAbort` 处理中止场景
          
      -   支持自定义回滚消息
          

## **🔗 相关资源**

-   [ZetaChain 官方文档](https://www.zetachain.com/docs)
    
-   [教程链接](https://www.zetachain.com/docs/developers/tutorials/call)
    
-   协议合约：`@zetachain/protocol-contracts`
    

## **📊 命令总结**

| 操作 | 命令 | 方向 | 说明 |
| --- | --- | --- | --- |
| 调用 | connected call | 外部链 → ZetaChain | 纯消息传递 |
| 存款 | connected deposit | 外部链 → ZetaChain | 资产转移 |
| 存款+调用 | connected deposit-and-call | 外部链 → ZetaChain | 资产+消息 |
| 调用 | universal call | ZetaChain → 外部链 | 纯消息传递 |
| 提款 | universal withdraw | ZetaChain → 外部链 | 资产转移 |
| 提款+调用 | universal withdraw-and-call | ZetaChain → 外部链 | 资产+消息 |

* * *

**提示：** 使用前请确保已正确配置 RPC 节点和私钥，并在测试网络上进行充分测试。
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->







# **ZetaChain EVM 核心工具速查表**

| 工具 / 组件名称 | 核心用途 | 调用 / 使用方式 |
| --- | --- | --- |
| Cosmos SDK + CometBFT + Cosmos EVM | 底层技术栈，提供模块化架构、快速最终性（~4 秒）、全 EVM 兼容性 | 无需手动调用，开发时直接受益（合约无需修改即可部署） |
| ZetaChain CLI | 本地环境搭建、合约部署、跨链交易调用、测试网账号管理 | 终端执行命令（如 npx hardhat localnet npx hardhat account） |
| Localnet | 本地多链模拟环境，测试跨链合约 / 交易（支持 EVM、Solana、Sui 等链） | 通过 CLI 启动（npx hardhat localnet），本地调试无需依赖公网测试网 |
| Gateway 合约 | 跨链交互统一入口（入链 / 出链）：处理存款、跨链调用、资产 mint/burn | 合约调用（如 EVM 链调用 GatewayEVM，ZetaChain 侧调用 GatewayZEVM） |
| ZRC-20 标准 | 跨链代币标准，映射多链资产（如 BTC、SOL），兼容 ERC-20 接口 | 合约开发时遵循标准，通过 Fungible 模块部署或调用已有 ZRC-20 合约 |
| ContractRegistry | 存储协议合约（网关、ZRC-20）元数据和地址，避免调用错误 | 链上查询（通过 RPC 或 Explorer），开发时直接引用注册的合约地址 |
| CrossChain 模块 | 管理跨链交易（CCTX）生命周期，跟踪交易状态（待入链 / 待出链 / 已完成） | 无需手动调用，跨链交易时自动触发，可通过 API/Explorer 查询交易状态 |
| Fungible 模块 | 部署 ZRC-20 代币、管理跨链资产存款和流动性 | 合约调用（如存款、 mint 代币），或通过 CLI 间接触发 |
| Observer 模块 | 协调验证者完成跨链事件观测、投票，保障交易安全性 | 底层自动运行，开发者无需干预，仅需关注交易是否通过验证 |
| RPC 节点 | 连接测试网 / 主网，发送交易、查询链上数据（如账户余额、交易记录） | 通过 API 调用（如 Postman、代码中集成），需记录官方 RPC 入口 |
| Faucet 工具 | 领取测试网 ZETA 代币，用于支付跨链交易 Gas 费 | 访问官方 Faucet 页面或通过 CLI 命令（npx hardhat faucet）领取 |
| Explorer 浏览器 | 查询跨链交易状态、合约部署记录、账户明细，用于调试和验证 | 访问官方 Explorer 页面，输入交易哈希 / 合约地址查询 |
| Threshold Signature Scheme（TSS） | 验证者协同签名跨链交易，避免私钥单点风险 | 底层自动执行，开发者无需操作，仅需确保交易通过 TSS 签名验证 |

  
今天刚刚看ZetaChain的文档有点懵，所以想先把涉及到的工具整理一份出来。
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
