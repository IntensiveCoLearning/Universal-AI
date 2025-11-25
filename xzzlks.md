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
