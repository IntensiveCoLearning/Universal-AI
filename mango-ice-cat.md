---
timezone: UTC+8
---

# mango

**GitHub ID:** mango-ice-cat

**Telegram:** @mangoice

## Self-introduction

code everything

## Notes

<!-- Content_START -->
# 2025-12-01
<!-- DAILY_CHECKIN_2025-12-01_START -->
# 1,打卡签到
<!-- DAILY_CHECKIN_2025-12-01_END -->

# 2025-11-30
<!-- DAILY_CHECKIN_2025-11-30_START -->

# 1，打卡签到

# 2， **Day 7 学习笔记：通用 DeFi 赛道预热 & Idea 收敛**

## 🎯 今日目标复盘

-   梳理 ZetaChain 能做的「通用 DeFi」模式
    
-   思考跨链场景的实际用户需求
    
-   生成 1–2 个初步项目想法（基于 ZRC-20 + Universal Assets）
    
-   为之后的 Hackathon 想方向奠定基础
    

* * *

# 🧠 一、我对通用 DeFi 能做什么的整体理解

回顾前六天的学习，我把 ZetaChain 能实现的能力总结成三个关键点：

* * *

## **1️⃣ 所有链的资产都能在 ZetaChain 被“统一表示”**

依靠：

-   ZRC-20（代表外链资产）
    
-   Universal Asset 标准
    

**多链 ≠ 多种逻辑  
在 ZetaChain = 一套逻辑搞定所有链**

这意味着：

-   不用部署多条链
    
-   不用通过 bridge
    
-   跨链操作写一个函数就完成
    

非常适合做聚合类、自动化类协议。

* * *

## **2️⃣ 原生跨链操作可以被“编排”成 DeFi 流程**

ZetaChain 的 messaging / swap 功能让我意识到：

> 一笔调用可以触发多链多步操作。

例如：

-   存 BTC → 放大 → 抵押 ETH LST → 借 USDC → 自动再质押
    
-   全链都能做，但只需要一次调用
    

我认为这是 Universal DeFi 的真正意义。

* * *

## **3️⃣ 通用 Token / NFT 可以承载“多链一致的状态”**

例如：

-   一个全链 VIP 通行证
    
-   一个跨链储蓄凭证
    
-   一个跨链矿机 NFT
    
-   一个全链收益凭证
    

状态统一，不分链。

* * *

# 🔍 二、我整理出来的通用 DeFi 模式（根据 ZetaChain 能力推演）

结合官方资料 + 我的理解，我整理了几类非常适合做的方向：

* * *

### **① 全链 Savings / Earn（跨链统一储蓄）**

-   用户可以存任何链的资产
    
-   策略执行在 ZetaChain
    
-   收益发放到任意链
    
-   不需要 bridge，不需要知道策略在哪条链运行
    

* * *

### **② 全链 Yield Aggregator（收益聚合器）**

ZetaChain 非常适合做：

-   把不同链的收益合并
    
-   自动复投
    
-   自动跨链调仓
    
-   自动从收益高的链切换到收益更高的链
    

* * *

### **③ Restaking + 多链抵押（跨链抵押借贷）**

-   存 BTC → 抵押 → 借 ETH → 抵押 → 借 USDC
    
-   多链抵押品
    
-   多链借贷
    
-   一套逻辑，无需部署多链
    

* * *

### **③ 跨链聚合支付 / 路由器**

-   用户用链 A 支付
    
-   收款方在链 B 收钱
    
-   用 ZRC-20 映射实现
    

* * *

### ④ **通用 NFT / 通用资产衍生品**

-   跨链会员
    
-   游戏 NFT 在所有链能共用
    
-   跨链矿机凭证
    
-   多链身份 NFT
    

* * *

整理完这些后，我开始结合自己的理解，为 Hackathon 收敛几个实际方向。

* * *

# 三、今日实践 / 作业：我的 2 个项目 Idea

以下是我认真想过，结合 ZetaChain 优势后写出的两个方向。

* * *

## 1，**全链跨链储蓄 & 自动收益路由器（Universal Auto-Savings）**

### **目标用户**

想要收益但完全不懂多链的普通用户

-   想赚 DeFi 收益，但不想管理多链的普通用户
    
-   资产分布在多条链上的“DeFi 用户”
    
-   想把 BTC、ETH、BNB 等都放到统一策略的小白
    

* * *

### **想解决的问题**

-   现在所有 multi-chain vault 都需要桥、需要手动选链
    
-   用户不知道哪条链当前收益最好
    
-   多链操作复杂、手续费高
    
-   不想多钱包多操作
    
-   想要“我存一个 BTC，你自动替我在最优链执行策略”
    

* * *

### **粗略的跨链逻辑**

1.  用户存入 BTC（或任意链资产）
    
2.  ZetaChain 铸造 ZRC-20-BTC
    
3.  Vault 策略合约判断：
    
    -   当前收益最高的是什么？
        
    -   ETH 链？BNB 链？Polygon？
        
4.  ZetaChain 自动跨链把资产放到目标链执行策略
    
5.  定期把收益跨链回流给 ZetaChain
    
6.  用户随时可以从任意链取款
    
7.  可加入“最低收益保障”模块
    

### 使用方式

-   用户 → 存一次
    
-   协议 → 自动跨链执行策略
    
-   提取 → 任意链
    

* * *

* * *

## 2，跨链抵押信用账户（Omni-Collateral Account）

### **目标用户**

-   有 BTC，但想借稳定币（USDC、USDT）
    
-   有 ETH LST，但想在其他链进行抵押
    
-   想要“一次抵押 → 全链借贷”的工程师或 DeFi 用户
    

* * *

### 想解决的问题

-   现在的跨链借贷极其麻烦
    
-   都需要 bridge → deposit → borrow → bridge back
    
-   风险高、流程复杂、手续费高
    
-   不同链清算逻辑不同
    

ZetaChain 能原生解决。

* * *

### 创新点

-   建立 **单一跨链抵押仓位（Unified Cross-chain Vault）**
    
-   BTC 在 chain A，ETH 在 chain B，但它们都能作为统一抵押
    
-   可以借出稳定币到任意链（例如 Solana、BSC 等）
    
-   清算 → 在 ZetaChain 一次性执行，极大提升安全性
    

### 使用方式

-   用户存入：任意链资产
    
-   借出：任意链稳定币
    
-   清算：单点合约完成
    

一句话：

> 用户只需要一笔调用 → 实现跨多链的“抵押 → 借贷 → 分发”。

* * *

# 📚 今日总结

我对 ZetaChain 的认知进一步明确：

> **它不是在每条链部署一个协议，而是用一条链（ZetaChain）统筹所有链的资产与逻辑。**

今天生成两个 idea 后，我其实感觉：

-   ZRC-20 + Messaging 真的是解决多链痛点的终极组合
    
-   未来的 DeFi 协议可能真的会是 “一次调用，多链执行”
    

明天（Day 8）我会开始搭建自己的合约开发环境，为后面的 Demo 做准备。
<!-- DAILY_CHECKIN_2025-11-30_END -->

# 2025-11-29
<!-- DAILY_CHECKIN_2025-11-29_START -->



# 1，打卡签到

# 2， **Day 6 学习笔记：Universal DeFi & Demo 实战**

## 今日目标复盘

-   跑通一个官方 Demo（我选择了 **Swap Tutorial**）
    
-   亲手完成一次“调用一次 → 多链动作自动完成”的操作
    
-   弄懂 ZetaChain 如何帮我跨链处理 swap
    

* * *

# 🛠️ 一、环境准备（我今天实际做的）

### 1\. 克隆官方示例仓库

```
git clone https://github.com/zeta-chain/example-contracts
cd example-contracts
```

### 2\. 进入 swap 示例目录

```
cd swap
```

### 3\. 安装依赖

```
npm install
```

第一次安装花了几分钟，主要是 Hardhat、ethers 等包。

### 4\. 配置测试网 RPC

在 `.env` 中设置 ZetaChain Athens3 测试网 RPC：

```
ZETA_CHAIN_RPC=https://rpc.athens3.zetachain.com
PRIVATE_KEY=钱包私钥
```

* * *

# 🔧 二、部署合约（实际执行）

Swap demo 是一个 **跨链 swap 入口合约**，部署在 Zeta EVM 上。

执行部署脚本：

```
npx hardhat deploy-zeta-swap --network zeta_testnet
```

（我的实际日志）：

```
Deploying ZetaSwap...
ZetaSwap deployed at 0x54e1f27f0e3b6d1b2b003f0xxxxxxxxxxxxx
```

记录下合约地址。

* * *

# 三、发起一次 Swap 调用（执行了实际的 CLI）

官方示例中提供了一个简单的脚本：

```
npx hardhat swap --network zeta_testnet
```

执行后日志大概如下：

```
Sending swap request...
Tx: 0x1eabcb80bxxxxxxxxxxxxxxxxxxxx
Waiting for ZetaChain to process...

Swap request sent successfully!
```

这一步就完成了：

> 我在 ZetaChain EVM 上发起了一笔 swap，请求把某个 ZRC-20 资产换成另一个资产。

* * *

# 四、在浏览器中查看跨链操作

我在 explorer 里观察这笔交易

能看到三层动作：

### **1️⃣ EVM 层调用（我发起的交易）**

-   我调用 ZetaSwap 合约的 `swap()` 方法
    
-   交易发到了 ZetaChain EVM
    

### **2️⃣ ZetaChain 内部处理（ZetaCore 层）**

-   TSS 接管请求
    
-   解析目标链、目标资产
    
-   执行系统内部的跨链逻辑
    

### **3️⃣ 完成跨链 swap**

-   ZetaChain 帮我发起外链交易（例如某个外链）
    
-   或者内部完成 ZRC-20 ↔ ZRC-20 的兑换
    

整个过程我只做了一次调用：

> **我发起一次调用 → ZetaChain 帮我跨多条链执行动作。  
> 这就是 Universal DeFi 的体验！**

* * *

# 📘 五、关键命令记录（我的操作清单）

| 操作 | 命令 |
| --- | --- |
| 克隆仓库 | git clone ... |
| 安装依赖 | npm install |
| 部署合约 | npx hardhat deploy-zeta-swap --network zeta_testnet |
| 发起 swap | npx hardhat swap --network zeta_testnet |
| 查看交易 | 使用 explorer 查看 tx hash |

* * *

# 六、Day 6 作业内容

## ✔ 1. **你是从哪里发起的调用？**

我从：

```
ZetaChain EVM（Athens3 测试网）
```

发起了交易，调用的是我刚部署的：

```
ZetaSwap.sol
```

这一笔交易其实只是普通的 EVM tx，看起来像普通合约调用，但里面附带了“跨链指令”。

* * *

## ✔ 2. **最终在 ZetaChain 上发生了什么？**

从浏览器的事件日志来看，完整流程是：

1.  我调用了 ZetaSwap 的 `swap()`
    
2.  合约把指令交给 ZetaChain 的系统合约
    
3.  ZetaChain TSS 读取到 swap 请求
    
4.  解析跨链目标链、目标资产、用户地址
    
5.  在内部执行多链转换逻辑
    
6.  外链（或内部）完成资金调拨
    
7.  最终给我目标链上的资产
    

总结一句话：

> **我只发起了“一笔 EVM 调用”，ZetaChain 却替我做了跨链 swap 的所有事情。**

这跟传统做法（bridge + dex + bridge back）相比简直爽太多。

* * *

# 🧠 今日总结

Day 5 学了概念  
Day 6 在今天我终于真正“看见了”跨链动作在链上运行的样子。

我今天最大的收获：

> **ZetaChain 的跨链，不是 Bridge，不依赖外部，不需要二次操作，调用一次即可。**

这种「真正统一跨链逻辑」的体验，是其他链提供不了的。
<!-- DAILY_CHECKIN_2025-11-29_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->





# 1，打卡签到

# **2，今日学习内容**

# **ZetaChain Day 5 学习笔记：Universal DeFi & 全链资产基础**

## 今日目标

-   搞懂 ZRC-20 标准是如何实现「跨链资产的统一表示」
    
-   理解 Universal Token / Universal NFT 的概念
    
-   弄清为什么多链资产在 ZetaChain 上能用同一套逻辑调用
    
-   完成今天的概念思考作业
    

* * *

# **一、ZRC-20 是什么？（我自己的理解）**

我带着 ERC-20 的认知去看 ZRC-20，其实它们的接口几乎一样，但核心差异有三点：

* * *

## **1️⃣ ZRC-20 不是链上原生资产，而是**“多链资产的本地化代理”

举个例子吧：

-   用户在 BTC 链上有 1 BTC
    
-   用户通过 ZetaChain 跨链存入
    
-   ZetaChain 会在 **Zeta EVM** 上铸造一个 **1 ZRC-20 BTC** 代表用户的真实 BTC
    

它不是包装（不像 wBTC），更像是：

> “我拥有一张 BTC 的跨链支票，随时可以兑回去”

这就是 ZRC-20 的厉害之处。

* * *

## **2️⃣ ZRC-20 合约是由系统自动部署，不是开发者自己发的 token**

与 ERC-20 不同，ZRC-20 不需要：

-   无需人为部署
    
-   无需自己定义 decimals、name、symbol
    

系统会根据外部链资产直接生成对应的 ZRC-20：

-   ZRC-BTC
    
-   ZRC-ETH
    
-   ZRC-USDT
    
-   ZRC-MATIC
    
-   ……
    

作为开发者，我只需要调用，完全**不用担心跨链来源**。

* * *

## **3️⃣ ZRC-20 在合约层面自带跨链能力**

**ERC-20 的转账仅限本链。但 ZRC-20 将复杂的跨链逻辑抽象化了：**

-   **链内转账** 使用 ERC-20 标准的 $transfer(\\dots)$ 函数。
    
-   **跨链提款** 使用 ZRC-20 特有的 $withdraw(\\dots)$ 函数。
    

**尽管函数名不同，但它们都采用** **ERC-20 风格的极简调用方式**，实现了开发者用 **一套 Solidity 逻辑控制所有连接链资产** 的目标。系统会根据调用的函数 ($transfer$ 或 $withdraw$) 自动判断并执行相应的链内或跨链逻辑。”

```
zrc20.transfer(to, amount);
zrc20.withdraw(to, amount);
```

但系统会判断对方是不是其他链的地址，并自动走跨链转账逻辑。  
这一点对开发者来说是非常爽的 —— **用一套 Solidity 逻辑控制所有链**。

* * *

# **二、Universal Token（通用资产）理解**

官方文档说的 “Universal Token” 通俗理解就是：

> 一个资产在 ZetaChain 上代表了它在所有链上的身份。

和 ZRC-20 的关系：

-   ZRC-20：一种实现「通用资产」的规范（专门对应外部资产）。
    
-   Universal Token / NFT：一个更广泛的设计理念，可自定义。
    

也就是说：

**ZRC-20 是通用资产的一种标准化工具**。

* * *

# **三、Universal NFT（跨链 NFT）的概念（我个人的理解）**

Universal NFT 的核心就是：

-   NFT 不再属于某条链
    
-   它的属性、权限、状态在所有链一致
    
-   可以由不同链的用户共同使用
    

不像现在的：

-   以太坊上的 NFT
    
-   Polygon 上的 NFT
    
-   Solana 上的 NFT  
    都是孤岛。
    

Universal NFT 就像：

> 一个全球统一的 NFT，跨所有链都能访问、使用、更新状态

适合用在：

-   跨链会员卡
    
-   游戏中的身份 ID
    
-   多链 DAO 通行证
    
-   多链收益凭证
    

* * *

# **四、今天完成的实践 / 作业**

### ✔ **1\. ZRC-20 与 ERC-20 的直观区别（开发者视角）**

| 项目 | ERC-20 | ZRC-20 |
| --- | --- | --- |
| 部署 | 手动部署合约 | 系统自动生成（对应外链资产） |
| 链范围 | 只能在本链使用 | 跨链可用（关键差异） |
| 资产来源 | 自己 mint 或铸造 | 表征外链真实资产 |
| 跨链能力 | 需要 bridge 支持 | 原生支持跨链发转账 |
| 安全模型 | 依赖 bridge 或 L2 | 由 ZetaChain TSS 管理外链资产 |
| 使用复杂度 | 每个链都要重新部署 | 所有链只需一套逻辑 |

一句话总结：

> **ERC-20 是哪条链就属于哪条链；ZRC-20 是所有链的统一资产形态。**

* * *

### ✔ **2\. 举一个「通用资产」的应用场景**

## **跨链储蓄**

我觉得这是最实际的应用场景：

-   用户存的是 BTC
    
-   协议策略使用的是 ETH LST
    
-   奖励发放在 Polygon
    
-   收益展示在 ZetaChain
    
-   退出时用户想要 Solana 的 USDC
    

传统做法要：

-   好几次 bridge
    
-   手续费超高
    
-   操作复杂
    
-   存取时间超长
    
-   还要让用户学一堆链
    

但有 ZetaChain：

-   入金可以是 BTC（ZRC-20-BTC）
    
-   协议内部用 Zeta EVM 调智能合约
    
-   跨链收益自动发放
    
-   退出时可以直接跨链到任何链
    
-   所有操作对用户就是一次操作
    

这种体验是传统链没办法实现的。

* * *

# 今日总结

今天理解了 ZetaChain 的核心哲学：

> **ZetaChain 不是做一个新链，而是做所有链的统一层。  
> ZRC-20 就是让所有链资产实现“无边界化”的工具。**

跨链不是 bridge，而是：

-   原生
    
-   自动
    
-   安全
    
-   可编程
    

这为后续的 Day 6 — Day 7 的合约练习奠定了基础。
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->







# 1，打卡签到

# 2，今日学习内容

## 1\. 什么是 Universal App（全链应用）

经过阅读官方文档，我总结出 Universal App 的核心概念：

```
传统 Web3 = 只在一个链上运行
Universal App = 一个合约，可以同时对多个链的资产 / 信息进行操作
```

依托 ZetaChain 的跨链 Messaging，Universal App 可以做到：

-   **一个合约，管理多链资产（BTC / ETH / BNB 等）**
    
-   **在链 A 触发，链 B 执行逻辑**
    
-   **不用部署多个跨链合约，不用自己写桥逻辑**
    
-   **所有跨链安全由 ZetaChain 代办**
    

> 这种架构一眼就理解成类似「一个总后台，多个前端分店」，  
> 而 ZetaChain 就是帮我跑物流的“跨链快递公司”。

* * *

## 2\. Hello World Demo 的结构理解

我看了官方文档，发现 Hello World 跨链 Demo 一般分成三部分：

* * *

### **（1）合约层（Solidity）**

这个合约会包含：

-   `onCrossChainCall`
    
-   `crossChainMessage`
    
-   `_zetaChainReceive()`
    

简单说，就是接收消息 → 处理 → 回调。

我的理解：

```
链 A：前端发起 → CLI/SDK 调用 → 发消息到 ZetaChain
ZetaChain：中转消息
链 B：合约收到消息 → 执行逻辑
```

Hello World 最小版本可以是：

```
emit Hello("Hello from Ethereum");
```

* * *

### **（2）前端 / 调用层**

通常使用：

-   CLI
    
-   Hardhat task
    
-   SDK
    
-   或者自己写一个 HTML 前端 + ethers.js
    

Hello World 用 CLI 已经够了。

* * *

### **（3）RPC / 钱包 / 节点连接层**

这里主要是：

-   使用 ZetaChain 的 testnet RPC
    
-   从 wallet（Keplr / MetaMask）发 tx
    
-   测试跨链事件是否触发
    

* * *

# 今天建立的心智模型

我把 Universal App 的思维总结成一句话：

> 不是多链合约，而是一个能够“感知多链”的合约。

传统方式：  
👉 每条链部署一个合约 + 自己写桥消息 + 自己负责安全

ZetaChain：  
👉 一个主合约即可，跨链消息由 ZetaChain 负责  
👉 类似“写一次逻辑，多链都能让你执行”

把复杂封装掉，非常容易上手。

* * *

# 自己计划做的第一个 Universal App（作业）

按照要求，我写了我希望的“第一个 demo 应用”：

### 🧪 **我决定做的 Demo：跨链 Hello World（带链名输出）**

逻辑：

```
从链 A 发送消息 → 合约在链 B 输出：
"Hello from A to B!"
```

进一步扩展：

-   记录调用链的信息
    
-   计数器累加（跨链调用次数）
    
-   前端展示调用结果
    

我觉得这对理解跨链生命周期非常直观。

* * *

# Hello World 的工具链

官方推荐 Hardhat，所以我决定：

### **我选择的工作流：ZetaChain CLI + Hardhat（不用 Foundry）**

原因：

-   Hardhat 插件齐全
    
-   官方教程大部分使用 Hardhat
    
-   对 EVM 新手更友好
    
-   测试网调试更容易
    

具体流程如下：

### ✔️ 会用到：

-   ZetaChain CLI
    
-   Hardhat（部署、编译）
    
-   测试网（非本地链）
    
-   钱包：MetaMask + ZetaChain Testnet RPC
    

### ❌ 不用到：

-   本地链（Hardhat node）
    
-   Foundry（后面可选）
    

* * *

# \## 📝 总结（Day4）

今天不写代码，但完成了最关键的一步：

### **理解架构 + 拆解 Hello World 的所有模块 + 制定工作流**

这会让明天开始写合约的时候不会迷路。
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->









# 1，打卡签到

# 2，Day 3 笔记 — ZetaChain & Universal Blockchain 核心概念

**日期**：2025-11-26  
**目标**：理解通用区块链概念、Universal App、Gateway，并用图示描述架构。

* * *

## 1️⃣ 学习目标回顾

-   理解 **通用区块链 / Universal EVM / Universal App / Omnichain Smart Contract** 的概念
    
-   能够画出 ZetaChain + 多条公链 + Gateway 的结构图
    
-   在笔记中总结核心概念，用自己的话描述
    

* * *

## 2️⃣ 笔记

### 3.1 阅读文档与理解

-   先浏览了 **Universal App 概览**，发现 **Universal App** 就是可以在 **多条公链**上执行的智能合约应用
    
-   文档里提到的 **Omnichain Smart Contract** 核心：
    
    -   一次编写 → 多链执行
        
    -   不需要用户手动跨链桥接资产
        
-   Gateway 的作用是 **连接不同链**，负责消息传递、资产转移的可靠性和安全性
    

> 我的理解：  
> Gateway 就像高速公路的收费站，负责把不同链的数据/资产安全传输到目标链，ZetaChain 就是核心路网。

* * *

### 3.2 用自己的话总结

**Universal App**：

> 可以跨链运行的应用，单份代码可在多条链上调用，无需为每条链单独开发。

**Gateway**：

> 是通用应用和不同区块链之间的桥梁，处理消息、交易、资产的跨链逻辑。

**Universal EVM**：

> 提供统一的 EVM 环境，让 Solidity/Hardhat/Foundry 的合约无需修改即可在多链上执行。

* * *

### 3.3 画图

我在笔记里画了一个简易架构图（ASCII 版）：

```
          +----------------+
          |  ZetaChain     |
          |  Core Layer    |
          +----------------+
             /     |      \
            /      |       \
      Ethereum    Solana   Bitcoin
        |           |        |
      Smart       Smart     Smart
      Contract    Contract  Contract
       (EVM)      (Rust)    (BTC script)
             ^ Gateway ^
```

> -   中心是 ZetaChain，负责跨链消息传递
>     
> -   每条链保留自身的智能合约特性
>     
> -   Gateway 处理跨链调用的安全性和一致性
>     

* * *

### 3.4 思考与笔记

-   ZetaChain 核心价值：**降低跨链开发门槛**，让开发者只关注应用逻辑，不必处理每条链的细节。
    
-   对比传统跨链：以前需要 wrap token / 跨链桥，现在 ZetaChain 直接原生支持跨链资产。
    
-   结合 Day 2 学到的 CLI，我计划 **后续自己动手搭建一个 Hello World Universal App**，先在本地链跑一遍 Swap / Messaging 流程。
    

* * *

### 3.6 今日收获

-   理解了 **Universal App、Gateway、Universal EVM** 的概念
    
-   能够用自己的话描述它们的作用
    
-   画出了简易架构图，帮助理解跨链逻辑
    
-   对后续动手操作有清晰规划
    

* * *

**总结**：  
Day 3 的重点是 **概念理解 + 架构梳理**，我觉得最重要的是把 Gateway 的作用理解透彻，这样后续写跨链合约 / 使用 CLI 才不会迷路。
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->











# 1，今日打卡，加油

# 2，#️⃣ Day 2 学习目标（总结）

-   **完成 ZetaChain 本地/云端开发环境**
    
    -   安装 ZetaChain CLI
        
    -   掌握测试网 RPC / Faucet / Explorer 的入口
        
-   **完成第一次 Qwen API 调用**
    
    -   使用 Postman / curl / Python / Node 调一次 Qwen
        
    -   能看到模型返回（成功 or 报错均算完成）
        
-   **明确自己 的学习目标**
    

# Part 1｜ZetaChain CLI 安装与环境准备

## 1\. 为什么需要 CLI？

ZetaChain CLI 用途：

-   创建项目模板（Universal EVM）
    
-   部署合约
    
-   与 ZetaChain 网络交互（testnet）
    
-   调用 messaging/swap demo
    

是 ZetaChain 开发最核心的工具链之一。

# 2\. 安装 ZetaChain CLI（Windows → WSL2 推荐）

因为我的系统是Windows，所以使用 WSL2 + Ubuntu。

## ✔️ **步骤 1：启用 WSL2**

PowerShell 管理员模式执行：

```
wsl --install
```

重启后自动安装 Ubuntu。

## **步骤 2：安装依赖**

进入 Ubuntu 终端：

```
sudo apt update
sudo apt install -y unzip curl git
```

* * *

## ✔️ **步骤 3：安装 ZetaChain CLI**

安装 ZetaChain CLI：

```
npm install -g zetachain
```

尝试运行：

* * *

## ✔️ **步骤 4：初始化 ZetaChain 项目**

```
zetachain query chains list
```

```
该命令会打印当前连接到 ZetaChain 的所有链
```

# 🟧 Part 2｜ZetaChain 测试网入口记录

* * *

## ✔️ **① RPC 入口（ZetaChain EVM Testnet）**

```
https://www.zetachain.com/docs/reference/network/api
```

* * *

## ✔️ **② Faucet（领测试币）**

```
https://www.zetachain.com/docs/reference/faucet
```

* * *

## ✔️ **③ Explorer**

```
https://www.zetachain.com/docs/reference/explorers
```

* * *

## ✔️ **④ ChainID / Network 信息**

| 项目 | 值 |
| --- | --- |
| 网络名 | ZetaChain Athens Testnet |
| Chain ID | 7001 |
| 货币符号 | ZETA |

* * *

## ✔️ **⑤ 将 RPC 加入 Metamask**

```
网络名称：ZetaChain Athens
RPC：http(s)：https://zetachain-athens-evm.blockpi.network/v1/rpc/public
chainId：7001
symbol：ZETA
```

# 🟦 Part 3｜Qwen API 调通（核心任务）

* * *

* * *

# ✔️ Python 示例（最简单）

```
from openai import OpenAI

client = OpenAI(
    api_key="YOUR_QWEN_API_KEY",
    base_url="https://dashscope.aliyuncs.com/compatible-mode/v1"
)

resp = client.chat.completions.create(
    model="qwen-max",
    messages=[{"role": "user", "content": "用一句话介绍 ZetaChain 是什么？"}]
)

print(resp.choices[0].message)
```

* * *

# 🟩 运行后应看到类似输出：

```
ZetaChain 是一个支持原生跨链消息与资产转移的通用区块链。
```
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->













# 1，签到打卡

# **2，ZetaChain 项目介绍**

## **概述**

ZetaChain 是一个开创性的 Layer 1 区块链平台，被誉为"**第一个通用区块链**"（Universal Blockchain），旨在解决区块链碎片化问题，实现所有链之间的无缝互操作性。

## **核心价值主张**

-   🔗 **解决区块链碎片化**：实现所有链之间的无缝互操作性
    
-   💫 **支持跨链价值转移**：无需桥接或包装代币
    
-   📨 **跨链消息传递**：实现真正的链间通信
    
-   🛠️ **原生全链智能合约**：支持 omnichain smart contracts
    

## **项目发展历程**

| 时间 | 里程碑 |
| --- | --- |
| 2021年7月 | 项目启动 |
| 2024年初 | 主网上线 |
| 当前状态 | 连接10+条链，TVL超3亿美元，用户超500万 |

## **核心技术特点**

### **架构基础**

-   **基于**：Cosmos SDK 和 Tendermint 共识机制
    
-   **安全机制**：Proof of Stake (PoS)
    

### **关键创新**

**🔥 全链智能合约**

-   合约可直接读写多链状态
    
-   支持非智能合约链（如比特币、Dogecoin）的外部管理
    
-   实现原生资产转移
    

**🌐 去中心化连接**

-   每个验证节点配备"观察者"（observers）
    
-   实时监控外部链事件
    
-   确保无单点故障的互操作性
    

**👨‍💻 开发者友好**

-   EVM 兼容，支持 Solidity
    
-   提供 SDK 和工具，简化跨链开发
    

**🛡️ 安全性与效率**

-   使用 Threshold BLS 提升可扩展性
    
-   低费用、高速跨链交换（例如 BTC/EVM/Solana 间）
    

**🎯 生态集成**

-   ZetaHub 作为用户界面，探索 dApps 和网络活动
    
-   支持 DeFi、NFT、治理和 AI 集成
    

## **支持的区块链**

**已支持**：

-   ✅ Ethereum
    
-   ✅ Polygon
    
-   ✅ BSC
    
-   ✅ Solana
    
-   ✅ Avalanche
    
-   ✅ Bitcoin
    
-   ✅ Dogecoin
    

**未来规划**：将持续扩展支持更多区块链

## **ZETA 代币与 Tokenomics**

### **代币基本信息**

-   **代币名称**：ZETA
    
-   **总供应量**：21 亿枚
    
-   **主要用途**：网络治理和激励
    

### **代币用途**

-   💰 支付 gas 费
    
-   🔒 安全 PoS（staking/bonding/slashing）
    
-   🔄 跨链转移/交换/消息传递
    
-   💧 流动性提供
    
-   🔥 代币燃烧
    

### **代币分配**

| 类别 | 比例 | 描述 |
| --- | --- | --- |
| 社区与生态 | 20.8% | 奖励早期贡献者、钱包/社交活动参与者 |
| 验证者/staking | 15% | 网络安全激励 |
| 团队/顾问 | 20% | 锁定 vesting |
| 投资者/基金会 | 25% | 种子/私募轮 |
| 流动性/营销 | 19.2% | 交易所上市、社区活动 |

### **释放计划**

-   主网上线后逐步解锁
    
-   **2025年4月**：有 1500 万美元代币解锁事件
    
-   **奖励程序**：包括主网用户空投，鼓励参与开发和使用
    

## **团队与融资**

### **核心团队**

-   **创始人**：Ankur Nandwani
    
    -   前 Coinbase 员工
        
    -   Basic Attention Token (BAT) 共同创建者
        
-   **团队成员**：包括前 Coinbase/Binance 员工，如 Dan Romero、Sam Rosenblum
    

### **顾问团队**

-   Nathalie McGrath（Coinbase 前 HR 主管）
    
-   Juan Suarez（Coinbase 前法律顾问）
    

### **融资情况**

-   **种子/私募轮**：超 2700 万美元
    
-   **主要投资者**：包括 Polygon 的 JD Kanani、顶级做市商和基金
    
-   **IEO**：2024年在 OKX Jumpstart 进行
    
    -   0.5% 供应量（2.1 亿 ZETA）分配给 BTC/OKB 质押者
        

## **路线图与里程碑**

### **已完成**

-   ✅ 主网上线（2024）
    
-   ✅ 多链连接
    
-   ✅ ZetaHub 启动
    

### **2025年重点**

-   🚀 Threshold BLS 签名提升性能
    
-   🛠️ Grokipedia 生态工具
    
-   ⛓️ 比特币/ETH 深度集成
    
-   🤖 AI Buildathon 黑客松（8月）
    
    -   奖金 9000 美元
        
    -   使用 Gemini AI + ZetaChain 工具
        

### **2026年愿景**

-   🌉 跨链 staking（用 BTC/ETH 质押安全网络）
    
-   🔐 ZK 证明 dApps
    
-   🧠 AI/数据集成
    

### **近期更新**

-   **2025年6月**：网络升级（区块高度 8728280）
    
-   **合作伙伴**：与 Google Cloud 合作黑客松
    
-   **未来发展**：ZetaChain 2.0 提案，支持链抽象
    

## **合作伙伴与生态**

### **关键合作伙伴**

-   🤝 **Google Cloud**：AI 黑客松合作
    
-   🤝 **Alibaba Cloud**：加速亚洲区块链采用
    
-   🤝 **EnsoFi**：流动性层合作
    
-   🤝 **LXDAO**：开源项目支持
    

### **生态系统**

-   **DeFi**：跨链交换等应用
    
-   **NFT**：跨链 NFT 生态
    
-   **游戏**：全链游戏体验
    
-   **社区活动**：韩国社区事件（如 ZUNO Galaxy 活动，抽奖 USDT）
    

### **社区动态**

-   🐦 **X/Twitter**：活跃讨论合规（MiCAR/DFSA）、TVL 增长
    
-   👨‍🏫 **开发者活动**："残酷共学"课程（ZetaChain + Qwen AI）
    
-   📊 **市场表现**：TVL 持续增长，社区活跃度高
    

# **3，**对 ZetaChain 的理解与分析

ZetaChain 的本质：它到底在解决什么问题？ZetaChain 不是又一个“跨链桥”，而是试图终结“跨链桥时代”的基础设施。

ZetaChain 要做的是：把所有链都当成自己的“Layer 2”来管理。  
它通过在自己的 L1 上运行全节点观察者（Observer + Signer），实现了：

-   比特币、Dogecoin 这类非智能合约链也能被原生调用
    
-   无需包装、无需桥、无需第三方托管，直接在 ZetaChain 合约里操作外部链资产
    

一句话总结：  
ZetaChain 想成为“区块链世界的 IPv6”——让所有老链、新链天然互通，不再需要无数个翻译器（桥）。
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
