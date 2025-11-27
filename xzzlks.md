---
timezone: UTC+8
---

# 吴乃泽

**GitHub ID:** xzzlks

**Telegram:** @wunaize

## Self-introduction

编程初学者，希望学习到更多AI区块链方面知识，致力于学习新技术！

## Notes

<!-- Content_START -->
# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->
# Day 4：Universal App + Hello World 心智模型学习笔记

**日期**：2025年11月27日 星期四 **核心主题**：Universal App认知深化与Hello World Demo落地规划

## 一、学习目标拆解与核心导向

今日学习完成“认知构建”与“落地规划”两大核心任务，为后续开发打基础：

1.  **认知目标**：理解Universal App合约“跨链原生”的本质——即合约逻辑天然覆盖多条公链，而非单链合约的“跨链适配版”。
    
2.  **规划目标**：明确Hello World Demo的技术栈选型与工作流，清晰界定“合约开发-前端交互-RPC连接”三大模块的核心职责，避免后续开发方向混乱。
    

## 二、学习资料重点梳理

### （一）核心资料：ZetaChain Developers EVM / Tutorials 部分

资料地址：[https://www.zetachain.com/docs/developers](https://www.zetachain.com/docs/developers)

-   **重点关注模块**：**EVM 章节**：核心提炼“Universal EVM与传统EVM的差异”——支持跨链指令（如`zetaSend`），能直接读取非EVM链数据，这是开发全链合约的技术基石。
    
-   **Tutorials 结构**：观察教程中“合约-部署-交互”的通用流程，发现所有Demo均包含“跨链参数配置”（如指定目标链ID、网关地址），这是Universal App的标志性特征。
    
-   **延伸思考**：传统单链Hello World合约仅实现“存储/打印字符串”，而全链版需考虑“跨链传递信息”——比如在EVM链触发合约，让Solana链能读取到该信息，这是今日规划的核心差异点。
    

### （二）扩展资料：Tutorials 中的 Swap / Messaging 结构

-   **共性结构提炼**：无论Swap（跨链兑换）还是Messaging（跨链消息），均遵循“**触发链合约 → ZetaChain核心层 → 目标链合约**”的三段式结构，其中ZetaChain通过Gateway自动完成跨链数据传递。
    
-   **对Hello World的启发**：可简化该结构，实现“单触发链 → ZetaChain → 多目标链”的消息同步，即完成全链版Hello World的核心逻辑。
    

## 三、Universal App 合约的核心特征

1\. 部署载体唯一：核心合约仅部署在ZetaChain，无需在以太坊、比特币等链重复部署；

2\. 跨链指令原生：合约中可直接调用ZetaChain提供的跨链API（如消息发送、资产转移）；

3\. 链间数据互通：能主动读取或被动接收来自不同公链的数据，实现全链状态同步。

## 四、实践作业：应用构想与Demo工作流规划

### （一）Universal App 核心逻辑构想

结合全链应用特性，我计划开发的首个Universal App具体逻辑如下：

1.  核心功能：用户在任意一条支持的公链（如以太坊测试网）发起交易，输入一段文本消息，该消息会被同步存储到ZetaChain，并可被其他公链（如Solana测试网）的用户读取，实现“一条消息，全链可见”。
    
2.  存储功能：合约中定义一个映射（mapping），关联“消息ID”与“消息内容+发送链ID+发送时间”；
    
3.  跨链同步：用户在以太坊发送消息后，合约通过ZetaChain API将消息ID同步至Solana网关，Solana用户可通过调用合约查询该消息ID对应的内容；
    
4.  查询功能：提供公开函数，支持按消息ID或发送链ID查询历史消息。
    

### （二）Hello World Demo 工作流确定

1\. 开发工具链选型：CLI + Hardhat

-   ZetaChain CLI提供了合约部署、跨链交易触发等核心命令，是与ZetaChain交互的必备工具；
    
-   Hardhat是以太坊生态成熟的开发框架，支持Solidity编译、测试、部署全流程，且ZetaChain提供Hardhat插件（如`@zetachain/hardhat-zeta`），可无缝适配Universal EVM；
    
-   **流程**：Hardhat编译合约 → ZetaChain CLI部署合约至测试网 → Hardhat编写测试用例验证功能 → CLI触发跨链交互。
    

2\. 运行环境选择：ZetaChain 测试网

-   ZetaChain测试网提供完整的跨链基础设施（RPC、Faucet、Explorer），可真实模拟以太坊、Solana等公链的跨链交互，且测试币获取便捷，便于验证全流程。
    
-   测试网无资产损失风险，可反复调试跨链参数（如链ID、Gas设置）。
    
-   已提前获取ZetaChain测试网ZETA代币（通过Faucet），并配置好测试网RPC地址（写入Hardhat配置文件）。
    

### 今日收获：

理解了ZetaChain作为“跨链大脑”的核心价值；明确了Hello World Demo体现“跨链数据交互”的全链特性；完成工具链与环境选型，为后续开发做好了铺垫。
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->

# Day 3：ZetaChain & Universal Blockchain 核心概念学习笔记

**日期**：2025年11月26日 星期三 **核心主题**：Universal Blockchain系列概念解析与ZetaChain架构可视化

## 一、学习目标拆解与达成标准

1.  **概念理解**：不仅需掌握“通用区块链/Universal EVM/Universal App/Omnichain Smart Contract”的定义，更要厘清四者间的逻辑关联，能结合ZetaChain场景举例说明。
    
2.  **架构可视化**：绘制的架构图需明确“ZetaChain-多条公链-Gateway”的层级关系与数据流向，标注核心组件的核心作用，做到“图清意明”。
    

## 二、核心学习资料梳理（含重点方向）

| 资料类别 | 资料名称与地址 | 重点学习方向 |
| --- | --- | --- |
| 核心资料 | Universal Apps 概览https://www.zetachain.com/docs/start/app | 1. Universal App与传统跨链应用的差异；2. 基于ZetaChain开发Universal App的优势；3. 典型应用场景案例 |
| 核心资料 | 开发者总览（Universal EVM等）https://www.zetachain.com/docs/developers | 1. Universal EVM的技术特性；2. Gateway的跨链交互机制；3. ZetaChain跨链技术的核心原理 |
| 扩展资料 | 为什么在ZetaChain上开发？https://www.zetachain.com/docs/start/build | 提炼ZetaChain的核心竞争力，关联Universal系列概念的价值 |
| 扩展资料 | ZetaChain架构设计https://www.zetachain.com/docs/developers/architecture/overview | 快速抓取“节点网络”“跨链协议层”等关键架构模块，辅助理解整体逻辑 |

## 三、核心概念解析（基于资料的个人理解）

### （一）基础概念关联

核心逻辑：**通用区块链（ZetaChain）** 通过 **Universal EVM** 提供开发底座，支撑开发者构建 **Universal App**，而 **Omnichain Smart Contract（全链智能合约）** 是Universal App的核心实现载体，**Gateway** 则是实现跨链交互的关键桥梁。

### （二）关键概念

**1\. Universal App（通用应用）**简单来说，Universal App是一种“一次开发，全链可用”的区块链应用，它不需要为每条公链单独适配代码，就能在Bitcoin、Ethereum、Solana等不同链上与用户和资产交互。举个例子：传统跨链DeFi应用，可能需要在ETH链部署一套合约，在BSC链再部署一套，且两套合约间需额外开发跨链同步逻辑；而基于ZetaChain的Universal App，只需开发一套核心逻辑，就能同时服务于多条链的用户，用户在ETH链的资产和Solana链的资产可直接通过该应用完成交互，无需切换应用或依赖第三方跨链工具。核心价值：降低开发者跨链开发成本，提升用户跨链使用体验，打破不同公链间的“信息孤岛”和“资产孤岛”。

**2\. Gateway（网关）**Gateway是ZetaChain实现“跨链通信”的核心组件，相当于连接ZetaChain与其他公链的“翻译官”和“传送带”。它的核心作用主要有两个：

一是“信息翻译与传递”：不同公链的交易格式、数据标准不同，当某条公链（如Ethereum）上发生用户操作时，Gateway会将该操作的关键信息（如用户地址、操作类型、资产金额）转换为ZetaChain能识别的格式，传递给ZetaChain核心网络；同时，ZetaChain的指令也会通过Gateway转换为对应公链的格式，下发到目标公链执行。

二是“资产跨链保障”：在资产跨链过程中，Gateway会协同ZetaChain的质押机制，确保原链资产锁定与目标链资产 mint（铸造）、burn（销毁）的同步性，防止资产双重花费，保障跨链资产的安全性。比如用户将ETH链上的USDT跨到Solana链，Gateway会确认ETH链上的USDT已锁定后，通知ZetaChain在Solana链上为用户铸造等量的跨链USDT，整个过程由Gateway全程监控验证。

**3\. Universal EVM（通用以太坊虚拟机）**EVM是以太坊生态的核心开发环境，而很多公链（如BSC、Polygon）虽兼容EVM，但仍需单独适配。ZetaChain的Universal EVM，是在EVM基础上做了“跨链扩展”的虚拟机，它不仅支持开发者使用Solidity等EVM生态的编程语言，更能让基于它开发的智能合约具备“跨链调用能力”——合约可以直接读取其他非EVM链（如Bitcoin、Solana）的数据，或向这些链发送操作指令，这是传统EVM和兼容EVM所不具备的核心能力。

**4\. Omnichain Smart Contract（全链智能合约）**这是Universal App的“大脑”，是部署在ZetaChain上的智能合约，它能通过Gateway与多条公链进行交互。与传统合约不同，它的逻辑中包含了跨链交互的原生支持，比如可以在同一合约函数中，同时处理Ethereum的ETH和Solana的SOL的兑换逻辑，无需依赖外部跨链协议。】

## 四、实践作业：架构图绘制与说明

### （一）ZetaChain 核心架构图（ZetaChain + 多公链 + Gateway）

![exported_image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/xzzlks/images/2025-11-26-1764160241728-exported_image.png)

### （二）架构图说明

1.  **核心层级**：ZetaChain核心网络是整个架构的“中枢”，其中Universal EVM为全链智能合约提供运行环境，跨链协议层负责处理所有跨链交互逻辑，节点网络通过共识机制保障数据安全与不可篡改。
    
2.  **连接层级**：Gateway是ZetaChain与各公链的“专属连接通道”，每条公链对应一个独立的Gateway，确保跨链数据传递的精准性和安全性。
    
3.  **数据流向**： 正向流：用户在某条公链（如Ethereum）上发起操作后，操作数据先传递至对应Gateway，由Gateway完成格式转换后提交给ZetaChain的跨链协议层处理；反向流：ZetaChain处理完成后，将执行结果通过对应Gateway转换为目标公链的格式，下发至目标公链（如Solana），最终完成跨链操作的闭环。
    

-   **收获**：1. 厘清了Universal系列核心概念的逻辑关系，不再混淆；2. 理解了Gateway在跨链中的核心作用，掌握了ZetaChain架构的核心层级；3. 通过绘图将抽象的架构具象化，加深了对“全链交互”的理解。
    
-   **不足**：1. 对Universal EVM的技术实现细节（如如何兼容非EVM链）理解不够深入；2. 架构图中未体现ZetaChain的质押机制与Gateway的协同逻辑，需进一步补充。
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->


### ZetaChain CLI 安装与验证（本地环境：Windows）

1.  **安装步骤**： 前置依赖：确认已安装Go（版本≥1.20）。打开命令提示符（CMD）或PowerShell，输入`go version`验证；若未安装，访问Go官网（[https://go.dev/dl/）下载Windows版安装包，勾选“Add](https://go.dev/dl/）下载Windows版安装包，勾选“Add) Go to PATH”选项后完成安装。
    
2.  安装CLI：在CMD或PowerShell中执行命令`go install github.com/zeta-chain/cli/cmd/zetacli@latest`，等待命令执行完成。
    
3.  验证安装：输入`zetacli --version`，若成功输出版本信息（如v1.0.0），则说明安装完成。
    
4.  **遇到的问题与解决**： 问题1：执行`zetacli`命令时提示“不是内部或外部命令，也不是可运行的程序”。
    
5.  解决1：右键“此电脑”→“属性”→“高级系统设置”→“环境变量”，在“用户变量”的“Path”中添加`%GOPATH%\bin`（GOPATH默认路径为C:\\Users\\你的用户名\\go），添加后重启CMD或PowerShell即可。
    
6.  问题2：安装Go后执行`go version`仍提示命令无效。
    
7.  解决2：重新运行Go安装包，确保“Add Go to PATH”已勾选；若已勾选，手动在“系统变量”的“Path”中添加Go安装路径（通常为C:\\Program Files\\Go\\bin），重启终端验证。
    

### **测试网关键入口汇总**：

-   RPC地址：[https://zetachain-athens-evm.blockpi.network/v1/rpc/public（文档获取的具体地址）](https://zetachain-athens-evm.blockpi.network/v1/rpc/public（文档获取的具体地址）)
    
-   Faucet链接：[https://zetachain.faucetme.pro/（输入钱包地址即可领取测试币，每日限额）](https://zetachain.faucetme.pro/（输入钱包地址即可领取测试币，每日限额）)
    
-   Explorer地址：[https://testnet.zetachain.explorers.guru/](https://testnet.zetachain.explorers.guru/)
    

### Qwen API 连通性测试

Postman测试

1.  设置请求方式：POST，请求URL：[https://dashscope.aliyuncs.com/api/v1/services/aigc/text-generation/generation。](https://dashscope.aliyuncs.com/api/v1/services/aigc/text-generation/generation。)
    
2.  设置请求头：Content-Type: application/json；Authorization: Bearer xxx。
    
3.  设置请求体：与终端测试一致的JSON内容，具体如下： `{ "model": "qwen-7b", "input": { "prompt": "请简单介绍ZetaChain" }, "parameters": { "temperature": 0.7 } }`
    
4.  发送请求，结果：同样返回200状态码及有效响应，验证Postman环境连通性正常。
    

1\. 收获：成功完成ZetaChain CLI本地安装、Qwen API连通性测试，明确了ZetaChain测试网核心服务的使用入口，为后续开发奠定了环境基础。

2\. 不足：对ZetaChain CLI的具体命令使用尚不熟悉，Qwen API的高级参数配置（如流式响应、上下文管理）未深入探索。
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->



# ZetaChain

### 概述

ZetaChain是实现跨链互操作性的区块链项目，使智能合约可以访问多个区块链的数据和资产，为开发者构建了一个“全链”开发平台

## 功能特点

## 链式编排

能通过单一智能合约协调跨多条区块链的交易，在同一框架内，基于ZetaChain的应用可以管理来自不同区块链的代币转账和合约调用并发起向连接网络的交易。链式编排能力简化了开发流程，降低了发生错误的可能性，并确保你的应用可以随着更多的区块链集成而扩展。

### 前后兼容性

在ZetaChain部署的应用会自动连接到所有ZetaChain生态系统中的网络，即使有新区块链的加入，你的应用依然可以与这些网络进行兼容。此功能简化了维护，无需因新区块链加入而更新合约或重新部署，提供了稳定的用户体验。

### 转移本地令牌

ZetaChain 上通用应用的核心功能之一，能通过本地代币在所有连接链之间转移价值，意味着用户可以执行涉及每个区块链实际原生资产的交易。跨链代币转移功能提升了流动性，无需中介或 复杂的令牌包装机制。简化了用户体验，降低了跨链交易相关风险，增强了跨链运营中的信任和可靠性。

### 熟悉的开发者体验

ZetaChain在UEVM上运行智能合约， 该系统与以太坊的EVM完全兼容。意味着开发者可以使用Solidity或其他兼容EVM语言开发智能合约。支持流行开发工具，如Foundry、Hardhat、Slither，使开发者能使用熟悉的工作流程、调试工具和测试框架，使流程更加完善，打造更高效、更少出错的通用应用。

### Gasless EVM执行环境

用户可以调用运行在ZetaChain的EVM上的应用，以及所有连接链的合约，只需在连接链上支付gas。降低了用户开支，提升应用可访问性

# 开发环境配置与部署

在开始之前需配置以下工具：

Node.js（v21或更晚版本）运行 ZetaChain CLI 并管理 JavaScript 依赖

Yarn 或 npm 用于安装和更新项目库

Git 管理项目资源，与他人合作编程

JQ 轻量级命令行JSON处理器， 解析Localnet输出和编写shell脚本

Foundry 快速、模块化的以太坊工具包，编译合约、管理依赖和运行 Localnet

### 环境配置

安装ZetaChain CLI：npm install -g zetachain

重要工具，进行跨链调用、转账代币、跟踪跨链交易，管理跨多个网络的账户，查询等

打印所有当前连接在ZetaChain上的链：zetachain query chains list

用于确认设置是否正常

Localnet：环境包括ZetaChain、EVM、Solana、Sui和TON，是构建和测试通用应用最快的方法，无需依赖公共测试网。

运行Localnet需安装Foundry，它提供了 EVM 开发的核心工具，是启动的必要条件。

### Foundry安装

（1）预编译的二进制文件，从GitHub上下载：[https://github.com/foundry-rs/foundry/releases](https://github.com/foundry-rs/foundry/releases)

（2）使用Foundryup，Foundryup 是 Foundry 工具链的官方安装程序，终端执行以下命令：curl -L [https://foundry.paradigm.xyz](https://foundry.paradigm.xyz) | bash

注意！Foundryup目前不支持Powershell或命令提示符（Cmd）Windows用户需安装并使用Git BASH或WSL作为终端

（3）使用最新稳定版的Rust构建，旧版需使用以下命令更新：rustup update stable

Windows用户还需安装最新版的Visual Studio并安装“用C++进行桌面开发”工作负载

（4）通过Cargo安装：cargo install --git [https://github.com/foundry-rs/foundry](https://github.com/foundry-rs/foundry) --profile release --locked forge cast chisel anvil

foundryup --branch master 从master分支安装 foundryup --path path/to/foundry 指定Foundry的安装路径

（5）手动从本地Foundry仓库副本构建：

\# clone the repository

git clone [https://github.com/foundry-rs/foundry.git](https://github.com/foundry-rs/foundry.git)

cd foundry

\# install Forge

cargo install --path ./crates/forge --profile release --force --locked

\# install Cast

cargo install --path ./crates/cast --profile release --force --locked

\# install Anvil

cargo install --path ./crates/anvil --profile release --force --locked

\# install Chisel

cargo install --path ./crates/chisel --profile release --force --locked

（6）GitHub Actions CI安装：[https://github.com/foundry-rs/foundry-toolchain](https://github.com/foundry-rs/foundry-toolchain)

（7）Docker容器拉取最新版本：docker pull [ghcr.io/foundry-rs/foundry:latest](http://ghcr.io/foundry-rs/foundry:latest)

Foundry仓库本地构建Docer镜像：docker build -t foundry .
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
