---
timezone: UTC+8
---

# Ryan

**GitHub ID:** Appler-R

**Telegram:** @Appler_Ry

## Self-introduction

AI 开发者, Qwen 实战派，看好Web3+ 模块化链
(如 Zetchain)的下一代应用落地。

## Notes

<!-- Content_START -->
# 2025-12-02
<!-- DAILY_CHECKIN_2025-12-02_START -->
Qwen-Agent 是一个开发框架，专门用于构建基于大语言模型（LLM）的智能体应用。它对标 LangChain 等框架，但**针对 Qwen（通义千问）模型的能力（如超长上下文、指令跟随、工具调用）进行了深度优化**。

Qwen-Agent 的核心架构可以拆解为四个主要部分：**LLM（大脑）、Agent（统筹者）、Tools（手脚）、Memory（记忆）**。

![图片.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Appler-R/images/2025-12-02-1764675171958-__.png)

### 跑的小脚本

### 介绍zetachain

![图片.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Appler-R/images/2025-12-02-1764675413714-__.png)

### 转换大小写

![图片.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Appler-R/images/2025-12-02-1764677201350-__.png)
<!-- DAILY_CHECKIN_2025-12-02_END -->

# 2025-12-01
<!-- DAILY_CHECKIN_2025-12-01_START -->


### ZetaChain 上能做的通用 DeFi 模式：

### **全链 DEX (Omnichain Swap)**

**核心**：原生资产直接兑换 (如 BTC ↔ ETH)。

**杀手锏**：**无桥、无封装 (No Wrapped Assets)**。用户体验像单链交易一样丝滑，无需持有中间链 Gas。

**全链借贷 (Omnichain Lending)**

**核心**：多链资产抵押借贷 (如 存入原生 BTC → 借出原生 USDT)。

**杀手锏**：**原生 BTC 抵押品**。清算逻辑在 zEVM 内部毫秒级完成，消除了跨链通讯延迟带来的坏账风险。

**比特币金融 (BTC-Fi)**

**核心**：赋予比特币智能合约能力 (如 BTC 流动性挖矿、BTC 参与 IDO)。

**杀手锏**：**可编程性**。让比特币不离网也能运行复杂逻辑，激活万亿级休眠资产。

**全链稳定币 (Omnichain Stablecoin)**

**核心**：一篮子多链资产抵押 (BTC + ETH + BNB) → 铸造统一稳定币 (zUSD)。

**杀手锏**：**极致资本效率**。生成的稳定币是 Universal Token，可瞬间流转至任何链使用。
<!-- DAILY_CHECKIN_2025-12-01_END -->

# 2025-11-30
<!-- DAILY_CHECKIN_2025-11-30_START -->



## 周记

### 资产双子星：ZRC-20 vs Universal Assets

![PixPin_.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Appler-R/images/2025-11-30-1764507685421-PixPin_.png)

### 全链互操作架构：从 Messaging 到 Swap

![PixPin_.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Appler-R/images/2025-11-30-1764507742446-PixPin_.png)

## idea1

### 全链定投协议 (Omni-DCA)

**目标用户**

-   手里持有大量 **Bitcoin** 的老矿工或囤币党。
    
-   想投资以太坊生态（如 ETH, UNI, AAVE），但不想操作复杂的跨链桥，也不想一次性梭哈，只想**定投**。
    

**想解决的问题**

**操作繁琐**：以前要定投，得每周手动把 BTC 充到交易所，换成 ETH，提现到钱包。

**Gas 费贵**：每周跨链一次，Gas 费直接吃掉定投的利润。

**自动化缺失**：比特币网络本身不支持“每周自动转账”这种智能合约逻辑。

### 粗略的跨链逻辑 (Architecture)

1.  **一次性充值**：用户从 Bitcoin 网络向 ZetaChain 的 `DCA_Contract` 一次性转入 **1 BTC**，并附言：`"每周买 0.1 ETH 发给我的以太坊钱包 0xABC..."`。
    
2.  **状态记录**：ZetaChain 合约收到 ZRC-20 BTC，记录在用户的“定投账户”里。
    
3.  **自动化执行 (Cron Job)**：
    
    -   合约设定一个定时任务（Automation）。
        
    -   **每周一**：合约自动从用户余额里扣除 **ZRC-20 BTC**。
        
    -   **自动换汇**：调用内置 Uniswap 池，把 BTC 换成 **ZRC-20 ETH**。
        
    -   **自动发货**：调用 `ZRC20(ETH).withdraw()`。
        
4.  **结果**：用户躺着不动，每周一他的以太坊钱包里就会自动收到一笔 ETH。
    

通用资产使用方式

**输入资产**：**ZRC-20 BTC** (作为资金源)。

**输出资产**：**ZRC-20 ETH** (作为目标资产，随即被提现)。

**核心价值**：利用 ZetaChain 的合约逻辑（时间控制），赋予了比特币“时间支付”的能力。

## idea2

### 原生比特币抵押稳定币 (Native-BTC Stablecoin)

**—— “不卖币，不封装 wBTC，直接印出全链通用的美元”**

1\. 目标用户

-   急需流动性（USDT/USDC）去抄底或消费，但坚决**不想卖掉手中 BTC** 的死忠粉。
    
-   不信任 wBTC（担心中心化托管商跑路）的用户。
    

2\. 想解决的问题

**信任风险**：在以太坊上借钱通常要抵押 wBTC。如果 BitGo（wBTC 发行商）出事，wBTC 归零，借贷协议崩盘。

**碎片化**：在 A 链借出的稳定币，很难直接去 B 链用。

3\. 跨链逻辑 (Architecture)

-   **超额抵押**：用户在 Bitcoin 网络向 ZetaChain 转入 **1 BTC**。
    
-   **锁定与铸造**：
    
    -   `onCrossChainCall` 触发。
        
    -   合约锁定 **ZRC-20 BTC** 作为抵押品（假设价值 $60,000）。
        
    -   合约根据 50% 抵押率，凭空铸造出 30,000 个 `zUSD` (Universal Token)。
        

**自由流通**：

-   这个 `zUSD` 是 ZetaChain 原生代币。
    
-   用户可以在合约里写逻辑：“把这 30,000 zUSD 发到我的 **Polygon** 钱包去买 NFT”。
    
-   ZetaChain 协议会将 zUSD 销毁，并在 Polygon 上释放等值的稳定币给用户。
    

**还款赎回**：用户还回 30,000 zUSD，合约销毁这些币，然后调用 `withdraw()` 把 1 BTC 原路退回用户的比特币地址。

4\. 通用资产使用方式

**抵押物**：**ZRC-20 BTC** (被锁在 ZetaChain 合约里，而不是被封装)。

**新资产**：**zUSD** (这是一个 **Universal Token**)。

**核心价值**：它创造了一种“全链通用购买力”。你用比特币做抵押，

生成的稳定币可以直接去 Solana、Polygon、Ethereum 任何地方消费。
<!-- DAILY_CHECKIN_2025-11-30_END -->

# 2025-11-29
<!-- DAILY_CHECKIN_2025-11-29_START -->




通过discord领了测试币，上了一下测试链

克服了资金不足、地址格式错误、命令拼接断行等困难，成功将合约部署到 **Athens Testnet**。

### 从哪里发起的调用？(The Origin)

（ WSL 系统

**数字身份：** **MetaMask 钱包**（地址 `0x737FB...dD7B9`）。

虽然钱包是装在浏览器插件里的，但当你把**私钥**粘贴到终端命令里时，

**Foundry (forge)** 这个工具就临时获得了代表你“签字”的权力。

**发送路径是这样的：**

1.  **打包与签名 (Local)**：
    
    -   Foundry 先把你写的 `PiggyBank.sol` 编译成机器能读懂的乱码（Bytecode/字节码）。
        
    -   然后，它用你的**私钥**对这堆乱码进行加密签名。这相当于你在信封上盖了个红章：“我同意支付手续费，请把这段代码存到区块链上。”
        
2.  **投递 (RPC)**：
    
    -   你的电脑通过网络，把这个签好名的“信封”（交易数据）发送给了 **RPC 节点**（`zetachain-athens-evm.blockpi.network`）。
        
    -   RPC 节点就像是一个“邮局”，它收到了你的信，检查签名无误后，把它广播给了 ZetaChain 网络里的验证者节点。
        

![全球.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Appler-R/images/2025-11-29-1764426603073-__.png)

最终在 ZetaChain 上发生了什么？

如下：

```
sequenceDiagram
```

```
    participant User as 你的电脑 (WSL)
```

```
    participant Wallet as 你的私钥 (0x737...)
```

```
    participant RPC as RPC 节点 (BlockPi)
```

```
    participant ZetaChain as ZetaChain 网络 (zEVM)
```

```
    participant Contract as 新合约 (PiggyBank)
```

```
    Note over User, Wallet: 1. 编译代码 & 本地签名
```

```
    User->>Wallet: 调用私钥签名
```

```
    Wallet-->>User: 生成已签名的交易数据(Raw Tx)
```

```
    Note over User, RPC: 2. 广播交易
```

```
    User->>RPC: 发送交易请求
```

```
    RPC->>ZetaChain: 广播给验证者节点
```

```
    Note over ZetaChain, Contract: 3. 上链执行
```

```
    ZetaChain->>ZetaChain: 验证签名 & 扣除 Gas (ZETA)
```

```
    ZetaChain->>Contract: 创建新地址 0xBf68...
```

```
    ZetaChain->>Contract: 写入 PiggyBank 的代码
```

```
    Contract-->>User: 返回合约地址 (部署成功)
```

-   **发起点**：你的**本地终端**（利用私钥签名）。
    
-   **传输者**：**RPC 节点**（互联网上的公共网关）。
    
-   **终点**：**ZetaChain 区块链**。
    
-   **结果**：区块链的账本上多了一行记录——**“地址 0xBf68… 现在属于 PiggyBank 合约，**
    
-   **它的代码逻辑是X（一个会存钱、会取钱、绝对铁面无私的自动柜员机程序）”**
<!-- DAILY_CHECKIN_2025-11-29_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->








## ZetaChain ZRC-20 跨链资产统一与 Swap 流程图

1.  **外部资产入链** — 用户从源链（如 Base、Ethereum）发送资产到 TSS 托管地址（非中心化）。
    
2.  **Shadow Token 铸造** — ZetaChain 观察者检测到入账后，链上验证者共识触发 zEVM 中的 ZRC-20 合约铸造等额影子代币（zETH、zBTC 等）。
    
3.  **zEVM 内 DeFi 操作** — 影子代币在 zEVM 中与 Uniswap、借贷协议等原生交互（完全 ERC-20 兼容），实现无缝 Swap。
    
4.  **跨链提取** — 用户销毁 ZRC-20，TSS 阈值签名解锁原链资产并转账，全程非封装、无中心化托管风险。
    

关键特性：**非封装、TSS 安全、zEVM 兼容、统一表示**。

![ZetaChain_ZRC-20_跨链资产统一与_Swap_流程图.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Appler-R/images/2025-11-28-1764320485189-ZetaChain_ZRC-20_________Swap____.png)

## 多链资产在 ZetaChain 上如何被统一表示？

ZetaChain 通过 **ZRC-20 标准** 来实现对所有外部链资产（如比特币、外部 EVM 链的 ETH、BSC 的 BNB 等）的统一表示。

-   **统一替身（Shadow Tokens）：** ZRC-20 就像外部资产在 ZetaChain 上的 **“影子”**。例如，比特币（BTC）在 ZetaChain 上被表示为 **zBTC**，以太坊上的 ETH 被表示为 **zETH**。
    
-   **zEVM 统一管理：** 所有这些“影子资产”都是 ZRC-20 代币合约，它们与 ZetaChain 的 zEVM 虚拟机完全兼容。这意味着开发者在 ZetaChain 上，可以像操作本地 ERC-20 代币一样，操作来自任何外部链的资产。
    
-   **非封装（Non-Wrapped）：** 这里的 ZRC-20 不是传统的 **Wrapped Token (封装代币)**，它不需要通过锁定中心化托管方的金库来铸造。ZRC-20 的铸造和销毁完全由 ZetaChain 的外部观察者和签名者安全控制。
    

## ZRC-20 和普通 ERC-20 的直观区别（开发者视角）

| 特性 | ZRC-20 (zETH, zBTC) | 普通 ERC-20 (USDC, DAI) |
| 基础功能 | 兼容所有 ERC-20 功能 (transfer, balanceOf) | 具有基础的代币功能 |
| 核心区别 | 额外包含 deposit() 和 withdraw() 接口 | 无跨链接口 |
| 资产来源 | 来自外部区块链（如比特币链、以太坊链） | 仅在部署的链上发行 |
| 直观感受 | 它是资金在外部链的“操作遥控器”。 | 它就是资金本身。 |

### 核心区别在于：`deposit()` 和 `withdraw()`

对于开发者来说，最大的区别就是 ZRC-20 赋予了你直接操控外部资产的权力，而无需处理复杂的跨链消息。

_我的 Swap 合约正是通过调用_ `withdraw()`_，完成了最终的跨链转账。_

### _项目一_

## 全链收益聚合器 (Universal Yield Aggregator)

**痛点：** 比特币持有者 (BTC) 想要赚取以太坊上的 DeFi 收益 (USDC)，通常需要跨链桥、封装、切网络，非常麻烦。

**通用资产方案：** 用户全程只操作 BTC，中间的“换汇、生息”由 ZetaChain 自动完成。

| 步骤 | 角色 | 动作 (Action) | 资金形态变化 (Under the hood) |
| 1. 存款 | 用户 | 在 Bitcoin 网络 向聚合器地址转账 1 BTC。 | Native BTC (比特币链) → ZRC-20 BTC (ZetaChain) |
| 2. 自动策略 | Universal App | 1. 监测到 ZRC-20 BTC 入账。2. 自动在 Uniswap 将 BTC 换成 USDC。3. 将 USDC 存入 Aave 借贷协议。 | ZRC-20 BTC → ZRC-20 USDC → aUSDC (生息资产) |
| 3. 躺赚 | 时间 | 随着时间推移，USDC 产生利息。 | aUSDC 数量增加 |
| 4. 取款 | 用户 | 发起“取回所有资金”指令。 | - |
| 5. 自动结算 | Universal App | 1. 从 Aave 取出本金+利息 (e.g. 1.05 BTC 等值的 USDC)。2. 自动换回 ZRC-20 BTC。3. 调用 withdraw()。 | aUSDC → ZRC-20 USDC → ZRC-20 BTC → Native BTC |
| 6. 到账 | 用户 | 在 Bitcoin 网络 收到 1.05 BTC。 | 用户全程只看到了 BTC 变多了，没感知到中间变成了 USDC。 |

## 全链通用 NFT (Universal NFT Pass)

**痛点：** 项目方发 NFT，不得不选边站（发在以太坊贵，发在 Solana 没人气）。 **通用资产方案：** NFT 部署在 ZetaChain，任何链的用户都能买、都能用。

| 维度 | 传统 NFT | 通用 NFT (Universal NFT) |
| 购买支付 | 必须持有该链代币 (如 ETH) | 全链通吃：用户可以用 BTC、ETH、BNB 或 MATIC 直接支付铸造费用。 |
| 铸造逻辑 | msg.value (仅限本链代币) | 监听 onCrossChainCall，无论收到哪种 ZRC-20 代币，只要价值达标，就给用户 Mint 一个 NFT。 |
| 身份验证 | 用户必须切到以太坊钱包签名 | 用户可以用 Bitcoin 钱包 签名，证明自己持有该 NFT (因为 ZetaChain 连接了所有链)。 |
| 跨链转移 | 需要跨链桥，风险高 | NFT 永远留在 ZetaChain 上不动，只是所有权在变。用户可以将 NFT “发送”到别的链地址，本质是改了映射关系。 |

通用资产就是“以不变应万变”：

资产（ZRC-20）和逻辑（合约）都在 ZetaChain 上不动，但通过连接器（ZRC-20 接口）吸纳各种资金流。
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->









![1.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Appler-R/images/2025-11-27-1764250272995-1.png)

终于克服了依赖缺失和路径配置等重重报错，**成功在本地跑通了最核心的跨链 Swap 业务模拟**。

![图片.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Appler-R/images/2025-11-27-1764258066434-__.png)
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->











### Universal EVM 与 Universal App

_这是 ZetaChain 最具革命性的地方。_

Universal EVM (通用 EVM / zEVM)

### 它是什么：ZetaChain 是一条公链，它的执行层叫 zEVM。

它不仅兼容以太坊（你可以用 Foundry 写代码），

更拥有“上帝视角”，比如你可以通过Zetachain上的Universal App，你能够监控所有网络（Bitcoin、Ethereum等),调用所有代币

即 普通的 EVM 只能读取自己链上的数据。而 zEVM 可以直接读取和写入连接链（Connected Chains）上的状态。

**左手抓来 BTC**：合约感知到用户转入了比特币（ZetaChain 把它映射为 ZRC-20 BTC）。

**原地通过 DEX 交易**：合约在 ZetaChain 内部的流动性池里，直接把 ZRC-20 BTC 换成 ZRC-20 ETH。

-   注意：这一步完全发生在 ZetaChain 内部，速度极快，不需要去比特币或以太坊网络排队确认。
    

**右手递出 ETH**：合约判定交易完成，把 ZRC-20 ETH 销毁，并指示以太坊网络上的 Gateway 给用户转真正的 ETH。

普通的 EVM 只能读取自己链上的数据。而 zEVM 可以直接读取和写入连接链（Connected Chains）上的状态。

比喻：普通公链像是一个只能处理本国货币的“本地银行”；

Universal EVM 是一个“国际中央银行”，能同时看到并操作所有国家的账户。

![图片.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Appler-R/images/2025-11-26-1764164375932-__.png)

**这里的“上帝视角”优势在哪？**

既然都要付钱，ZetaChain 的优势是什么？

优势在于：打包支付 (Gas Abstraction)

传统跨链：你需要钱包里同时有 BTC（付比特币 Gas）、有 ZETA（付中间层 Gas）、有 ETH（付以太坊 Gas）。

没 ETH 你就寸步难行。

**ZetaChain**：你可以只用 BTC。

当你发送 BTC 时，ZetaChain 的智能合约可以在内部自动把一小部分 BTC 换成 ZETA 和 ETH，

帮你把后面所有的路费都交了。

用户体验：我付了 1.0 BTC，收到了相当于 0.99 BTC 的以太坊代币。中间的损耗自动扣除，我不需要关心具体细节。

损耗是物理存在的，无法避免。但在代码层面，**ZetaChain 允许开发者编写逻辑，从用户输入的资产中自动扣除这些费用**，从而让用户感觉不到“不仅要跨链，还得去买各种奇怪的代币当 Gas”的痛苦。

### 我举个例子，比如你要去日本买个手办

**按照传统跨链模式，你需要：**

1.  把你的非日元法币换成日元
    
2.  拿着你的日元去找到a中介，让他给你开一张手办代金券
    
3.  你去日本商店兑换，那么问题来了，商店不受怎么办，那只好乖乖换回日元
    

**ZetaChain 模式**：

1.  你直接去日本商店刷 Visa 卡（美元等）。
    
2.  Visa（ZetaChain）在后台自动计算汇率、扣除手续费、处理结算。
    
3.  **日本商店直接收到了真日元**。
    

### 总结

ZetaChain 的价值在于：

1.  **原生对原生**（不留垃圾封装代币）。避免假币和桥被黑的风险
    
2.  **单点逻辑**（一套代码管全链）。
    
3.  **唤醒比特币**（让 BTC 能跑智能合约）。
    

_省 Gas 费步骤只是因为这个架构太高效了，顺便实现的一个小功能而已。_

### Universal App (通用应用 / Omnichain Smart Contract)

它是什么：这是一种只部署在 ZetaChain 上的智能合约。

### **区别：**

传统跨链：需要在 A 链部署合约，B 链部署合约，中间再搞个桥。

Universal App：“一次部署，全链通用”。你只需要在 ZetaChain 上写一份代码，就可以管理以太坊上的 ETH、比特币网络上的 BTC、以及 Polygon 上的 USDC。

核心逻辑：用户在其他链操作，ZetaChain 上的这个合约会响应并处理逻辑。

### 工作流程

```
简易架构
```

```
    %% 定义样式
```

```
classDef external fill:#f9f,stroke:#333,stroke-width:2px;
classDef zeta fill:#ccf,stroke:#333,stroke-width:4px;

classDef component fill:#fff,stroke:#333,stroke-dasharray: 5 5;
subgraph "Connected Chain A (例如: Ethereum)"
User_A[用户] --> Gateway_A[Gateway 合约]
```

```
    end
```

```
    subgraph "ZetaChain (中间枢纽)"
Observer[观察者节点] -->|1. 监听事件| zEVM[Universal EVM / 通用合约]
zEVM -->|2. 处理逻辑 & 状态变更| TSS[TSS 签名者节点]
```

```
    end
```

```
 subgraph "Connected Chain B (例如: Bitcoin / Polygon)"
 TSS -->|3. 写入/转账| Gateway_B[Gateway / Vault]
 Gateway_B --> User_B[接收者]  
  end
```

```
    %% 连接关系
```

```
    Gateway_A -.-> Observer
   class User_A,Gateway_A,Gateway_B,User_B external;
   class zEVM,Observer,TSS zeta;
```

**工作流程解释 (The Workflow):**

假设你要做一个“全链去中心化交易所 (DEX)”，用户想用 Ethereum 上的 ETH 买 Bitcoin 上的 BTC。

1.  **触发 (Inbound)**：
    
    -   用户在 **Ethereum** 上，向 **Gateway 合约** 发送 ETH（附带一条消息：“我要买 BTC”）。
        
2.  **观察 (Observation)**：
    
    -   ZetaChain 的 **观察者节点 (Observer)** 盯着所有连接链。它看到了 Gateway 上发生的这笔交易。
        
3.  **处理 (Processing within zEVM)**：
    
    -   这笔交易被确认为有效，ZetaChain 上的 **Universal App (通用合约)** 开始运行。
        
    -   它在合约内部计算：收到了多少 ETH，按当前汇率能换多少 BTC。
        
    -   _关键点_：这里不需要在比特币网络上部署合约（因为比特币不支持）。所有逻辑都在 ZetaChain 上完成。
        
4.  **写出 (Outbound)**：
    
    -   Universal App 计算完毕，指示 **TSS 签名者节点**：“去比特币网络，给用户 B 转 0.1 个 BTC”。
        
    -   TSS 节点生成签名，在 **Bitcoin 网络** 上执行转账。
        

### 三、 关键组件详解

1\. Gateway 的角色与跨链消息流程

-   **角色**：Gateway 是 ZetaChain 在其他链（如 Ethereum, BSC）上设立的“大使馆”或“收发室”。
    
-   **对于智能合约链 (EVM)**：它是一个真实的智能合约。用户把资产存进去（Lock），或者调用它来发送消息。
    
-   **对于非智能合约链 (Bitcoin)**：它表现为一个 TSS Vault 地址（一个多签钱包地址），用户往这个地址转账即视为与 Gateway 交互。
    
-   **流程总结**： `用户调用 Gateway` -> `事件 (Event) 被发出` -> `ZetaChain 捕获并处理` -> `ZetaChain 修改自身状态 或 触发目标链 Gateway 操作`。
    

2\. Connected Chains (连接链) 的意义

这里包括了 Bitcoin, Ethereum, Solana, BNB Chain 等。

-   **资产统一**：以前 BTC 就是 BTC，ETH 就是 ETH，老死不相往来。ZetaChain 将它们连接起来，让开发者可以把它们当作**ZRC-20 代币**（一种在 ZetaChain 上代表外部资产的标准）在同一个合约里操作。
    
-   **赋予比特币智能**：这是最大的意义。Bitcoin 本身没有智能合约，但通过连接到 ZetaChain，你可以在 Universal App 里写逻辑来控制 BTC。这相当于**给比特币外挂了一个智能合约层**。
    
-   **解决碎片化**：用户不需要关心你的应用部署在哪，他们只需要用自己习惯的钱包（比如 MetaMask 连以太坊，或者 Unisat 连比特币），就能使用你的服务。
    

普通 EVM：是单机版游戏。只能玩自己本地的数据。

Universal EVM ：是联网版的指挥中心。它把所有外部链的资产都“投影”到了自己身上（通过 ZRC-20）。

优势：在用 Foundry 写代码时，感觉就像是在操作本地变量一样，但实际上你是在指挥 Bitcoin 和 Ethereum 上的真实资产流动。

### 关于ZRC-20,我自己画了个表

![1.png33.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Appler-R/images/2025-11-26-1764148308407-1.png33.png)

## **Universal App** 和 **ZRC-20** 的核心逻辑：

-   在不接触比特币底层代码的情况下，用 Solidity 这种简单语言去操控比特币。
    
-   你的合约是部署在中间的 ZetaChain 上，远程指挥两边的链。
    

**今天简单小结**

-   **Windows + WSL + VS Code + Warp** 的这套“黄金开发流”，个人觉得非常好用，warp自然语言确实方便
    
-   在 VS Code 里写代码，去 Warp 里跑命令。
    
-   **写了主合约 (**`PocketMoney.sol`**)**：定义了一个逻辑，“谁调用我，我就给谁发 ZRC-20 格式的比特币”。
    
-   **写了测试脚本 (**`PocketMoney.t.sol`**)**：模拟了一个“上帝”环境，假装测试网里的 ZRC-20 合约是存在的，并成功欺骗虚拟机完成了提现。
    

在运行过程中，发现了一个bug，AI给我跑的代码出现了问题，恰恰是我今天刚学的知识点：十六进制字符串是用字符0~9，a~f来显示，发现并改正的AI语法错误：把 `hex"..."` **修正为了** `bytes("...")`

### `（因为 h与x不在规定的十六进制范围）`

-   **文件混淆**：把测试代码误贴进源文件导致报错，然后成功把它们分拆回 `src` 和 `test` 文件夹。
    

![图片.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Appler-R/images/2025-11-26-1764161484779-__.png)![图片.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Appler-R/images/2025-11-26-1764161452115-__.png)

### 关于哈希

**哈希值 = 数据的唯一指纹。**

给任意数据（一句话、一部电影、一个区块），用哈希算法（如 SHA-256）一算，输出固定长度乱码（如64位十六进制），

**即数据经函数处理得到产物**

具备三大特性：

**唯一性**：不同输入，几乎必然得不同哈希；

**不可逆**：知道哈希，无法反推原数据；

**雪崩效应**：原数据改一个标点，哈希全变。

**_在区块链里怎么用？_**

每个区块含前一个区块的哈希 → 串成链，改前面任一块，后面所有哈希全错；

交易用发送者私钥签名，验证时用公钥+哈希 → 证明“这人确实发了这内容”；

Merkle树用哈希聚合交易 → 一个根哈希代表所有交易，轻节点靠它快速验证某笔交易是否存在。

### **这是哈希值的十六进制字符串表示，为64个字符，对应256位（bit）二进制。**

**哈希值本质：一个256位（32字节）的二进制数**，例如：

10100001101100101100001111010100…11110000

**为方便显示/传输**，转为十六进制字符串：

每4位二进制 → 1个十六进制字符（0-9, a-f）

256位 ÷ 4 = 64个字符

例：a1b2c3d4…f0（共64字符）

**本质**：0-9、a-f只是**显示方式**，底层仍是比特流。换种编码（如Base64），字符就不同——但哈希值本身不变。

一句话：哈希是让“篡改可被瞬间发现”的数学锚点。
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->



















## 1\. 开发环境处理 (WSL Linux)

起步发现WSL被 Docker 占用、WSL 无法启动、忘记密码。

解决： 成功切换默认发行版、暴力重置 Root 密码、修复了 Python 虚拟环境组件。

结果： 拥有了一个稳定、独立的 Linux 开发系统，这是所有高级开发的基石。

wsl --set-default Ubuntu

wsl

![换.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Appler-R/images/2025-11-25-1764053478589-_.png)

## 2\. 掌握了 ZetaChain 底层工具

CLI（命令行界面）

我在领水龙头测试币时发现CLI钱包与EVM钱包区别：

**CLI:链名开头（如zeta1,cosmos1等）**

生态：Cosmos SDK系列；

使用方式：命令行，开发，脚本交易，节点交互等；

**EVM：0x开头**；

生态：以太坊，BNB等

使用方式：浏览器插件，移动钱包，连接DAPP

安装编译 ZetaChain 所需的 Go 语言和系统工具。 代码 (WSL 中执行)：

**关键突破： 学会了“远程节点 (RPC)”的概念。当本地节点跑不通时，通过连接全球公共节点来获取数据。**

RPC 节点 = 别人的全节点 + 你通过接口远程调用它

你本地不跑区块链节点，也可以照样读写链上的数据，因为有人在远程替你跑好了完整节点，你只需要通过网络向它请求即可。

可以简单理解为本地数据库与云数据库

![1ew.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Appler-R/images/2025-11-25-1764054544623-1ew.png)

### **Qwen库的安装**

![库.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Appler-R/images/2025-11-25-1764055872186-_.png)

```
# 安装基础工具
```

```
sudo apt update && sudo apt install git make build-essential curl python3.12-venv -y
```

```
# 安装 Go 1.22
wget https://go.dev/dl/go1.22.0.linux-amd64.tar.gz
sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go1.22.0.linux-amd64.tar.gz
echo 'export PATH=$PATH:/usr/local/go/bin:~/go/bin' >> ~/.bashrc && source ~/.bashrc
```

```
下载源码编译 zetacored，并将其移动到公共目录以便 Python 调用。 代码 (WSL 中执行)：
```

```
Bash
```

```
# 下载并编译
git clone https://github.com/zeta-chain/node
cd node
```

```
make install
```

```
# 关键：复制到公共目录 (解决 Python 找不到命令的问题)
sudo cp ~/go/bin/zetacored /usr/local/bin/
sudo chmod +x /usr/local/bin/zetacored
```

```
创建钱包 & 获取地址，说明： 生成钱包并在本地保存私钥，同时获取 EVM (0x) 格式地址。
```

```
Bash
```

```
# 创建新账户
```

```
zetacored keys add myaccount
# 查看 0x 格式地址
zetacored debug addr <自己的zeta1地址>
mkdir -p ~/qwen-project && cd ~/qwen-project
python3 -m venv venv
source venv/bin/activate
```

```
pip install dashscope requests
```

### 解决区块链连接问题 (关键点)

```
说明： 本地没有节点，必须在代码中指定远程公共节点 (RPC) 才能查到数据。 代码逻辑 (Python 中)：
```

```
Python
```

```
# 必须加上这个参数，否则会报 Connection Refused
```

```
NODE_URL = "https://zetachain-testnet-rpc.polkachu.com:443"
cmd = f"zetacored query bank balances {MY_ADDRESS} --node {NODE_URL} --output json"
```

```
运行最终完成的脚本。 代码 (WSL 中执行)：
```

```
Bash
```

```
# 激活环境 (如果没激活)
```

```
source venv/bin/activate
```

```
# 运行查价助手
```

```
python agent_price.py
```

```
# 运行 AI 资产分析 
```

```
python agent_final.py
```

## 3\. 构建了第一个 AI Agent (智能体)

开始只有一段死的 Python 代码，疯狂报错，让人难受

我在一番折腾（AI与资料查询，debug）后，搞出了能自动执行 Linux 命令的 Python 脚本。

呈现形式：agent\_[final.py](http://final.py) 和 agent\_[price.py](http://price.py) 成功实现了：

感知： 去区块链/网络上抓取数据（查余额、查币价）。

思考： 模拟 AI 进行逻辑判断。

行动： 生成人类可读的报告。

WSL 启动失败： 发现是docker-desktop 捣乱 → 已修复。

权限被拒： Permission denied / 找不到文件 → 学会了 sudo 和 cp。

Python 报错： ensurepip / dashscope 缺失 → 学会了 venv 环境管理。

API 报错： 401 Invalid Key → 学会了写仿真代码绕过验证。

**主要两个搞了agent**

agent\_[final.py](http://final.py)：第一个 Web3 资产管理脚本。

agent\_[price.py](http://price.py)：实时的加密货币行情助手。

**_嘿，在经历了v1与v2两个失败后，终于成功了_**

![调试.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Appler-R/images/2025-11-25-1764055394585-__.png)

### 此为final

![图片.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Appler-R/images/2025-11-25-1764054600969-__.png)

**自己重新跑时，忘了输入wsl，第一次用Linux，原谅自己啦，要注意与win终端区别**

### `cd ~/qwen-project`：准确回到了项目“大本营”。

### `source venv/bin/activate`：成功激活了装备包（注意到了绿色的 `(venv)`）。

### `python agent_final.py`：顺利运行了脚本。

### 此为price

![图片.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Appler-R/images/2025-11-25-1764054787635-__.png)

被自己逗笑了，test输成text了，再原谅自己一次hh

**此为测试Qwen**

![图片.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Appler-R/images/2025-11-25-1764055110297-__.png)

## 此为远程测试

![图片.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Appler-R/images/2025-11-25-1764055268964-__.png)

# 总结：今天在本地跑通了 AI + Web3 最小可行性产品 (MVP)

## 但这远远不够

## 我发现还有许多问题需要处理，如文本不够简洁，环境配置还需加深理解等

## 歌未竟
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->























## 了解了一些基础知识

ZetaChain 账户你的链上身份（地址 + 私钥）

RPC 节点接口，跟链交互的“网关”

Faucet 测试网水龙头，领免费币

Explorer 区块链浏览器，看交易和合约

CLI zetacored 命令行工具

Qwen 账号 自己手机号注册即可

API Key调用 Qwen 的密钥

基础请求（Chat/Completion）：两种标准调用方式

chat为主，做聊天、Agent、带系统提示的场景

completion为老式补全，打辅助的

主要操作：

ZetaChain：领水（Faucet）→ 用公共 RPC → CLI 创建钱包

Qwen：注册百炼 → 拿 API-Key → 用 OpenAI 格式调用

社区的Lancy推荐了一款AI终端[Warp](https://www.warp.dev/j)

推荐使用

![1.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Appler-R/images/2025-11-24-1763995573422-1.png)

## 调用了Qwen的api

![2.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Appler-R/images/2025-11-24-1763995628808-2.png)![图片.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Appler-R/images/2025-11-24-1763996430477-__.png)

嘿，社区氛围很好，我们一起前进！
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
