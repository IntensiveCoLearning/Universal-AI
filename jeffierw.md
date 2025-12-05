---
timezone: UTC+8
---

# 害虫

**GitHub ID:** jeffierw

**Telegram:** @yepwan

## Self-introduction

小白

## Notes

<!-- Content_START -->
# 2025-12-05
<!-- DAILY_CHECKIN_2025-12-05_START -->
# Day 12

1.  端到端 Demo 串联
    

仓库代码：[https://github.com/jeffierw/zetachain/tree/main/qwen\_agent\_demo/zeta\_interface\_agent.py](https://github.com/jeffierw/zetachain/tree/main/qwen_agent_demo/zeta_interface_agent.py)
<!-- DAILY_CHECKIN_2025-12-05_END -->

# 2025-12-04
<!-- DAILY_CHECKIN_2025-12-04_START -->

````markdown
# Day 11

1. Qwen-Agent × ZetaChain（接口层设计）

仓库代码：https://github.com/jeffierw/zetachain/tree/main/qwen_agent_demo/zeta_interface_agent.py

```shell
(venv) yepw@yepwdeMacBook-Pro qwen_agent_demo % python zeta_interface_agent.py

======================================================================
  DeFi Swap 意图解析 + Zeta 接口层 Agent
======================================================================

📋 配置信息:
   模型: qwen-plus
   工具: parse_swap_intent + zeta_interface_layer

======================================================================
  DeFi Swap 意图解析 + Zeta 接口层 Agent
======================================================================

💡 提示：
  - 输入 DeFi Swap 相关的自然语言，例如：
    • 帮我在 Base 上用 10 USDC 换成 ETH
    • 把我 50 U 兑换成 Polygon 上的 MATIC
  - Agent 会自动解析并返回 JSON 格式的结果，并生成 ZetaChain 交易计划

  输入 'exit'、'quit' 或 '退出' 来结束对话
======================================================================

👤 你: 帮 帮我在 Base 上用 10 USDC 换成 ETH

🤖 Agent: 2025-12-04 23:17:21,041 - base.py - 780 - INFO - ALL tokens: 15, Available tokens: 57800
2025-12-04 23:17:24,496 - base.py - 780 - INFO - ALL tokens: 75, Available tokens: 57800


🔧 [调用工具] parse_swap_intent
   参数: {"text": "帮我在 Base 上用 10 USDC 换成 ETH"}
✅ [工具返回] {"chain": "base", "tokenIn": "USDC", "tokenOut": "ETH", "amount": "10"}

📋 解析结果 (JSON):
{
  "chain": "base",
  "tokenIn": "USDC",
  "tokenOut": "ETH",
  "amount": "10"
}

🚀 [ZetaChain 接口层] 已根据解析结果生成交易计划：

=== 解析后的意图（parse_swap_intent 输出） ===
{
  "chain": "base",
  "tokenIn": "USDC",
  "tokenOut": "ETH",
  "amount": "10"
}

=== 规划得到的合约调用计划 ===
{
  "入口链": "zetachain",
  "目标链": "base",
  "调用合约": "OmniSwapController (0xZETA_OMNI_SWAP_CONTROLLER)",
  "调用方法": "swapAcrossChains",
  "调用参数": {
    "目标链": "base",
    "输入ZRC20": "0xZRC20_USDC_ON_ZETA",
    "数量": "10",
    "目标代币": "ETH",
    "接收者": "0xYourWallet",
    "远程适配器": "BaseSwapAdapter",
    "远程DEX路由器": "UniswapV3Router02"
  },
  "备注": [
    "在 ZetaChain 上使用 ZETA 支付 gas。",
    "通过连接器 0xBASE_ZETA_CONNECTOR 转发到 base。",
    "远程交换由适配器 BaseSwapAdapter 使用 UniswapV3Router02 处理。"
  ]
}

✅ 综述：准备发起的交易（示意）
- 链路: zetachain -> base
- 合约调用: OmniSwapController.swapAcrossChains
- 参数: {"目标链": "base", "输入ZRC20": "0xZRC20_USDC_ON_ZETA", "数量": "10", "目标代币": "ETH", "接收者": "0xYourWallet", "远程适配器": "BaseSwapAdapter", "远程DEX路由器": "UniswapV3Router02"}
- 备注: 在 ZetaChain 上使用 ZETA 支付 gas。 | 通过连接器 0xBASE_ZETA_CONNECTOR 转发到 base。 | 远程交换由适配器 BaseSwapAdapter 使用 UniswapV3Router02 处理。
```
````
<!-- DAILY_CHECKIN_2025-12-04_END -->

# 2025-12-03
<!-- DAILY_CHECKIN_2025-12-03_START -->


````markdown
# Day 10

1. 设计一个工具：parse_swap_intent(text)，返回结构化 JSON

仓库代码：https://github.com/jeffierw/zetachain/tree/main/qwen_agent_demo/defi_agent.py

```shell
  (venv) yepw@yepwdeMacBook-Pro qwen_agent_demo % python defi_agent.py

======================================================================
  DeFi Swap 意图解析 Agent
======================================================================

📋 配置信息:
   模型: qwen-plus
   工具: parse_swap_intent

======================================================================
  DeFi Swap 意图解析 Agent
======================================================================

💡 提示：
  - 输入 DeFi Swap 相关的自然语言，例如：
    • 帮我在 Base 上用 10 USDC 换成 ETH
    • 把我 50 U 兑换成 Polygon 上的 MATIC
  - Agent 会自动解析并返回 JSON 格式的结果

  输入 'exit'、'quit' 或 '退出' 来结束对话
======================================================================

👤 你: 帮我在 Base 上用 10 USDC 换成 ETH

🤖 Agent: 2025-12-03 23:43:37,703 - base.py - 780 - INFO - ALL tokens: 15, Available tokens: 57834
2025-12-03 23:43:38,620 - base.py - 780 - INFO - ALL tokens: 75, Available tokens: 57834


🔧 [调用工具] parse_swap_intent
   参数: {"text": "帮我在 Base 上用 10 USDC 换成 ETH"}
✅ [工具返回] {"chain": "base", "tokenIn": "USDC", "tokenOut": "ETH", "amount": "10"}

📋 解析结果 (JSON):
{
  "chain": "base",
  "tokenIn": "USDC",
  "tokenOut": "ETH",
  "amount": "10"
}

👤 你: 把我 50 U 兑换成 Polygon 上的 MATIC

🤖 Agent: 2025-12-03 23:43:45,848 - base.py - 780 - INFO - ALL tokens: 62, Available tokens: 57834
2025-12-03 23:43:46,640 - base.py - 780 - INFO - ALL tokens: 121, Available tokens: 57834


🔧 [调用工具] parse_swap_intent
   参数: {"text": "把我 50 U 兑换成 Polygon 上的 MATIC"}
✅ [工具返回] {"chain": "polygon", "tokenIn": "USDT", "tokenOut": "MATIC", "amount": "50"}

📋 解析结果 (JSON):
{
  "chain": "polygon",
  "tokenIn": "USDT",
  "tokenOut": "MATIC",
  "amount": "50"
}
```
````
<!-- DAILY_CHECKIN_2025-12-03_END -->

# 2025-12-02
<!-- DAILY_CHECKIN_2025-12-02_START -->



````markdown
# Day 9

1. 跑通一个 Qwen-Agent 官方示例 自定义一个简单 Tool

代码仓库：https://github.com/jeffierw/zetachain/tree/main/qwen_agent_demo

```python
(venv) yepw@yepwdeMacBook-Pro qwen_agent_demo % python agent_with_tools.py

======================================================================
  Qwen-Agent 交互式工具助手
======================================================================

📋 配置信息:
   模型: qwen-plus
   可用工具: to_uppercase, calculate_sum, string_info

======================================================================
  开始对话
======================================================================

💡 提示：
  - 你可以让 Agent 将字符串转大写
  - 你可以让 Agent 计算两个数的和
  - 你可以让 Agent 分析字符串信息
  - 也可以直接聊天，Agent 会自动判断是否需要调用工具

  输入 'exit'、'quit' 或 '退出' 来结束对话
======================================================================

👤 你: 计算 888 加 112

🤖 Agent: 2025-12-02 23:35:48,779 - base.py - 780 - INFO - ALL tokens: 10, Available tokens: 57914
2025-12-02 23:35:49,665 - base.py - 780 - INFO - ALL tokens: 57, Available tokens: 57914


🔧 [调用工具] calculate_sum
   参数: {"a": 888, "b": 112}
✅ [工具返回] 888.0 + 112.0 = 1000.0

🤖 Agent: 888 加 112 等于 1000。

👤 你: 把 zetachain 转换成大写

🤖 Agent: 2025-12-02 23:36:32,158 - base.py - 780 - INFO - ALL tokens: 39, Available tokens: 57914
2025-12-02 23:36:33,205 - base.py - 780 - INFO - ALL tokens: 66, Available tokens: 57914


🔧 [调用工具] to_uppercase
   参数: {"text": "zetachain"}
✅ [工具返回] 转换结果: ZETACHAIN

🤖 Agent: ZETACHAIN

```
````
<!-- DAILY_CHECKIN_2025-12-02_END -->

# 2025-12-01
<!-- DAILY_CHECKIN_2025-12-01_START -->




````markdown
# Day 8

1. 输入一段提示词，请 Qwen 生成一段对 ZetaChain 的介绍

- 选择模型 qwen-plus
- 调用参数

```js
  const prompt =
  "请用简洁易懂的语言介绍 ZetaChain 是什么？它的核心特点是什么？";

  try {
    const completion = await openai.chat.completions.create({
      model: "qwen-plus",
      messages: [
        { role: "system", content: "你是一个技术领域的专业助手。" },
        { role: "user", content: prompt },
      ],
    });
  }
```
````
<!-- DAILY_CHECKIN_2025-12-01_END -->

# 2025-11-30
<!-- DAILY_CHECKIN_2025-11-30_START -->





# Day 7

## Multi-Chain Vault — 通用 NFT + FT 抵押借贷池

1.  目标用户
    

持有各种链 NFT（ETH、Solana、BTC Ordinals）的玩家

想用 NFT / FT 抵押贷款的人

想把多链 NFT 统一管理、估值的人

2.  想解决的问题
    

NFT 分散在各链，难估价、难抵押

每个链都要部署不同借贷池，体验差

多链抵押风险管理困难

3.  使用方式
    

借助 ZetaChain 把各链 NFT 当作“统一 NFT 表示”（ZRC-721）

Vault 合约在 ZetaChain 上根据 Oracle 计算抵押价值

用户可以：

抵押 ETH NFT + Solana NFT + Polygon NFT

统一借出 USDC / ZETA / BTC

平台管理跨链清算，当抵押品风险过高，可调用目标链的 connected-contract 执行锁定/回收
<!-- DAILY_CHECKIN_2025-11-30_END -->

# 2025-11-29
<!-- DAILY_CHECKIN_2025-11-29_START -->






# Day 6

1.  运行文档 Example Messaging
    

-   创建项目：
    
    1.  `npx zetachain new --project messaging`
        
    2.  安装前端和合约依赖：
        
    
    ```shell
    cd messaging
    yarn
    forge soldeer update
    ```
    
    3.  编译合约 `forge build`
        
    4.  .env 输入私钥 `PRIVATE_KEY=...`
        
-   消息合同：
    
    1.  合约中导入依赖: `import "@zetachain/standard-contracts/contracts/messaging/contracts/Messaging.sol";`
        
    2.  合约继承： `contract Example is Messaging { ... }`
        
    3.  构造函数新增初始化参数：
        
    
    ```solidity
    constructor(
      address payable _gateway,
      address owner,
      address _router
    ) Messaging(_gateway, owner, _router) {}
    ```
    
    4.  合约内部实现：
        
    
    -   实现`onMessageReceive`, 当跨链消息成功到达时，目的链会自动调用此函数
        
    
    ```solidity
      function onMessageReceive(
          bytes memory data,
          bytes memory sender,
          uint256 amount,
          bytes memory asset
      ) internal override {
          //...
      }
    ```
    
    -   实现`onMessageRevert`, 当目的合同的 onMessageReceive 失败（例如因无效呼叫数据或逻辑错误）时，触发此函数
        
    
    ```solidity
    function onMessageRevert(
        bytes memory data,
        bytes memory sender,
        uint256 amount,
        bytes memory asset
    ) internal override {
        //...
    }
    ```
    
    -   实现`onRevert`, 当消息在路由过程中未能到达目的链时，会调用该函数。它在源链上执行。
        
    
    ```solidity
    function onRevert(RevertContext calldata context)
        external
        payable
        override
        onlyGateway
    {
        if (context.sender != router) revert Unauthorized();
        //...
    }
    ```
    
    -   发送消息 要发起跨链消息，合约必须调用 `depositAndCall` EVM 网关上的功能。这个功能是从传递消息 根据发送 gas 代币是原生 ETH 还是 ERC-20 货币，支持两种不同参数调用的`depositAndCall`
        
-   从哪里发起的调用？
    
    1.  源链上提供代币 （例如 ETH）。可以是原生 gas（如 ETH）或任何支持的 ERC-20。
        
    2.  通过 --target-token 指定目标代币 ，它指向 ZRC-20，代表你想在目的链上交付的代币（例如以太坊 ETH）。
        
-   在 ZetaChain 上发生了什么？
    
    3.  部分提供的金额会自动兑换到目的链（以太坊 ETH）的 ZRC-20 版本的 gas 代币中，用于覆盖消息在目的链上的执行成本。
        
    4.  剩余金额被兑换到目标代币 （可能是同一代币），并转发到目标合约。
<!-- DAILY_CHECKIN_2025-11-29_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->







# Day 5

1.  ZRC-20 和普通 ERC-20 的直观区别
    

-   白名单的 ERC-20 token 和 Zeta 原生 token 都通过 ZRC-20 标准再 ZetaChain 展示
    
-   存款过程：连接链将白名单的 ERC-20 token 存入 ERC-20 的 TSS 地址，并在 ZetaChain 铸造相同的 ZRC-20 token
    
-   ZRC-20 token 只能在 ZetaChain 铸造，在 ZetaChain 铸造的 ERC-20 token 不能跨链到别的连接链
    

2.  举一个【通用资产】可能的应用场景。
    

-   全链 DEX，全链订单簿撮合系统
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->








# Day 4

1.  自己想做的第一个 Universal App 想实现的“打印 / 记录 / 简单逻辑”是什么?
    

-   跨链支付的 ai 投资助手？
    

2.  测试网使用 Hardhat
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->









# Day 3

1.  什么是 Universal App？
    

-   Universal App 是 ZetaChain 上的智能合约，原生连接以太坊、BNB 和比特币等其他区块链。
    

2.  什么是 Gateway?
    

-   是一个接口，作为连接链上的合约与 ZetaChain 上的通用应用之间交互的统一入口。
    
-   分别部署在连接链和 Zeta 链，处理双方的合约调用逻辑。
    

3.  ZetaChain 跨链示意图
    

-   [figma](https://www.figma.com/file/mYXNTORUuvGVaQ01SF7h9Y?type=design&node-id=0%3A1&mode=design)
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->










# Day 2

1.  安装了 zetachain cli
    

-   npm install -g zetachain@latest
    
-   导入本地 eth 钱包：zetachain accounts import --type evm --private-key <私钥>
    
-   新建项目：zetachain new
    

2.  钱包连接 zetachain testnet
    

-   链 ID: 7001
    
-   链 rpc: [https://zetachain-athens-evm.blockpi.network/v1/rpc/public](https://zetachain-athens-evm.blockpi.network/v1/rpc/public)
    
-   faucet: [https://www.zetachain.com/docs/reference/faucet](https://www.zetachain.com/docs/reference/faucet)
    

3.  Qwen API 调用
    

-   获取 api-key: [https://bailian.console.aliyun.com/?tab=model#/api-key](https://bailian.console.aliyun.com/?tab=model#/api-key)
    
-   curl 调用： `shell curl -X POST https://dashscope.aliyuncs.com/compatible-mode/v1/chat/completions \ -H "Authorization: Bearer $Qwen_API_KEY" \ -H "Content-Type: application/json" \ -d '{ "model": "qwen-plus", "messages": [ { "role": "system", "content": "You are a helpful assistant." }, { "role": "user", "content": "你是谁？" } ] }' {"choices":[{"message":{"role":"assistant","content":"我是通义千问（Qwen），由阿里巴巴云研发的超大规模语言模型。我能够回答问题、创作文字，如写故事、公文、邮件、剧本等，还能进行逻辑推理、编程，甚至表达观点和玩游戏。我支持多种语言，包括但不限于中文、英文、德语、法语、西班牙语等。\n\n如果你有任何问题或需要帮助，欢迎随时告诉我！"},"finish_reason":"stop","index":0,"logprobs":null}],"object":"chat.completion","usage":{"prompt_tokens":22,"completion_tokens":84,"total_tokens":106,"prompt_tokens_details":{"cached_tokens":0}},"created":1764084567,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-f89dbfaf-fdd5-4d4a-a2c8-ab47ec1b3c63"}%`
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->











# Day 1

学习了[zeta文档](https://www.zetachain.com/docs/start)中的简介部分

1.  是什么 ZetaChain 是首个实现所有区块链生态系统原生连接的通用区块链。
    
2.  为什么
    
    1.  通过单一合约与大部分区块链实现互操作性
        
    2.  向前的兼容性，不需要为未来产生的新区块链修改部署的合约
        
    3.  与市面跨链方案不同，所有操作的代币都是原生的
        
    4.  通用UEVM环境，开发者可直接上手部署合约
        
    5.  跨链操作只需在连接链支付gas,而无需在操作链支付
        
3.  如何使用
    
    1.  Zeta实现了一个“Universal Apps”，其实是一个合约，可以接受来自任何连接链的合约调用、消息和Token转移。
        
    2.  支持比特币，并且通过gas费抽象，通过UEVM,在连接链获取gas费用，使得跨链时就可以支付足够的gas费已完成跨链。
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
