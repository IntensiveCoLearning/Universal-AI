---
timezone: UTC+8
---

# lihua

**GitHub ID:** Free-riding

**Telegram:** @nowhere

## Self-introduction

Again and again ~

## Notes

<!-- Content_START -->
# 2025-11-30
<!-- DAILY_CHECKIN_2025-11-30_START -->
# 跨链SWAP

普通的链只能从它当前链进行swap

ZetaChain的swap工作流程

从其他链上的U转换成ZRC-20ETH，在转化成对应的token，最后提到目标链上

Ethereum ETH ——> **ZRC20 ETH.ETH---------swap-------------ZRC20 BTC** ——>Bitcoin BTC

```
function onCrossChainCall(
    bytes calldata origin, //源链信息
    uint256 originChainID, //源链 ChainID
    address zrc20, //源链 ZRC-20 代币合约地址
    uint256 amount, //代币数量
    bytes calldata message //跨链信息（包括了目标链ID，目标资产，数量等等）
) external {
    //1.解析信息
    (address targetToken, uint256 minAmount) = abi.decode(
        message,
        (address, uint256)
    );
    
    //2.跨链交互逻辑的实现
    uint256 amountOut = calculateSwapAmount(amount);
    require(amountOut >= minAmount, "invalid output");

    //3.提取到目标链上
    IZRC20(targetToken).withdraw(amountOut, recipient, targetChainID);
}
```
<!-- DAILY_CHECKIN_2025-11-30_END -->

# 2025-11-29
<!-- DAILY_CHECKIN_2025-11-29_START -->

# ZetaChain函数

```
interface IZRC20{
    #存储
    function deposit() external payable;
    #提取
    function withdrraw(uint256 amount)external;
    #转账
    function transfer（address to,uint256 amount）external returns（bool）;
    #查询余额
    function balanceOf(address account) external view returns (uint256);
}
```
<!-- DAILY_CHECKIN_2025-11-29_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->


# 跨链

也称互操作性，是指不同的区块链系统之间能够进行**数据和资产**的通信和协同工作的能力。

### **跨链的常见实现方式**

**跨链桥**（Cross-chain Bridge）

-   用户在源链锁定资产，目标链铸造等值的“封装资产”（如 WBTC、wETH）。 ------- A锁B铸，B销毁A释放，中间会有消息监听
    

### ZetaChain的跨链

-   星线型结构
    

ZetaChain作为中心hub，其他链通过ZetaChain进行交互

# Universal EVM(通用的以太坊虚拟机)

也成为全链以太坊虚拟机，使ZetaChain可以直接操作以太坊和其他链上的资产。以往在不同链上开发跨链应用，需要学习不同环境下的技术：

-   Solana rust
    
-   Sui Move
    
-   Bitcoin 模板
    

ZetaChain使用通用智能合约完成不同链上的操作，极大降低了学习成本

# Universal Contract(通用智能合约)

ZetaChain内置了不同跨链操作的智能合约模板，向外暴露接口，内部相当于是黑盒，用户只需关注接口的规范性，不需要关心底层做的交互

# Universal DAPP(通用应用)

能实时的读取并更新连接网络状态。得益于ZetaChain的GateWay客户端，这个客户端充当了观测者和验证者。

ZetaChain有三种不同的节点：观察者，验证者，签名者（有不同的激励机制来确保安全性）

状态监听：

观测者监听其他链的网络（BitCoin,ETH等）的信息，这些网络与ZetaChian的GateWay发生交互后，观测者就会读取这些网络的状态，经过验证者的5秒的出块确认（未来会继续降低）。在通过GateWay与目标链进行交互。

资产转移：

其他链上符合要求的资产会转化成ZRC-20（通用资产）放到一个pool里面，用于资产的流动。

# GateWay(网关)

不同的链，ZetaChain对应了不同的网关，部署了不同网关合约

大致的跨链流程为：A链——网关——ZetaChain——通用合约处理——网关——B链

# 架构

TODO

ZetaChain 的共识机制
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->



加班，明天补笔记
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->




### **1\. 通用区块链（Universal Blockchain）**

-   **含义**：指一种支持跨多条区块链运行、具备广泛兼容性和互操作能力的区块链架构或网络。
    
-   **特点**：
    
    -   不局限于单一链的共识、状态或资产；
        
    -   通常通过中继、桥接、共享安全或模块化设计实现多链协同；
        
    -   可作为“中间层”或“聚合层”，协调多个异构链之间的交互。
        
-   **示例项目**：Cosmos（通过 IBC 协议）、Polkadot（通过 XCM 和平行链）、LayerZero、Chainlink CCIP 等。
    

* * *

### **2\. Universal EVM（通用以太坊虚拟机）**

-   **含义**：一种在多条区块链上统一部署和运行 EVM（Ethereum Virtual Machine）兼容智能合约的能力。
    
-   **特点**：
    
    -   允许开发者用 Solidity 编写一次合约，即可部署到多个 EVM 兼容链（如 Polygon、Arbitrum、Optimism、BNB Chain、Avalanche C-Chain 等）；
        
    -   “Universal” 强调的是跨链一致性：相同的字节码、相同的调用逻辑、相同的账户模型；
        
    -   有时也指通过抽象层实现“一次部署，多链运行”的运行时环境。
        
-   **目标**：消除 EVM 链之间的碎片化，提升开发效率和用户体验。
    
-   **相关技术**：EVM 等效性（EVM Equivalence）、统一账户系统、跨链合约部署工具。
    

* * *

### **3\. Universal App（通用应用）**

-   **含义**：指可在多条区块链上无缝运行、无需为每条链单独部署或适配的应用程序。
    
-   **特点**：
    
    -   用户无论使用哪条链（如 Ethereum、Base、zkSync、Solana 等），都能访问同一个应用；
        
    -   应用后端或智能合约逻辑能自动感知并适配不同链的环境；
        
    -   通常依赖跨链消息传递（Cross-chain Messaging）或通用账户抽象（如 ERC-4337 + 跨链钱包）。
        
-   **目标**：提升用户体验（无感知切换链）、聚合流动性、避免应用碎片化。
    

* * *

### **4\. Omnichain Smart Contract（全链智能合约）**

-   **含义**：一种原生支持跨多条区块链执行逻辑的智能合约，可在多个链之间传递状态、资产或消息，并保持一致性。
    
-   **关键特征**：
    
    -   合约逻辑分布在多个链上，但被视为一个整体；
        
    -   通过跨链通信协议（如 LayerZero、Wormhole、Axelar、Hyperlane）实现链间同步；
        
    -   支持跨链条件执行（例如：在 Chain A 上锁定资产，自动在 Chain B 上释放）。
        
-   **与传统跨链桥的区别**：
    
    -   传统桥：资产从 A 链“搬”到 B 链（1:1 锚定）；
        
    -   Omnichain 合约：逻辑本身跨链存在，状态动态同步，无需“搬运”。
        

在 Web3 中，**Universal App** 通常指：

> **一种无需强制用户绑定特定钱包、链或身份，即可在多链、多钱包、多设备上无缝使用的去中心化应用（dApp）。**

**核心特征：**

-   **链无关（Chain-agnostic）**：支持以太坊、Polygon、Solana、Arbitrum 等多条区块链。
    
-   **钱包无关（Wallet-agnostic）**：用户可以用 MetaMask、Coinbase Wallet、Phantom、Smart Wallet（如 ERC-4337 账户抽象钱包）等任意钱包登录。
    
-   **基于去中心化身份（DID）**：用户以一个统一的身份（如 ENS、.eth 名称，或 DID 标准如 EIP-725/EIP-4361）登录，而不是依赖某个钱包地址。
    
-   **状态可迁移 / 跨链一致体验**：用户在不同链上的资产、行为、声誉可被聚合，提供一致的 UI/UX。
    

在 Web3 中，Gateway 通常指：

连接 Web2 与 Web3、或连接不同区块链/协议之间的“入口”或“桥梁”服务。

| 去中心化身份网关（Identity Gateway） | 允许用户用邮箱、Google 登录等方式创建/恢复钱包（如 Web3Auth、Privy），降低 Web3 入门门槛。 |
| 跨链网关（Cross-chain Gateway） | 实现资产或消息在不同链之间传递，比如 LayerZero、Synapse、Wormhole 提供的跨链桥服务（虽然“Bridge”更常用，但有时也称 Gateway）。 |
| API 网关（Web3 API Gateway） | 开发者通过统一接口访问多链数据，如Alchemy、Infura、Moralis提供的节点+API服务，隐藏底层区块链复杂性。 |
| 应用入口网关（App Gateway） | 某些 dApp 或钱包提供“网关”功能，让用户从 Web2 应用（如 Twitter、Gmail）直接进入 Web3 体验（例如：通过 Farcaster Frame 嵌入互动）。 |

# ZetaChain 架构图

```
          +---------------------+
          |                     |
          |    ZetaChain        |  ← 中心链（共识层 + 跨链逻辑）
          |   (Central Chain)   |
          |                     |
          +----------+----------+
                     |
          +----------+----------+
          |                     |
          |      Gateway        |  ← 跨链网关（监听/中继/验证）
          |                     |
          +----------+----------+
                     |
      +--------------+--------------+
      |              |              |
+-----v-----+  +-----v-----+  +-----v-----+
|           |  |           |  |           |
|  Bitcoin  |  | Ethereum  |  |  Solana   |
| (外链 A)  |  | (外链 B)  |  | (外链 C)  |
|           |  |           |  |           |
+-----------+  +-----------+  +-----------+
```
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->





# ZetaChain和区块链一些基础

## ZetaChain

-   ZetaChain是 主链（Layer 1）
    
-   全链智能合约，开发者编写一次智能合约，即可在任意连接链上接收事件并执行逻辑
    
-   ZetaChain 的独特优势在于支持非智能合约链如：Bitcoin
    
-   通过协议层原生支持跨链消息与资产，无需桥或封装
    

## Layer 1 和Layer 2

### L1

-   Layer 1 是指底层的主区块链协议本身，它主要负责共识机制（如 PoW、PoS），数据存储（账本），原生代币（如 BTC、ETH）和基础安全性与去中心化
    
-   L1 的扩容通过**直接修改底层协议**实现
    
-   安全性高（直接由主链共识保障）
    
-   去中心化程度通常较高
    
-   无需依赖外部系统
    
-   升级困难（需全网共识，硬分叉风险）
    
-   扩容能力有限（如 Bitcoin TPS ≈ 7，Ethereum ≈ 15–30）
    
-   交易费用高、速度慢（尤其在高负载时）
    

### L2

-   **Layer 2 是构建在 Layer 1 之上的附加协议或网络**，目的是**提升交易吞吐量、降低费用，同时继承 L1 的安全性**。L2 本身不独立维护完整区块链，而是将大部分计算或存储“移出主链”，只在必要时与 L1 交互。
    
-   **高 TPS**：可达数千甚至上万笔/秒（如 zkSync 目标 2000+ TPS）
    
-   **低 Gas 费**：交易成本降低 10–100 倍
    
-   **快速确认**：部分操作秒级完成
    
-   **兼容 EVM**：多数 Rollup 支持以太坊智能合约无缝迁移
    
-   **复杂性高**：用户需理解“桥接”“提款延迟”等概念
    
-   **提款延迟**（Optimistic Rollup）：通常需 7 天挑战期
    
-   **中心化风险**（早期 L2）：排序器（Sequencer）可能由单一实体控制
    
-   **碎片化**：L2 之间互通困难（如 Arbitrum 和 Optimism 资产不能直接互转）
    

## ZetaChain的领水地址(需要登陆Discord)

[FAUCETME](https://zetachain.faucetme.pro/)

## 重新注册了Discord

## 重新注册了国内版的阿里云百炼（国外的需要用非内地手机号）

[阿里云百炼](https://bailian.console.aliyun.com/?tab=model#/efm/model_experience_center/text)

## 下载了PostMan

[PostMan](https://www.postman.com/downloads/)

需要注意处理器版本，看看是不是RAM版本

## 初始化ZetaChain环境

```
npx zetachain@latest new --project hello
cd hello
yarn
forge soldeer update
```

初始化成功后，会自动生成一个通用合约模板

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

// 引入 ZetaChain 协议合约中的 UniversalContract 接口
import "@zetachain/protocol-contracts/contracts/zevm/interfaces/UniversalContract.sol";

// 继承 UniversalContract，成为“全链合约”
contract Universal is UniversalContract {
    // 定义一个事件，用于日志输出
    event HelloEvent(string, string);

    // 重写 onCall 方法，处理跨链调用
    function onCall(
        MessageContext calldata context,   // 跨链消息的上下文信息
        address zrc20,                     // 发送的 ZRC-20 代币地址（如 zBTC, zETH）
        uint256 amount,                    // 发送的代币数量
        bytes calldata message             // 用户自定义的跨链消息数据
    ) external override onlyGateway {      // 仅允许 ZetaChain 网关调用
        // 从 message 中解码出一个字符串
        string memory name = abi.decode(message, (string));
        // 触发事件，打印 "Hello: [name]"
        emit HelloEvent("Hello: ", name);
    }
}
```

我当时报错说是编译器是0.8.30以上版本，在0.8.26前面加^就行了表示当前合约支持大于0.8.26,小于0.9.0的版本

* * *

## **一、**`onCall` **是什么？**

在 ZetaChain 的 **Universal Contract（通用合约）模型**中：

> `onCall` **是跨链消息到达 ZetaChain 时自动触发的回调函数**。

当用户在**任意已连接链**（如 Bitcoin、Ethereum、Dogecoin）上向 ZetaChain 发起跨链请求（例如发送资产 + 附加消息），ZetaChain 的协议层会：

1.  验证该跨链交易；
    
2.  将其封装为一个标准化的“跨链消息”；
    
3.  **自动调用目标合约的** `onCall` **方法**，传递相关参数。
    
4.  因此，`onCall` 就是合约的“跨链入口”。
    

## **二、**`onCall` **的四个参数详解**

### **1.** `MessageContext calldata context`

这是一个结构体，包含跨链调用的**元信息**。定义通常如下（来自 ZetaChain SDK）：

```
struct MessageContext {
    address origin;      // 源链标识（如 Bitcoin 链的地址常量）
    address caller;      // 源链上发起交易的用户地址（映射为 Zeta 格式）
    uint256 nonce;       // 防重放攻击的随机数
    bytes32 hash;        // 消息哈希
}
```

* * *

### **2.** `address zrc20`

-   表示用户在源链上发送的**原生资产**（如 BTC、ETH、DOGE）在 ZetaChain 上对应的 **ZRC-20 代币地址**。
    
-   例如：BTC → `zBTC`，ETH → `zETH`。
    
-   若用户**只发消息没发资产**，则 `zrc20 = address(0)`。
    

* * *

### **3.** `uint256 amount`

-   用户在源链发送的资产数量（已转换为 ZRC-20 的最小单位）。
    
-   例如：发送 0.01 BTC → `amount = 1_000_000`（satoshi）。
    
-   若无资产转移，则为 `0`。
    

### **4.** `bytes calldata message`

-   **用户自定义的任意数据**，是你实现通用逻辑的关键。
    
-   通常使用 `abi.encode(...)` 在源链构造，用 `abi.decode(...)` 在 `onCall` 中解析。
    

string memory name = abi.decode(message, (string));表示用户在源链发送了一个字符串（如 “Alice”），合约将其解码并用于事件。

## **三、**`onlyGateway` **修饰符：安全核心**

```
external override onlyGateway
```

-   `onlyGateway` 是 ZetaChain 协议提供的**关键安全机制**。
    
-   它确保 `onCall` **只能由 ZetaChain 的官方网关合约（Gateway）调用**。
    
-   **普通用户或其它合约无法直接调用** `onCall`，防止伪造跨链消息。
    

## 四、执行流程示例

假设用户想从 **Ethereum** 调用这个合约，并传入名字 `"Alice"`：

使用 ZetaChain SDK（如 @zetachain/zeta-eth）：

```
const message = abi.encode(["string"], ["Alice"]);
await zetaConnector.send({
  destinationAddress: "0xYourUniversalContractOnZeta",
  message: message,
  zrc20: "0x0000000000000000000000000000000000000000", // 无资产
  amount: 0,
});
```

步骤 2：ZetaChain 处理

-   ZetaChain 观察者检测到 Ethereum 上的调用；
    
-   验证通过后，在 ZetaChain 主网执行：
    

```
universalContract.onCall(
    context: { origin: ETHEREUM_CHAIN, caller: userAddrOnZeta, ... },
    zrc20: 0x0,
    amount: 0,
    message: 0x...（"Alice" 的 ABI 编码）
);
```

### **步骤 3：合约执行**

-   解码 `message` 得到 `"Alice"`；
    
-   触发事件：`HelloEvent("Hello: ", "Alice")`；
    
-   开发者可通过事件监听器得知跨链调用成功。
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->










# 第一份WEB3笔记

## 一.ZetaChain开发文档入口

-   [https://www.zetachain.com/docs/developers](https://www.zetachain.com/docs/developers)
    

## 二.ZetaChain是什么（粗浅的理解）

1.  一个可以原生访问任何区块链的通用区块链
    
2.  可以跨链去调用不同的DAPP
    
3.  使用“全链智能合约”，一个合约可以监听和响应多条链上的事件，并在任意链上执行操作。
    

## 三.配置ZetaChain开发环境

### 1\. 配置Ubuntu远程开发环境（以下所有操作都在远程环境中进行）

(1)具体的视频资料：[wsl + vscode](https://www.bilibili.com/video/BV1fu4m1A79r/?spm_id_from=333.1007.top_right_bar_window_custom_collection.content.click&vd_source=429af1a95897362b1265c3245c2759bb)

(2)在资源管理器中查看远程环境中的文件：

```
在地址栏输入（只能是wsl2）：
\\wsl$
然后回车就行
```

(3)查看当前的wsl版本

```
wsl --list --verbose
输出示例：
  NAME            STATE           VERSION
* Ubuntu-22.04    Running         2
  Debian          Stopped         1
```

### 2.下载Foundry

在远程环境中使用以下命令（速度巨慢）

```
curl -L https://foundry.paradigm.xyz | bash
```

### 3.下载Node.js

官网命令参考：适用于Linux且使用nvm和npm的Node.js

```
# 下载并安装 nvm：
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash

# 代替重启 shell
\. "$HOME/.nvm/nvm.sh"

# 下载并安装 Node.js：
nvm install 24

# 验证 Node.js 版本：
node -v # Should print "v24.11.1".

# 验证 npm 版本：
npm -v # Should print "11.6.2".

```

但是在第一句命令就遇到了错误（悲~）：

```
fatal: unable to access 'https://github.com/nvm-sh/nvm.git/': GnuTLS recv error (-110): The TLS connection was not properly terminated.
Failed to clone nvm repo. Please report this!
```

以下是AI给出的解决方案，亲测可用

```
# 1. 手动克隆 nvm 仓库到 ~/.nvm
git clone https://github.com/nvm-sh/nvm.git ~/.nvm

# 2. 激活 nvm（临时）
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # 加载 nvm

# 3. 添加到 shell 配置文件（永久生效）
echo 'export NVM_DIR="$HOME/.nvm"' >> ~/.bashrc
echo '[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"' >> ~/.bashrc
echo '[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"' >> ~/.bashrc

# 4. 重新加载配置
source ~/.bashrc

# 5.安装完成后验证
nvm --version
nvm install --lts
nvm use --lts
node -v
npm -v
```

## 四.注册Qwen 账号

控制台入口：[阿里云控制台](https://home.console.alibabacloud.com/home/dashboard/ProductAndService)
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
