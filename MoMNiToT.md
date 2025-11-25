---
timezone: UTC+8
---

# 王思哲

**GitHub ID:** MoMNiToT

**Telegram:** @WMon_03SEP

## Self-introduction

计科专业的大一本科生，正在努力向成为一位Dapp全栈开发者迈进

## Notes

<!-- Content_START -->
# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->
# **Day2：环境和工具实战**

## **Qwen API调用**

利用 curl 完成了Qwen API的调用，有了输出

**调用PowerShell：**

```
 # 1. 确保环境变量已设置（如果没提前设置，可直接替换 $env:DASHSCOPE_API_KEY 为你的密钥字符串）
 if (-not $env:DASHSCOPE_API_KEY) {
     Write-Warning "未检测到 DASHSCOPE_API_KEY 环境变量，请先设置：`$env:DASHSCOPE_API_KEY = '你的密钥'"
     # 直接填写密钥的话，解开下面一行注释并替换：
     # $apiKey = "sk-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
 } else {
     $apiKey = $env:DASHSCOPE_API_KEY
 }
 ​
 # 2. 定义请求头（对应原 curl 的 -H 参数）
 $headers = @{
     "Authorization" = "Bearer $apiKey"  # 授权信息（复用环境变量）
     "Content-Type"  = "application/json" # 声明请求体为 JSON 格式
 }
 ​
 # 3. 定义请求体（对应原 curl 的 -d 参数，用哈希表转 JSON 避免语法错误）
 $body = @{
     model = "qwen-plus"  # 模型名称
     messages = @(        # 对话消息数组
         @{
             role    = "system"
             content = "You are a helpful assistant."
         },
         @{
             role    = "user"
             content = "你是谁？"
         }
     )
 } | ConvertTo-Json  # 自动转换成标准 JSON 字符串（避免手动写 JSON 漏引号/逗号）
 ​
 # 4. 发送请求并获取响应（Invoke-RestMethod 自动解析 JSON 响应）
 try {
     $response = Invoke-RestMethod `
         -Uri "https://dashscope.aliyuncs.com/compatible-mode/v1/chat/completions" `
         -Method Post `
         -Headers $headers `
         -Body $body `
         -ErrorAction Stop
 ​
     # 输出响应结果（格式化显示，更易读）
     $response | ConvertTo-Json -Depth 10
 }
 catch {
     # 错误处理（显示详细报错，方便排查）
     Write-Error "请求失败：$($_.Exception.Message)"
     if ($_.Exception.Response) {
         # 显示接口返回的错误详情（如阿里云的错误码、提示）
         $errorStream = New-Object System.IO.StreamReader($_.Exception.Response.GetResponseStream())
         $errorDetails = $errorStream.ReadToEnd() | ConvertFrom-Json
         Write-Error "接口错误详情：$($errorDetails | Format-List | Out-String)"
     }
 }
```

拿大模型改写的官网的调用，问就是太笨了就连-H是Unix命令都不知道，长见识了

**输出 Response：**

```
 {
     "choices":  [
                     {
                         "message":  {
                                         "role":  "assistant",
                                         "content":  "Hello! It looks like your message might have gotten scrambled or didn\u0027t come through fully. Could you please clarify or provide more details about what you\u0027re asking? I\u0027m here to help! ð"
                                     },
                         "finish_reason":  "stop",
                         "index":  0,
                         "logprobs":  null
                     }
                 ],
     "object":  "chat.completion",
     "usage":  {
                   "prompt_tokens":  20,
                   "completion_tokens":  40,
                   "total_tokens":  60,
                   "prompt_tokens_details":  {
                                                 "cached_tokens":  0
                                             }
               },
     "created":  1764074426,
     "system_fingerprint":  null,
     "model":  "qwen-plus",
     "id":  "chatcmpl-271a0df8-df81-4017-95d3-89def8385f4a"
 }
```

## **CLI使用**

**CLI’s Features：**

-   Scaffold new ZetaChain universal apps from templates 从模板快速搭建新的 ZetaChain 通用应用
    
-   Spin up a local multi-chain development environment (EVM, Solana, etc.) in one command 在一个命令中启动本地多链开发环境（EVM、Solana 等）
    
-   Query cross-chain fees, contracts, balances, cross-chain transaction, tokens, and more 查询跨链费用、合约、余额、跨链交易、代币等
    
-   Make cross-chain calls between Solana, Sui, Bitcoin, TON, and universal apps on ZetaChain 在 Solana、Sui、Bitcoin、TON 和 ZetaChain 上的通用应用之间进行跨链调用
    
-   Transfer supported tokens across connected chains 在连接的链之间转移支持的代币
    

![image-20251125205643580](file:///C:/Users/%E7%8E%8B%E6%80%9D%E5%93%B2/Documents/MOCODE/Universal%20BlockChain/Day2%EF%BC%9A%E7%8E%AF%E5%A2%83%E5%92%8C%E5%B7%A5%E5%85%B7%E5%AE%9E%E6%88%98.assets/image-20251125205643580.png?lastModify=1764076471)

有些警告，但是创建了样例项目模板

## **测试网的入口**

先补一些基础概念：

1.  RPC（Remote Procedure Call，远程过程调用）
    

-   **核心定义**：是应用程序（钱包、DApp、开发工具）与区块链节点之间的 “通信桥梁”，本质是一组标准化的接口协议。
    
-   **作用**：区块链节点存储着全网的账本数据，你的钱包转账、部署智能合约、查询余额等操作，都需要通过 RPC 接口发送指令给节点，节点处理后返回结果 —— 没有 RPC，应用就无法和区块链网络交互。
    
-   **测试网场景**：测试网 RPC 是专门对接测试链节点的地址（比如之前提到的 Goerli 测试网 RPC），开发者在 MetaMask、Hardhat/Truffle 等工具中配置测试网 RPC，才能让本地应用连接到测试链，而非主链。
    

2.  Faucet（水龙头）
    

-   **核心定义**：测试网专属的免费代币发放工具（“水龙头” 形象比喻 “流出” 代币）。
    
-   **作用**：主网代币有实际价值，而测试网代币无价值，仅用于开发和测试（比如部署合约、测试转账功能）。Faucet 的核心作用就是给开发者的测试钱包地址发放测试代币，解决测试链上 “无币可用” 的问题。
    
-   **测试网场景**：不同测试网有专属 Faucet（如 BSC 测试网 Faucet），通常需要连接钱包、完成简单验证（如 Twitter/Discord 认证）后领取，部分 Faucet 会限制每日领取次数 / 数量。
    

3.  Explorer（区块浏览器）
    

-   **核心定义**：可视化查询区块链数据的工具，相当于区块链的 “搜索引擎”。
    
-   **作用**：可以查询所有公开的链上数据，包括：交易哈希、区块高度、钱包余额、智能合约代码、代币流转记录、交易状态（成功 / 失败）等。开发者能通过它排查测试中的问题（比如转账没到账，查交易哈希看是否失败、失败原因）。
    
-   **测试网场景**：测试网 Explorer（如 Sepolia Etherscan）只展示对应测试链的数据，和主网 Explorer（如 Etherscan）相互独立，能清晰查看测试链上的操作记录，验证合约部署、交易等行为是否成功。
    

先附上一个Sepolia：

**PRC：**

```
 https://sepolia.infura.io/v3/<YOUR_API_KEY>
 https://rpc.sepolia.org (亚洲)
 https://rpc2.sepolia.org (欧洲)
```

**Faucet:**

```
 https://sepolia-faucet.pk910.de (需Twitter认证)
 https://faucet.sepolia.dev (需Discord认证)
 https://cloud.google.com/application/web3/faucet/ethereum/sepolia
```

**Explorer**: [https://sepolia.etherscan.io](https://sepolia.etherscan.io)
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->

**Universal APP：**

ZetaChain 的通用应用程序（Universal Apps）能够与多个区块链（如以太坊、比特币、索拉纳等）无缝交互，通过智能合约实现跨链操作。

-   通用应用程序可以接受和发起跨链的合约调用和代币转移，实现复杂的跨链交易。
    
-   通用应用程序通过 ZetaChain 的通用 EVM 部署，该 EVM 支持跨链互操作性，使得现有合约可以通过修改获得跨链能力。
    
-   用户可以通过交互网关合约来调用通用应用程序，网关合约允许用户传递数据和代币。
    
-   每个连接链都有一个单一的网关合约，用于接收代币存款和调用通用应用程序。
    
-   ZRC-20 代币是与 ERC-20 兼容的跨链代币，可以退回到原始链。
    
-   通用应用程序的 `onCall` 方法可以访问额外的上下文信息，如原始发送者地址和链 ID。
    
-   通用应用程序可以通过交易所在 ZetaChain 上交换代币，并调用网关合约将代币退回到连接链。
    
-   通用应用程序提供气体抽象，自动处理跨链气体费用，用户只需提供足够的代币即可。
    
-   通用应用程序支持多种区块链，包括 EVM 链、比特币、索拉纳、TON、Sui，以及未来将支持的 Cosmos（通过 IBC）等。
    
-   比特币用户可以通过发送 BTC 和数据到网关地址来调用通用应用程序，无需在其他链上拥有账户或获取气体代币。
    

**Universal EVM：**

ZetaChain 是一个基于 CosmosSDK 和 CometBFT 共识引擎的 Proof of Stake (PoS) 区块链，提供了一个具有内置跨链交互功能的智能合约平台，使得开发跨链应用成为可能。

-   ZetaChain 是一个基于 CosmosSDK 和 CometBFT 共识引擎的 PoS 区块链，提供了完整的 EVM 兼容性。
    
-   ZetaChain 采用中心 - 边缘模型，通过核心验证器和观察者 - 签署者验证器确保跨链消息和交易的一致处理。
    
-   ZetaChain 的关键模块包括跨链模块、观察者模块、可替换模块、激励模块和权限模块，它们共同支持跨链交易的处理和网络安全。
    
-   在 ZetaChain 和连接的外部链上部署的协议合约提供了标准化的交易入口点，并管理跨链交易的元数据。
    
-   ZetaChain 通过代币抵押和正负激励机制确保经济安全，并鼓励验证器诚实行事。
    
-   跨链交易工作流程确保了资产和数据在 ZetaChain 和连接的区块链之间的安全转移，无论是入站还是出站交易。
    
-   ZetaChain 为开发者提供了一个简化跨链应用开发的平台，使他们能够专注于应用逻辑，而不是基础设施。
    

**Gateway：**

1.  **Gateway 的作用**：它是一个跨链通信的中介，允许不同区块链之间的合约和通用应用程序进行交互。
    
2.  **连接链上的 Gateway 实现**：不同的连接链使用不同的技术实现 Gateway，包括智能合约、程序和 TSS MPC 地址。
    
3.  **Gateway 的统一性**：每个连接的链只有一个 Gateway，用于所有通用应用程序的交互。
    
4.  **支持的功能**：Gateway 支持存入和提取各种代币，以及与合约调用相结合的操作。
    
5.  **特定链的特殊功能**：不同的连接链支持不同的存取资产功能，例如比特币只支持 BTC 存入，Solana 支持 SOL 和 SPL 代币。
    
6.  **单资产操作限制**：目前，Gateway 只支持一次操作一个资产，但未来将支持多资产操作。
    
7.  **跨链操作的反转处理**：Gateway 提供了处理跨链操作失败时的退款机制，以增加交易的灵活性和安全性。
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
