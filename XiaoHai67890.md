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
