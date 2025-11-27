---
timezone: UTC+8
---

# jvbaoge

**GitHub ID:** jvbaoge1

**Telegram:** @jvhuohai

## Self-introduction

just share ，dyor ，hope to earn  空投不撸枉少年  新协议我先上车  粉丝少但 Alpha 多 | 私信来薅  空投收割机  新协议猎手  0→1 成长中 | 专啃早期 Alpha

## Notes

<!-- Content_START -->
# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->
1\. 我的第一个 Universal App 想实现的功能：

当用户从任意连接链（如 Ethereum、BNB 等）发送一条文本消息（例如 "Hello from Ethereum!"），我的 Universal Contract 会：

\- 记录发送者地址

\- 记录来源链 ID

\- 存储消息内容

\- 发出一个 CrossChainGreeting 事件

后续可通过前端或区块浏览器查看所有跨链问候记录。

2\. 开发工作流决定：

\- **开发框架**：Hardhat（因官方教程和工具链支持完善）

\- **部署网络**：ZetaChain Athens 3 测试网（支持真实跨链交互，可领测试币）

\- **不使用本地链**：因为本地环境无法模拟多链互操作场景
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->

### **1\. Universal App 是什么？**

Universal App（通用应用）是部署在 **ZetaChain** 上的一种智能合约，它能 **原生地与多个区块链（如 Ethereum、Bitcoin、Solana 等）进行交互**。

-   它可以 **接收来自任何已连接链的消息和代币**（比如用户从以太坊发来 ETH，或从比特币发来 BTC）。
    
-   它也可以 **主动向这些链发起交易**，例如把 ETH 换成 BNB，再转给 BNB 链上的地址。
    
-   它运行在 **Universal EVM（通用以太坊虚拟机）** 上，兼容 Solidity，开发者可以像写普通 EVM 合约一样开发，但获得跨链能力。
    
-   最关键的是：**一次用户操作（比如一笔比特币转账）就能触发多链联动逻辑**，而不需要用户分别操作每条链。
    

### **2\. Gateway 大概做什么？**

Gateway（网关）是 **每条连接到 ZetaChain 的公链上的一个特殊合约（或地址）**，它是用户与 Universal App 交互的“入口”。

-   用户想从 Ethereum、Bitcoin 或 Solana 调用 ZetaChain 上的 Universal App，**必须先和该链上的 Gateway 交互**。
    
    -   在 EVM 链（如 Ethereum）：调用 Gateway 合约，传入目标 Universal App 地址 + 数据 + 代币。
        
    -   在 Bitcoin：向 Gateway 地址发送 BTC，并在交易备注中附带消息（OP\_RETURN）。
        
-   Gateway 负责 **捕获用户请求并将其转发给 ZetaChain 的验证者网络**，最终触发 Universal App 的 `onCall` 函数。
    
-   它也负责 **把 ZetaChain 的响应（比如跨链转账）执行回目标链**。
    

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/jvbaoge1/images/2025-11-26-1764165068328-image.png)
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->


📝 Day 2：环境与工具实战（ZetaChain + Qwen）

提交人：jvbaoge1

日期：2025 年 11 月 25 日

环境：Windows + WSL（Ubuntu），Node.js v24.11.1

✅ 一、ZetaChain CLI 安装与验证

1\. 环境准备

Node.js ≥ 18：✅ 已安装（v24.11.1）

npm：✅ v11.6.2

2\. 安装 CLI

npm install -g zetachain@latest

3\. 验证安装

zetachain --help

输出包含以下核心命令：

new：创建 Universal App 项目模板

accounts：管理钱包账户

query：查询余额、跨链费用、合约等

localnet：启动本地多链开发环境（EVM、Solana 等）

✅ 结论：CLI 安装成功，具备开发基础能力。

📚 二、ZetaChain 测试网关键信息整理

根据官方文档 ZetaChain Reference ，整理 Athens Testnet 关键入口如下：

网络名称

Athens Testnet

Chain ID

7001

RPC Endpoint

[https://zetachain-athens-evm.blockpi.network/v1/rpc/public](https://zetachain-athens-evm.blockpi.network/v1/rpc/public)

Faucet（领测试 ZETA）

[https://labs.zetachain.com/faucet](https://labs.zetachain.com/faucet)

区块浏览器（ZetaScan）

[https://athens3.explorer.zetachain.com/](https://athens3.explorer.zetachain.com/)

ZETA 代币

Native token，ZRC-20 形式可在

Contract Addresses

查询

✅ 已记录至本地笔记，供后续开发调用。

🌐 三、Qwen API 连通性测试

1\. 平台选择

使用 阿里云百炼平台（DashScope 升级版）

模型：qwen-turbo（低延迟、免费额度充足）

curl -X POST [https://dashscope.aliyuncs.com/api/v1/services/aigc/text-generation/generation](https://dashscope.aliyuncs.com/api/v1/services/aigc/text-generation/generation) \\

\-H "Authorization: Bearer sk-xxxxx" \\

\-H "Content-Type: application/json" \\

\-d '{

"model": "qwen-turbo",

"input": {

"messages": \[{"role": "user", "content": "你好，通义千问！"}\]

}

}'

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/jvbaoge1/images/2025-11-25-1764075133884-image.png)

🔒 说明：真实 API Key 仅用于本地测试，未写入任何代码或文档。

3\. 实际测试结果（本地成功）

{

"output": {

"text": "你好！不过我更喜欢你叫我通义千问。很高兴见到你！……"

}

}

四、未来两周学习目标（2025.11.25 – 2025.12.09）

根据 ZetaChain Developers 教程路线，制定计划如下：

完成核心教程：

✅ Getting Started（已了解）

🔄 First Universal Contract（下一步）

🔄 Messaging

🔄 Build a Web App

技术实践：

使用 zetachain new 创建第一个 Universal Contract

通过 zetachain localnet 启动本地多链环境

实现 EVM → ZetaChain 的跨链消息传递

在 ZetaScan 上追踪交易状态

AI + 区块链融合探索：

用 Qwen API 解释 cross-chain call failed 错误

自动生成 Universal Contract 的 NatSpec 注释

🛡️ 五、安全与提交说明

所有代码/笔记存放于本地 ~/zetachain 仓库

使用 .gitignore 排除敏感路径：

gitignore

.zetachain/ # CLI 私钥目录

.env # 环境变量

绝不提交：API Key、私钥、助记词、真实地址

GitHub 仓库：[https://github.com/jvbaoge1/zetachain](https://github.com/jvbaoge1/zetachain)
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->



成功的部署了环境，从以太坊链（Sepolia）发送到 ZetaChain Universal Contract 的跨链调用过程

[https://github.com/jvbaoge1/zetachain](https://github.com/jvbaoge1/zetachain)

# **我的 ZetaChain 开发笔记**

> 一个纯新手从零开始学习 ZetaChain 的记录仓库。

## **📂 目录结构**

-   `tutorials/`：官方或自研教程实践
    
    -   `hello` —— [Hello 教程](https://www.zetachain.com/docs/developers/tutorials/hello)：第一个跨链 Universal App
        

## **🛠️ 开发环境**

-   **系统**：Windows 11 + WSL2 (Ubuntu)
    
-   **工具链**：
    
    -   Node.js + Yarn
        
    -   Foundry (`forge`, `cast`)
        
    -   ZetaChain CLI
        
-   **重要原则**：
    
    -   所有项目必须放在 **WSL 的 Linux 文件系统**（如 `~/zetachain/`）
        
    -   **不要**在 `/mnt/c/` 或 `/mnt/e/` 里开发（会出权限问题！）
        
    -   国内用户设置 Yarn 镜像：
        
        ```
        yarn config set registry https://registry.npmmirror.com
        ```
        

## **🚀 今天成就（2025-11-24）**

✅ 成功在 Localnet 上部署 `Universal` 合约  
✅ 从模拟的 Ethereum 链发送 "hello" 消息  
✅ 在 Localnet 终端看到：`[ZetaChain]: Event from onCall`  
🎉 我是跨链开发者啦！

\[Ethereum\] Processing Called event from 0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266 to 0x5bf5b11053e734690269C6B9D438F8C9d48F528A

\[ZetaChain\] Universal contract 0x5bf5b11053e734690269C6B9D438F8C9d48F528A executing onCall (context: {“chainID”:“11155112”,“sender”:“0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266”,“senderEVM”:“0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266”}), zrc20: 0x2ca7d64A7EFE2D62A725E2B35Cf7230D6677FfEe, amount: 0, message: 0x0000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000568656c6c6f000000000000000000000000000000000000000000000000000000)

\[ZetaChain\] Event from onCall: {“\_type”:“log”,“address”:“0x5bf5b11053e734690269C6B9D438F8C9d48F528A”,“blockHash”:“0x1abe21dfb999394922742b539b5f7ea4bee2cf435e84e43fd42d96839c7fe4f2”,“blockNumber”:139,“data”:“0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000748656c6c6f3a2000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000568656c6c6f000000000000000000000000000000000000000000000000000000”,“index”:0,“removed”:false,“topics”:\[“0x39f8c79736fed93bca390bb3d6ff7da07482edb61cd7dafcfba496821d6ab7a3”\],“transactionHash”:“0x9ce7ce063be9718e2020252b6385bc1e6a5ecdbf264dbe5f60db4226b5974cdc”,“transactionIndex”:0}
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
