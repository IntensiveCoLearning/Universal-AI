---
timezone: UTC+8
---

# jianlin chen

**GitHub ID:** Airjiannan05

**Telegram:** @none

## Self-introduction

在校计算机大学生一枚，专攻ai前沿技术，有相关的链上开发基础，对区块链加ai的发展有浓厚的兴趣。Blockchain is the future.

## Notes

<!-- Content_START -->
# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->
**学习目标**

-   理解 ZRC-20、Universal Token / NFT 的基本概念和作用。
    
-   明白多链资产在 ZetaChain 上如何被统一表示。
    

**学习资料**

-   ZRC-20 标准
    
-   [https://www.zetachain.com/docs/developers/evm/zrc20/](https://www.zetachain.com/docs/developers/evm/zrc20/)
    
-   Universal 资产文档
    
-   [https://www.zetachain.com/docs/developers/standards/overview](https://www.zetachain.com/docs/developers/standards/overview)
    

**扩展资料（可选）**

-   Example code repo（以后会用到，今天先收藏）
    
-   [https://github.com/zeta-chain/example-contracts](https://github.com/zeta-chain/example-contracts)
    

**实践 / 作业**

-   在笔记中写出：
    
    -   ZRC-20 和普通 ERC-20 的直观区别（从开发者视角）。
        
    -   举一个「通用资产」可能的应用场景（比如跨链储蓄、通用 NFT 通行证等）。
        

# ZRC-20

## 基本概念

ZetaChain 上对外部链原生资产与 ERC-20 的“原生表示”。当从以太坊/BNB/比特币等连接链把资产“存入”ZetaChain 时，协议在连接链侧托管这些资产（TSS 地址或托管合约），**并在 ZetaChain 上按 1:1 铸造对应的 ZRC-20**。**（托管映射）**提出时则销毁 ZRC-20，并从托管处把原生资产打回目标链。

## 性质

-   只能由协议铸造
    
-   同名代币跨不同源链被当作不同资产（如 ETH Mainnet 的 USDT 与 BSC 的 USDT 是两个 ZRC-20）。
    
-   支持查询/扣除跨链所需 gas 费用
    
-   可实现资产回退（revert）处理。
    

## 使用场景

-   跨链资产转移：源链资产-->ZRC-20-->目标链资产，
    
-   Gas 抽象：合约可用部分输入资产兑换成目的链 gas 对应的 ZRC-20 以支付跨链执行费用。
    

# Universal Token（通用代币）

## 基本概念

一种“可在任意链铸造与转移、保持总量与元数据一致”的跨链 ERC-20 标准。

**但是他不是托管映射的方式，而是同一种代币在多链上的原生流转，它会在源链销毁，在目标链按同一供应铸造。**

## 架构

两类合约：

-   Universal（部署在 ZetaChain，必需）：充当跨链中枢与信任根。
    
-   Connected（部署在各 EVM 连接链，可选多条）：与 Universal 建立互信映射。
    

## 转移路径

任意链之间的转移都会经由 ZetaChain 路由（例如 Ethereum → ZetaChain → BNB）

-   费用模型：
    
    -   EVM → ZetaChain：无额外跨链费。
        
    -   ZetaChain → EVM：以 ZETA 支付，对应自动兑换为目标链 gas 的 ZRC-20。
        
    -   EVM → EVM：源链 gas 资产承担费用，协议会在 ZetaChain 上完成所需 ZRC-20 兑换。
        

# Universal NFT（通用 NFT，ERC-721 级）

## 定义

可在任意连接链铸造与转移的 NFT 标准。跨链时源链销毁、目标链按相同 tokenId 重铸；元数据保持一致，tokenId 跨链持久。

## 架构

也是Universal和Connected两个合约，并通过互信列表限制只接受同一集合的跨链调用。

# ZRC-20 vs Universal Token/NFT：本质差异

-   ZRC-20：代表“外部链已有资产”的在链内的**可编程镜像**，由协议管理铸销，适合“把外部价值带入 ZetaChain 进行逻辑编排后再原样提出”。
    
-   Universal Token/NFT：你自己的“原生跨链资产标准”，在多链之间通过**“销毁/重铸”**保持单一供应与一致性，不依赖外部链既有资产或托管余额。
    

# ZRC-20 和普通 ERC-20 的直观区别

从开发者（Developer）的视角来看，**ZRC-20** (ZetaChain Request for Comment 20) 和普通的 **ERC-20** 的核心区别在于：**资产的“管辖范围”和“交互能力”**。

简单的一句话总结：**ERC-20 是“孤岛内”的账本，而 ZRC-20 是连接外部原生链资产的“遥控器”。**

以下是具体的直观区别：

### 资产的所有权与映射逻辑 (State & Ownership)

-   **普通 ERC-20:**
    
    -   **逻辑：** 资产只存在于当前这条链（如 Ethereum 或 Polygon）的合约状态里。
        
    -   **开发者视角：** 如果你想把 ETH 跨链变成 Polygon 上的 ETH，你需要依赖第三方“桥”（Lock & Mint 机制），生成的是一个 Wrapped Token（如 WETH）。这个 WETH 和原链的 ETH 是割裂的，合约无法直接感知原链状态。
        
-   **ZRC-20:**
    
    -   **逻辑：** ZRC-20 代币代表了**外部链上的原生资产**（如 Ethereum 上的 ETH，Bitcoin 上的 BTC，BSC 上的 BNB），这些资产被锁定在 ZetaChain 的 TSS（阈值签名）账户中。
        
    -   **开发者视角：** 你在 ZetaChain 上部署合约，可以通过 ZRC-20 合约直接管理和操作外部链的资产。你虽然在操作 ZetaChain 上的代币，但协议层会自动将其转化为外部链的原生交易。
        

### 核心接口功能的扩展 (`withdraw` 是大杀器)

这是代码层面最直观的区别。ZRC-20 兼容 ERC-20 接口（意味着可以配合 Uniswap, Metamask 使用），但多了一个关键功能。

-   **普通 ERC-20:**
    
    -   标准方法：`transfer`, `approve`, `transferFrom`, `balanceOf`。
        
    -   **局限：** 调用 `transfer` 只是改变了当前链上的数字，外部世界毫无变化。
        
-   **ZRC-20:**
    
    -   标准方法：`transfer`, `approve`... (同 ERC-20)。
        
    -   **扩展方法：** `withdraw(bytes recipient, uint256 amount)`。
        
    -   **开发者视角：** 当你在合约里调用 `ZRC20.withdraw(btc_address, amount)` 时，**魔法发生了**：ZetaChain 协议层会捕获这个事件，通过 TSS 节点在**比特币主网**上发起一笔真正的 BTC 转账给 `btc_address`。
        
    -   **直观感受：** 你在写 Solidity，但你居然控制了比特币网络！
        

### Gas 费用的处理 (Gas Abstraction)

-   **普通 ERC-20:**
    
    -   你在以太坊上转账，消耗 ETH；在 Polygon 上转账，消耗 MATIC。逻辑很简单，也是隔离的。
        
-   **ZRC-20:**
    
    -   **要解决的问题：** 跨链操作通常**需要用户持有目标链的 Gas 代币**。
        
    -   **开发者视角：** ZRC-20 内置了 Gas 扣除逻辑。当你调用 `withdraw` 把资产提回原链（例如提现 BTC 回比特币网络）时，ZetaChain 协议会自动计算比特币网络的 Gas 费，并直接从你提现的 ZRC-20 金额中扣除（以 ZRC-20 代币支付）。
        
    -   **优势：** 你的用户不需要持有 BTC 也能完成涉及 BTC 的复杂交互，因为 Gas 费是自动从交易额里“扣”出来的。
        

### 对比特币（非智能合约链）的支持

这是 ZRC-20 最具颠覆性的地方。

-   **普通 ERC-20:**
    
    -   无法直接与比特币交互。你只能使用 WBTC（中心化托管的 ERC-20 代币）。你无法通过 Solidity 代码触发一笔比特币主网的转账。
        
-   **ZRC-20:**
    
    -   ZetaChain 为 BTC 这种非智能合约链的原生资产创建了 ZRC-20 版本的 BTC。
        
    -   **开发者视角：** 你可以写一个 Uniswap 风格的 DEX 合约（Solidity），一边是 ZRC-20 ETH，一边是 ZRC-20 BTC。用户通过比特币钱包转账 BTC 到 ZetaChain TSS 地址，你的合约自动接收并 Swap 成 ETH，然后用户在以太坊钱包收到 ETH。全程不需要 WBTC 这种中间商。
        

### 交互流程对比 (举例：跨链 Swap)

假设你要做一个应用：**用户支付 BTC，获得 ETH。**

-   **普通 ERC-20 开发路径：**
    
    -   用户去中心化交易所把 BTC 换成 WBTC。
        
    -   用户把 WBTC 跨链桥接到以太坊。
        
    -   用户在以太坊 DEX 用 WBTC 换 ETH。
        
    -   _开发者需要接入多个协议，用户体验极其割裂。_
        
-   **ZRC-20 开发路径 (Omnichain Contract)：**
    
    -   你在 ZetaChain 上部署一个合约。
        
    -   合约监听：当收到 BTC (通过 ZRC-20 hook `onCrossChainCall`)。
        
    -   合约逻辑：自动将 ZRC-20 BTC 换成 ZRC-20 ETH (在 ZetaChain 本地 Swap)。
        
    -   合约逻辑：调用 ZRC-20 ETH 的 `withdraw` 方法，填入用户的 ETH 地址。
        
    -   结果：用户发出 BTC，收到了 ETH。
        
    -   _开发者只需在一个合约里写逻辑，即由 ZetaChain 编排全链。_
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->

**学习目标**

-   建立对 “全链应用 / Universal App 合约” 的直观理解。
    
-   清楚后面要实现的 Hello World / Demo 会包含哪些模块（合约 + 前端 + RPC）。
    

**学习资料**

-   继续浏览 ZetaChain Developers 中的 EVM / Tutorials 部分
    
-   [https://www.zetachain.com/docs/developers](https://www.zetachain.com/docs/developers)
    

**扩展资料（可选）**

-   如果有时间，可提前快速扫一眼 Tutorials 中的 Swap / Messaging 结构。
    

**实践 / 作业**

-   在笔记中写出：
    
    -   自己想做的第一个 Universal App 想实现的“打印 / 记录 / 简单逻辑”是什么。
        
-   为后续的 Hello World Demo 决定一种工作流：
    
    -   使用 CLI + Hardhat / Foundry？
        
    -   用本地链还是测试网？
        

# 1.实现一个简单的合约

## 环境建立

初始化一个新项目 ZetaChain CLI（命令行界面）。

```Solidity
npx zetachain@latest new --project hello
cd hello
yarn 或者 npm install
forge soldeer update
```

## hello模板

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.26;

import "@zetachain/protocol-contracts/contracts/zevm/interfaces/UniversalContract.sol";

contract Universal is UniversalContract {
    event HelloEvent(string, string);

    function onCall(
        MessageContext calldata context,
        address zrc20,
        uint256 amount,
        bytes calldata message
    ) external override onlyGateway {
        string memory name = abi.decode(message, (string));
        emit HelloEvent("Hello: ", name);
    }
}
```

## testnet 部署

```Solidity
UNIVERSAL=$(forge create Universal \
  --rpc-url https://zetachain-athens-evm.blockpi.network/v1/rpc/public \
  --private-key $PRIVATE_KEY \
  --broadcast \
  --json | jq -r .deployedTo)
```

```Solidity
#查看合约地址：
echo $UNIVERSAL
```

用合约地址在[https://testnet.zetascan.com/上查询即可。](https://testnet.zetascan.com/上查询即可。)

![](https://ai.feishu.cn/space/api/box/stream/download/asynccode/?code=MjEwZmU0OWYzNTQxZmEyZjFkNTMzNzU5NWVjZGI1OGVfZkwxMXNSM3l6UjhGdkJmdzFtNjFaalhtb3NXUVlvd0lfVG9rZW46TWZRd2J1Mm5rb0QyVnp4bEFMMWM5eEhZbkllXzE3NjQyNTAzNzc6MTc2NDI1Mzk3N19WNA)

其实还可以这样编译和部署，这个会返回所有的部署信息：

```Solidity
# 1. 加载 Node 环境
source /root/.nvm/nvm.sh

# 2. 安装依赖（如果未安装）
npm install

# 3. 编译合约
npx hardhat compile

# 4. 部署
# 创建 .env 文件并配置私钥
echo "PRIVATE_KEY=你的64位私钥" > .env
npx tsx commands/index.ts deploy --private-key $(grep PRIVATE_KEY .env | cut -d '=' -f2)
```

# 2.CrossChainEcho (跨链传声筒)

目标：**我在以太坊（Sepolia）喊一句，ZetaChain 必须能听见并记下来。**

-   **输入 (Input)：**
    
    -   不转账代币，只从 Sepolia 发一笔交易，带上一段话（字符串，比如 "Hello Zeta!"）。
        
-   **简单逻辑 (Logic)：**
    
    -   ZetaChain 上的合约收到那串乱糟糟的十六进制数据后，得把它**解码**成人类能看懂的文字。
        
    -   顺便检查一下是不是发了个空消息。
        
-   **记录与打印 (Record & Print)：**
    
    -   **记下来：** 用个小本本（`mapping`）在链上存好：_“谁（Address）在什么时候说了什么”_。
        
    -   **吼出来：** 虽然不能 `console.log`，但我可以 `emit Event`。只要能在 ZetaScan 浏览器上看到这个 Event 弹出来，就算大功告成！
        

使用CLI + Hardhat+测试网
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->


ZetaChain & Universal Blockchain 核心概念

**学习目标**

-   理解 “通用区块链 / Universal EVM / Universal App / Omnichain Smart Contract” 的基本含义。
    
-   能用图的方式画出：ZetaChain + 多条公链 + Gateway 的大致结构。
    

**学习资料**

-   Universal Apps 概览
    

[https://www.zetachain.com/docs/start/app](https://www.zetachain.com/docs/start/app)

-   开发者总览（Universal EVM / Gateway / Cross-Chain）
    

[https://www.zetachain.com/docs/developers](https://www.zetachain.com/docs/developers)

**扩展资料（可选）**

-   为什么在 ZetaChain 上面开发？
    

[https://www.zetachain.com/docs/start/build](https://www.zetachain.com/docs/start/build)

-   ZetaChain 架构设计（粗略看一遍）
    

[https://www.zetachain.com/docs/developers/architecture/overview](https://www.zetachain.com/docs/developers/architecture/overview)

**实践 / 作业**

-   在笔记中用自己的话写出：
    
    -   Universal App 是什么？
        
    -   Gateway 大概做什么？
        
-   画一张简单的架构图：
    
-   ZetaChain 中心 + Bitcoin / Ethereum / Solana 等外围链 + Gateway。
    

# What is Universal App ?

## 理解

我的理解：部署在区块链（ZetaChain）上的一个智能合约，但它可以**同时连接和操作其他区块链。**

总结一下特点：

**1\. 单个合约，统治所有** 传统的跨链应用需要在每条链上都部署合约。你在以太坊部署一个。你在币安链部署一个。你在 Polygon 部署一个。 ZetaChain 的 Universal App 只需要**部署一次**。它部署在 ZetaChain 的 EVM（以太坊虚拟机）兼容层上。它就能控制连接到网络的所有链。这极大降低了开发难度。

**2\. 赋予比特币智能合约能力**

比特币本身不支持复杂的智能合约。普通的 DeFi 应用无法直接使用原生比特币。 Universal App 解决了这个问题。它通过 ZetaChain 的账户系统持有比特币。开发者可以在 Universal App 里编写逻辑来控制这些比特币。用户可以在 Uniswap 风格的界面里使用原生比特币交易。不需要把比特币包装成 WBTC。

**3\. ZRC-20 标准：万能转接头** ZetaChain 发明了一种代币标准叫 **ZRC-20**。 当用户把比特币、以太坊或 DOGE 充值进 ZetaChain 系统时。这些资产在 ZetaChain 内部会显示为 ZRC-20 代币。 开发者写代码时。他们只需要处理 ZRC-20 代币。这就像处理普通的 ERC-20 代币一样简单。 当用户提款时。ZRC-20 代币会被销毁。ZetaChain 会在对应的源链（比如比特币网络）上把原生资产转回给用户。

## 示例

```Solidity
pragma solidity 0.8.26;
 
import "@zetachain/protocol-contracts/contracts/zevm/interfaces/UniversalContract.sol";
 
contract UniversalApp is UniversalContract {
    function onCall(
        MessageContext calldata context,
        address zrc20,
        uint256 amount,
        bytes calldata message
    ) external virtual override {
        // ...
    }
}
```

## [Calling Universal App](https://www.zetachain.com/docs/start/app#calling-universal-apps)s

用户可以通过与连接链上的**网关合约**交互来调用通用应用。

每个连接链都有一个单一的网关合约，该合约会暴露向通用应用存入和调用代币的方法。用户可以同时传递数据以及调用通用应用时的令牌。

例如:一名以太坊用户向一个通用应用发送了1个ETH和一条消息“hello”

![](https://ai.feishu.cn/space/api/box/stream/download/asynccode/?code=NzFjZDQ0MGY5ZTU2M2U3ZWYyZWI0OGRmMGMwOWU3YjBfVEViMjlHbjZseVBicmdLQ00ydFlmdXd2dFY3MVJ5WkRfVG9rZW46S29iMWI0aXVMb21HRmN4TkFJR2NYZllmbmliXzE3NjQxNjU5Njg6MTc2NDE2OTU2OF9WNA)

# Gateway

-   功能上，它是连接 ZetaChain 和外部区块链（如以太坊、币安链、Polygon）的关键接口。
    
-   本质上，Gateway 是**部署在连接链（Connected Chains）上的智能合约**。
    

ZetaChain 本身是一条链。以太坊（Ethereum）是另一条链。它们俩语言不通，无法直接对话。ZetaChain 开发团队**在以太坊上部署了一个智能合约**，这个合约就叫 **Gateway**。所有进出 ZetaChain 的资金和信息，都要经过这个合约。

_注：对于比特币这种不支持智能合约的链，Gateway 的形式是一个受 TSS 技术控制的托管地址，功能类似，但实现方式不同。_

## Gateway 的核心职责

Gateway 主要负责处理两个方向的流量：**进（Inbound）**和 **出（Outbound）**。

A. 资产“进”站（存款/充值）

当你想把以太坊上的 USDT 放到 ZetaChain 上用时：

1.  **用户操作：** 你把 USDT 发送到以太坊上的 **Gateway 合约地址**。
    
2.  **锁定/托管：** Gateway 收到你的钱。它把钱锁在合约里。这笔钱现在由 Gateway 保管。
    
3.  **发出信号：** Gateway 会在以太坊上生成一个“事件日志”（Event Log）。
    
4.  **ZetaChain 观察：** ZetaChain 的观察者节点（Observers）盯着 Gateway。它们看到了这个信号。
    
5.  **铸造资产：** ZetaChain 在自己的网络里，凭空印出等量的 **ZRC-20 USDT** 给你。
    

**简单说：** Gateway 在外部链上替你保管原版资产，作为凭证。

B. 资产“出”站（提现/支付）

当你在 ZetaChain 上操作完毕，想把 USDT 提回以太坊钱包时：

1.  **用户操作：** 你在 ZetaChain 上销毁（Burn）了你的 ZRC-20 USDT。
    
2.  **ZetaChain 决策：** ZetaChain 网络验证这笔交易。网络节点达成共识：“好的，我们要把钱还给他。”
    
3.  **TSS 签名：** ZetaChain 的节点们共同生成一个签名（私钥是切片保存的，没有任何一个人能单独控制）。
    
4.  **执行命令：** 这个签名被发送给以太坊上的 **Gateway 合约**。
    
5.  **释放资金：** Gateway 验证签名。确认是 ZetaChain 官方发来的指令。它从锁定的资金池里解锁 USDT，转回给你的以太坊钱包。
    

**简单说：** Gateway 听从 ZetaChain 网络的集体指令，释放资金。

C. 信息传递（跨链调用）

Gateway 不止管钱，还管数据。

-   **调用外部合约：** 你在 ZetaChain 上的 Universal App 可以发出指令。这个指令传给 Gateway。Gateway 再去调用以太坊上的 Uniswap 或 Aave 合约。
    
-   **传递普通信息：** 它可以把一段文本或数据从这条链传到那条链。
    

## 关键技术点理解：MPC-TSS（**Threshold Signature Scheme-门限签名方案）**

核心原理：私钥分片

传统的区块链钱包有一把**私钥**。这把私钥像一把物理钥匙。谁拿着它，谁就能打开保险柜（控制资金）。如果私钥丢了，钱就丢了。如果私钥被偷了，钱就被偷了。

TSS 改变了这个规则。

**TSS 的做法：**

-   系统不生成一把完整的私钥。
    
-   系统通过数学算法生成很多个\*\***“私钥碎片**”\*\*（Key Shares）。
    
-   系统把这些碎片分给不同的人（在 ZetaChain 里是验证节点）。
    
-   **关键点：** 完整的私钥从来没有出现过。连拿到碎片的人都不知道完整的私钥长什么样。
    

工作机制：门限（Threshold）

“门限”是一个数字。它代表“最少需要多少人同意”。

我们假设一个场景：

-   **总人数 (n)：** 100 个节点。
    
-   **门限值 (t)：** 67 个节点。
    

**运作流程：**

1.  有人发起一笔提款请求。
    
2.  节点们检查这笔请求是否合法。
    
3.  如果节点同意，它就用自己的“碎片”进行一次数学计算。
    
4.  当有 67 个节点完成了计算。
    
5.  系统把这些计算结果拼在一起。
    
6.  数学魔术发生了：这产生了一个**有效的签名**。
    
7.  区块链看到这个签名。它认为这是合法的。它执行交易。
    

如果只有 66 个人同意。或者黑客偷了 50 个碎片。签名无法生成。交易会失败。

TSS 和 多重签名 (Multisig) 的区别

你可能会把 TSS 和多重签名钱包混淆。它们看起来很像。它们都需要多人同意。但它们在技术上有很大不同。

**多重签名 (Multisig)：**

-   **发生在链上：** 区块链知道有 3 个人要签名。
    
-   **成本高：** 3 个人签名。区块链要存 **3 个签名**的数据。手续费（Gas）很贵。
    
-   **隐私差：** 所有人都能看到是哪 3 个地址签了名。
    

**TSS 签名：**

-   **发生在链下：** 节点们在私下里进行数学计算。
    
-   **成本低：** 无论 100 人还是 1000 人参与。最终生成的签名**只有一个**。它看起来像普通的单人签名。手续费很便宜。
    
-   **隐私好：** 没人知道具体是哪几个人签的。外界只看到结果。
    

为什么 ZetaChain 必须用 TSS？

ZetaChain 无法在比特币网络上部署智能合约。比特币不支持这个。ZetaChain 必须拥有一个比特币地址来保管用户的 BTC。

**问题来了：** 谁掌握这个比特币地址的私钥？

-   如果是 CEO 掌握。他可能卷款跑路。
    
-   如果是多重签名。比特币网络对签名数量有限制（通常最多十几个人）。ZetaChain 有几百个节点。多重签名做不到。
    

**TSS 是唯一的解决方案：**

-   它能让几百个节点共同管理一个比特币地址。
    
-   它不需要比特币网络升级或支持新功能。
    
-   它让比特币网络以为这只是一个普通的个人用户在操作。
    

安全性优势

**没有单点故障：** 你需要一把钥匙开门。钥匙断了就麻烦了。TSS 没有完整的钥匙。如果一个节点断网了。或者一个节点的硬盘坏了。这没关系。只要剩下的节点数量还够“门限值”。网络就能正常工作。

**刷新机制 (Key Rotation)：** TSS 支持一种叫“密钥刷新”的技术。节点们可以定期更新手里的碎片。虽然碎片变了，但它们组合出来的那个“虚拟公钥”地址不变。 这意味着黑客必须在**同一时间**攻破 67 个节点才能偷到钱。如果黑客今天攻破一个，明天攻破一个。因为碎片已经刷新了。旧碎片就作废了。这极大地提高了安全性。

# 架构图

这个很好描述了他们之间的关系xxx

![](https://ai.feishu.cn/space/api/box/stream/download/asynccode/?code=MGRiYWRlZWIyYTBkNDFhZWYzM2I4OGU4MDkyNzgwNjBfblVhRU12TER3RTIydHc5b0c3YzdQaVVndEViR3EyOFBfVG9rZW46U2hqYmJzTUdsb09ueHV4elJFRmNZZWl5blFlXzE3NjQxNjU5Njg6MTc2NDE2OTU2OF9WNA)

# 通用区块链 / Universal EVM /Omnichain Smart Contract

### 通用区块链 (Universal Blockchain)

**定位：基础设施层 / 状态机**

这是一个专门为跨链互操作性构建的 Layer 1 区块链。它不仅仅是一个传输信息的“中继器”。它维护着一个全局状态。这个状态包含了所有连接链（比特币、以太坊等）的账户和资产信息。

**核心技术机制：**

-   **去中心化观察者 (Decentralized Observers)：** ZetaChain 的验证节点运行着一种名为“观察者”的客户端。这些客户端实时扫描外部链（如 Bitcoin 内存池或 Ethereum 区块）。它们寻找特定的交易或事件。一旦发现，节点们会通过共识机制确认这个事件确实发生了。这解决了“读”的问题。
    
-   **TSS 门限签名 (Threshold Signature Scheme)：** 这是“写”的核心。区块链没有单一的私钥来控制外部资产。系统生成一个数学上的分布式私钥。这个私钥被切分成碎片。碎片分发给所有验证节点。只有超过 2/3 的节点同时也签署交易，外部链上的 Gateway 才会执行操作。这保证了没有任何单一实体能窃取资金。
    
-   **基于 Cosmos SDK 的共识：** 通用区块链通常基于 Cosmos SDK 构建。这提供了极快的区块确认速度（Instant Finality）。这对于处理高频的跨链交易非常关键。
    

### Universal EVM (通用以太坊虚拟机)

**定位：执行引擎层 / 兼容与扩展**

标准的 EVM 是一个封闭的沙盒。它只能修改自身链上的数据。Universal EVM 打破了这个沙盒。它在标准 EVM 的基础上增加了 **zEVM 模块**。

**核心技术特性：**

-   **ZRC-20 资产映射标准：** 这是 Universal EVM 的核心创新。当比特币或以太坊进入系统时，系统会自动创建一个 ZRC-20 代币合约。这个合约是外部原生资产在 ZetaChain 内部的“影子”。
    
    -   开发者调用 ZRC-20 合约的 `transfer` 函数。
        
    -   Universal EVM 不仅更新内部账本。
        
    -   它还会触发底层的 TSS 模块，在外部链上发起真实转账。
        
-   **同步执行环境：** 在传统跨链中，A 链和 B 链是异步的。你在 A 链发令，不知道 B 链什么时候执行。在 Universal EVM 中，操作是同步的。你在一个区块内就能完成“读取输入 -> 计算逻辑 -> 改变状态”的过程。
    
-   **Gas 费抽象 (Gas Abstraction)：** 在 Universal EVM 中运行合约时，系统会自动计算外部链需要的 Gas 费。用户可以用一种代币（如 ZETA）支付所有费用。系统后台会自动把这些费用兑换成 ETH 或 BTC，用来支付外部矿工费。
    

### Omnichain Smart Contract (全链智能合约)

**定位：业务逻辑层 / 统一编排**

这是部署在 Universal EVM 上的智能合约代码。它实现了“单点部署，全网运行”。

**与传统跨链合约的区别：**

-   **传统模型 (Bridge/Messaging)：** 你需要在以太坊写一个合约。你需要在币安链写一个合约。你还需要在两条链之间架设一个消息中继。这增加了攻击面。状态也是割裂的。
    
-   **全链模型 (Omnichain)：** 你只在 ZetaChain 上写**一个**合约。这个合约拥有“上帝视角”。
    
    -   **输入：** 它可以接收来自比特币网络的 UTXO 作为触发条件。
        
    -   **逻辑：** 它可以执行复杂的 DeFi 逻辑（如 AMM 交易、借贷清算）。
        
    -   **输出：** 它可以把结果资产分散发送到 Polygon 和 Solana。
        
-   **原子性与回滚 (Atomicity & Revert)：** 全链合约需要处理复杂的失败情况。如果外部链（如以太坊）的交易因为滑点过大失败了，全链合约包含了一套回滚机制。这保证了资金要么交易成功，要么原路退回。它不会卡在中间状态。
    

### 总结三者的协作流

1.  **通用区块链** 提供了物理连接和安全共识（眼睛和手）。
    
2.  **Universal EVM** 提供了一个能理解多链资产的计算环境（大脑）。
    
3.  **Omnichain Smart Contract** 告诉系统具体要做什么业务（思维逻辑）。
    

# Q&A

**Q：ZetaChain 决策与 TSS 签名它们两者的核心关系是什么？**

**A：** 它们是大脑**和手**的关系。

-   **ZetaChain 决策**是“大脑”。它负责判断。它决定这笔钱该不该转。
    
-   **TSS 签名**是“手”。它负责执行。它负责把钱实际转出去。 只有大脑发出了指令，手才会动。
    

**Q：在资产“出站”时，第一步发生了什么？**

**A：** 第一步是**决策（共识）**。 当你在 ZetaChain 上发起提现。你会先销毁（Burn）你手里的 ZRC-20 代币。 ZetaChain 的所有验证节点会检查这笔交易。 节点们会确认：“是的，他确实销毁了代币。他的操作是合法的。” 当超过 2/3 的节点都确认了这件事。这就形成了“ZetaChain 决策”。

**Q：TSS 签名是什么时候开始的？**

**A：** TSS 签名紧跟着决策之后开始。 一旦节点们达成了“决策”。它们就会拿出各自保存的私钥碎片。 它们开始进行多方计算（MPC）。 它们共同生成一个签名。这个签名就是给外部链（比如以太坊）看的通行证。

**Q：为什么不能跳过决策，直接签名？**

**A：** 因为没有节点拥有完整的私钥。 私钥被切分成了很多碎片。每个节点只有一片。 如果你想生成有效的签名。你需要大部分节点配合你。 如果网络没有达成“决策”。其他节点就不会配合你进行计算。 你就无法生成签名。你就无法动用 Gateway 里的资金。

**Q：Gateway 怎么知道这个签名是有效的？**

**A：** Gateway 不关心背后的决策过程。 Gateway 只认签名。 ZetaChain 生成的 TSS 签名。在数学上看起来和一个标准的以太坊签名完全一样。 Gateway 看到签名是正确的。它就会认为这是 ZetaChain 官方发来的指令。 它就会打开金库放行资金。

**Q：这一套机制解决了什么安全问题？**

**A：** 它解决了\*\*“内部作恶”\*\*的问题。 在传统的中心化交易所里。管理员一个人就能拿私钥卷款跑路。 在 ZetaChain 里。没有任何一个人、或者一个节点拥有完整的私钥。 想要把钱转走。必须通过全网的“决策”。 这保证了只有合法的交易才能让资金出站。
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->



**学习目标**

-   本地 / 云端完成基础开发环境落地。
    
-   能描述自己接下来 2 周的学习目标。
    

**学习资料**

-   ZetaChain CLI
    
-   [https://github.com/zeta-chain/cli](https://github.com/zeta-chain/cli)
    
-   ZetaChain Node / RPC / Faucet / Explorer / 测试币获取
    
-   [https://www.zetachain.com/docs/reference/](https://www.zetachain.com/docs/reference/)
    
-   Qwen API 参考（含 OpenAI 兼容方式）
    
-   [https://www.alibabacloud.com/help/zh/model-studio/qwen-api-reference](https://www.alibabacloud.com/help/zh/model-studio/qwen-api-reference)
    

**扩展资料（可选）**

-   再快速浏览一遍 ZetaChain Developers
    
-   [https://www.zetachain.com/docs/developers](https://www.zetachain.com/docs/developers)
    

**实践 / 作业**

-   安装或尝试使用 ZetaChain CLI（本地或云端环境均可）。
    
-   了解测试网 RPC、Faucet、Explorer 的入口，记录在自己的笔记中。
    
-   在终端或 Postman 里完成一次 Qwen API 的简单请求（哪怕只是 200 报错也可以，先打通连通性）。
    

# 1.安装ZetaChain CLI

## ZetaChain CLI

ZetaChain CLI：用于构建和交互 [ZetaChain](https://www.zetachain.com/) 通用应用的命令行接口。它是开发者、节点验证者（Validators）和高级用户用来“控制”和“查询”ZetaChain 网络的“遥控器”。由于 ZetaChain 是基于 Cosmos SDK 构建的，其 CLI 的操作逻辑与大多数 Cosmos 生态的链非常相似。

### 核心组件：`zetacored`

ZetaChain 的主要二进制文件通常被称为 `zetacored`。它集成了运行节点和作为客户端交互的所有功能。

### 主要功能模块

ZetaChain CLI 的功能主要分为以下几大类：

A. 钱包与密钥管理 (Key Management)

用于生成和管理账户。

-   **创建账户：** 生成新的助记词和私钥。
    
-   **恢复账户：** 导入助记词。
    
-   **查看地址：** 获取你的 ZetaChain 地址（通常以 `zeta` 开头）。
    
-   _命令示例：_ `zetacored keys add mywallet`
    

B. 查询网络状态 (Querying)

用于从区块链上获取只读信息，不需要消耗 Gas 费。

-   **查余额：** 查看 ZETA 代币余额。
    
-   **查交易：** 追踪跨链交易的状态。
    
-   **查区块信息：** 查看当前区块高度。
    
-   **查跨链信息：** 查询 ZetaChain 观察到的比特币或以太坊网络的状态（这是 ZetaChain 特有的，涉及 `observer` 模块）。
    
-   _命令示例：_ `zetacored query bank balances [地址]`
    

C. 发送交易 (Transactions)

用于向网络写入数据，需要消耗 ZETA 作为 Gas 费。

-   **转账：** 发送 ZETA 代币给其他人。
    
-   **质押 (Staking)：** 将代币质押给验证者以获得奖励。
    
-   **治理 (Governance)：** 对网络提案进行投票。
    
-   **部署合约：** 尽管通常使用 Hardhat/Foundry 部署 EVM 合约，但 CLI 也可用于底层交互。
    
-   _命令示例：_ `zetacored tx bank send [发送者] [接收者] 1000000azeta`
    

D. 节点运营 (Node Operations)

这是验证者使用的功能。

-   **启动节点：** 运行全节点或验证者节点，同步区块数据。
    
-   **初始化：** 配置创世文件 (genesis.json)。
    
-   _命令示例：_ `zetacored start`
    

## 先决条件

-   Node.js ≥ 18
    
-   Git（用于模板克隆）
    
-   （可选）Docker ≥ 24 for`localnet`
    

## 本地运行

```PowerShell
npx zetachain@next new
```

## 全局安装

```PowerShell
npm install -g zetachain@latest
```

## 验证

```PowerShell
zetachain query chains list
// 此命令会打印出当前连接到 ZetaChain 的所有链，即成功
```

![](https://av1ln7qpa6l.feishu.cn/space/api/box/stream/download/asynccode/?code=NjVhODdjZGNjOWM0NmQ1NTJlZDBhMjkwNmJjNGFjNmVfbWwzMWIwV1p5N3dsdm41dkVmbHhkQUNDd3dCbHpCWVJfVG9rZW46TmlNWmJuQm96b2U5TjZ4YVJnV2N1ZmkwbndkXzE3NjQwNjQ2ODQ6MTc2NDA2ODI4NF9WNA)

## Example

创建一个新项目：

```Plain
zetachain new
```

启动localnet：

```Plain
zetachain localnet start
```

![](https://av1ln7qpa6l.feishu.cn/space/api/box/stream/download/asynccode/?code=NDVmMTc3MDRiMDg4Mzc5MWY2OTA5NTE3YmUwNmUyOGVfZFo1UHEwUTVxMHpEVk5TV09RQWZ5MFAzT29DUFdudWpfVG9rZW46U1lGYWJQZHZwb0J6WDR4WGdZT2NJNUowblJnXzE3NjQwNjQ2ODQ6MTc2NDA2ODI4NF9WNA)

查询跨链余额：

```Plain
zetachain query balances --evm <钱包地址>
```

注意：要有钱包（无则创建： npx zetachain accounts create --name <name> ）

![](https://av1ln7qpa6l.feishu.cn/space/api/box/stream/download/asynccode/?code=YmEwOWNmODg0MGQ5NGJlNGE3NDUxNjBhZDA2ZmZkYjNfR0M3OWh0ZkJTS1pqRkZZSTRyMDhTdG5oVU5Dcnh5dzNfVG9rZW46VUx5VmJveWllb3M5NXp4SlYwbWNuM09IbmNoXzE3NjQwNjQ2ODQ6MTc2NDA2ODI4NF9WNA)

## CLI参考

完整指挥文件：

[https://www.zetachain.com/docs/reference/cli/](https://www.zetachain.com/docs/reference/cli/)

```Plain
zetachain docs
```

或者配合任何命令使用：`--help`

```Plain
zetachain accounts --help
```

# 2.项目前置准备

1\. Faucet领取测试币：

[https://www.zetachain.com/docs/reference/faucet/](https://www.zetachain.com/docs/reference/faucet/)

[https://zetachain.faucetme.pro/](https://zetachain.faucetme.pro/)

![](https://av1ln7qpa6l.feishu.cn/space/api/box/stream/download/asynccode/?code=OTY1Yjc5MzVlMzlmYjRhODQ2Y2FjYmE3YWE3M2QyYWFfcmdOdDg2dkI4bDljWHJqVjlDNVZsRnNQbHp1cHE3cktfVG9rZW46VVk3amJzQnEzb0R3MEJ4U0JDQmNiOEtjbmpoXzE3NjQwNjQ2ODQ6MTc2NDA2ODI4NF9WNA)

2.主网和测试网：

[https://www.zetachain.com/docs/developers/chains/list/](https://www.zetachain.com/docs/developers/chains/list/)

3.RPC 节点：

[https://www.zetachain.com/docs/reference/network/api](https://www.zetachain.com/docs/reference/network/api)

使用allthatnode:

[https://www.allthatnode.com/project.dsrv?seq=1c82ed14a14d215be82f31de6e1027359381cdbc](https://www.allthatnode.com/project.dsrv?seq=1c82ed14a14d215be82f31de6e1027359381cdbc)

![](https://av1ln7qpa6l.feishu.cn/space/api/box/stream/download/asynccode/?code=MzRlOTRmMTZkNDU3ZDI1NjkwN2RkZmMzMGZhMDY4MWFfbDA1RkZIang2R3VLc3ZubXR4S2JkbEt6YWdTdld1c2NfVG9rZW46RnZXeGJqQXJtb0trVVF4WHFwbmN6eTN5bjRkXzE3NjQwNjQ2ODQ6MTc2NDA2ODI4NF9WNA)

# 3.回顾一下RPC（**Remote Procedure Call**（远程过程调用））

RPC (Remote Procedure Call，远程过程调用)是一种进程间通信 (IPC)技术。

它允许一台计算机上的程序（客户端）调用另一台计算机（服务器）上的子程序或函数，而程序员无需显式编写远程交互的底层网络代码。

在区块链（特别是 ZetaChain 这种基于 Cosmos SDK 且兼容 EVM 的架构）的上下文中，RPC 的技术定义和作用如下：

### 1\. 技术定义：区块链节点的 API 层

区块链网络由分布式的**\*\*节点 (Nodes)\*\*** 组成。这些节点维护着账本的\*\*状态 (State)\*\*（如账户余额、合约代码、存储槽数据）。节点通常运行在一个隔离的环境中。

RPC 接口是节点软件暴露给外部世界的\*\***标准 API 端点**\*\*。它使得外部客户端（如您的 ZetaChain CLI、DApp 前端、索引器）能够与节点进行通信。

从架构上看，它遵循 **Client-Server 模型**：

-   **Client (Stub):** 您的开发环境。它将本地的方法调用（如 `query balances`）打包（序列化）成网络请求。
    
-   **Transport:** 数据包通过网络协议（通常是 HTTP 或 WebSocket）传输。
    
-   **Server (Node):** 区块链节点接收请求，解包，在本地状态机中执行查询或将交易注入内存池 (Mempool)，然后将结果序列化返回。
    

### 2\. ZetaChain 中的双重 RPC 架构（关键专业知识）

由于 ZetaChain 是基于 Cosmos SDK 构建并兼容 EVM（以太坊虚拟机）的，它的 RPC 架构比普通公链更复杂，通常包含两层：

A. JSON-RPC (EVM 层)

这是以太坊生态的标准协议。当您使用 `npm install -g zetachain` 安装的 JS CLI 开发智能合约时，主要交互的是这一层。

-   **协议标准：** JSON-RPC 2.0 (无状态、轻量级)。
    
-   **数据格式：** JSON。
    
-   **典型端口：** 8545。
    
-   **主要用途：**
    
    -   部署 Solidity 合约 (`eth_sendRawTransaction`)。
        
    -   调用合约方法 (`eth_call`)。
        
    -   获取 EVM 层的区块和交易信息。
        
-   **示例 Payload：**
    
    ```JSON
    {
      "jsonrpc": "2.0",
      "method": "eth_getBalance",
      "params": ["0xYourAddress", "latest"],
      "id": 1
    }
    ```
    

B. gRPC & REST (Cosmos SDK 层)

这是 ZetaChain 底层共识和跨链模块的通信协议。当您涉及质押 (Staking)、治理或底层跨链观察者 (Observer) 数据时，会用到这一层。

-   **gRPC:** 基于 HTTP/2，使用 Protocol Buffers (Protobuf) 进行二进制序列化。性能极高，延迟低。主要用于节点间通信或高性能客户端（如 `zetacored` Go CLI）。
    
    -   _典型端口：9090_
        
-   **REST (LCD):** 将 gRPC 封装为 HTTP 接口，方便网页端调用。
    
    -   _典型端口：1317_
        

### 3\. 开发中的关键概念

在专业开发中，理解 RPC 意味着你需要关注以下指标：

1.  **Endpoint (端点 URL):**
    

1.  这是 RPC 服务的入口地址。
    
    -   _Public RPC:_ 官方或社区提供的免费节点（如您在教程中自动连接的）。通常有\*\*速率限制 (Rate Limit)\*\*，比如每秒只能请求 10 次。
        
    -   _Private RPC:_ 使用 Infura、Alchemy 或 BlockPi 等基础设施服务商提供的专用节点，高并发、更稳定。
        

2.  **WebSockets (WSS) vs HTTP:**
    
    1.  **HTTP:** 它是“拉”模式 (Pull)。您的 CLI 必须不断询问“交易确认了吗？”
        
    2.  **WebSockets:** 它是“推”模式 (Push)。您可以建立长连接，订阅事件（如 `logs`），一旦链上有特定事件发生，节点会主动通过 RPC 推送消息给您的程序。
        
3.  **状态最终性 (Finality):**
    

1.  通过 RPC 查询到的数据取决于节点的同步状态。如果连接的 RPC 节点落后于网络（未同步），您获取的数据就是旧的。
    

### 总结

在您的开发流程中：

**RPC 是将您的业务逻辑（JavaScript/TypeScript 代码）与去中心化网络状态（ZetaChain 区块链数据）连接起来的通信协议和网关。**

当您执行 `zetachain query` 时，您实际上是在构造一个 **HTTP 请求**，发送给远程服务器上的 **JSON-RPC 接口**，服务器查询其本地的 **LevelDB/RocksDB 数据库**，然后将十六进制的结果返回给您并进行解码。

# 完成一次 Qwen API 的简单请求

看到群友“菜鸟Appler-R"的调用，重现了一下hhh

![](https://av1ln7qpa6l.feishu.cn/space/api/box/stream/download/asynccode/?code=OGU4YTRmNGNkMzVkMWJiZWYwMTQxNTg1ZTIyZGM3MGJfQVVnajhMUTdtejF0RklUVDZyeHdEQWlZbTJMN0R6R2lfVG9rZW46Tm5wTmJNNnJLb2V4d014UlV5VmNKNTRObkJjXzE3NjQwNjQ2ODQ6MTc2NDA2ODI4NF9WNA)
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->




# DAY1

**学习资料**

-   ZetaChain 总文档
    
-   [https://www.zetachain.com/docs/](https://www.zetachain.com/docs/)
    
-   ZetaChain Developers（入口总览）
    
-   [https://www.zetachain.com/docs/developers](https://www.zetachain.com/docs/developers)
    
-   Qwen 总文档（Quickstart / Key Concepts）
    
-   [https://qwen.readthedocs.io/zh-cn/latest/](https://qwen.readthedocs.io/zh-cn/latest/)
    

# 什么是ZetaChain

目标：不同链之间互联，能够原生访问任何区块链，让加密货币变得像互联网一样易于访问、多样化和互联

ZetaChain 是一个赋能开发者创建通用，能够无缝交互任何区块链的应用。

官网的developer界面

![](https://av1ln7qpa6l.feishu.cn/space/api/box/stream/download/asynccode/?code=ODYzZjU5M2FjMDgyOGIzZTFmMWU5MDMwYzYyZjZmZTlfNFZydGwwZDF0SEIxSzA0VEpPOWJJOTFxWVR2VnQ3MjNfVG9rZW46WFU2ZmJoQ0xmb05lcUd4OGZoWmN5ano1blFlXzE3NjM5Njk2MzM6MTc2Mzk3MzIzM19WNA)

# 环境搭建

## 先决条件

-   [**Node.js**](https://nodejs.org/en)**（推荐v21或更晚版本）：**必须 运行 ZetaChain CLI 并管理 JavaScript 依赖。(node -v)
    
-   **Yarn或者NPM：**用于安装和 更新项目库。两种方法都可以，随你喜欢。(npm version)
    
-   [**Git**](https://git-scm.com/)**:**管理项目资源必不可少 编程和与他人协作。
    
-   [**JQ**](https://jqlang.org/)**:**一个轻量级命令行JSON处理器， 对于解析Localnet输出和编写shell脚本非常有用。(win bash:winget install jqlang.jq)
    
-   [**Foundry**](https://getfoundry.sh/)**:**一个快速、模块化的以太坊工具包 发展。你会用它来编译契约、管理依赖和运行 Localnet。
    

### Windows

在 Windows 上安装 Foundry 需要一些额外的步骤，因为`foundryup` 目前不支持 PowerShell 或命令提示符 (Cmd)。您需要使用 Windows Subsystem for Linux (WSL) 或 Git BASH 来进行安装。\[[1](https://www.google.com/url?sa=E&q=https%3A%2F%2Fvertexaisearch.cloud.google.com%2Fgrounding-api-redirect%2FAUZIYQF7zCQXvNYyf1lIuwj_SykCNnDx-Fs34VfoYyf3ctjVjTHBXDt9T0oqAn542wsnl088smS_Lj9n5l0cI11E24eFj4qehO9A6AVp_pYN5L5ge9jDRaDvVX27gbPWQ6fqk1C7PraRa8LPv4_aAiap)\]\[[2](https://www.google.com/url?sa=E&q=https%3A%2F%2Fvertexaisearch.cloud.google.com%2Fgrounding-api-redirect%2FAUZIYQGzBjoCSkl_rKeTK2BbapcE-MR8_4zguUZ9YJcyYxxAba1j2J8NNYf7Ngy3woDdVyZklosYbRjydKx1rmO2L3EchMCOQmvlb60R0ca6L4t2ptSATdjLG3pCcg9-kkO_oWYGTzb3fsmBgw9C8qVaXox6pr0W8S2782AXTvGgkg7KxFeIBh6SWdN_l4YBnA%3D%3D)\]

**步骤 1：安装 Windows Subsystem for Linux (WSL)**

1.  打开 PowerShell 或 Windows 命令提示符，并以管理员身份运行。
    
2.  输入以下命令来安装 WSL：
    
3.  codeBash
    

```Plain
wsl --install
```

**步骤 2：在 WSL 中安装 Foundry**

1.  打开您的 WSL 终端 (例如 Ubuntu)。
    
2.  运行与 macOS 和 Linux 相同的 `curl` 命令来安装 `foundryup`：
    

```Plain
curl -L https://foundry.paradigm.xyz | bash
```

1.  按照屏幕上的指示完成安装，然后运行 `foundryup` 来安装 Foundry 工具链：
    

```Plain
foundryup
```

## 环境建设

Install the ZetaChain CLI:

```Shell
npm install -g zetachain
```

Run:

```Shell
zetachain query chains list
```

这个命令可以打印所有当前连接在ZetaChain上的链条

对于本地开发，推荐运行Localnet，一个自给自足的平台。该环境包括ZetaChain、EVM、Solana、Sui和TON。Localnet 是构建和测试通用应用最快的方法，无需依赖公共测试网。

注意：必须要有Foundry才可以运行Localnet

# qwen账号注册和进入控制台

1.进入阿里云百炼，注册，创建API key即可

[https://bailian.console.aliyun.com/?tab=model#/model-market/detail/qwen3-max](https://bailian.console.aliyun.com/?tab=model#/model-market/detail/qwen3-max)
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
