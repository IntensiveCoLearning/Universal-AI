---
timezone: UTC+8
---

# K9z4g0d

**GitHub ID:** Kgzsgod

**Telegram:** @kgzsgod

## Self-introduction

大四计算机在读，熟悉区块链基础概论，以及DeFi等区块链扩展应用，希望可以在此次共学中学到知识

## Notes

<!-- Content_START -->
# 2025-12-02
<!-- DAILY_CHECKIN_2025-12-02_START -->
```Python
import os
import json
from qwen_agent.agents import Assistant
from qwen_agent.tools.base import BaseTool, register_tool


# ==========================================
# 1. 定义自定义 Tool (继承 BaseTool)
# ==========================================

# 工具一：计算两数之和
@register_tool('sum_two_numbers')
class SumTwoNumbers(BaseTool):
    # description 非常重要，LLM 通过它来决定是否调用此工具
    description = '计算两个数字的和。当用户询问数学加法问题时使用。'

    # parameters 定义参数格式（JSON Schema），LLM 会严格按照这个格式生成参数
    parameters = [{
        'name': 'a',
        'type': 'number',
        'description': '第一个数字',
        'required': True
    }, {
        'name': 'b',
        'type': 'number',
        'description': '第二个数字',
        'required': True
    }]

    def call(self, params: str, **kwargs):
        # params 是 LLM 传回来的 JSON 字符串
        params = json.loads(params)
        a = params['a']
        b = params['b']
        result = a + b
        return str(result)


# 工具二：字符串转大写
@register_tool('uppercase_text')
class UppercaseText(BaseTool):
    description = '将输入的文本字符串转换为大写字母。'
    parameters = [{
        'name': 'text',
        'type': 'string',
        'description': '需要转换的文本',
        'required': True
    }]

    def call(self, params: str, **kwargs):
        params = json.loads(params)
        text = params['text']
        return text.upper()


# ==========================================
# 2. 配置 Agent
# ==========================================

def run_agent():
    # 请替换为你自己的 API KEY，或者确保环境变量 DASHSCOPE_API_KEY 已存在
    # os.environ['DASHSCOPE_API_KEY'] = 'sk-xxxxxxxxxxxx'

    # 定义 LLM 的配置
    llm_cfg = {
        # 使用开源模型或者 API 模型
        'model': 'qwen-plus',
        'model_server': 'dashscope',
        'api_key': os.environ.get('DASHSCOPE_API_KEY'),
        # 'generate_cfg': {'top_p': 0.8} # 可选生成参数
    }

    # 定义系统提示词 (Persona)
    system_instruction = '''你是一个乐于助人的 AI 助手。
    你可以使用工具来解决数学问题或处理文本。
    如果用户的问题可以使用工具解决，请务必调用工具。'''

    # 初始化 Assistant (Qwen-Agent 的核心类)
    bot = Assistant(
        llm=llm_cfg,
        system_message=system_instruction,
        function_list=['sum_two_numbers', 'uppercase_text'],  # 注册我们刚才定义的工具
    )

    # ==========================================
    # 3. 测试 Agent
    # ==========================================

    print("--- 测试 1: 数学计算 ---")
    query1 = "请帮我计算 1234 加 5678 等于多少？"
    messages = [{'role': 'user', 'content': query1}]

    # bot.run 会返回一个生成器，我们需要遍历它获取最终结果
    last_response = ""
    for response in bot.run(messages=messages):
        last_response = response

    # 打印最终回复
    print(f"用户: {query1}")
    # Qwen-Agent 的 response 结构中，content 是最终文本
    print(f"AI: {last_response[-1]['content']}")
    print("\n" + "=" * 30 + "\n")

    print("--- 测试 2: 文本处理 ---")
    query2 = "把字符串 'hello qwen agent' 变成大写。"
    messages = [{'role': 'user', 'content': query2}]

    last_response = ""
    for response in bot.run(messages=messages):
        last_response = response

    print(f"用户: {query2}")
    print(f"AI: {last_response[-1]['content']}")


if __name__ == '__main__':
    run_agent()
```

![](https://ai.feishu.cn/space/api/box/stream/download/asynccode/?code=ZTNmMWZlYTdmZDc1YWZjYjRkNjg5ZjgyY2YzY2Y2MzZfdm5VWGVvYWlnYkNqdEM4b2RqYlZPN2d6YWZtZFdCYkFfVG9rZW46SXhnNGIzS3hEb3dydjl4WXhGemN2UnpQbkIwXzE3NjQ2ODQ5NjA6MTc2NDY4ODU2MF9WNA)
<!-- DAILY_CHECKIN_2025-12-02_END -->

# 2025-12-01
<!-- DAILY_CHECKIN_2025-12-01_START -->

调用的模型是qwen-plus，参数为默认值

![](https://ai.feishu.cn/space/api/box/stream/download/asynccode/?code=ZWJlMzIwNGY4MmQ3YzVhOWVkN2RkOTVmNDUzNjY4ZGRfbFdkQ1A2UjZzOXpJcUpBeDlzbThDTzhrSnBBNE1wTEFfVG9rZW46UDZGYWJlTGlab0NwbDF4VE9jeWM3RjlpblRmXzE3NjQ1OTQzOTc6MTc2NDU5Nzk5N19WNA)

```YAML
python3 chat.py
Qwen CLI 已启动，输入 exit 退出。
我: 介绍一下zetachain
Qwen: ZetaChain 是一个专注于跨链互操作性的去中心化区块链平台，旨在实现不同区块链之间的无缝通信与交互。它通过提供统一的基础设施，让开发者能够构建“全链”（omnichain）应用，即可以在多个区块链上运行并直接与各条链上的资产和数据互动的应用。

以下是 ZetaChain 的一些核心特点和功能：

### 1. **真正的全链互操作性**
ZetaChain 不依赖于封装资产或桥接合约，而是通过其共识机制和验证节点网络，直接与多条区块链（包括比特币、以太坊、BNB Chain、Solana 等）进行通信。这使得它可以监听其他链上的事件，并在这些链上执行操作。

### 2. **无需封装资产**
传统跨链桥通常需要将资产“锁定+铸造”模式来实现转移，这会生成封装资产（如 wBTC、stETH），带来额外风险。而 ZetaChain 支持原生资产的跨链交互，避免了封装带来的复杂性和安全问题。

### 3. **全链智能合约（Omni-Chain Smart Contracts）**
ZetaChain 允许开发者编写运行在其自身链上的智能合约，但这些合约可以直接与其他链交互。例如：
- 检测比特币链上的交易；
- 在以太坊上触发转账；
- 响应 Solana 上的事件。

这种能力极大简化了跨链应用的开发流程。

### 4. **支持外部区块链（包括非智能合约链）**
ZetaChain 不仅支持像以太坊这样的智能合约平台，还支持没有智能合约功能的链（如比特币）。这是通过其“观察者节点”（observers）来监控外部链的状态变化实现的。

### 5. **去中心化与安全性**
ZetaChain 使用 PoS（权益证明）共识机制，由去中心化的验证节点网络维护网络安全。其设计强调抗审查性和去信任化，减少对中心化托管方或多重签名钱包的依赖。

### 6. **ZETA 代币**
ZETA 是 ZetaChain 的原生代币，用于：
- 支付交易手续费；
- 质押参与网络验证；
- 参与治理投票。

### 7. **开发者友好**
ZetaChain 提供 SDK 和工具链，支持使用 Solidity 编写智能合约（兼容 EVM），降低了开发者进入门槛。同时提供测试网、文档和激励计划，推动生态发展。

---

### 应用场景
- **全链 DeFi 应用**：用户可以用任意链上的资产参与借贷、交易等。
- **跨链游戏（GameFi）**：NFT 和游戏代币可在多链间自由流动。
- **跨链身份与社交协议**：构建基于多链用户行为的身份系统。
- **自动化跨链执行器**：例如，“当 BTC 价格跌破某阈值时，在以太坊上平仓 ETH 抵押头寸”。

---

### 当前状态（截至 2024 年）
- 主网上线时间：2024 年初。
- 已集成主流公链：Bitcoin、Ethereum、BNB Chain、Polygon、Solana、Base 等。
- 生态项目逐步增长，涵盖 DEX、借贷、钱包、预言机等领域。
- 获得多家知名风投支持，如 Pantera Capital、OKX Ventures、Crypto.com Capital 等。

---

### 总结
ZetaChain 的愿景是成为“连接所有区块链的操作系统”，打破当前区块链孤岛的局面。相比其他跨链方案（如 Cosmos IBC、Polkadot XCM 或 LayerZero），ZetaChain 更注重通用性和对非智能合约链的支持，具有独特优势。

如果你是开发者或项目方希望构建真正意义上的“全链应用”，ZetaChain 提供了一个强大且灵活的基础设施选择。

> 官网：https://www.zetachain.com
> 文档：https://docs.zetachain.com
> GitHub：https://github.com/zeta-chain

是否需要我为你介绍如何在 ZetaChain 上部署第一个全链智能合约？
```
<!-- DAILY_CHECKIN_2025-12-01_END -->

# 2025-11-30
<!-- DAILY_CHECKIN_2025-11-30_START -->


### idea 1：原生 BTC 全链抵押借贷协议 (Omnichain Native BTC Lending)

-   **目标用户**：持有比特币（BTC）但希望获得流动性或收益，且不愿意使用中心化封装代币（如 WBTC）的用户。
    
-   **想解决的问题**：BTC 资产沉淀巨大但链上金融属性弱；现有的跨链桥存在安全隐患，用户操作繁琐。
    
-   **跨链/通用资产使用方式**：
    
    -   用户直接从比特币主网转账 BTC 到 ZetaChain 的 TSS 地址。
        
    -   ZetaChain 上的 Omnichain 智能合约自动识别并接收 **ZRC-20 BTC**。
        
    -   合约将 ZRC-20 BTC 作为抵押品，贷出稳定币（如 ZRC-20 USDC）。
        
    -   用户可选择将借出的 USDC 直接提现到 Ethereum 或 Polygon 网络（一键完成）。
        

### idea 2：全链收益聚合器 (Cross-Chain Yield Aggregator)

-   **目标用户**：在多条链上分散持有资产（ETH, BNB, MATIC），希望寻找最高收益但不想频繁跨链操作的 DeFi 玩家。
    
-   **想解决的问题**：跨链寻找高收益 Gas 费高、步骤多（审批、桥接、再质押），且资产碎片化管理困难。
    
-   **跨链/通用资产使用方式**：
    
    -   用户在任意链（如 BSC）将资产存入协议合约。
        
    -   利用 ZetaChain 的 **Cross-Chain Messaging** 功能，将资产汇聚为 ZetaChain 上的 ZRC-20 通用流动性。
        
    -   智能合约自动侦测接入链（如 Ethereum, Arbitrum）上的最高收益池（如 Aave 或 Curve）。
        
    -   合约自动将资金 Swap 并通过 Messaging 投放到目标链的高收益协议中，实现“一处存款，全链生息”。
<!-- DAILY_CHECKIN_2025-11-30_END -->

# 2025-11-29
<!-- DAILY_CHECKIN_2025-11-29_START -->



昨天跟着官网跑了一遍Swap，今天顺便给Messaging跑了（有大坑），然后周末稍微放松一下 嘻嘻

## Messaging实操

Messaging合约主要实现了在两个不同网络上进行通信

还是老三样，新建项目，安装依赖，编译合约（编译合约前先等等 。。。）

```Shell
//新建swap项目
zetachain new --project swap

//进入swap项目并安装依赖
cd swap
yarn

//更新
forge soldeer update

//编译合约
forge build
```

### 大问题

这里Messaging.sol文件中代码出了问题

先看一下Messaging.sol继承的zetachain官方的Messaging.sol合约文件中的contructor构造器代码

![](https://ai.feishu.cn/space/api/box/stream/download/asynccode/?code=MTUzNzUzOGY0MjEzZGMwODM0NjExOTE3OTRiYWM2MTVfUUNmTFRPSWkyMmhKWWV3cDdsR2w1czFXazFXZXpoZHRfVG9rZW46WFFrcGJuV1hUbzBVTVh4ZkxEUGNhMmkzblplXzE3NjQ0MjMxMTI6MTc2NDQyNjcxMl9WNA)

再看一下要部署上链的Messaging合约

![](https://ai.feishu.cn/space/api/box/stream/download/asynccode/?code=MWIyYzNlNmQxNWFjZGI1ZGMxMjExNDk1ZGYzYWVmNDRfRkY1ZHZ3R1pWaTlrdlF5N0JUWGE0RVFDZkFhYWNpV0lfVG9rZW46WU8yTWJvVFEyb2s0SEN4elRsQWNFalZCblZmXzE3NjQ0MjMxMTI6MTc2NDQyNjcxMl9WNA)

因为该合约继承了上面的合约，构造器的参数入参顺序需要和上面保持一致，如果不一致的话，在执行后面将两个不同网络连接时会出现权限错误的报错

这里需要考虑一点，在不同网络连接之前，需要分别先将messaging合约部署到base sepolia 和 eth sepolia上，为什么在部署的时候没有报错呢？

> 简单来说，区块链并不认识“我”是谁，也不认识谁是gateway，他只管执行指令，只要指令符合语法规则，交易就会显示“成功”，owner和gatway都是一串地址，所以他并不会部署合约上链时进行区分

### 部署messaging合约

因为想要实现从base sepolia到eth sepolia的消息传输，所以需要分别将合约部署到base sepolia和eth sepolia上

部署到base sepolia：

```Shell
MESSAGING_BASE=$(./commands/index.ts deploy --rpc https://sepolia.base.org --private-key $PRIVATE_KEY | jq -r .contractAddress)

//查看MESSAGING_BASE合约的地址
echo $MESSAGING_BASE
0xf0dd8EA83cA5033B0fb3ee5312eB9696f5853F62
```

部署到eth sepolia:

```Shell
MESSAGING_ETHEREUM=$(./commands/index.ts deploy --rpc https://sepolia.drpc.org --private-key $PRIVATE_KEY | jq -r .contractAddress)

//查看MESSAGING_ETHEREUM合约的地址
echo $MESSAGING_ETHEREUM
0x2323c57F0B8A8717ABC1B510745f4021f0F4C144
```

### 连接两个合约

在两个合约能够跨链通信之前，需要相互信任，保证安全性。

因此需要使用合约中的setConnected()函数来实习

```SQL
//base连eth
./commands/index.ts connect \
  --contract $MESSAGING_BASE \
  --target-contract $MESSAGING_ETHEREUM \
  --rpc https://sepolia.base.org \
  --target-chain-id 11155111 \
  --private-key $PRIVATE_KEY
{"contractAddress":"0xf0dd8EA83cA5033B0fb3ee5312eB9696f5853F62","chainId":"11155111","connected":"0x2323c57F0B8A8717ABC1B510745f4021f0F4C144","transactionHash":"0x3bb3312b85b16c3285f852916b28cc26e096851772a1e9a098cfab987fc1243e"}

//eth连base
./commands/index.ts connect \
  --contract $MESSAGING_ETHEREUM \
  --target-contract $MESSAGING_BASE \
  --rpc https://sepolia.drpc.org \
  --target-chain-id 84532 \
  --private-key $PRIVATE_KEY
{"contractAddress":"0x2323c57F0B8A8717ABC1B510745f4021f0F4C144","chainId":"84532","connected":"0xf0dd8EA83cA5033B0fb3ee5312eB9696f5853F62","transactionHash":"0xaf7a92e25bd5bc2abbc9d33ffca394f5e650363f66ebe483311a2baf253b89ee"}
```

### 发送跨链消息

使用以下终端命令，将hello字符串从base的合约传到eth的合约上

```SQL
./commands/index.ts message 
 --rpc https://sepolia.base.org 
 --private-key $PRIVATE_KEY 
 --contract $MESSAGING_BASE 
 --target-contract $MESSAGING_ETHEREUM 
 --types string 
 --values hello 
 --target-token 0x05BA149A7bd6dC1F937fA9046A9e05C05f3b18b0 
 --amount 0.005
 
 //交易详细信息
 {"contractAddress":"0xf0dd8EA83cA5033B0fb3ee5312eB9696f5853F62","targetContract":"0x2323c57F0B8A8717ABC1B510745f4021f0F4C144","targetToken":"0x05BA149A7bd6dC1F937fA9046A9e05C05f3b18b0","message":"0x0000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000568656c6c6f000000000000000000000000000000000000000000000000000000","transactionHash":"0x84aaec6261d009f840a8ca2388d52018121ee6b6e288ec596617f44de7839820","amount":"0.005"}
```

### 查看跨链交易

```YAML
npx zetachain query cctx --hash 0x84aaec6261d009f840a8ca2388d52018121ee6b6e288ec596617f44de7839820
```

![](https://ai.feishu.cn/space/api/box/stream/download/asynccode/?code=YzExZjIzYTE5YzU2MDI3NWU5YjZhM2MzOGJjMzUzMzRfNkRRcVVQQkhyRzJoMnRQbE0yUVBnSjg1a0kwN25ucnlfVG9rZW46QXZzTGJYYlNOb1dwMm54ZEpCdmNkQlcxbmVmXzE3NjQ0MjMxMTI6MTc2NDQyNjcxMl9WNA)

可以看到68656c6c6f就是hello的十六进制，已经被传输出去
<!-- DAILY_CHECKIN_2025-11-29_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->




### Swap实操

首先先通过下面几条命令新建swap项目以及更新和安装对应依赖

```Shell
//新建swap项目
zetachain new --project swap

//进入swap项目并安装依赖
cd swap
yarn

//更新
forge soldeer update

//编译合约
forge build
```

接下来部署swap合约到测试网上

```Shell
//这里得到了universal的地址并传入universal变量中
UNIVERSAL=$(npx tsx commands deploy --private-key $PRIVATE_KEY | jq -r .contractAddress) && echo $UNIVERSAL
0x3b73b9285D77f22179C70B5834d00A63468EC646
```

分别得到evm发送方地址和speolia中eth的zrc-20地址

```Shell
RECIPIENT=$(cast wallet address $PRIVATE_KEY) && echo $RECIPIENT
0xAa79ED23Adf4A21fE58736d60fEEcec17BD9E33B

ZRC20_ETHEREUM_ETH=$(zetachain q tokens show --symbol ETH.ETHSEP -f zrc20) && echo $ZRC20_ETHEREUM_ETH
0x05BA149A7bd6dC1F937fA9046A9e05C05f3b18b0
```

发起一个从sepolia到base的swap交易

```SQL
npx zetachain evm deposit-and-call \
  --chain-id 11155111 \
  --amount 0.001 \
  --types address bytes bool \
  --receiver $UNIVERSAL \
  --values $ZRC20_BASE_ETH $RECIPIENT true \
  --private-key $PRIVATE_KEY

From:   0xAa79ED23Adf4A21fE58736d60fEEcec17BD9E33B
To:     0x3b73b9285D77f22179C70B5834d00A63468EC646 on ZetaChain
Amount: 0.001 native tokens
Refund: 0xAa79ED23Adf4A21fE58736d60fEEcec17BD9E33B
Call on revert: false

Contract call details:
Function parameters: 0x236b0DE675cC8F46AE186897fCCeFe3370C9eDeD, 0xAa79ED23Adf4A21fE58736d60fEEcec17BD9E33B, true
Parameter types: ["address","bytes","bool"]

? Proceed with the transaction? yes
Transaction hash: 0xfdd43db63ed3e2f24b0484d6077db5561d850c756c197d0ccd73576a58a542cb
```

得到交易哈希后可以从sepolia测试网上进行查看[https://sepolia.etherscan.io/](https://sepolia.etherscan.io/)

虽然成功了，但是只是进行transfer，因为要swap的代币太少，导致在测试网上的流动性不足，所以真正swap的并没有执行

![](https://ai.feishu.cn/space/api/box/stream/download/asynccode/?code=N2FlNTQxY2I1NTk4YjZmYzIzZDBjNDZiZjU3ZjYyMWNfRmJEcFF3VU03a0pFZEoyWkNBWU1SYWNpaHp1NjFwbXZfVG9rZW46VkI2R2JSQlBEb21NYUp4UlllNWM5ck1WbkhkXzE3NjQzMzY4NDQ6MTc2NDM0MDQ0NF9WNA)

也可使用命令查看跨链的上下文

```SQL
zetachain query cctx --hash 0xfdd43db63ed3e2f24b0484d6077db5561d850c756c197d0ccd73576a58a542cb
11155111 → 7001 ❌ Reverted
CCTX:     0x1e4a7ff5e985c2551f65b4733af616a1f48b80609724fbad2121b0453f882c96
Tx Hash:  0xfdd43db63ed3e2f24b0484d6077db5561d850c756c197d0ccd73576a58a542cb (on chain 11155111)
Tx Hash:  0xa2907f86bafdacf38c211e5e1ba4fc65df88eff3a0acd1518a662e517f0e9c6e (on chain 7001)
Sender:   0xAa79ED23Adf4A21fE58736d60fEEcec17BD9E33B
Receiver: 0x3b73b9285D77f22179C70B5834d00A63468EC646
Message:  000000000000000000000000236b0de675cc8f46ae186897fccefe3370c9eded000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000000000014aa79ed23adf4a21fe58736d60feecec17bd9e33b000000000000000000000000
Amount:   1000000000000000 Gas tokens
Error:    {"type":"contract_call_error","message":"contract call failed when calling EVM with data","error":"execution reverted: ret 0x: evm transaction execution failed","method":"depositAndCall0","contract":"0x6c533f7fE93fAE114d0954697069Df33C9B74fD7","args":"[{[170 121 237 35 173 244 162 31 229 135 54 214 15 238 206 193 123 217 227 59] 0xAa79ED23Adf4A21fE58736d60fEEcec17BD9E33B 11155111} 0x05BA149A7bd6dC1F937fA9046A9e05C05f3b18b0 1000000000000000 0x3b73b9285D77f22179C70B5834d00A63468EC646 [0 0 0 0 0 0 0 0 0 0 0 0 35 107 13 230 117 204 143 70 174 24 104 151 252 206 254 51 112 201 237 237 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 96 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 20 170 121 237 35 173 244 162 31 229 135 54 214 15 238 206 193 123 217 227 59 0 0 0 0 0 0 0 0 0 0 0 0]]"}

7001 → 11155111 ✅ Revert executed
Reason for revert: 
Reason for abort:  
Revert Address:    0xAa79ED23Adf4A21fE58736d60fEEcec17BD9E33B
Call on Revert:    false
Abort Address:     0x0000000000000000000000000000000000000000
Revert Message:    -
Revert Gas Limit:  200000
Tx Hash:          0xb14a43346253c871fb77c656042935c0d55b1b705efb5a51cd2d225f46e20e6c (on chain 11155111)
```

### ZRC-20与ERC-20的区别

首先最本质的区别是：

-   erc-20 是一个单链代币的标准，只存在一条链上
    
-   zrc-20 是一个多链代币的标准，可以在其他链上并跨链使用
    

更核心的区别：

1.  erc-20的transfer只是在同一条链上，zrc-20可以转账可以触发跨链转账
    
2.  在zetachain上可以使用任意zrc-20支持的代币支付gas fee，erc-20只能使用原生代币进行支付gas fee
    
3.  erc-20合约可以自己部署，zrc-20由zetachain官方统一部署
    

### 跨链存储

在多链生态的背景下，可能会同时拥有许多资产，比如btc，eth，bnb，sol等

如果想要在以太坊进行存储获利，需要先将资产转移到ETH中，还想要在solana进行流动性挖矿，又需要将eth中的资产转移到solana中

缺点：多链操作复杂，每次换链都需要跨链费，等待时间长

使用通用资产即可解决这一问题：

可以将任意链的资产映射成zetachain上的统一代币

-   BTC - ZRC-BTC
    
-   ETH - ZRC-ETH
    
-   USDT - ZRC-USDT
    

用户直接存入上面在zetachian链上的资产，只需要看zetachain上的界面，更加简单便捷，省去了很多跨链步骤和跨链费
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->





## 自己想做的第一个 Universal App 想实现的“打印 / 记录 / 简单逻辑”是什么。

> 想做一个全链留言板，用户可以从任何链提交留言，ZetaChain 统一记录。

## 为后续的 Hello World Demo 决定一种工作流：

> 使用Foundry + ZetaChain测试网。
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->






### Qwen

刚看到notion中还有实践作业。。。

使用终端的curl命令完成了对qwen api 请求

![](https://xvadt6dwbiv.feishu.cn/space/api/box/stream/download/asynccode/?code=YzlhZmFiZjI3YzVhYzJlZGE3MmIxZmViODk4Y2Q2MjNfc3EzZnp0Q1o5bElOd3A5QkpsalZVUGVrTk1zVkdyZFZfVG9rZW46S21BbGJ2cEF2b3VxWnR4eGhVcWNhNTFkbk9nXzE3NjQxNDM0MTg6MTc2NDE0NzAxOF9WNA)

通过以下python代码可以在终端与qwen进行交互

```Python
import os
from openai import OpenAI

client = OpenAI(
    api_key=os.getenv("DASHSCOPE_API_KEY"),
    base_url="https://dashscope.aliyuncs.com/compatible-mode/v1",
)

messages = [
    {"role": "system", "content": "You are a helpful assistant."}
]

print("Qwen CLI 已启动，输入 exit 退出。")

while True:
    user_input = input("我: ")
    if user_input.lower() in ["exit", "quit"]:
        break

    messages.append({"role": "user", "content": user_input})

    response = client.chat.completions.create(
        model="qwen-plus",
        messages=messages
    )

    reply = response.choices[0].message.content
    print("Qwen:", reply)

    messages.append({"role": "assistant", "content": reply})
```

![](https://xvadt6dwbiv.feishu.cn/space/api/box/stream/download/asynccode/?code=ZTgyNjBhYzRjOGYwNDcyZjY0Y2MwNTM0MjQ3NGNiNjlfTTFnYzBlYkNhbU1BbjV1bzU0dFRkOUVTZUtxMHpoQ1lfVG9rZW46WXlmNGJoQ3pqb1NZa0d4MTNjSmNLZ2JMbmdmXzE3NjQxNDM0MTg6MTc2NDE0NzAxOF9WNA)

之前的实践作业在前两天的笔记都有提到，就不一一返回重新截取了

### Gateway

Gateway就是ZetaChain的智能合约与其他链进行交互的接口，可以通过这个接口进行消息传输，发送交易等操作

针对不同的连接链有不同的操作方式

Gateway主要实现了的功能需要区分连接链上的功能和zetachain的功能

连接链上的Gateway的功能：

-   存入原生代币作为gas向zetachain的合约发送调用请求
    
-   受zetachain支持的erc-20代币也可向zetachain的合约发送请求
    

zetachain的Gateway的功能：

-   将zeta或者erc-20代币提取到连接链上
    
-   将代币提取到连接链上发送合约调用
    

Gateway的架构图

![](https://xvadt6dwbiv.feishu.cn/space/api/box/stream/download/asynccode/?code=YWVlZDUyMzE4MjVlM2FkZDU0MmJiZTJhN2YwOTk3ZmRfSXFCcUdWZHdyc0VXaGxFQk9DOXdEaTlpa2lnQ0YyVm1fVG9rZW46SzVQdmJjbGdpb1B6WGR4V01Od2M1em8zblpkXzE3NjQxNDM0MTg6MTc2NDE0NzAxOF9WNA)
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->







## 部署在本地的universal合约

```Solidity
npx zetachain localnet start
```

使用如上终端命令，打开zetachain的anvil本地测试环境

在命令行中输入`forge build`编译foundry项目中的Universal合约

在输入如下命令将Universal合约部署上链

```Shell
forge create Universal \
  --rpc-url http://127.0.0.1:8545 \
  --private-key $PRIVATE_KEY \
  --broadcast
[⠊] Compiling...
No files changed, compilation skipped
Deployer: 0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266
Deployed to: 0x5bf5b11053e734690269C6B9D438F8C9d48F528A
Transaction hash: 0x57d6b6f7efdec82004cc021f837e2d7bee84a92e477d5c6f3e6a0817033f68c2
```

可以看到已经完成部署上链，发送方和接收方的地址以及交易的哈希

Universal合约就是实现了当其他链通过zetachain跨链调用这个合约时，他会解码传入的内容，并触发打印名字的事件（并没有实际资产转移，只单纯输出消息）

## 部署在测试网的Universal合约

首先先将zetachain testnet添加到metamask中

![](https://xvadt6dwbiv.feishu.cn/space/api/box/stream/download/asynccode/?code=M2Q2ODI3NWIxN2FjYjc3MGQ5OWQ0OGFhYTJlYjRlMzhfeDdQdThONkRuSXd3a2RUNVhKZkRjYkE2R0FhTXZTV3BfVG9rZW46TjJmSWJ0WVJub2ZReWl4Zkh0bmN0OEJCbkZKXzE3NjQwNzU5OTU6MTc2NDA3OTU5NV9WNA)

[https://zetachain.faucetme.pro/](https://zetachain.faucetme.pro/)

通过官方水龙头获取zeta代币，然后将私钥改成测试网地址的私钥，在输入如下命令即可将Universal合约部署在测试网上

```Shell
forge create Universal \
  --rpc-url https://zetachain-athens-evm.blockpi.network/v1/rpc/public \
  --private-key $PRIVATE_KEY \
  --broadcast \
  --json

{
  "deployer": "0xAa79ED23Adf4A21fE58736d60fEEcec17BD9E33B",
  "deployedTo": "0x525087bA0FF1f84c8E93846f1D0fC7D5709D37Af",
  "transactionHash": "0x64680383161a811a369e60adaaf34f0b7bcdbba1f725642ea3cee212e66c83d0"
}
```

得到的交易哈希可以从区块链浏览器中查到[https://testnet.zetascan.com/](https://testnet.zetascan.com/)

![](https://xvadt6dwbiv.feishu.cn/space/api/box/stream/download/asynccode/?code=ZTY3YTZlYjg5N2NjYWRjNTFhNTZiZjA5ZThjYzljNzlfdk56TEVadk9JdFQ4NHNvVTQ4STBlS1h6UjlrRW44SnhfVG9rZW46WTFWdWJRTmttbzVrR0N4b0xXbGN1dVhUbmplXzE3NjQwNzU5OTU6MTc2NDA3OTU5NV9WNA)
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->








搭建了zetachain的cli环境

阅读了zetachain的官方文档并搜索了相关资料，理清了他的逻辑

zetachian并不是传统意义上的桥接跨链，而是通过在zetachain上部署智能合约，实现从一个链到另一个链的操作

比如现在我想要从比特币链上转账btc到以太坊链上，我只需要在zetachain上部署执行转账合约，此时中间的过程：

1.  比特币链触发转账操作
    
2.  zetachain链监听到该交易
    
3.  zetachain链节点达成共识
    
4.  执行跨链转账合约逻辑
    
5.  生成转账交易
    

对比传统跨链来说确实有优势，速度快延迟短，也不需要考虑各种跨链fee
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
