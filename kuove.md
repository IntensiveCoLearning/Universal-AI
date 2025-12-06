---
timezone: UTC+8
---

# Steph

**GitHub ID:** kuove

**Telegram:** @stephkuove

## Self-introduction

对web3感兴趣的开发者

## Notes

<!-- Content_START -->
# 2025-12-06
<!-- DAILY_CHECKIN_2025-12-06_START -->
构思黑客松demo,回顾之前学习内容
<!-- DAILY_CHECKIN_2025-12-06_END -->

# 2025-12-04
<!-- DAILY_CHECKIN_2025-12-04_START -->

参加workshop，学习simple demo
<!-- DAILY_CHECKIN_2025-12-04_END -->

# 2025-12-03
<!-- DAILY_CHECKIN_2025-12-03_START -->


尝试借用模型生成Tools

![wechat_2025-12-03_213428_281.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/kuove/images/2025-12-03-1764768877982-wechat_2025-12-03_213428_281.png)
<!-- DAILY_CHECKIN_2025-12-03_END -->

# 2025-12-02
<!-- DAILY_CHECKIN_2025-12-02_START -->



# **最小化 Qwen-Agent 示例：自定义工具**

# **概述**

这个示例展示如何在 Qwen-Agent 中创建和使用自定义工具。包括两个简单的工具：

1.  **StringToUppercase** - 将字符串转换为大写
    
2.  **AddNumbers** - 计算两个数的和
    

## **快速开始**

### **1\. 运行示例**

# 激活虚拟环境

source venv/bin/activate

# 运行示例

python examples/minimal\_agent\_with\_[tools.py](http://tools.py)

### **2\. 输出示例**

# 🔧 直接工具调用演示

📌 StringToUppercase 工具: 输入: 'hello world' 输出: {"original": "hello world", "uppercase": "HELLO WORLD"}

📌 AddNumbers 工具: 输入: 10 + 25 输出: {"num1": 10, "num2": 25, "sum": 35}

# 🤖 最小化 Qwen-Agent 示例 - 自定义工具测试

📝 测试 1: 请把 'hello world' 转换为大写 已将 'hello world' 转换为大写：HELLO WORLD ✅ 完成

📝 测试 2: 计算 42 加 58 的结果 42 加 58 的结果是 100。 ✅ 完成

## **核心概念**

### **自定义工具的三个步骤**

### **步骤 1: 继承 BaseTool 并注册**

from qwen\_[agent.tools](http://agent.tools).base import BaseTool, register\_tool

@register\_tool('string\_to\_uppercase') # 工具名称 class StringToUppercase(BaseTool): description = 'Convert a string to uppercase letters' parameters = \[ { 'name': 'text', 'type': 'string', 'description': 'The text to convert to uppercase', 'required': True, } \]

### **步骤 2: 实现 call() 方法**

def call(self, params: str, \*\*kwargs) -> str: # 解析 JSON 参数 args = json.loads(params) text = args\['text'\]

```
# 执行业务逻辑
result = text.upper()

# 返回 JSON 格式结果
return json.dumps({
    'original': text,
    'uppercase': result
}, ensure_ascii=False)
```

### **步骤 3: 在 Agent 中注册工具**

bot = Assistant( llm=llm\_cfg, function\_list=\['string\_to\_uppercase', 'add\_numbers'\], # 注册工具 )

## **工具参数规范**

每个工具必须定义 parameters 列表，指定参数的名称、类型和是否必需：

parameters = \[ { 'name': 'param\_name', # 参数名 'type': 'string|number|...', # 参数类型 'description': '...', # 参数描述 'required': True, # 是否必需 } \]

支持的类型：

-   string - 字符串
    
-   number - 数字（整数或浮点数）
    
-   boolean - 布尔值
    
-   array - 数组
    
-   object - 对象
    

## **实际应用场景**

### **场景 1: 直接调用工具（不通过 Agent）**

uppercase\_tool = StringToUppercase() params = json.dumps({'text': 'hello world'}) result = uppercase\_[tool.call](http://tool.call)(params) print(result) # {"original": "hello world", "uppercase": "HELLO WORLD"}

### **场景 2: 通过 Agent 使用工具**

bot = create\_agent() messages = \[{'role': 'user', 'content': '请把 hello world 转换为大写'}\]

for response in [bot.run](http://bot.run)(messages=messages): # Agent 自动调用工具并处理结果 print(response\['content'\], end='', flush=True)

## **文件结构**

/Users/gwy/study/AI-Learning/Qwen-Agent/ ├── examples/ │ ├── minimal\_agent\_with\_[tools.py](http://tools.py) # 本示例文件 │ ├── assistant\_add\_custom\_[tool.py](http://tool.py) # 原始参考示例 │ └── ... ├── qwen\_agent/ │ ├── tools/ │ │ ├── [base.py](http://base.py) # BaseTool 基类 │ │ └── ... │ └── ... └── ...

## **常见问题**

### **Q1: 工具如何被调用？**

Agent 会自动分析用户请求，决定是否需要调用工具。如果需要，会：

1.  生成工具调用的参数
    
2.  调用相应的工具
    
3.  将工具结果反馈给 LLM
    
4.  生成最终回复
    

### **Q2: 参数是否必须是 JSON 字符串？**

是的。所有参数都通过 JSON 字符串传递给 call() 方法，需要使用 json.loads() 解析。

### **Q3: 返回值格式有要求吗？**

返回值也必须是 JSON 字符串，使用 json.dumps() 序列化。建议使用 ensure\_ascii=False 以支持中文。

### **Q4: 能否给工具添加更多参数？**

可以。在 parameters 列表中添加更多参数定义，在 call() 方法中相应地解析即可。

## **下一步学习**

1.  查看 examples/assistant\_add\_custom\_[tool.py](http://tool.py) 了解更复杂的工具实现
    
2.  学习如何创建带有返回值验证的工具
    
3.  学习如何创建异步工具
    
4.  学习如何集成第三方 API
    

## **相关文件**

-   **主示例文件**: examples/minimal\_agent\_with\_[tools.py](http://tools.py)
    
-   **参考示例**: examples/assistant\_add\_custom\_[tool.py](http://tool.py)
    
-   **工具基类**: qwen\_agent/tools/[base.py](http://base.py)
    
-   **完成报告**: COMPLETION\_[REPORT.md](http://REPORT.md)
    
-   **运行指南**: RUN\_[GUIDE.md](http://GUIDE.md)
    

## **成功标志**

✅ 直接工具调用成功

✅ 字符串转大写工具正常运作

✅ 数字相加工具正常运作

✅ Agent 能够自动识别和调用工具

✅ 流式响应和结果处理正常
<!-- DAILY_CHECKIN_2025-12-02_END -->

# 2025-12-01
<!-- DAILY_CHECKIN_2025-12-01_START -->




调用api key，在代码中使用qwen

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/kuove/images/2025-12-01-1764593837910-image.png)
<!-- DAILY_CHECKIN_2025-12-01_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->





| 特性 | ERC-20 | ZRC-20 |
| --- | --- | --- |
| 应用平台 | 主要在 以太坊（Ethereum） 及其兼容网络（EVM 链）。 | 在 ZetaChain 上运行。 |
| 资产范围 | 只能代表和管理部署在同一条链上的资产。 | 可以管理和操作所有 ZetaChain 连接的外部链上的原生资产（如 ETH, BNB, BTC）。 |
| 跨链能力 | 不具备原生跨链能力。需要通过**桥接（Bridging）/包裹（Wrapping）**机制实现跨链。 | 具备原生全链（Omnichain）能力。它代表了外部链上被锁定在 ZetaChain 托管地址中的原生资产。 |
| 交易燃料 | 交易费用（Gas）通常使用该链的原生代币（例如，在以太坊上使用 ETH）。 | 可直接在 ZetaChain 智能合约中接收和使用外部链的原生 Gas 资产（通过 ZRC-20 形式）。 |
| 底层实现 | 基础的代币功能（transfer, approve, balanceOf）。 | 是 ERC-20 的扩展，包含了额外的逻辑和接口，使其能够与 ZetaChain 的网关（Gateway）协议交互，实现资产的存入和取出。 |

**通用 NFT 通行证 (Universal NFT Passport)**

**概念**

通用 NFT 通行证（或称为全链 NFT）允许一个 NFT 在**一个区块链**上被铸造和拥有，但其**实用性、访问权限或状态**可以被**任何其他连接的区块链**上的应用验证和使用。

**工作原理**

1.  **NFT 铸造:** ◦ 一个 NFT（如一个“VIP 会员卡”）在 **Ethereum** 链上被铸造。
    
2.  **状态追踪 (ZetaChain 监听):** ◦ ZetaChain 持续监听 Ethereum 上的 NFT 合约，跟踪该 NFT 的当前**所有者地址**。
    
3.  **通用 NFT 通行证应用:** ◦ 一个应用（例如一个游戏）部署在 **Polygon** 上，但它需要验证用户是否拥有这个 Ethereum 上的 NFT。 ◦ 传统的做法是让用户桥接 NFT（创建一个包裹版本），这会产生新的合约和碎片化风险。 ◦ 在 ZetaChain 方案中，**Polygon 上的应用**向 **ZetaChain** 发送一个查询请求：“地址 $X$ 是否拥有这个 Ethereum 上的 VIP NFT？”
    
4.  **跨链验证:** ◦ ZetaChain 利用其状态观测能力和全链智能合约，直接确认地址 $X$ 是 Ethereum 上的 NFT 所有者。 ◦ ZetaChain 将**验证结果**（`True` 或 `False`）安全地发送回 Polygon 上的应用。
    
5.  **授予权限:** ◦ Polygon 上的游戏应用根据 ZetaChain 的验证结果，授予用户游戏内的 VIP 特权。
    

**应用价值**

• **消除碎片化:** NFT 的本体始终保存在其铸造的原生链上（例如 Ethereum），无需在其他链上创建包裹或镜像版本，保证了 NFT 的单一真实来源。 • **全链实用性:** NFT 的所有权可以解锁任何连接链上的实用功能，如访问权限、折扣或投票权。 • **跨链身份:** NFT 可以充当用户的**全链身份凭证**，允许用户在任何支持 ZetaChain 的链上应用中使用其身份，而无需将资产移动到目标链。
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->






![wechat_2025-11-27_220450_264.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/kuove/images/2025-11-27-1764252340702-wechat_2025-11-27_220450_264.png)

使用测试链实现hello信息传递
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->







Universal apps在24号的笔记中总结过，

-   可以接收已连接的链上的合约调用
    
-   可以触发合约调用，进行代币在不同链上的转换
    
-   完全兼容 EVM 链（如以太坊、BNB 和 Polygon）、比特币、Solana、TON 和 Sui。
    
-   原生 Gas 和支持的 ERC-20 代币以 ZRC-20 代币的形式呈现。ZRC-20 代币无需许可即可提取回其原始链。
    

[**Gateway**](https://www.zetachain.com/docs/developers/evm/gateway)

适配不同的链的网关，与Universal apps进行连接交互

The implementation of the gateway depends on the connected chain:

-   EVM chains: a gateway smart contract
    
-   Solana: a gateway program
    
-   Bitcoin: a TSS MPC gateway address managed by a network of observer-signer validators
    

[https://embed.figma.com/design/mYXNTORUuvGVaQ01SF7h9Y/Gateway-for-Universal-Apps?node-id=0-1&p=f&embed-host=notion&footer=false&theme=system](https://embed.figma.com/design/mYXNTORUuvGVaQ01SF7h9Y/Gateway-for-Universal-Apps?node-id=0-1&p=f&embed-host=notion&footer=false&theme=system)

支持以下特性（在不同的链上）

-   将原生 Gas 代币存入 ZetaChain 上的Universal apps或账户
    
-   将支持的 ERC-20 代币（包括 ZETA 代币）存入 ZetaChain 上的Universal apps或账户
    
-   存入原生 gas 代币并向通用Universal apps发出合约调用（可传递任意数据）
    
-   存入受支持的 ERC-20 代币，并向Universal apps发出合约调用（可传递任意数据）。
    
-   向Universal apps发出合约调用（传递任意数据）
    

在ZetaChain上的特性（从Universal apps向其他链上调用合约或者提取代币）

-   将 ZRC-20 代币作为原生 gas 或 ERC-20 代币提取到已连接的链上
    
-   将 ZETA 代币提取到连接的链
    
-   将代币提取到已连接的链上并进行合约调用
    
-   在连接链上进行合约调用
    

目前只支持单链调用，同时支持回滚，调用失败时可以取消当前的操作
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->








1.  注册qwen，获取api，能在本地通过node调用
    
2.  阅读[**Getting Started**](https://www.zetachain.com/docs/developers/tutorials/intro)
    
3.  [Faucet](https://www.zetachain.com/docs/reference/faucet)： [这个用过](https://zetachain.faucetme.pro/)
    
4.  [RPC](https://www.zetachain.com/docs/reference/network/api)
    
5.  [Explorer](https://www.zetachain.com/docs/reference/explorers)
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->









-   Universal apps:
    
    -   can receive contract calls and tokens from users and contracts on connected chains
        
    -   can trigger contract calls and token transfers to connected chains
        
    -   can automatically handle gas for cross-chain transactions
        
    -   are fully compatible with EVM chains (like Ethereum, BNB, and Polygon), Bitcoin, Solana, TON, Sui. Support for Cosmos (through IBC) and other chains is coming soon.
        
-   Native gas and supported ERC-20 tokens are represented as ZRC-20 tokens. A ZRC-20 token can be permissionlessly withdrawn back to its original chain.
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
