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
# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->
# **Day4：心智模型**

EVM架构：

NaN.  中心辐射模型：中心ZetaChain和辐射外部区块链
      
NaN.  核心验证者：参与共识，维护块的状态，有奖励和惩罚
      
NaN.  观察者签名验证者：检测和处理跨链交易，以投票形式，用TSS确保密钥不是一人独有
      

EVM模块：

NaN.  跨链模块：管理交易记录、维护交易状态、存储交易细节（CCTX有两个）
      
NaN.  观察者模块：管理观察者节点、事件投票、定义共识规则
      
NaN.  可替代模块：把其他链的代币全部转化为ZRC-20
      
NaN.  排放模块：计算奖励、发放奖励
      
NaN.  权限模块：对敏感事件的进一步限制监管
      

EVM协议合约：

NaN.  GatewayZEVM、ZRC-20、ContractRegistry
      
NaN.  GatewayEVM、ERC20Custody、ContractRegistry
      

💫 **跨链流程**

NaN.  入站：在外部链交易Gateway、观察者模块观察和投票、跨链模块创建交易、可替代模块转化代币
      
NaN.  出站：发起要求、验证者准备、TSS签名、提交广播、跨链模块更新交易状态
      

![屏幕截图 2025-11-27 205336.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/MoMNiToT/images/2025-11-27-1764249452364-_____2025-11-27_205336.png)

**💫Gas费**

1.  入链：只付「源链的原生 gas 费」
    
2.  直接调用：需要付「ZetaChain EVM 的 gas 费」，有基础费（销毁） + 小费（奖励）
    
3.  出链：需要付「提现 gas 费」，而且只能用对应ZRC代币
    

跨链操作：

1.  CCTX是主要内容、里面包含Inbound 和 Outbound
    
2.  CCTX追踪：用入链Inbound哈希算CCTX，用前一个CCTX算后一个CCTX
    

所以所有的跨链操作都可以根据入站CCTX和出站CCTX的存在与否，以及他们的连接状态判断操作状态

工作流：CLI + Hardhat + ZetaChain Localnet
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->

# **Day3：核心概念**

## **Universal Apps 概览**

通用应用概念：

一个通用应用是部署在 ZetaChain 上的智能合约，可以完成跨越多个链复交易

**💥 补充：EVM**

EVM 本质是一个去中心化的、沙盒式的虚拟计算机，专门负责执行以太坊网络上的智能合约代码。所有以太坊节点都会运行 EVM，确保全网对智能合约的执行结果达成一致，是智能合约能在区块链上稳定、可信运行的基础。

💫 **核心调用逻辑**

NaN.  调用入口：网关合约（Gateway contract）
      

-   每个接入 ZetaChain 的公链，都有且只有一个专属的网关合约；
    
-   这个合约是用户和 “通用应用” 交互的唯一通道，主要做两件事：接收用户存入的代币、提供调用通用应用的接口；
    
-   用户调用时，既能传 “自定义数据”（比如交易指令、NFT 参数），也能传 “代币”（比如 ETH），不用分开操作。
    

2.  核心触发机制：onCall 方法 —— 通用应用的 “启动开关”
    

用户调用通用应用的动作，会自动触发通用应用内置的 `onCall` 方法，这个方法能接收三类关键信息：

-   自定义消息：任意格式的数据，比如例子里的 “hello”（测试信息）、“I want BNB”（兑换指令），也可以是接收地址、NFT 铸造属性、代币类型等交易核心信息；
    
-   ZRC-20 代币：这是 ZetaChain 的核心设计 —— 所有 “连接链” 的原生代币（比如 ETH、BNB）或支持的 ERC-20 代币，在 ZetaChain 上都会生成对应的 “映射版本”（ZRC-20），且 ZRC-20 兼容 ERC-20 标准；
    
-   上下文信息：比如谁发起的调用（原始发送者地址）、从哪条链发起的（链 ID），方便应用验证身份、判断来源。
    

有关 BTC：

只需要钱包地址 + “BTC+自定义数据” 传输给 GateWay，网关端口映射成 ZRC-20 BTC 后加入通用应用，使得通用应用更加”通用”，并不需要额外做对 BTC 这类没有智能合约的链的适配；且用户只需要有 BTC 就行。

💫 **Gas Abstraction**

-   入站调用：仅需支付自己所在链的常规费用
    
-   出战调用：需要付 Gas 费，但是会和传入的代币一起计算，不需要额外支付，只需要数量足够
    

🕳 **一个例子：**

假设比特币用户想做 “用 1 BTC 换 BNB，并存到自己的 BNB 链地址”，全程操作和底层逻辑：

NaN.  用户操作：打开比特币钱包，签署一笔交易 —— 向比特币链的 ZetaChain 网关地址，发送 1 BTC + 数据 “换 BNB，目标地址：0xXXX（BNB 链地址）”；
      
NaN.  底层第一步（入站）：BTC 到网关后，自动映射成 ZetaChain 上的 1 个 ZRC-20 BTC；同时触发通用应用的`onCall`方法（之前讲的触发机制）；
      
NaN.  底层第二步（应用处理）：
      
      ① 通用应用收到 ZRC-20 BTC 和 “换 BNB” 的指令；
      
      ② 计算需要多少 BNB 链的 gas：假设需要 0.01 BNB 对应的 ZRC-20 BNB；
      
      ③ 在 ZetaChain 的 DEX 上，用 1 个 ZRC-20 BTC 中的 “0.01 BTC 对应的 ZRC-20 BTC”，兑换成 ZRC-20 BNB（作为 gas）；
      
      ④ 剩下的 “0.99 BTC 对应的 ZRC-20 BTC”，兑换成 ZRC-20 BNB（用户最终要的资产）；
      
NaN.  底层第三步（出站）：
      
      ① 通用应用调用 BNB 链的网关合约；
      
      ② 扣除之前兑换好的 ZRC-20 BNB（支付 BNB 链的 gas）；
      
      ③ 把剩下的 ZRC-20 BNB 转回 BNB 链，自动变成原生 BNB，存入用户指定的 BNB 链地址；
      
NaN.  用户最终结果：只签了一笔比特币交易，没管任何 gas 细节，最后在自己的 BNB 链地址收到了对应的 BNB。
      

![微信图片_20251126224340_92_5.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/MoMNiToT/images/2025-11-26-1764168285207-_____20251126224340_92_5.png)
<!-- DAILY_CHECKIN_2025-11-26_END -->

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
    

![屏幕截图 2025-11-25 211518.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/MoMNiToT/images/2025-11-25-1764076531636-_____2025-11-25_211518.png)

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
