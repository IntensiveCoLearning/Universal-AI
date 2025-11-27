---
timezone: UTC+8
---

# XiaoHai67890

**GitHub ID:** XiaoHai67890

**Telegram:** @XiaoHai489

## Self-introduction

嘿嘿

## Notes

<!-- Content_START -->
# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->
## 笔记Day4

**目标：**

1.  搞清楚什么是 **Universal App / 全链应用合约**。
    
2.  搭好后面 Hello World Demo 的心智模型：
    
    -   合约在 ZetaChain 上
        
    -   前端在浏览器
        
    -   调用通过连接链上的 Gateway + RPC 完成（例如 Arbitrum / Base 等测试网）。
        

可以把它想象成：

> 「所有链（ETH、BNB、BTC…）都只是**客户端**，真正的业务逻辑集中在 ZetaChain 上那一个 Universal App 合约。」

* * *

## 2\. 什么是 Universal App（全链应用）？

### 2.1 官方定义（翻译 + 压缩版）

-   **Universal App = 部署在 ZetaChain 的智能合约**，但它**天然连着别的链**（Ethereum、BNB、Bitcoin 等）。
    
-   和普通合约不同，它可以：
    
    -   接收来自任意连接链的：**合约调用 + 消息 + 代币转账**
        
    -   反过来再发起：**跨链合约调用 + 代币转账**。
        
-   Universal App 部署在 **Universal EVM** 上，这个 EVM 在普通 EVM 之上扩展了跨链互操作能力。
    

### 2.2 关键组件

1.  **onCall 函数：跨链入口**
    
    智能合约：
    
    ```
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
    
    -   所有从其它链（例如 Base Sepolia）经由 Gateway 过来的调用，最终都会进入 `onCall`。
        
    -   `context` 告诉你：源链 ID、调用者地址等。
        
    -   `zrc20` + `amount` 代表从源链带来的 Token（映射为 ZRC-20）。
        
    -   `message` 是任意 ABI 编码的数据（Hello 例子里是一个字符串）。
        
2.  **ZRC-20：统一表示跨链资产**
    
    -   每个连接链上的原生 gas / ERC-20，在 ZetaChain 上都有一个对应的 **ZRC-20**。
        
    -   例如：
        
        -   Ethereum 上的 ETH ↔ ZetaChain 上的 `ZRC-20 ETH`
            
        -   这些 ZRC-20 代币可以随时“提现”回原链（ZRC-20 ETH → 以太坊上的 ETH）。
            
3.  **Gateway：所有链进入 Universal App 的“门”**
    
    -   每条连接链上有一个 **Gateway 合约**，专门负责：
        
        -   接收用户 / 合约的调用、消息、代币
            
        -   转发到 ZetaChain 上指定的 Universal App。
            
    -   调用路径是：
        
        > 源链 EVM 合约 / 用户 → 源链 Gateway → ZetaChain Universal App →（可选）再往其它链发射调用
        
4.  **对 Bitcoin 的支持（很重要的卖点）**
    
    -   BTC 用户可以直接往 Gateway 地址 **发送一笔 BTC + data**，就完成对 Universal App 的调用。
        
    -   只需要**一笔 Bitcoin 交易**，无需在其他链开钱包、准备 gas（由 Universal App 帮忙处理 gas，称为 gas 抽象）。
        

### 2.3 心智模型一句话总结

> **Universal App = 一个部署在 ZetaChain 的“总控合约”。  
> 所有其他链只是入口（Gateway），资产都映射成 ZRC-20，业务逻辑集中在 onCall 里。**

* * *

## 3\. Hello World Universal App 教程拆解

### 3.1 效果

官方的 _First Universal Contract_ 教程里，你会完成：

-   编写一个 Universal 合约，当收到跨链调用时：
    
    -   从 `message` 里解码一个字符串
        
    -   `emit HelloEvent("Hello: ", name)`
        
-   在 **Localnet 或 Testnet** 上部署该合约。
    
-   从一个连接链（本地 EVM / Base Sepolia）通过 Gateway 发起跨链调用，看到 ZetaChain 上的事件日志。
    

### 3.2 环境与工具

1.  **ZetaChain CLI + Foundry 是官方推荐组合**
    
    从教程和 Intro 文档：
    
    -   CLI：创建项目、启动 Localnet、发起跨链调用、查询 CCTX。
        
    -   Foundry：`forge build` 编译、`forge create` 部署、`cast` 管理私钥等。
        
    
    初始化 Hello 项目：
    
    ```
    npx zetachain@latest new --project hello
    cd hello
    yarn
    forge soldeer update
    ```
    
2.  **Localnet：本地多链实验室**
    
    -   `npx zetachain localnet start` 可以在本机拉起：ZetaChain + 多条链（EVM、Solana、Sui、TON），以及预部署好的 Gateway、ZRC-20、Uniswap 池等。
        
    -   可以理解为 “ZetaChain on ”：
        
        -   快速调试跨链逻辑
            
        -   不花真实测试币
            
        -   一条命令就能重置环境
            
3.  **Testnet：对接真实公共测试网**
    
    -   教程中使用 ZetaChain testnet（Athens）+ Base Sepolia 等测试网。
        
    -   通过公共 RPC + Gateway 合约地址完成跨链调用，整体流程更接近真实生产环境。
        

### 3.3 合约侧的逻辑

核心逻辑：

-   `Universal` 合约实现 `UniversalContract` 接口，必须实现 `onCall`。
    
-   `onCall` 收到参数：
    
    -   `context.chainID`：源链 ID
        
    -   `context.sender`：源链调用 Gateway 的地址
        
    -   `zrc20`：源链资产映射
        
    -   `amount`：资产数量
        
    -   `message`：ABI 编码的业务参数
        
-   Hello 示例里只做一件事：
    
    -   `string name = abi.decode(message, (string));`
        
    -   `emit HelloEvent("Hello: ", name);`
        

> 心智模型：**这是一个“跨链 printf”** —— 从任何链发一个字符串，它在 ZetaChain 上打印出 `Hello: XXX` 事件。

### 3.4 调用

大致步骤：

1.  启动 Localnet
    
    ```
    npx zetachain localnet start
    ```
    
2.  `forge build` 编译合约。
    
3.  从本地 EVM（Anvil）拿一个预置私钥 + 测试币。
    
4.  使用 `forge create` 把 Universal 合约部署到 ZetaChain。
    
5.  找到连接链（如本地 ETH）的 Gateway 地址。
    
6.  用 CLI 从连接链调用 Universal：
    
    ```
    npx zetachain evm call \
      --rpc http://localhost:8545 \
      --gateway $GATEWAY_EVM \
      --receiver $UNIVERSAL \
      --private-key $PRIVATE_KEY \
      --types string \
      --values hello
    ```
    
7.  在 Localnet 终端里看到 `[ZetaChain]: Event from onCall` 的日志（说明 Universal App 收到了跨链消息）。
    

Testnet 版本则是类似的 `npx zetachain evm call --chain-id 84532 ...`，只是把 RPC、链 ID 换成公共测试网。

* * *

## 4\. 前端 + RPC 心智模型

在 **Build a Web App** 教程里，会在 Hello 合约之上加一层 React 前端：

### 4.1 前端

1.  连接一个 EVM 测试网钱包（例如 Arbitrum Sepolia）。
    
2.  使用 ZetaChain Toolkit 的 `evmCall` 发送跨链调用到 Gateway。
    
3.  记录源链的交易 hash。
    
4.  轮询 ZetaChain 的 CCTX API，拿到 ZetaChain 上执行的交易 hash。
    
5.  在 UI 中展示：
    
    -   源链 explorer 链接
        
    -   ZetaChain explorer 链接
        

> 于是你有了一个**完整的跨链调用闭环**：用户在前端点一下按钮 → 源链发送交易 → ZetaChain 执行 → 用户看到两个链上的结果。

### 4.2 RPC / 网络角色拆分

可以简单地把“后端 + RPC”想象为三类东西：

1.  **源链 RPC**（例如 Arbitrum Sepolia 的 RPC）
    
    -   负责发送用户签名的交易（调用 Gateway）。
        
2.  **ZetaChain RPC / API**
    
    -   负责查询 CCTX 状态、ZetaChain 上的交易执行情况。
        
3.  **Gateway 合约地址 / 协议 Contracts**
    
    -   写死在前端配置中，或通过 CLI / Docs 查到
        
    -   统一入口，前端不会直接连 ZetaChain 的 Universal 合约，而是先打到 Gateway，再由协议转发到 Universal App。
        

* * *

## 5\. 为后续 Swap / Messaging 铺路：Universal App 可以做什么？

在 Hello 之后，官方 Tutorials 会带你做：Swap / Messaging 等更复杂例子。它们本质上只是 **onCall 里面的逻辑变复杂** 而已：

1.  **跨链留言本 / 事件记录**
    
    -   Hello 已经是最小版本：跨链字符串 → 事件。
        
2.  **跨链 Swap（示例思路）**
    
    -   从 Ethereum 接收 ZRC-20 ETH + “我想要 BNB” 的 message
        
    -   在 ZetaChain 上用 DEX 把 ZRC-20 ETH 换成 ZRC-20 BNB
        
    -   再通过 Gateway 把 ZRC-20 BNB 提现到 BNB Chain 用户地址。
        
3.  **跨链 NFT / Game / Social**
    
    -   所有状态与规则写在 ZetaChain 的 Universal App 中。
        
    -   不同链的用户只需通过「本链的钱包 + Gateway」，就能玩同一套逻辑。
        

> 心智模型：**你只写一份“主脑合约”，不同链的用户排队来问它问题 / 给它资产。**

* * *

## 6\. 工具 & 工作流选择建议（CLI + Hardhat / Foundry？Localnet 还是 Testnet？）

我觉得需要思考这个问题：

后面的 Hello World / Demo，打算用哪一套工具 + 哪个网络？

来做一一个对比 + 建议吧

### 6.1 开发工具：Hardhat vs Foundry

ZetaChain 官方说明：平台原生支持 Foundry、Hardhat、Slither、Ethers.js 等主流工具，你可以继续用自己熟悉的工作流。

**Hardhat：**

-   优点：
    
    -   JS / TS 生态友好，插件多（与前端、脚本集成方便）。
        
    -   自带 Hardhat Network，本地调试 EVM 很顺手。
        
-   适合：
    
    -   你更熟悉 JS / TS，喜欢用 Hardhat 脚本部署。
        
    -   想和前端工程整合得很紧（比如一套 monorepo）。
        

**Foundry：**

-   优点：
    
    -   更偏 **Solidity 原生**，`forge build / forge test` 很快。
        
    -   CLI 体验统一，和 ZetaChain 官方教程深度绑定（包括 Localnet、`forge create` 等）。
        

### 6.2 网络：Localnet vs Testnet

**Localnet（个人比较推荐）：**

-   优点：
    
    -   本地跑在 Docker / 本机，多链一起启动，极快迭代。
        
    -   不依赖测试网 RPC 稳定性，不用到处找 faucet。
        
    -   可以自由重置环境，适合“乱试”。
        
-   缺点：
    
    -   与真实用户环境有一点差距（没有真实的测试网 Explorer UX）。
        
    -   需要本地环境配置（Docker、Foundry 等）。
        

**Testnet：**

-   优点：
    
    -   完全贴近真实环境：公共 RPC、浏览器（Blockscout / Basescan）、真实延迟。
        
    -   前端 Demo 给别人看时更“像真的”。
        
-   缺点：
    
    -   依赖测试网状态（RPC 慢 / 挂）。
        
    -   流程稍长，跨链确认需要等待时间。
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->

# 学习笔记 Day 3：ZetaChain & Universal Blockchain 核心概念

## 1\. 整体认识：什么是 “Universal Blockchain / Universal EVM”？

-   **ZetaChain 是什么？**  
    ZetaChain 是一个通用的 Layer 1 区块链（Universal L1），用 PoS 共识 + Cosmos SDK + Cosmos EVM 搭起来，本身就是一条可编程的链，但它把“跨链能力”写进了底层共识和 EVM 里。
    
-   它的目标是：
    
    > 让开发者在一条链（ZetaChain）上写合约，就能原生地管理 Bitcoin、以太坊、Solana 等多条链上的资产和状态——也就是 **Universal Blockchain** 的含义。
    

**几个核心术语在 ZetaChain 里的对应：**

-   **Universal Blockchain**：  
    一条“覆盖所有链”的基础设施链。它自己是 L1，但可以与 EVM 链、非 EVM 链、甚至无智能合约的链（如比特币）互通，在这条链上执行的逻辑可以“伸手”操作其他链。
    
-   **Universal EVM（ZetaChain 的 Universal EVM）**：  
    兼容普通 EVM，同时在 EVM 层面直接内置跨链能力（跨链消息、资产映射 ZRC-20 等），专门用来开发 **Universal Apps**。
    
-   **Omnichain Smart Contract / Universal Contract**：  
    部署在 ZetaChain Universal EVM 上的智能合约，一份合约就可以：
    
    -   接收来自任意已连接区块链的消息 / 代币；
        
    -   在任意连接链上发起合约调用或资产转移；
        
    -   像“跨链总控室”一样协调多链操作。
        

* * *

## 2\. 用自己的话：Universal App 是什么？

> **“一份合约，管所有链的事。”**

官方的定义：Universal App 是部署在 ZetaChain 的 Universal EVM 上、原生连接任意其他区块链（包含比特币、EVM 链、非 EVM 链等）的智能合约应用。

**我自己的理解：**

1.  **部署位置**：
    
    -   只部署在 **ZetaChain 上**（Universal EVM）。
        
    -   外围链上不需要布置一堆“同构合约副本”，最多只需要 Gateway 等网关合约。
        
2.  **能干什么**：
    
    -   接收来自任意连接链的：
        
        -   合约调用 / 消息；
            
        -   代币存入（例如 BTC、ETH、USDC 等）。
            
    -   再从 ZetaChain 这边发起：
        
        -   对任意链上合约的调用；
            
        -   跨链转账、跨链 swap 等。
            
3.  **体验上的效果**：
    
    -   一个比特币用户，只在 BTC 钱包里签一笔交易，就能通过某个 universal app 把 BTC 换成以太坊上的 USDC，自己甚至不需要有以太坊地址里的 gas。
        
4.  **和传统多链 dApp 的差别：**
    
    | 传统多链 dApp | Universal App（在 ZetaChain 上） |
    | --- | --- |
    | 每条链部署一套合约，逻辑分散、状态分裂 | 一份合约在 ZetaChain，统一持久状态 |
    | 跨链 usually 依赖桥 + 外部消息网络 | 跨链能力写在链里，由 ZetaChain 共识保证 |
    | 复杂的桥配置、跨链安全面多点、开发维护重 | 开发者只维护 ZetaChain 这套逻辑，跨链由基础设施兜底 |
    

**一句话记忆：**

> Universal App 就是“部署在 ZetaChain 上的全链 dApp”，它有多链读写能力和多链资产总控能力，用户可以在任意连接链上用自己熟悉的钱包和资产，与它交互。

* * *

## 3\. 自己的理解：Gateway 大概做什么？

官方文档：

> Gateway 是一个接口，为连接链上的合约和 ZetaChain 上的 universal apps 提供统一的入口（unified entry point）。

**我自己的理解：**

可以把 Gateway 想成 **“每条外部公链上的 ZetaChain 总服务台 / 前台”**，它主要负责：

1.  **统一入口（Entry Point）**
    
    -   在每条连接链上只有一个 Gateway 合约，所有“想跟 ZetaChain 通话”的用户或合约，都要通过它。
        
    -   用户在以太坊、BSC、Polygon、甚至比特币网络上，都只需要和本链的 Gateway 交互，不需要直接“找上”ZetaChain。
        
2.  **消息 + 资产的“打包、转交、拆包”**
    
    -   当用户在某条链上调用 Gateway 时，可以同时附带：
        
        -   代币（比如 1 ETH、100 USDC）；
            
        -   数据 payload（比如 “帮我把这个 USDC 转到 BNB 链上的某个地址”）。
            
    -   Gateway 负责把这两者封装成一条跨链“任务”，交给 ZetaChain。
        
    -   等 ZetaChain 上的 Universal App 执行完逻辑后，提现 / 释放资产时，也会通过目标链的 Gateway 把结果落地到对应链上。
        
3.  **对开发者的意义**
    
    -   开发者只需要面对一个统一接口：
        
        -   “从本链 → ZetaChain → 其他链” 都是同一套调用模式。
            
    -   安全性、跨链路由、观察者 + TSS 签名这些麻烦事，交给 ZetaChain 底层和 Gateway 处理。
        

**一句话记忆：**

> Gateway 是每条外部链通往 ZetaChain 的“跨链网关合约”，负责统一入口 + 代币托管 + 消息转发，是 Universal App 对外的门面。

* * *

## 4\. ZetaChain & Universal App & Gateway 的关系

从开发视角看，可以这样抽象：

1.  **底层链：ZetaChain（Universal L1 + Universal EVM）**
    
    -   负责共识、跨链消息、安全、执行 universal contracts。
        
2.  **应用层：Universal Apps**
    
    -   部署在 Universal EVM 上，写一次合约，面向所有连接链用户。
        
3.  **接入层：Gateway（每条外部链一个）**
    
    -   外部链的所有调用，都要先进 Gateway，再由 ZetaChain 节点网络观察、共识，最后路由到 Universal App。
        

可以对比成一个“总行 + 各地支行”的结构：

-   ZetaChain = 总行，总账 + 风控 + 产品逻辑；
    
-   Universal App = 总行的具体业务系统；
    
-   各链 Gateway = 各地支行的业务窗口，负责接待用户、收取现金（代币）、把指令送回总行，执行完以后再给用户出账。
    

* * *

## 5\. 简易架构图

作业：**ZetaChain 中心 + Bitcoin / Ethereum / Solana 等外围链 + Gateway**：  

```
                [ Bitcoin 链 ]
                      |
                  (BTC Gateway)
                      |
    [ Ethereum 链 ] --+           
          |           |           
   (ETH Gateway)      |
                      v
         +-------------------------+
         |       ZetaChain         |
         |   (Universal Blockchain |
         |        + Universal EVM) |
         +-------------------------+
                      ^
                      |
            [ Universal Apps ]
   (Omnichain / Universal Contracts)
         |         |          |
         |         |          |
   跨链 DEX   跨链 NFT   跨链支付等


                [ Solana 链 ]
                      |
                 (Solana Gateway)
                      |
                      +----→ 连接到 ZetaChain
```

第二张图（感觉更能体现和任意链家湖？）：

```
         Any Chain A        Any Chain B        Any Chain C
          (BTC/ETH/...)     (Solana/TON/...)   (L2/其他)
               \                 |                /
                \   [ Gateways on each chain ]  /
                 \           |                 /
                  +----------v----------------+
                  |        ZetaChain         |
                  |  Universal EVM + Apps    |
                  +--------------------------+
```
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->


Day 2 学习笔记：环境与工具实战（ZetaChain + Qwen）

——————————————  
0\. 今日学习目标 & Checklist  
——————————————

今日目标：

1.  在本地或云端把 ZetaChain 开发环境（CLI 为主）跑起来（安装、简单命令）。
    
2.  搞清楚 ZetaChain 测试网的 RPC / Faucet / Explorer 入口并记录。
    
3.  在终端或 Postman 里完成一次 Qwen API 请求（成功返回或报错都可以，打通连通性最重要）。
    

——————————————

1.内容

1.1 技术栈范围

链：ZetaChain（Localnet + Testnet）  
工具：ZetaChain CLI、ZetaChain Explorer、Faucet 等  
模型：Qwen 系列（通过 OpenAI 兼容接口调用）

1.2 能力目标

（1）环境与基础

-   能在本地或云端搭好 ZetaChain CLI 环境。
    
-   能连接测试网，执行基础查询 / 交互命令。
    

（2）跨链与 Universal Apps 概念

-   理解 ZetaChain 是一个基于 Cosmos SDK、兼容 EVM 的 L1 区块链。
    
-   理解 Universal EVM 的基本概念（跨链交易记录、状态更新等）。
    

（3）Qwen API 使用

-   熟练用 OpenAI 兼容方式调用 Qwen（HTTP / Python SDK / Postman）。
    
-   知道如何选择不同的 Qwen 模型（如通用对话、代码模型、长文档模型等）。
    

——————————————  
2\. ZetaChain 快速回顾  
——————————————

2.1 ZetaChain 是什么？

ZetaChain 是一个基于 Cosmos SDK 的 L1 区块链，同时兼容 EVM。  
核心卖点是跨链 / 通用（universal / omnichain）：  
通过 Universal EVM，合约可以原生处理来自多个链的资产与消息，管理跨链交易状态。

2.2 开发者入口

官方文档仓库：[https://github.com/zeta-chain/docs](https://github.com/zeta-chain/docs)  
在线文档入口：[https://www.zetachain.com/docs/](https://www.zetachain.com/docs/) （包含 developers、reference、教程等）

——————————————  
3\. ZetaChain CLI：安装与基础使用  
——————————————

3.1 CLI 简介

“zetachain” 是官方提供的 CLI 工具（npm 包名同为 zetachain）。  
主要能力包括：

-   创建和管理 ZetaChain universal app 项目；
    
-   与 ZetaChain 网络交互（查询链、启动本地环境等）；
    
-   在多链（EVM / Solana / Bitcoin / Sui / TON 等）上进行操作（依场景与版本而定）。
    

可以把它看作 ZetaChain 开发者的开路利器。

3.2 前置依赖（本地）

常见依赖（以 Node 版 CLI 为主）：

-   Node.js（建议 18 及以上）
    
-   npm 或 yarn
    
-   Git
    
-   可选：  
    · Docker（用于 Localnet 等多链环境）；  
    · Foundry / 其他 EVM 开发工具。
    

3.3 安装方式

3.3.1 临时使用（npx），适合快速试用：

npx zetachain@latest --help  
或  
npx zetachain@latest new

无需全局安装，适合一次性试验。

3.3.2 全局安装：

npm install -g zetachain

安装完成后可以通过以下命令验证：

zetachain --help  
zetachain --version

3.4 基础命令示例

实际命令以当前 CLI 帮助信息 “zetachain --help” 为准，这里只写出典型交互思路。

查看 CLI 帮助：

zetachain --help

（如果支持）列出可用链：

zetachain query chains list

创建新项目（示例）：

zetachain new my-universal-app

建议你在笔记中记录自己实际运行的命令和输出摘要：

命令：zetachain query chains list  
输出摘要：

-   ...  
    结论：CLI 已正常连接网络 / 有报错信息 ...
    

——————————————  
4\. ZetaChain 测试网：RPC / Faucet / Explorer  
——————————————

4.1 测试网网络参数（Network Details）

ZetaChain Testnet（EVM 侧）常用参数：

Chain ID (EVM)：7000  
Chain ID (Cosmos)：zetachain\_7000-1  
Denom：azeta（链上最小单位）  
Symbol：ZETA  
Decimals：18  
HD Path：m/44'/60'/0'/0/0  
Bech32 prefix：zeta  
Explorer：[https://zetascan.com](https://zetascan.com)  
EVM RPC：[https://zetachain-evm.blockpi.network/v1/rpc/public](https://zetachain-evm.blockpi.network/v1/rpc/public)

你可以在钱包（如 MetaMask）里这样配置测试网：

Network Name: ZetaChain Testnet  
RPC URL: [https://zetachain-evm.blockpi.network/v1/rpc/public](https://zetachain-evm.blockpi.network/v1/rpc/public)  
Chain ID: 7000  
Currency: ZETA  
Block Explorer:[https://zetascan.com](https://zetascan.com)

4.2 测试网 RPC / API

公共 EVM RPC 和其他公共端点会在官方 RPC/API Endpoints 文档中列出。  
公共 RPC 更适合钱包或前端使用，高频 / 后端场景建议使用专用私有节点。

4.3 Faucet（测试币）

ZETA 用途：在 ZetaChain 上作为 gas，用于部署和调用合约、发起交易。  
测试网 ZETA 无任何货币价值，仅用于测试开发。

常见获取方式（ZETA）：

-   Google Cloud Web3 Faucet
    
-   FaucetMe：[https://zetachain.faucetme.pro](https://zetachain.faucetme.pro)
    

4.4 Explorer：ZetaScan

官方推荐的区块浏览器：[https://zetascan.com](https://zetascan.com)

功能：

-   查看区块、交易、合约、地址余额等；
    
-   支持主网 / 测试网切换。
    

——————————————  
5\. Qwen API：OpenAI 兼容调用实战  
——————————————

5.1 整体认知

Qwen（通义千问）是阿里云提供的大模型系列，可在 Model Studio / 阿里云百炼 中使用。

官方提供两种主要调用协议：

-   OpenAI 兼容协议
    
-   DashScope 原生协议
    

Day 2 重点放在 OpenAI 兼容：

只要替换 API Key、base\_url 和 model，基本可以无缝迁移原本的 OpenAI 代码。

5.2 获取 API Key 与环境变量

官方推荐步骤：

-   在阿里云百炼控制台创建 API Key；
    
-   把 Key 配置到环境变量中，例如：
    

export DASHSCOPE\_API\_KEY="sk-xxxx"

后续所有调用（curl / Python / Postman）都通过  
Authorization: Bearer $DASHSCOPE\_API\_KEY  
来使用，避免把 Key 写死在代码里。

5.3 在终端用 curl 完成一次 Qwen 请求（作业核心）

以官方 OpenAI 兼容 ChatCompletion 接口为例。

请求地址：

POST [https://dashscope.aliyuncs.com/compatible-mode/v1/chat/completions](https://dashscope.aliyuncs.com/compatible-mode/v1/chat/completions)

示例命令：

export DASHSCOPE\_API\_KEY="sk-xxxx" # 若已写入 shell 配置文件可忽略

curl --location '[https://dashscope.aliyuncs.com/compatible-mode/v1/chat/completions](https://dashscope.aliyuncs.com/compatible-mode/v1/chat/completions)'  
\--header "Authorization: Bearer $DASHSCOPE\_API\_KEY"  
\--header "Content-Type: application/json"  
\--data '{  
"model": "qwen-plus",  
"messages": \[  
{ "role": "system", "content": "You are a helpful assistant." },  
{ "role": "user", "content": "用一句话介绍一下 ZetaChain 是什么？" }  
\]  
}'

如果成功，返回结构会与 OpenAI 的 ChatCompletion 类似（含 choices\[0\].message.content）。  
即便是 401 / 403 等错误，也说明「网络连通 + 认证路径」是通的，只是权限或 Key 有问题，对 Day 2 作业来说依然有价值。

5.4 使用 Python（OpenAI SDK）调用 Qwen（兼容模式）

根据官方 OpenAI 兼容文档，可以使用最新版 openai SDK 直接指向 DashScope 端点：

from openai import OpenAI  
import os

def call\_qwen():  
client = OpenAI(  
api\_key=os.getenv("DASHSCOPE\_API\_KEY"),  
base\_url="[https://dashscope.aliyuncs.com/compatible-mode/v1](https://dashscope.aliyuncs.com/compatible-mode/v1)",  
)

```
completion = client.chat.completions.create(  
    model="qwen-plus",  # 也可使用其他受支持的 Qwen 模型  
    messages=[  
        {"role": "system", "content": "You are a helpful assistant."},  
        {"role": "user", "content": "帮我用一句话解释 ZetaChain 的核心特性。"},  
    ],  
)  

print(completion.choices[0].message.content)  
```

if **name** == "**main**":  
call\_qwen()

要点回顾：

-   api\_key 使用阿里云百炼的 Key；
    
-   base\_url 必须改为 [https://dashscope.aliyuncs.com/compatible-mode/v1](https://dashscope.aliyuncs.com/compatible-mode/v1) （或相应地域的 URL）；
    
-   其余参数、返回结构与 OpenAI ChatCompletion 基本一致。
    

5.5 使用 Postman 调用 Qwen 的参考配置

新建 Request：

-   Method：POST
    
-   URL：[https://dashscope.aliyuncs.com/compatible-mode/v1/chat/completions](https://dashscope.aliyuncs.com/compatible-mode/v1/chat/completions)
    

Headers：

Authorization: Bearer YOUR\_DASHSCOPE\_API\_KEY  
Content-Type: application/json

Body（raw + JSON）示例：

{  
"model": "qwen-plus",  
"messages": \[  
{ "role": "system", "content": "You are a helpful assistant." },  
{ "role": "user", "content": "你好，请简单介绍下 ZetaChain。" }  
\]  
}

观察返回：

-   正常：状态码 200，返回 choices；
    
-   异常：401（Key 问题）、429（限流）、4xx（参数错误）等。
    

5.6 常见错误与自查 Checklist

401 / invalid\_api\_key：

-   检查 API Key 是否正确，是否拷贝多了空格；
    
-   检查 Authorization 头格式是否为 Bearer <API\_KEY>。
    

403 / 权限不足：

-   检查对应地域、模型是否已在控制台开通。
    

4xx / Content-Type 或参数错误：

-   确认 Content-Type: application/json；
    
-   检查 model 名称是否为官方文档列出的合法值。
    

网络问题：

-   用浏览器或 ping / curl -v 检查是否被网络环境阻断。
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->



它有一个叫 **Universal EVM** 的东西，本质是「带跨链能力的 EVM」，你在这条链上写一个合约，就可以从这一个逻辑中心去操作多条链（包括以太坊、BNB、Solana 甚至比特币）的资产和合约，是一个专门为跨链 / 全链应用准备的开发环境

第二，它提供了 **Universal Assets 标准**，包括 Universal NFT 和 Universal Token，你可以按它给的标准直接做「可以在多链间铸造和转移的 NFT / ERC-20 代币」，不需要自己重新设计跨链协议。

第三，它列出了 **Connected Chains + Tutorials**：告诉你目前支持哪些链（EVM、Solana、Sui、TON、Bitcoin 等）以及可以对这些链做哪些操作，同时给了一组循序渐进的教程（Getting Started、First Universal Contract、Build a Web App、Swap、Messaging 等），让你可以从「跑通第一个通用合约」一路做到「实现一个跨多链的 Swap / 消息调用应用」。

简单来做个总结吧：**Developers 这页 = ZetaChain 上做跨链应用的入口导航：Universal EVM（怎么写）、Universal Assets（用什么标准发资产）、Connected Chains + Tutorials（能连谁、怎么一步步做出来）**
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
