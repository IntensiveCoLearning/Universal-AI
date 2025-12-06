---
timezone: UTC+8
---

# jianlin chen

**GitHub ID:** Airjiannan05

**Telegram:** @none

## Self-introduction

在校计算机大学生一枚，专攻ai前沿技术，有相关的链上开发基础，对区块链加ai的发展有浓厚的兴趣。Blockchain is the future.

## Notes

<!-- Content_START -->
# 2025-12-06
<!-- DAILY_CHECKIN_2025-12-06_START -->
打卡学习先
<!-- DAILY_CHECKIN_2025-12-06_END -->

# 2025-12-05
<!-- DAILY_CHECKIN_2025-12-05_START -->

事已至此，先打个卡吧
<!-- DAILY_CHECKIN_2025-12-05_END -->

# 2025-12-04
<!-- DAILY_CHECKIN_2025-12-04_START -->


# Day 10：DeFi 意图解析（从自然语言到结构化参数）

**学习目标**

-   让 Agent 能从自然语言中提取 DeFi 操作参数（链名、代币、金额等）。
    
-   完成一个最小的“解析层”。
    

**学习资料**

-   Qwen-Agent 文档中的 Tools / Function Calling 部分
    
-   [https://qwen.readthedocs.io/en/v2.5/framework/qwen\_agent.html](https://qwen.readthedocs.io/en/v2.5/framework/qwen_agent.html)
    
-   Qwen API 参考（了解参数格式）
    
-   [https://www.alibabacloud.com/help/zh/model-studio/qwen-api-reference](https://www.alibabacloud.com/help/zh/model-studio/qwen-api-reference)
    

**扩展资料（可选）**

-   参考你未来想做的通用 DeFi 项目 idea，提前想想还需要哪些字段。
    

**实践 / 作业**

-   设计一个工具：`parse_swap_intent(text)`，返回结构化 JSON，例如：
    
-   `{ "chain": "base", "tokenIn": "USDC", "tokenOut": "ETH", "amount": "10" }`
    
-   让 Agent 能处理以下输入：
    
    -   “帮我在 Base 上用 10 USDC 换成 ETH”
        
    -   “把我 50 U 兑换成 Polygon 上的 MATIC”
        

![](https://ai.feishu.cn/space/api/box/stream/download/asynccode/?code=ZGUzMGI2NjM4ZGQ5OTQwOWNlMTE1OWM3NGQ0ZDY2NzhfNjBjZEpwbExGZTNKeGRWN09lZjVhbm1yOWdmMVZ5RllfVG9rZW46WU5GdmJKSWRqb0lpUUd4enN2YmM5QmhBblhmXzE3NjQ4NDI0MzI6MTc2NDg0NjAzMl9WNA)

解析层：

自然语言-->构建prompt(预构的system prompt+用户指令）-->调用Qwen API发送给Qwen解析-->从return的响应得到结构化数据-->规范检查-->JSON结果，保存在output

![](https://ai.feishu.cn/space/api/box/stream/download/asynccode/?code=N2FhYTM0NWU2ZTY0NWE3NzAwZmUyYzZlZGMyZmVlZTdfRFBjZzBFclFyM05jckpxeEZTQjlCOTVpUHpuVkJTUWVfVG9rZW46TG5IWmJyTzZrb2xmcEt4SXdobWNrRjRibnpkXzE3NjQ4NDI0MzI6MTc2NDg0NjAzMl9WNA)

结果：

![](https://ai.feishu.cn/space/api/box/stream/download/asynccode/?code=NmJlMDIzMTg1Zjc1NjAzYmU0MzQ5ZTRlYzY4MjVlMmJfZDkxMG9tQktwbUx1UU5oMTZwQm1YVktETTVVNEN6MGdfVG9rZW46Tk1LemJ6V1V4b1VIaFl4Z1pFVWNIUnE0bmVkXzE3NjQ4NDI0MzI6MTc2NDg0NjAzMl9WNA)

# Day 11：Qwen-Agent × ZetaChain（接口层设计）

**学习目标**

-   把解析出的 DeFi 参数，映射到实际的合约调用接口（哪怕先是伪代码）。
    
-   形成一层清晰的“中间层服务”。
    

**学习资料**

-   ZetaChain Developers（Swap / Messaging / 合约调用部分）
    
-   [https://www.zetachain.com/docs/developers](https://www.zetachain.com/docs/developers)
    
-   ZRC-20 标准（再次回顾）
    
-   [https://www.zetachain.com/docs/developers/evm/zrc20/](https://www.zetachain.com/docs/developers/evm/zrc20/)
    

**扩展资料（可选）**

-   Example code repo（查看 Swap / Messaging 相关代码结构）
    
-   [https://github.com/zeta-chain/example-contracts](https://github.com/zeta-chain/example-contracts)
    

**实践 / 作业**

-   写出一个后端伪代码或简单实现：
    
    -   接收 `parse_swap_intent` 返回值；
        
    -   根据不同链 / 不同 token 选择具体的合约 / 调用方式；
        
    -   暂时可以只在控制台打印“准备发起什么交易”。
        

## 回顾Swap/Messaging/合约调用部分

### 调用流程：

![](https://ai.feishu.cn/space/api/box/stream/download/asynccode/?code=ZGM5ZjNhYWQ5YzA3YTFiNThhZWMwZjViOWI2MjUyY2ZfM1RjQW1JVndvVzNzcW52d01zTUdCM0JhMVkyTHVLTFlfVG9rZW46R0gzQmJ3TVp0b3p5RXF4VnRXbGNsU1BQbnNjXzE3NjQ4NDI0MzI6MTc2NDg0NjAzMl9WNA)

### Swap

-   Swap合约主要做了三件事情：解析 payload、为目的链准备 gas、把剩余资产兑换为目标资产并 withdraw。
    
-   同时，他会在 ZetaChain 部署一个 Universal 合约作为“跨链兑换器”（如图所示）。任意连接链用户发送原生或 ERC-20 资产，ZetaChain 端接收为对应的 ZRC-20，完成兑换后再提取到目标链。
    

具体的调用流程：

![](https://ai.feishu.cn/space/api/box/stream/download/asynccode/?code=Zjk5MGZkYzQzMzgxOGYzZjdjZmI4YmJlYTlhMGZiNmZfRWVZWTAxa3JOY3d4bEdyN093SHVkYXZ5aGJCY3JwWkFfVG9rZW46VWRjT2JDOWZhb0xTazZ4SkxmZGNDVjBWbjRiXzE3NjQ4NDI0MzI6MTc2NDg0NjAzMl9WNA)

### Messaging

-   使用标准合约 Messaging.sol 实现跨链可信消息传递，合约需与对端互信绑定（setConnected / setUniversal）。
    
-   提供 onMessageReceive/onMessageRevert/onRevert **三个钩子**来处理成功、目的链失败、路由失败。
    

具体的调用流程：

![](https://ai.feishu.cn/space/api/box/stream/download/asynccode/?code=ZDc4ZWM5OTNkNTczOWZhYmY0MWM0YzRkNDMzZDc1MmZfZ2t1OEFqUWRZTlFaZVNZaDVsQWk4ekhJSG5QRGFGVTdfVG9rZW46UmIwN2JrZ2w0b1B5SGx4VjFHRGM1d0ZjbjBmXzE3NjQ4NDI0MzI6MTc2NDg0NjAzMl9WNA)

## 接口层设计

```JSON
// 完整流程
用户输入
    ↓
parse_swap_intent(text)
    ↓ 返回
Intent { chain, tokenIn, tokenOut, amount }
    ↓ 传入
SwapExecutor.executeSwap(intent)
    ↓ 内部调用链
    ├─ validateIntent(intent)           // 验证参数
    ├─ getChainConfig(chain)           // 获取链配置
    ├─ getTokenAddresses(intent)       // 查询代币地址
    ├─ selectDexRouter(chain)          // 选择 DEX
    ├─ buildTransactionParams(...)     // 构建交易参数
    ├─ simulateTransaction(...)        // 模拟执行
    └─ recordTransaction(...)          // 记录历史
    ↓ 返回
ExecutionResult { success, txParams }
```

![](https://ai.feishu.cn/space/api/box/stream/download/asynccode/?code=NWJiZWE2YjY2ZDc1NDAwY2FkNWIyZWNhNzUzN2YxYjhfMHVKTlo0bXE0NWZldjJFOGo1bXp2bnFxbWM2QnNJc2lfVG9rZW46WWk3MWJ0Y25Yb24xNFF4VjlMRmNzNzJCblNoXzE3NjQ4NDI0MzI6MTc2NDg0NjAzMl9WNA)
<!-- DAILY_CHECKIN_2025-12-04_END -->

# 2025-12-03
<!-- DAILY_CHECKIN_2025-12-03_START -->



有点忙，先打打卡
<!-- DAILY_CHECKIN_2025-12-03_END -->

# 2025-12-02
<!-- DAILY_CHECKIN_2025-12-02_START -->




### Day 8：Qwen AI 基础 & API 调用（实战）

**学习目标**

-   使用自己熟悉的语言完成一次 Qwen API 调用。
    
-   熟悉 Qwen 的基础参数、模型选择方式。
    

**学习资料**

-   Qwen 总文档（入门、示例代码）
    
-   [https://qwen.readthedocs.io/zh-cn/latest/](https://qwen.readthedocs.io/zh-cn/latest/)
    
-   Qwen API 参考（含 OpenAI 兼容示例）
    
-   [https://www.alibabacloud.com/help/zh/model-studio/qwen-api-reference](https://www.alibabacloud.com/help/zh/model-studio/qwen-api-reference)
    

**扩展资料（可选）**

-   Qwen API 平台
    
-   [https://qwen.ai/](https://qwen.ai/)
    

**实践 / 作业**

-   写一个最小脚本（Node.js / Python 均可）：
    
    -   输入一段提示词，请 Qwen 生成一段对 ZetaChain 的介绍。
        
    -   在终端打印返回内容。
        
-   在笔记中记录：你选择了哪个模型？调用参数是怎样的？
    

# 一个连接 ZetaChain 全链网络，结合通义千问 AI 的智能钱包分析工具

**ZetaChain AI Agent** 是一个基于 Node.js 开发的命令行工具，它能够：

1\. 📊 **实时查询跨链资产**：通过 ZetaChain CLI 自动获取你在多链（EVM、Solana、Bitcoin、Sui 等）上的真实钱包余额

2\. 🤖 **AI 智能分析**：调用阿里云通义千问大模型，对你的资产配置进行专业且幽默的点评

3\. 💬 **个性化建议**：根据资产情况，AI 会用不同的语气提供财务建议（资产少会被吐槽哦！）

**核心功能**

\- ✅ 自动连接 ZetaChain 网络获取多链余额

\- ✅ 支持 EVM（Ethereum/BSC/等）、Solana、Bitcoin、Sui 等多种区块链

\- ✅ 集成通义千问 AI 模型进行资产分析

\- ✅ 彩色命令行输出，可读性强

\- ✅ 自动解析余额数据并生成报告

提示词代码：

![](https://ai.feishu.cn/space/api/box/stream/download/asynccode/?code=ZGY5Yzk0MDE0NzgwN2VhN2VkMzU1OWVlYWM0YzI0ZjdfVGY3d1NHbExvcVJJWXFZdHg1akJ5OGFPNjdqdWsxcTBfVG9rZW46UkNNZGJ1UGc3b1lEMjd4eGNSU2NlV1NnbnljXzE3NjQ2Njc4Njg6MTc2NDY3MTQ2OF9WNA)

```Bash
model: "qwen-plus",
messages: [
             { role: "system", content: "你是一个幽默、喜欢吐槽的AI助手，同时也是一个专业的加密货币财务顾问。" },
             { role: "user", content: prompt }
           ]
```

### Day 9：Qwen-Agent 入门 & 简单 Tool

**学习目标**

-   理解 Qwen-Agent 框架的基本组成（LLM / Agent / Tools / Memory）。
    
-   搭建一个最小的 Agent，并挂一个简单 Tool。
    

**学习资料**

-   Qwen-Agent 文档
    
-   [https://qwen.readthedocs.io/en/v2.5/framework/qwen\_agent.html](https://qwen.readthedocs.io/en/v2.5/framework/qwen_agent.html)
    
-   Qwen-Agent GitHub（示例代码）
    
-   [https://github.com/QwenLM/Qwen-Agent](https://github.com/QwenLM/Qwen-Agent)
    

**扩展资料（可选）**

-   粗略了解下官方示例中的工具调用、对话流程。
    

**实践 / 作业**

-   跑通一个 Qwen-Agent 官方示例。
    
-   自定义一个简单 Tool，例如：
    
    -   把字符串转大写；
        
    -   计算两个数的和。
        
-   确认 Agent 能自动调用这个 Tool 并返回结果。
    

# 1.环境准备

## git一下

```Bash
git clone https://github.com/QwenLM/Qwen-Agent.git
```

```Bash
cd Qwen-Agent-main
```

### 全功能安装（支持 RAG、代码解释器、GUI 等功能）

```Plain
pip install -U "qwen-agent[rag,code_interpreter,python_executor,gui]"
```

## 模型服务配置

### 方法 1：使用 [DashScope](https://zhida.zhihu.com/search?content_id=253875265&content_type=Article&match_order=1&q=DashScope&zhida_source=entity) 官方服务（需申请 API KEY）

```Plain
export DASHSCOPE_API_KEY='your-api-key'
```

### 方法 2：本地部署服务端（以 [vLLM](https://zhida.zhihu.com/search?content_id=253875265&content_type=Article&match_order=1&q=vLLM&zhida_source=entity) 为例）

```Plain
from vllm import LLM, SamplingParams

llm = LLM(model="Qwen2-7B-Chat")
```

# 2.核心功能开发

## Tool的定义开发

例如，实现一个简单的计算器tool:

```Python
from qwen_agent.tools.base import BaseTool, register_tool
import json5

@register_tool('calculate')
class Calculator(BaseTool):
    description = '基础运算计算器'
    parameters = [{'name': 'formula', 'type': 'string'}]

    def call(self, params: str) -> float:
        return eval(json5.loads(params)['formula'])

# 工具调用示例
calc = Calculator()
print(calc.call('{"formula": "(3 + 5) * 2"}'))  # 输出 16.0
```

![](https://ai.feishu.cn/space/api/box/stream/download/asynccode/?code=MzUzNzM4OGQxZGM1N2JiYzk3ZTczYzFjMDZhMThlOGNfNnU3YzJxOXJ2TzE4bW9obXBTSHNENVVKVGJDdWIwT21fVG9rZW46WG9OdmJablNvb3hTTDR4cGJlVGM1b0t1bkFlXzE3NjQ2Njc4Njg6MTc2NDY3MTQ2OF9WNA)

## 官方示例

```Python
import pprint
import urllib.parse
import json5
from qwen_agent.agents import Assistant
from qwen_agent.tools.base import BaseTool, register_tool
from qwen_agent.utils.output_beautify import typewriter_print

# 步骤 1（可选）：添加一个名为 `my_image_gen` 的自定义工具。
@register_tool('my_image_gen')
class MyImageGen(BaseTool):
    # `description` 用于告诉智能体该工具的功能。
    description = 'AI 绘画（图像生成）服务，输入文本描述，返回基于文本信息绘制的图像 URL。'
    # `parameters` 告诉智能体该工具有哪些输入参数。
    parameters = [{
        'name': 'prompt',
        'type': 'string',
        'description': '期望的图像内容的详细描述',
        'required': True
    }]

    def call(self, params: str, **kwargs) -> str:
        # `params` 是由 LLM 智能体生成的参数。
        prompt = json5.loads(params)['prompt']
        prompt = urllib.parse.quote(prompt)
        return json5.dumps(
            {'image_url': f'https://image.pollinations.ai/prompt/{prompt}'},
            ensure_ascii=False)

# 步骤 2：配置您所使用的 LLM。
llm_cfg = {
    # 使用 DashScope 提供的模型服务：
    'model': 'qwen-max-latest',
    'model_type': 'qwen_dashscope',
    # 'api_key': 'YOUR_DASHSCOPE_API_KEY',
    # 如果这里没有设置 'api_key'，它将读取 `DASHSCOPE_API_KEY` 环境变量。

    # 使用与 OpenAI API 兼容的模型服务，例如 vLLM 或 Ollama：
    # 'model': 'Qwen2.5-7B-Instruct',
    # 'model_server': 'http://localhost:8000/v1',  # base_url，也称为 api_base
    # 'api_key': 'EMPTY',

    # （可选） LLM 的超参数：
    'generate_cfg': {
        'top_p': 0.8
    }
}

# 步骤 3：创建一个智能体。这里我们以 `Assistant` 智能体为例，它能够使用工具并读取文件。
system_instruction = '''在收到用户的请求后，你应该：
- 首先绘制一幅图像，得到图像的url，
- 然后运行代码`request.get`以下载该图像的url，
- 最后从给定的文档中选择一个图像操作进行图像处理。
用 `plt.show()` 展示图像。
你总是用中文回复用户。'''
tools = ['my_image_gen', 'code_interpreter']  # `code_interpreter` 是框架自带的工具，用于执行代码。
files = ['./examples/resource/doc.pdf']  # 给智能体一个 PDF 文件阅读。
bot = Assistant(llm=llm_cfg,
                system_message=system_instruction,
                function_list=tools,
                files=files)

# 步骤 4：作为聊天机器人运行智能体。
messages = []  # 这里储存聊天历史。
while True:
    # 例如，输入请求 "绘制一只狗并将其旋转 90 度"。
    query = input('\n用户请求: ')
    # 将用户请求添加到聊天历史。
    messages.append({'role': 'user', 'content': query})
    response = []
    response_plain_text = ''
    print('机器人回应:')
    for response in bot.run(messages=messages):
        # 流式输出。
        response_plain_text = typewriter_print(response, response_plain_text)
    # 将机器人的回应添加到聊天历史。
    messages.extend(response)
```

![](https://ai.feishu.cn/space/api/box/stream/download/asynccode/?code=OTlmMTUyOGRiMDNkODM2ZGU4ZWI1MWMwYmVmNjcxZGNfc3FzdmJ1bUpURDRCVjNxTnRpVzE3VXFLVHBiMmprTkhfVG9rZW46RUV3NmJCYmI4b3hEbXB4V2syd2Noc1JabkRlXzE3NjQ2Njc4Njg6MTc2NDY3MTQ2OF9WNA)![](https://ai.feishu.cn/space/api/box/stream/download/asynccode/?code=ZWYyMjIzZmNmYjYwNTZhZGJmNTA0ZmRkYTQzMTNiMmZfazdmcG5McUc3MHFIejZvcERSWldRSWRoNWNxaTBxYjVfVG9rZW46Rm0xcmI1blRJb1VDUXp4YUNXc2NkS2x2bmVkXzE3NjQ2Njc4Njg6MTc2NDY3MTQ2OF9WNA)

奇奇怪怪的图片xx
<!-- DAILY_CHECKIN_2025-12-02_END -->

# 2025-12-01
<!-- DAILY_CHECKIN_2025-12-01_START -->





明天补上xx
<!-- DAILY_CHECKIN_2025-12-01_END -->

# 2025-11-30
<!-- DAILY_CHECKIN_2025-11-30_START -->






**学习目标**

-   梳理在 ZetaChain 上能做的通用 DeFi 模式。
    
-   为黑客松赛道准备 1–2 个感兴趣的方向。
    

**学习资料**

-   回顾前几天所有与 ZRC-20、Universal Token / NFT、Swap / Messaging 相关内容。
    
-   [https://www.zetachain.com/docs/developers](https://www.zetachain.com/docs/developers)
    
-   [https://www.zetachain.com/docs/developers/evm/zrc20/](https://www.zetachain.com/docs/developers/evm/zrc20/)
    
-   [https://www.zetachain.com/docs/developers/standards/overview](https://www.zetachain.com/docs/developers/standards/overview)
    

**扩展资料（可选）**

-   考虑结合 LSDFi、Restaking、跨链聚合、收益管理等思路。
    

**实践 / 作业**

-   写出 1–2 个可能的通用 DeFi 项目 idea，包括：
    
    -   目标用户
        
    -   想解决的问题
        
    -   粗略的跨链 / 通用资产使用方式
        

# Universal NFT/Token

![](https://ai.feishu.cn/space/api/box/stream/download/asynccode/?code=ZThmMTgzMTkxYzY5NjE2YTY4ZDQxYTEzZjM2NjA1MGRfUDU2ZllZclZldTV3STZlUEFHTjhxUDNSOUtuMlhYWjlfVG9rZW46SDVmTWJBb0I0bzNqZHd4eDJlemM3dzR3bkJnXzE3NjQ1MTA5ODU6MTc2NDUxNDU4NV9WNA)

## 部署

-   在 ZetaChain 上部署 Universal 合约。这是必需步骤，因为 ZetaChain 充当所有跨链转账的中枢，即使是在已连接的 EVM 链之间转移也会经由它路由。
    
-   在任一受支持的 EVM 链（如 Ethereum、Base、Polygon、BNB）部署 Connected 合约。
    
-   在 ZetaChain 上的 Universal 合约中调用 setConnected(zrc20, connectedAddress)，其中：
    
    -   zrc20 为目标 EVM 链的“gas 代币”的 ZRC-20 合约地址。它用作链标识符。
        
    -   connectedAddress 为该 EVM 链上已部署的 Connected 合约地址（步骤 2）。
        
-   在已连接的 EVM 链上调用 ConnectedAsset.setUniversal(universalAddress)，其中 universalAddress 为 ZetaChain 上 Universal 合约地址（步骤 1）。
    

完成以上操作后，ZetaChain 与已连接的 EVM 链上即部署并建立了互信的资产合约。(**互信：Universal 合约和Connected 合约相互调用函数建立连接**）

如需支持更多 EVM 链，针对每条新链重复步骤 2 和 3。

**setConnected 与 setUniversal 是建立合约间“安全信任关系”的必需操作。每个合约都会校验跨链调用是否来自受信任的对应方。**

## Gas 费用

-   EVM → ZetaChain：不收取跨链费用。
    
-   ZetaChain → EVM：以 ZETA 支付跨链费用，**费用基于目标链的 ZRC-20 提现费**。ZETA 会自动兑换为目标链 gas 代币的 ZRC-20 用于执行。
    
-   EVM → EVM：跨链费用**以“源链”的 gas 代币支付**。例如从 Ethereum 转到 BNB，费用以 ETH 支付。ZetaChain 会使用 ZRC-20 ETH 覆盖执行并将其兑换为 ZRC-20 BNB 去调用目标链。
    

回退处理（Revert Handling）

-   若跨链转账在目标链失败（例如 gas 不足、合约拒绝或网络错误），资产将回退给原始发送者。
    
-   当两个已连接的 EVM 链之间的转账失败时，资产会回退给 ZetaChain 上的原始发送者，而不是回到最初的源 EVM 链。这样可以避免将资产退回到源链所需的高成本操作。发送者随后可以从 ZetaChain 再次发起到相同或其他链的转账。
    

# 可能的通用 DeFi 项目 idea

## Idea 1: 跨链一键储蓄金库（Omnichain Savings Vault）

-   目标用户
    
    -   想获得稳健收益、但不愿手动在多链搬砖的普通用户与 DAO 金库管理者。
        
-   想解决的问题
    
    -   多链稳定币与原生资产收益分散、费用高、操作复杂（桥接、换汇、签多笔交易）。
        
    -   收益与风险难以在多链间动态再平衡。
        
-   跨链 / 通用资产使用方式（粗略）
    
    -   用户从任一连接链存入原生代币/稳定币，合约在 ZetaChain 接收为对应 ZRC-20。
        
    -   金库在 ZetaChain 内部策略路由：
        
        -   部分兑换成目标链所需的 gas 对应 ZRC-20，用于后续外呼。
            
        -   将资金在 ZetaChain 统一池中进行代币间路由/兑换（如将多源 USDC/USDT 统一成目标资产）。
            
    -   使用 Gateway 发起多条链上操作：
        
        -   存入以太坊/BNB/Polygon 上的收益协议（或直接在 ZetaChain 上与 DEX/收益合约交互后 withdraw 到目标链协议）。
            
    -   定期再平衡：
        
        -   根据收益率和风险参数，跨链撤出并重新分配，失败则触发回退到 ZetaChain，等待下一次策略执行。
            
    -   提现：
        
        -   用户在任意链发起提现请求，金库在 ZetaChain 侧聚合并以目标链原生资产原路 withdraw 到用户链上地址（ZRC-20 销毁，目标链原生资产到用户）。
            

## Idea 2: 跨链抵押借贷聚合器（Universal CDP/Lending Aggregator）

-   目标用户
    
    -   想用任意链资产抵押，在另一条链获得目标资产流动性的交易者与做市商。
        
-   想解决的问题
    
    -   传统借贷市场受限于单链抵押与单链借款，跨链抵押/借款需多步桥接与清算监控，成本与风险高。
        
-   跨链 / 通用资产使用方式（粗略）
    
    -   抵押侧：
        
        -   用户在任一链抵押原生资产，**进入 ZetaChain 映射为 ZRC-20**；在 ZetaChain 记账抵押仓位。
            
        -   使用 Universal Token 作为平台记账和费用分润代币，可跨链增发/销毁保持总量一致。
            
    -   借款侧：
        
        -   借款目标链由用户指定（如在 Arbitrum 借 USDC，在 BNB 借 BNB），合约在 ZetaChain 查询该链 withdraw 费用并预留对应 ZRC-20 gas。
            
        -   如需换币，先在 ZetaChain 使用统一流动池/DEX 将抵押物部分兑换为目标借款资产的 ZRC-20，再 withdraw 到目标链用户地址。
            
    -   清算与回退：
        
        -   价格喂价与健康因子监控在 ZetaChain 侧统一进行，触发清算时，先在 ZetaChain 内部完成资产兑换与扣减，再对目标链仓位进行调用；若目标链执行失败，回退到 ZetaChain，重新尝试或在本链完成结算。
            
    -   还款与解押：
        
        -   用户在任意链归还债务资产，ZetaChain 收到对应 ZRC-20 后核销债务并释放抵押，按用户选定链 withdraw 原生资产回用户。
            

实现与开发要点（两类项目通用）

-   入口与消息：
    
    -   所有用户交互通过连接链 Gateway 进入，合约在 onCall 解析消息、金额与原始链上下文。
        
-   Gas 抽象与费用控制：
    
    -   在 ZetaChain 先查询目标链 withdraw 费，自动用一小部分资金兑换成目标链 gas 对应 ZRC-20。
        
-   资产路由：
    
    -   区分不同源链的“同名资产”对应不同 ZRC-20，做统一中间资产路由与定价。
        
-   失败回退：
    
    -   任何外呼失败自动回退到 ZetaChain，保留用户头寸或资金，给出重试/改路由选项。
        
-   安全与信任配置：
    
    -   对 Universal 与各 Connected 合约进行 setConnected/setUniversal 绑定，只接收来自可信对端的跨链调用。
        
    -   严格的可升级与权限控制、监控与风控开关。
        

## **Idea3:“ZetaMind 全链智能对冲基金” (ZetaMind Omnichain AI Hedge Fund)**

这是一个结合了 **ZetaChain 全链互操作性** 与 **AI Agent 主动管理** 的增强版 DeFi 提案。

### 目标用户 (Target Users)

-   **寻求 Alpha 收益的散户（Retail Alpha Seekers）**：
    
    -   不满足于稳定币 3-5% 的理财收益，希望通过波段交易、抄底逃顶或新闻事件套利获得更高回报，但缺乏全天候盯盘的时间或专业技能的用户。
        
-   **Web3 投资新手（Crypto Native Newbies）**：
    
    -   手里持有 USDT/USDC 或 ETH，不知道该去哪条链投资，也被复杂的跨链桥、Gas 费计算和钱包授权流程劝退的用户。
        
-   **DAO 金库与中小型机构（DAO Treasuries）**：
    
    -   拥有闲置资金，希望在保证一定安全性的前提下进行资产增值，但苦于无法统一管理分散在多条链上的资产，且缺乏专业的全职交易员团队。
        

### 想解决的问题 (Problems to Solve)

-   **AI 交易落地的“最后一公里”难题**：
    
    -   目前的 AI 交易大多停留在“给建议”阶段，无法直接控制链上资产。通过 ZetaChain，AI 可以作为一个经过授权的“操作员”，直接指挥全链资产，而无需持有资产私钥。
        
-   **多链资产管理的割裂与高磨损**：
    
    -   手动在以太坊、BNB Chain、Arbitrum 之间进行资产轮转（Rebalancing）极其繁琐，且每次操作都需要支付昂贵的跨链桥费用和 Gas 费。
        
-   **被动理财无法应对市场剧烈波动**：
    
    -   传统的跨链收益聚合器（Yield Aggregator）通常只是死板地存入借贷协议。当市场暴跌时，它们无法像交易员一样“空仓避险”；当市场暴涨时，也无法主动切换到高波动资产获利。
        
-   **交易时机的错失**：
    
    -   人类无法 24 小时监控突发新闻（如 SEC 批准 ETF、黑客攻击等）。AI 可以毫秒级捕捉链下数据并触发链上交易。
        

### 粗略的跨链 / 通用资产使用方式 (Rough Omnichain Asset Usage)

该架构利用 ZetaChain 作为**统一的资产结算层与指挥中心**，AI Agent 在链下决策，ZetaChain 合约在链上执行。

A. 资金注入与统一化 (Ingress & Unification)

1.  **用户操作**：用户在任意链（如 Ethereum, BSC, Bitcoin, Polygon）将原生资产（ETH, BTC, USDC 等）发送到 ZetaChain 的 TSS（阈值签名）金库地址。
    
2.  **ZetaChain 处理**：
    
    1.  全链合约侦测到存款，自动将其 Mint 为 ZetaChain 上的 **ZRC-20 代币**（如 ZRC-20 ETH, ZRC-20 BTC, ZRC-20 USDC）。
        
    2.  **统一流动性池**：所有用户的资产汇聚在 ZetaChain 上的一个主合约池中，形成巨大的流动性深度，而非分散在各条链。
        

B. AI 驱动的资产配置策略 (AI-Driven Allocation)

_这是核心差异点。AI 不直接触碰资金，而是通过“意图签名”来指挥合约。_

1.  **链下 AI 大脑**：
    
    1.  AI 模型实时分析 CEX 价格、链上 TVL 变动、宏观新闻 sentiment。
        
    2.  **生成指令**：AI 判定当前是牛市初期，决策：“将 40% 的 USDC 仓位换成 ETH，并存入 Arbitrum 上的高收益池，其余 60% 留在 ZetaChain 此时做流动性挖矿。”
        
    3.  **签名授权**：AI 使用其被合约加入白名单的专用私钥对该指令进行签名（Sign）。
        
2.  **链上验证与内部路由 (On-chain Verification & Routing)**：
    
    1.  ZetaChain 主合约接收 AI 的签名指令，验证通过后执行。
        
    2.  **内部快速兑换**：如果指令是“买入 ETH”，合约直接在 ZetaChain 内部的 DEX（如 Uniswap v2 fork）将 ZRC-20 USDC 兑换为 ZRC-20 ETH。**优势**：无需跨链，Gas 极低，即时完成。
        

C. 跨链外呼与互操作 (Outbound & Interoperability)

当 AI 决定去其他链“打金”或“交互”时：

1.  **Gas 自动处理**：合约自动将少部分资金兑换为目标链所需的 Gas 代币（如 ZRC-20 ARB 或 ZRC-20 MATIC）。
    
2.  **外呼调用**：
    
    1.  ZetaChain 合约调用 `Gateway` 发起跨链交易。
        
    2.  例如：将 ZRC-20 ETH 销毁，并在 Arbitrum 链上释放原生 ETH，同时附带一段 payload 指令调用 Arbitrum 上的 GMX 或 Aave 协议进行存款。
        
3.  **紧急避险（撤回）**：
    
    1.  如果 AI 监测到目标链协议（如 Curve）出现脱钩风险，立即发送“撤回”指令。ZetaChain 合约远程触发目标链的 withdraw 操作，将资金全额退回 ZetaChain 避险。
        

D. 利润分配与提现 (Settlement & Withdrawal)

1.  **定期再平衡**：AI 每 4-8 小时根据市场情况重新评估，重复上述 B 和 C 的步骤。
    
2.  **用户提现**：
    
    1.  用户在任意链发起提现请求。
        
    2.  系统计算用户份额对应的资产价值。
        
    3.  ZetaChain 合约将各种 ZRC-20 资产（可能是 AI 交易后的 BTC、ETH 混合物）统一兑换成用户想要的目标资产（如 USDC）。
        
    4.  通过 TSS 跨链将原生 USDC 发送到用户钱包。
        

![](https://ai.feishu.cn/space/api/box/stream/download/asynccode/?code=YWZkYTY3NmM4YjYxN2NhNjc4NTllYTBlMmQwOGUxY2RfcUZ1cXFXcUlVa3BYeElUQWROelRSWWRKbUU4ZExIMVFfVG9rZW46UWcxZGJTSndKb3dFaUV4M0VURmNEOGpEbk5wXzE3NjQ1MTA5ODU6MTc2NDUxNDU4NV9WNA)
<!-- DAILY_CHECKIN_2025-11-30_END -->

# 2025-11-29
<!-- DAILY_CHECKIN_2025-11-29_START -->







**学习目标**

-   能从官方示例中跑起来一个基础的跨链 Demo（建议 Swap 或 Messaging）。
    
-   亲手体验“调用一次全链 DeFi 动作”的流程。
    

**学习资料**

-   Tutorials（Swap / Messaging / Calls to-from EVM）
    
-   [https://www.zetachain.com/docs/developers/tutorials/swap](https://www.zetachain.com/docs/developers/tutorials/swap)
    
-   Example code repo
    
-   [https://github.com/zeta-chain/example-contracts](https://github.com/zeta-chain/example-contracts)
    

**扩展资料（可选）**

-   结合架构文档，再看一下对应调用在架构中的位置。
    

**实践 / 作业**

-   任选一个官方 Demo（推荐 Swap 或 Messaging），按文档说明跑通：
    
    -   本地或测试网都可以。
        
    -   记录下关键命令、配置项。
        
-   写一段文字：
    
    -   你是从哪里发起的调用？
        
    -   最终在 ZetaChain 上发生了什么？
        

# Swap

## [环境搭建](https://www.zetachain.com/docs/developers/tutorials/swap#setting-up-your-environment)

首先，使用CLI创建一个新的ZetaChain项目：

```Solidity
zetachain new --project swap
```

安装依赖：

```Solidity
cd swap
yarn
```

使用 Foundry 的包管理器拉取 Solidity 依赖：

```Solidity
forge soldeer update
```

编写合同：

```Solidity
forge build
```

## [在Testnet上部署](https://www.zetachain.com/docs/developers/tutorials/swap#option-1:-deploy-on-testnet)

将交换合约部署到 ZetaChain 测试网：

```Solidity
UNIVERSAL=$(npx tsx commands deploy --private-key $PRIVATE_KEY | jq -r .contractAddress) && echo $UNIVERSAL
```

这会使用指定的私钥部署预编译的合同， 输出已部署的地址。部署脚本会自动使用正确的网关和Uniswap路由器 测试网的地址。完成后，环境变量将包含你在测试网上部署的交换合约。你会在触发连接链的交换。

例如我用我的密钥部署后得到的地址：

![](https://ai.feishu.cn/space/api/box/stream/download/asynccode/?code=NWI0M2VjMzI3OWUyYjAyNzk1NTZmMmNjNDI2MWQ2YjJfZU9DMXQwNmF3WnY4VlJZSndKQUZ1d29UdzZDMG9DZkFfVG9rZW46Um1ZUGJzY1E3b2Z6eHB4U2tjQ2NhR1BPbkJkXzE3NjQ0MDMyMTI6MTc2NDQwNjgxMl9WNA)

通过私钥获取你的EVM发件地址：

```Solidity
RECIPIENT=$(cast wallet address $PRIVATE_KEY) && echo $RECIPIENT
```

查询代表以太坊 Sepolia ETH 的 ZRC-20 地址：

```Solidity
ZRC20_ETHEREUM_ETH=$(zetachain q tokens show --symbol sETH.SEPOLIA -f zrc20) && echo $ZRC20_ETHEREUM_ETH
```

# 理解swap合约

## 1.代币交换事件

```Solidity
event TokenSwap(
        bytes sender,
        bytes indexed recipient,
        address indexed inputToken,
        address indexed targetToken,
        uint256 inputAmount,
        uint256 outputAmount
    );
    
   /**
     * @notice 代币交换事件
     * @dev 记录每次成功的代币交换操作
     * @param sender 发起交换的用户地址（字节格式，支持不同链的地址格式）
     * @param recipient 接收代币的目标地址（字节格式）
     * @param inputToken 输入代币的 ZRC20 地址
     * @param targetToken 目标代币的 ZRC20 地址
     * @param inputAmount 输入的代币数量
     * @param outputAmount 输出的代币数量（扣除 gas 费用后）
     */
```

## 2.构造函数-禁止初始化器

```Solidity
/// @notice 构造函数，禁用初始化器以防止实现合约被直接初始化
    constructor() {
        _disableInitializers();
    }
```

## 3.初始化合约

```Solidity
/**
     * @notice 初始化合约
     * @dev 只能调用一次，设置 Uniswap 路由和 gas 限制
     * @param gasLimitAmount 回滚操作的 gas 限制
     * @param owner 合约所有者地址
     */
function initialize(
    uint256 gasLimitAmount,
    address owner
) external initializer {
    // 初始化 UUPS 升级功能
    __UUPSUpgradeable_init();
    
    // 初始化所有权管理，设置合约所有者
    __Ownable_init(owner);
    
    // 从系统注册表获取当前链的 Uniswap V2 路由地址
    (bool active, bytes memory uniswapRouterBytes) = registry
      .getContractInfo(block.chainid, "uniswapV2Router02");
    
    // 如果路由地址未激活，则抛出错误
    if (!active) revert InvalidAddress();
    
    // 将字节数组转换为地址类型
    uniswapRouter = address(uint160(bytes20(uniswapRouterBytes)));
    
    // 设置回滚操作的 gas 上限
    gasLimit = gasLimitAmount;
}
```

## 4.提现参数结构体

```Solidity
    struct Params {
        address target;
        bytes to;
        bool withdraw;
    }
```

用于在 withdraw 函数中传递参数：

-   target： 目标代币的 ZRC20 地址
    
-   to： 接收地址（字节格式，支持不同链）
    
-   withdraw： 是否提现到外部链（true）或留在 ZetaChain（false）
    

## 5.onCall函数-处理来自外部链的跨链交换请求

```Solidity
function onCall(
    MessageContext calldata context,
    address zrc20,
    uint256 amount,
    bytes calldata message
) external override onlyGateway {
    // 解码跨链消息，获取目标代币、接收地址和提现标志
    (address targetToken, bytes memory recipient, bool withdrawFlag) = abi
        .decode(message, (address, bytes, bool));

    // 处理 gas 费用并执行代币交换
    (uint256 out, address gasZRC20, uint256 gasFee) = handleGasAndSwap(
        zrc20,        // 输入代币
        amount,       // 输入数量
        targetToken,  // 目标代币
        withdrawFlag  // 是否需要提现
    );
    
    // 发出交换事件，记录此次交换
    emit TokenSwap(
        abi.encodePacked(context.sender),  // 解码原始发送者（来自其他链）
        recipient,                          // 接收者
        zrc20,                             // 输入代币
        targetToken,                       // 目标代币
        amount,                            // 输入数量
        out                                // 输出数量
    );
    
    // 执行提现或转账
    withdraw(
        Params({
            target: targetToken,
            to: recipient,
            withdraw: withdrawFlag
        }),
        abi.encodePacked(context.sender),  // 用于回滚的原始发送者
        gasFee,                            // gas 费用
        gasZRC20,                          // gas 代币
        out,                               // 输出金额
        zrc20                              // 原始输入代币（用于回滚）
    );
}
```

\- **调用者**: 只能由 ZetaChain Gateway 调用`onlyGateway` 修饰符）

\- **参数解码**: 从 `message` 中解析目标代币、接收地址和提现标志

\- **交换执行**: 调用 `handleGasAndSwap` 处理 gas 并执行交换

\- **事件记录**: 发出 `TokenSwap` 事件供链下监控

\- **最终操作**: 根据 `withdrawFlag` 决定是提现还是内部转账

举个例子：

用户从以太坊发送 USDC，想换成 BNB Chain 上的 BNB：

1\. 用户在以太坊调用 ZetaChain Gateway，发送 USDC

2\. Gateway 在 ZetaChain 上调用此合约的 `onCall` 函数 --调用函数，解码message

3\. 合约将 USDC 换成 BNB（ZRC20）--handleGasAndSwap ， emit TokenSwap

4\. 合约将 BNB 提现到用户的 BNB Chain 地址 --withdraw

## 6.**swap 函数 - ZetaChain 内部交换**

```Solidity
/**
 * @notice 在 ZetaChain 上直接发起代币交换
 * @dev 用户可以从 ZetaChain 发起交换，可选择提现到外部链
 * 
 * 使用场景：
 * - 用户在 ZetaChain 上持有代币，想换成其他代币
 * - 可以选择将兑换后的代币留在 ZetaChain 或提现到外部链
 * 
 * @param inputToken 输入代币的 ZRC20 地址
 * @param amount 输入代币数量
 * @param targetToken 目标代币的 ZRC20 地址
 * @param recipient 接收地址（字节格式）
 * @param withdrawFlag 是否提现到外部链
 */
function swap(
    address inputToken,
    uint256 amount,
    address targetToken,
    bytes memory recipient,
    bool withdrawFlag
) external {
    // 从调用者转入代币到合约
    bool success = IZRC20(inputToken).transferFrom(
        msg.sender,
        address(this),
        amount
    );
    
    // 检查转账是否成功
    if (!success) {
        revert TransferFailed(
            "Failed to transfer ZRC-20 tokens from the sender to the contract"
        );
    }

    // 处理 gas 费用并执行代币交换
    (uint256 out, address gasZRC20, uint256 gasFee) = handleGasAndSwap(
        inputToken,
        amount,
        targetToken,
        withdrawFlag
    );
    
    // 发出交换事件
    emit TokenSwap(
        abi.encodePacked(msg.sender),
        recipient,
        inputToken,
        targetToken,
        amount,
        out
    );
    
    // 执行提现或转账
    withdraw(
        Params({
            target: targetToken,
            to: recipient,
            withdraw: withdrawFlag
        }),
        abi.encodePacked(msg.sender),
        gasFee,
        gasZRC20,
        out,
        inputToken
    );
}
```

\- **调用者**: 任何持有 ZRC20 代币的用户

\- **代币转入**: 使用 `transferFrom` 将用户代币转入合约

\- **交换逻辑**: 与 `onCall` 相同，复用 `handleGasAndSwap` 和 `withdraw`

## 7.**handleGasAndSwap 函数 - Gas 处理与交换核心逻辑**

上面的oncall函数和swap函数都调用了这个函数

```Solidity
/**
 * @notice 处理 gas 费用并执行代币交换的核心逻辑
 * @dev 内部函数，先计算并预留 gas 费用，然后用剩余代币进行交换
 * 
 * 处理步骤：
 * 1. 如果需要提现，查询目标链的 gas 费用
 * 2. 检查输入金额是否足够支付 gas
 * 3. 如果 gas 代币与输入代币不同，先换一部分用于支付 gas
 * 4. 用剩余的输入代币交换成目标代币
 * 
 * @param inputToken 输入代币地址
 * @param amount 输入代币总量
 * @param targetToken 目标代币地址
 * @param withdraw 是否需要提现到外部链
 * @return out 交换得到的目标代币数量
 * @return gasZRC20 用于支付 gas 的代币地址
 * @return gasFee gas 费用金额
 */
function handleGasAndSwap(
    address inputToken,
    uint256 amount,
    address targetToken,
    bool withdraw
) internal returns (uint256, address, uint256) {
    uint256 inputForGas;      // 用于支付 gas 的输入代币数量
    address gasZRC20;         // gas 代币地址
    uint256 gasFee = 0;       // gas 费用金额
    uint256 swapAmount = amount;  // 实际用于交换的金额

    // 如果需要提现到外部链
    if (withdraw) {
        // 查询目标链的 gas 费用和 gas 代币类型
        (gasZRC20, gasFee) = IZRC20(targetToken).withdrawGasFee();
        
        // 计算支付 gas 所需的最少输入代币
        uint256 minInput = quoteMinInput(inputToken, targetToken);
        
        // 检查输入金额是否足够
        if (amount < minInput) {
            revert InsufficientAmount(
                "The input amount is less than the min amount required to cover the withdraw gas fee"
            );
        }
        
        // 如果 gas 代币就是输入代币，直接扣除 gas 费用
        if (gasZRC20 == inputToken) {
            swapAmount = amount - gasFee;
        } 
        // 如果 gas 代币与输入代币不同，需要先换一部分用于支付 gas
        else {
            inputForGas = SwapHelperLib.swapTokensForExactTokens(
                uniswapRouter,
                inputToken,    // 输入代币
                gasFee,        // 需要的 gas 代币数量
                gasZRC20,      // gas 代币类型
                amount         // 最大输入金额
            );
            swapAmount = amount - inputForGas;  // 剩余金额用于交换
        }
    }

    // 用剩余的输入代币交换成目标代币
    uint256 out = SwapHelperLib.swapExactTokensForTokens(
        uniswapRouter,
        inputToken,      // 输入代币
        swapAmount,      // 输入数量
        targetToken,     // 目标代币
        0                // 最小输出（0 表示接受任何数量，生产环境应设置滑点保护）
    );
    
    return (out, gasZRC20, gasFee);
}
```

## **8.withdraw 函数 - 执行转账或提现**

```Solidity
/**
 * @notice 执行代币转账或提现操作
 * @dev 根据参数决定是在 ZetaChain 内部转账还是提现到外部链
 * 
 * 两种模式：
 * 1. 提现模式 (withdraw = true): 
 *    - 授权 gateway 使用代币和 gas 费
 *    - 调用 gateway.withdraw() 发送到目标链
 *    - 如果失败会触发 onRevert 回滚
 * 
 * 2. 内部转账模式 (withdraw = false):
 *    - 直接在 ZetaChain 上转账给接收地址
 *    - 不需要跨链操作
 * 
 * @param params 提现参数（目标代币、接收地址、是否提现）
 * @param sender 原始发送者地址（用于回滚）
 * @param gasFee gas 费用金额
 * @param gasZRC20 用于支付 gas 的代币地址
 * @param out 要转账/提现的代币数量
 * @param inputToken 输入代币地址（用于回滚）
 */
function withdraw(
    Params memory params,
    bytes memory sender,
    uint256 gasFee,
    address gasZRC20,
    uint256 out,
    address inputToken
) internal {
    // 如果需要提现到外部链
    if (params.withdraw) {
        // 情况 1: gas 代币与目标代币相同
        if (gasZRC20 == params.target) {
            // 授权 gateway 使用 (代币金额 + gas 费用)
            if (!IZRC20(gasZRC20).approve(address(gateway), out + gasFee)) {
                revert ApprovalFailed();
            }
        } 
        // 情况 2: gas 代币与目标代币不同
        else {
            // 分别授权 gas 代币和目标代币
            if (!IZRC20(gasZRC20).approve(address(gateway), gasFee)) {
                revert ApprovalFailed();
            }
            if (!IZRC20(params.target).approve(address(gateway), out)) {
                revert ApprovalFailed();
            }
        }
        
        // 调用 gateway 执行跨链提现
        gateway.withdraw(
            abi.encodePacked(params.to),  // 目标链的接收地址
            out,                          // 提现金额
            params.target,                // 目标代币
            RevertOptions({
                revertAddress: address(this),           // 回滚时调用本合约
                callOnRevert: true,                     // 启用回滚回调
                abortAddress: address(0),               // 不使用中止地址
                revertMessage: abi.encode(sender, inputToken),  // 回滚信息
                onRevertGasLimit: gasLimit              // 回滚操作的 gas 上限
            })
        );
    } 
    // 如果只是在 ZetaChain 内部转账
    else {
        // 直接转账给接收地址
        bool success = IWETH9(params.target).transfer(
            address(uint160(bytes20(params.to))),  // 将 bytes 转换为地址
            out
        );
        
        // 检查转账是否成功
        if (!success) {
            revert TransferFailed(
                "Failed to transfer target tokens to the recipient on ZetaChain"
            );
        }
    }
}
```

\- **提现模式**:

\- 授权 gateway 使用代币

\- 设置回滚选项，确保失败时可以退款

\- 调用 gateway 的 `withdraw` 执行跨链操作

\- **内部转账模式**:

\- 直接在 ZetaChain 上转账

\- 不涉及跨链，更快且更便宜

## 9.**onRevert 函数 - 处理失败回滚**

```Solidity
/**
 * @notice 处理跨链操作失败时的回滚逻辑
 * @dev 当目标链无法接收代币时（如接收地址是无法接收代币的合约），会触发此函数
 * 
 * 回滚流程：
 * 1. 解码原始发送者和输入代币信息
 * 2. 将失败的代币换回原始代币
 * 3. 将原始代币退回给发送者
 * 
 * 常见失败场景：
 * - 目标地址是不支持接收代币的智能合约
 * - 目标链上的操作 gas 不足
 * - 目标链网络拥堵或暂时不可用
 * 
 * @param context 回滚上下文，包含失败的代币、数量和回滚消息
 */
function onRevert(RevertContext calldata context) external onlyGateway {
    // 解码回滚消息，获取原始发送者和输入代币
    (bytes memory sender, address zrc20) = abi.decode(
        context.revertMessage,
        (bytes, address)
    );
    
    // 将失败的代币（context.asset）换回原始代币（zrc20）
    (uint256 out, , ) = handleGasAndSwap(
        context.asset,   // 失败的代币（目标代币）
        context.amount,  // 失败的金额
        zrc20,          // 原始输入代币
        true            // 需要提现回原始链
    );

    // 将原始代币退回给发送者
    gateway.withdraw(
        sender,    // 原始发送者地址
        out,       // 退款金额
        zrc20,     // 原始代币
        RevertOptions({
            revertAddress: address(bytes20(sender)),  // 如果再次失败，直接发给发送者
            callOnRevert: false,                      // 不再调用回滚函数
            abortAddress: address(0),
            revertMessage: "",                        // 无需回滚消息
            onRevertGasLimit: gasLimit
        })
    );
}
```

## **10.quoteMinInput 函数 - 计算最小输入**

```Solidity
/**
 * @notice 计算支付 gas 费用所需的最少输入代币数量
 * @dev 用于提现前检查用户的输入金额是否足够支付跨链 gas
 * 
 * 计算逻辑：
 * 1. 查询目标链提现需要的 gas 费用和 gas 代币类型
 * 2. 如果输入代币就是 gas 代币，直接返回 gas 费用
 * 3. 如果不是，通过 Uniswap 计算需要多少输入代币才能换到足够的 gas 代币
 * 
 * 使用场景：
 * - 前端在发起交换前检查用户余额是否足够
 * - 合约内部验证输入金额是否满足最小要求
 * 
 * @param inputToken 输入代币地址
 * @param targetToken 目标代币地址（决定了目标链和 gas 类型）
 * @return 需要的最少输入代币数量
 */
function quoteMinInput(
    address inputToken,
    address targetToken
) public view returns (uint256) {
    // 查询目标链的 gas 费用信息
    (address gasZRC20, uint256 gasFee) = IZRC20(targetToken)
        .withdrawGasFee();

    // 如果输入代币就是 gas 代币，直接返回 gas 费用
    if (inputToken == gasZRC20) {
        return gasFee;
    }

    // 获取 WETH 地址（Uniswap 的基础交易对）
    address zeta = IUniswapV2Router01(uniswapRouter).WETH();

    // 构建交换路径
    address[] memory path;
    
    // 如果输入代币或 gas 代币是 WETH，使用两步路径
    if (inputToken == zeta || gasZRC20 == zeta) {
        path = new address[](2);
        path[0] = inputToken;
        path[1] = gasZRC20;
    } 
    // 否则使用三步路径：输入代币 -> WETH -> gas 代币
    else {
        path = new address[](3);
        path[0] = inputToken;
        path[1] = zeta;
        path[2] = gasZRC20;
    }

    // 通过 Uniswap 计算需要多少输入代币才能换到所需的 gas 代币
    uint256[] memory amountsIn = IUniswapV2Router02(uniswapRouter)
        .getAmountsIn(gasFee, path);

    // 返回第一个代币（输入代币）的所需数量
    return amountsIn[0];
}
```

## **11.\_authorizeUpgrade 函数 - 升级授权**

```Solidity
/**
 * @notice 授权合约升级
 * @dev UUPS 升级模式要求的函数，只有所有者可以升级合约
 * @param newImplementation 新的实现合约地址
 */
function _authorizeUpgrade(
    address newImplementation
) internal override onlyOwner {}
```
<!-- DAILY_CHECKIN_2025-11-29_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->








**学习目标**

-   理解 ZRC-20、Universal Token / NFT 的基本概念和作用。
    
-   明白多链资产在 ZetaChain 上如何被统一表示。
    

**学习资料**

-   ZRC-20 标准
    
-   [https://www.zetachain.com/docs/developers/evm/zrc20/](https://www.zetachain.com/docs/developers/evm/zrc20/)
    
-   Universal 资产文档
    
-   [https://www.zetachain.com/docs/developers/standards/overview](https://www.zetachain.com/docs/developers/standards/overview)
    

**扩展资料（可选）**

-   Example code repo（以后会用到，今天先收藏）
    
-   [https://github.com/zeta-chain/example-contracts](https://github.com/zeta-chain/example-contracts)
    

**实践 / 作业**

-   在笔记中写出：
    
    -   ZRC-20 和普通 ERC-20 的直观区别（从开发者视角）。
        
    -   举一个「通用资产」可能的应用场景（比如跨链储蓄、通用 NFT 通行证等）。
        

# ZRC-20

## 基本概念

ZetaChain 上对外部链原生资产与 ERC-20 的“原生表示”。当从以太坊/BNB/比特币等连接链把资产“存入”ZetaChain 时，协议在连接链侧托管这些资产（TSS 地址或托管合约），**并在 ZetaChain 上按 1:1 铸造对应的 ZRC-20**。**（托管映射）**提出时则销毁 ZRC-20，并从托管处把原生资产打回目标链。

## 性质

-   只能由协议铸造
    
-   同名代币跨不同源链被当作不同资产（如 ETH Mainnet 的 USDT 与 BSC 的 USDT 是两个 ZRC-20）。
    
-   支持查询/扣除跨链所需 gas 费用
    
-   可实现资产回退（revert）处理。
    

## 使用场景

-   跨链资产转移：源链资产-->ZRC-20-->目标链资产，
    
-   Gas 抽象：合约可用部分输入资产兑换成目的链 gas 对应的 ZRC-20 以支付跨链执行费用。
    

# Universal Token（通用代币）

## 基本概念

一种“可在任意链铸造与转移、保持总量与元数据一致”的跨链 ERC-20 标准。

**但是他不是托管映射的方式，而是同一种代币在多链上的原生流转，它会在源链销毁，在目标链按同一供应铸造。**

## 架构

两类合约：

-   Universal（部署在 ZetaChain，必需）：充当跨链中枢与信任根。
    
-   Connected（部署在各 EVM 连接链，可选多条）：与 Universal 建立互信映射。
    

## 转移路径

任意链之间的转移都会经由 ZetaChain 路由（例如 Ethereum → ZetaChain → BNB）

-   费用模型：
    
    -   EVM → ZetaChain：无额外跨链费。
        
    -   ZetaChain → EVM：以 ZETA 支付，对应自动兑换为目标链 gas 的 ZRC-20。
        
    -   EVM → EVM：源链 gas 资产承担费用，协议会在 ZetaChain 上完成所需 ZRC-20 兑换。
        

# Universal NFT（通用 NFT，ERC-721 级）

## 定义

可在任意连接链铸造与转移的 NFT 标准。跨链时源链销毁、目标链按相同 tokenId 重铸；元数据保持一致，tokenId 跨链持久。

## 架构

也是Universal和Connected两个合约，并通过互信列表限制只接受同一集合的跨链调用。

# ZRC-20 vs Universal Token/NFT：本质差异

-   ZRC-20：代表“外部链已有资产”的在链内的**可编程镜像**，由协议管理铸销，适合“把外部价值带入 ZetaChain 进行逻辑编排后再原样提出”。
    
-   Universal Token/NFT：你自己的“原生跨链资产标准”，在多链之间通过**“销毁/重铸”**保持单一供应与一致性，不依赖外部链既有资产或托管余额。
    

# ZRC-20 和普通 ERC-20 的直观区别

从开发者（Developer）的视角来看，**ZRC-20** (ZetaChain Request for Comment 20) 和普通的 **ERC-20** 的核心区别在于：**资产的“管辖范围”和“交互能力”**。

简单的一句话总结：**ERC-20 是“孤岛内”的账本，而 ZRC-20 是连接外部原生链资产的“遥控器”。**

以下是具体的直观区别：

### 资产的所有权与映射逻辑 (State & Ownership)

-   **普通 ERC-20:**
    
    -   **逻辑：** 资产只存在于当前这条链（如 Ethereum 或 Polygon）的合约状态里。
        
    -   **开发者视角：** 如果你想把 ETH 跨链变成 Polygon 上的 ETH，你需要依赖第三方“桥”（Lock & Mint 机制），生成的是一个 Wrapped Token（如 WETH）。这个 WETH 和原链的 ETH 是割裂的，合约无法直接感知原链状态。
        
-   **ZRC-20:**
    
    -   **逻辑：** ZRC-20 代币代表了**外部链上的原生资产**（如 Ethereum 上的 ETH，Bitcoin 上的 BTC，BSC 上的 BNB），这些资产被锁定在 ZetaChain 的 TSS（阈值签名）账户中。
        
    -   **开发者视角：** 你在 ZetaChain 上部署合约，可以通过 ZRC-20 合约直接管理和操作外部链的资产。你虽然在操作 ZetaChain 上的代币，但协议层会自动将其转化为外部链的原生交易。
        

### 核心接口功能的扩展 (`withdraw` 是大杀器)

这是代码层面最直观的区别。ZRC-20 兼容 ERC-20 接口（意味着可以配合 Uniswap, Metamask 使用），但多了一个关键功能。

-   **普通 ERC-20:**
    
    -   标准方法：`transfer`, `approve`, `transferFrom`, `balanceOf`。
        
    -   **局限：** 调用 `transfer` 只是改变了当前链上的数字，外部世界毫无变化。
        
-   **ZRC-20:**
    
    -   标准方法：`transfer`, `approve`... (同 ERC-20)。
        
    -   **扩展方法：** `withdraw(bytes recipient, uint256 amount)`。
        
    -   **开发者视角：** 当你在合约里调用 `ZRC20.withdraw(btc_address, amount)` 时，**魔法发生了**：ZetaChain 协议层会捕获这个事件，通过 TSS 节点在**比特币主网**上发起一笔真正的 BTC 转账给 `btc_address`。
        
    -   **直观感受：** 你在写 Solidity，但你居然控制了比特币网络！
        

### Gas 费用的处理 (Gas Abstraction)

-   **普通 ERC-20:**
    
    -   你在以太坊上转账，消耗 ETH；在 Polygon 上转账，消耗 MATIC。逻辑很简单，也是隔离的。
        
-   **ZRC-20:**
    
    -   **要解决的问题：** 跨链操作通常**需要用户持有目标链的 Gas 代币**。
        
    -   **开发者视角：** ZRC-20 内置了 Gas 扣除逻辑。当你调用 `withdraw` 把资产提回原链（例如提现 BTC 回比特币网络）时，ZetaChain 协议会自动计算比特币网络的 Gas 费，并直接从你提现的 ZRC-20 金额中扣除（以 ZRC-20 代币支付）。
        
    -   **优势：** 你的用户不需要持有 BTC 也能完成涉及 BTC 的复杂交互，因为 Gas 费是自动从交易额里“扣”出来的。
        

### 对比特币（非智能合约链）的支持

这是 ZRC-20 最具颠覆性的地方。

-   **普通 ERC-20:**
    
    -   无法直接与比特币交互。你只能使用 WBTC（中心化托管的 ERC-20 代币）。你无法通过 Solidity 代码触发一笔比特币主网的转账。
        
-   **ZRC-20:**
    
    -   ZetaChain 为 BTC 这种非智能合约链的原生资产创建了 ZRC-20 版本的 BTC。
        
    -   **开发者视角：** 你可以写一个 Uniswap 风格的 DEX 合约（Solidity），一边是 ZRC-20 ETH，一边是 ZRC-20 BTC。用户通过比特币钱包转账 BTC 到 ZetaChain TSS 地址，你的合约自动接收并 Swap 成 ETH，然后用户在以太坊钱包收到 ETH。全程不需要 WBTC 这种中间商。
        

### 交互流程对比 (举例：跨链 Swap)

假设你要做一个应用：**用户支付 BTC，获得 ETH。**

-   **普通 ERC-20 开发路径：**
    
    -   用户去中心化交易所把 BTC 换成 WBTC。
        
    -   用户把 WBTC 跨链桥接到以太坊。
        
    -   用户在以太坊 DEX 用 WBTC 换 ETH。
        
    -   _开发者需要接入多个协议，用户体验极其割裂。_
        
-   **ZRC-20 开发路径 (Omnichain Contract)：**
    
    -   你在 ZetaChain 上部署一个合约。
        
    -   合约监听：当收到 BTC (通过 ZRC-20 hook `onCrossChainCall`)。
        
    -   合约逻辑：自动将 ZRC-20 BTC 换成 ZRC-20 ETH (在 ZetaChain 本地 Swap)。
        
    -   合约逻辑：调用 ZRC-20 ETH 的 `withdraw` 方法，填入用户的 ETH 地址。
        
    -   结果：用户发出 BTC，收到了 ETH。
        
    -   _开发者只需在一个合约里写逻辑，即由 ZetaChain 编排全链。_
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->









**学习目标**

-   建立对 “全链应用 / Universal App 合约” 的直观理解。
    
-   清楚后面要实现的 Hello World / Demo 会包含哪些模块（合约 + 前端 + RPC）。
    

**学习资料**

-   继续浏览 ZetaChain Developers 中的 EVM / Tutorials 部分
    
-   [https://www.zetachain.com/docs/developers](https://www.zetachain.com/docs/developers)
    

**扩展资料（可选）**

-   如果有时间，可提前快速扫一眼 Tutorials 中的 Swap / Messaging 结构。
    

**实践 / 作业**

-   在笔记中写出：
    
    -   自己想做的第一个 Universal App 想实现的“打印 / 记录 / 简单逻辑”是什么。
        
-   为后续的 Hello World Demo 决定一种工作流：
    
    -   使用 CLI + Hardhat / Foundry？
        
    -   用本地链还是测试网？
        

# 1.实现一个简单的合约

## 环境建立

初始化一个新项目 ZetaChain CLI（命令行界面）。

```Solidity
npx zetachain@latest new --project hello
cd hello
yarn 或者 npm install
forge soldeer update
```

## hello模板

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.26;

import "@zetachain/protocol-contracts/contracts/zevm/interfaces/UniversalContract.sol";

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

## testnet 部署

```Solidity
UNIVERSAL=$(forge create Universal \
  --rpc-url https://zetachain-athens-evm.blockpi.network/v1/rpc/public \
  --private-key $PRIVATE_KEY \
  --broadcast \
  --json | jq -r .deployedTo)
```

```Solidity
#查看合约地址：
echo $UNIVERSAL
```

用合约地址在[https://testnet.zetascan.com/上查询即可。](https://testnet.zetascan.com/上查询即可。)

![](https://ai.feishu.cn/space/api/box/stream/download/asynccode/?code=MjEwZmU0OWYzNTQxZmEyZjFkNTMzNzU5NWVjZGI1OGVfZkwxMXNSM3l6UjhGdkJmdzFtNjFaalhtb3NXUVlvd0lfVG9rZW46TWZRd2J1Mm5rb0QyVnp4bEFMMWM5eEhZbkllXzE3NjQyNTAzNzc6MTc2NDI1Mzk3N19WNA)

其实还可以这样编译和部署，这个会返回所有的部署信息：

```Solidity
# 1. 加载 Node 环境
source /root/.nvm/nvm.sh

# 2. 安装依赖（如果未安装）
npm install

# 3. 编译合约
npx hardhat compile

# 4. 部署
# 创建 .env 文件并配置私钥
echo "PRIVATE_KEY=你的64位私钥" > .env
npx tsx commands/index.ts deploy --private-key $(grep PRIVATE_KEY .env | cut -d '=' -f2)
```

# 2.CrossChainEcho (跨链传声筒)

目标：**我在以太坊（Sepolia）喊一句，ZetaChain 必须能听见并记下来。**

-   **输入 (Input)：**
    
    -   不转账代币，只从 Sepolia 发一笔交易，带上一段话（字符串，比如 "Hello Zeta!"）。
        
-   **简单逻辑 (Logic)：**
    
    -   ZetaChain 上的合约收到那串乱糟糟的十六进制数据后，得把它**解码**成人类能看懂的文字。
        
    -   顺便检查一下是不是发了个空消息。
        
-   **记录与打印 (Record & Print)：**
    
    -   **记下来：** 用个小本本（`mapping`）在链上存好：_“谁（Address）在什么时候说了什么”_。
        
    -   **吼出来：** 虽然不能 `console.log`，但我可以 `emit Event`。只要能在 ZetaScan 浏览器上看到这个 Event 弹出来，就算大功告成！
        

使用CLI + Hardhat+测试网
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->










ZetaChain & Universal Blockchain 核心概念

**学习目标**

-   理解 “通用区块链 / Universal EVM / Universal App / Omnichain Smart Contract” 的基本含义。
    
-   能用图的方式画出：ZetaChain + 多条公链 + Gateway 的大致结构。
    

**学习资料**

-   Universal Apps 概览
    

[https://www.zetachain.com/docs/start/app](https://www.zetachain.com/docs/start/app)

-   开发者总览（Universal EVM / Gateway / Cross-Chain）
    

[https://www.zetachain.com/docs/developers](https://www.zetachain.com/docs/developers)

**扩展资料（可选）**

-   为什么在 ZetaChain 上面开发？
    

[https://www.zetachain.com/docs/start/build](https://www.zetachain.com/docs/start/build)

-   ZetaChain 架构设计（粗略看一遍）
    

[https://www.zetachain.com/docs/developers/architecture/overview](https://www.zetachain.com/docs/developers/architecture/overview)

**实践 / 作业**

-   在笔记中用自己的话写出：
    
    -   Universal App 是什么？
        
    -   Gateway 大概做什么？
        
-   画一张简单的架构图：
    
-   ZetaChain 中心 + Bitcoin / Ethereum / Solana 等外围链 + Gateway。
    

# What is Universal App ?

## 理解

我的理解：部署在区块链（ZetaChain）上的一个智能合约，但它可以**同时连接和操作其他区块链。**

总结一下特点：

**1\. 单个合约，统治所有** 传统的跨链应用需要在每条链上都部署合约。你在以太坊部署一个。你在币安链部署一个。你在 Polygon 部署一个。 ZetaChain 的 Universal App 只需要**部署一次**。它部署在 ZetaChain 的 EVM（以太坊虚拟机）兼容层上。它就能控制连接到网络的所有链。这极大降低了开发难度。

**2\. 赋予比特币智能合约能力**

比特币本身不支持复杂的智能合约。普通的 DeFi 应用无法直接使用原生比特币。 Universal App 解决了这个问题。它通过 ZetaChain 的账户系统持有比特币。开发者可以在 Universal App 里编写逻辑来控制这些比特币。用户可以在 Uniswap 风格的界面里使用原生比特币交易。不需要把比特币包装成 WBTC。

**3\. ZRC-20 标准：万能转接头** ZetaChain 发明了一种代币标准叫 **ZRC-20**。 当用户把比特币、以太坊或 DOGE 充值进 ZetaChain 系统时。这些资产在 ZetaChain 内部会显示为 ZRC-20 代币。 开发者写代码时。他们只需要处理 ZRC-20 代币。这就像处理普通的 ERC-20 代币一样简单。 当用户提款时。ZRC-20 代币会被销毁。ZetaChain 会在对应的源链（比如比特币网络）上把原生资产转回给用户。

## 示例

```Solidity
pragma solidity 0.8.26;
 
import "@zetachain/protocol-contracts/contracts/zevm/interfaces/UniversalContract.sol";
 
contract UniversalApp is UniversalContract {
    function onCall(
        MessageContext calldata context,
        address zrc20,
        uint256 amount,
        bytes calldata message
    ) external virtual override {
        // ...
    }
}
```

## [Calling Universal App](https://www.zetachain.com/docs/start/app#calling-universal-apps)s

用户可以通过与连接链上的**网关合约**交互来调用通用应用。

每个连接链都有一个单一的网关合约，该合约会暴露向通用应用存入和调用代币的方法。用户可以同时传递数据以及调用通用应用时的令牌。

例如:一名以太坊用户向一个通用应用发送了1个ETH和一条消息“hello”

![](https://ai.feishu.cn/space/api/box/stream/download/asynccode/?code=NzFjZDQ0MGY5ZTU2M2U3ZWYyZWI0OGRmMGMwOWU3YjBfVEViMjlHbjZseVBicmdLQ00ydFlmdXd2dFY3MVJ5WkRfVG9rZW46S29iMWI0aXVMb21HRmN4TkFJR2NYZllmbmliXzE3NjQxNjU5Njg6MTc2NDE2OTU2OF9WNA)

# Gateway

-   功能上，它是连接 ZetaChain 和外部区块链（如以太坊、币安链、Polygon）的关键接口。
    
-   本质上，Gateway 是**部署在连接链（Connected Chains）上的智能合约**。
    

ZetaChain 本身是一条链。以太坊（Ethereum）是另一条链。它们俩语言不通，无法直接对话。ZetaChain 开发团队**在以太坊上部署了一个智能合约**，这个合约就叫 **Gateway**。所有进出 ZetaChain 的资金和信息，都要经过这个合约。

_注：对于比特币这种不支持智能合约的链，Gateway 的形式是一个受 TSS 技术控制的托管地址，功能类似，但实现方式不同。_

## Gateway 的核心职责

Gateway 主要负责处理两个方向的流量：**进（Inbound）**和 **出（Outbound）**。

A. 资产“进”站（存款/充值）

当你想把以太坊上的 USDT 放到 ZetaChain 上用时：

1.  **用户操作：** 你把 USDT 发送到以太坊上的 **Gateway 合约地址**。
    
2.  **锁定/托管：** Gateway 收到你的钱。它把钱锁在合约里。这笔钱现在由 Gateway 保管。
    
3.  **发出信号：** Gateway 会在以太坊上生成一个“事件日志”（Event Log）。
    
4.  **ZetaChain 观察：** ZetaChain 的观察者节点（Observers）盯着 Gateway。它们看到了这个信号。
    
5.  **铸造资产：** ZetaChain 在自己的网络里，凭空印出等量的 **ZRC-20 USDT** 给你。
    

**简单说：** Gateway 在外部链上替你保管原版资产，作为凭证。

B. 资产“出”站（提现/支付）

当你在 ZetaChain 上操作完毕，想把 USDT 提回以太坊钱包时：

1.  **用户操作：** 你在 ZetaChain 上销毁（Burn）了你的 ZRC-20 USDT。
    
2.  **ZetaChain 决策：** ZetaChain 网络验证这笔交易。网络节点达成共识：“好的，我们要把钱还给他。”
    
3.  **TSS 签名：** ZetaChain 的节点们共同生成一个签名（私钥是切片保存的，没有任何一个人能单独控制）。
    
4.  **执行命令：** 这个签名被发送给以太坊上的 **Gateway 合约**。
    
5.  **释放资金：** Gateway 验证签名。确认是 ZetaChain 官方发来的指令。它从锁定的资金池里解锁 USDT，转回给你的以太坊钱包。
    

**简单说：** Gateway 听从 ZetaChain 网络的集体指令，释放资金。

C. 信息传递（跨链调用）

Gateway 不止管钱，还管数据。

-   **调用外部合约：** 你在 ZetaChain 上的 Universal App 可以发出指令。这个指令传给 Gateway。Gateway 再去调用以太坊上的 Uniswap 或 Aave 合约。
    
-   **传递普通信息：** 它可以把一段文本或数据从这条链传到那条链。
    

## 关键技术点理解：MPC-TSS（**Threshold Signature Scheme-门限签名方案）**

核心原理：私钥分片

传统的区块链钱包有一把**私钥**。这把私钥像一把物理钥匙。谁拿着它，谁就能打开保险柜（控制资金）。如果私钥丢了，钱就丢了。如果私钥被偷了，钱就被偷了。

TSS 改变了这个规则。

**TSS 的做法：**

-   系统不生成一把完整的私钥。
    
-   系统通过数学算法生成很多个\*\***“私钥碎片**”\*\*（Key Shares）。
    
-   系统把这些碎片分给不同的人（在 ZetaChain 里是验证节点）。
    
-   **关键点：** 完整的私钥从来没有出现过。连拿到碎片的人都不知道完整的私钥长什么样。
    

工作机制：门限（Threshold）

“门限”是一个数字。它代表“最少需要多少人同意”。

我们假设一个场景：

-   **总人数 (n)：** 100 个节点。
    
-   **门限值 (t)：** 67 个节点。
    

**运作流程：**

1.  有人发起一笔提款请求。
    
2.  节点们检查这笔请求是否合法。
    
3.  如果节点同意，它就用自己的“碎片”进行一次数学计算。
    
4.  当有 67 个节点完成了计算。
    
5.  系统把这些计算结果拼在一起。
    
6.  数学魔术发生了：这产生了一个**有效的签名**。
    
7.  区块链看到这个签名。它认为这是合法的。它执行交易。
    

如果只有 66 个人同意。或者黑客偷了 50 个碎片。签名无法生成。交易会失败。

TSS 和 多重签名 (Multisig) 的区别

你可能会把 TSS 和多重签名钱包混淆。它们看起来很像。它们都需要多人同意。但它们在技术上有很大不同。

**多重签名 (Multisig)：**

-   **发生在链上：** 区块链知道有 3 个人要签名。
    
-   **成本高：** 3 个人签名。区块链要存 **3 个签名**的数据。手续费（Gas）很贵。
    
-   **隐私差：** 所有人都能看到是哪 3 个地址签了名。
    

**TSS 签名：**

-   **发生在链下：** 节点们在私下里进行数学计算。
    
-   **成本低：** 无论 100 人还是 1000 人参与。最终生成的签名**只有一个**。它看起来像普通的单人签名。手续费很便宜。
    
-   **隐私好：** 没人知道具体是哪几个人签的。外界只看到结果。
    

为什么 ZetaChain 必须用 TSS？

ZetaChain 无法在比特币网络上部署智能合约。比特币不支持这个。ZetaChain 必须拥有一个比特币地址来保管用户的 BTC。

**问题来了：** 谁掌握这个比特币地址的私钥？

-   如果是 CEO 掌握。他可能卷款跑路。
    
-   如果是多重签名。比特币网络对签名数量有限制（通常最多十几个人）。ZetaChain 有几百个节点。多重签名做不到。
    

**TSS 是唯一的解决方案：**

-   它能让几百个节点共同管理一个比特币地址。
    
-   它不需要比特币网络升级或支持新功能。
    
-   它让比特币网络以为这只是一个普通的个人用户在操作。
    

安全性优势

**没有单点故障：** 你需要一把钥匙开门。钥匙断了就麻烦了。TSS 没有完整的钥匙。如果一个节点断网了。或者一个节点的硬盘坏了。这没关系。只要剩下的节点数量还够“门限值”。网络就能正常工作。

**刷新机制 (Key Rotation)：** TSS 支持一种叫“密钥刷新”的技术。节点们可以定期更新手里的碎片。虽然碎片变了，但它们组合出来的那个“虚拟公钥”地址不变。 这意味着黑客必须在**同一时间**攻破 67 个节点才能偷到钱。如果黑客今天攻破一个，明天攻破一个。因为碎片已经刷新了。旧碎片就作废了。这极大地提高了安全性。

# 架构图

这个很好描述了他们之间的关系xxx

![](https://ai.feishu.cn/space/api/box/stream/download/asynccode/?code=MGRiYWRlZWIyYTBkNDFhZWYzM2I4OGU4MDkyNzgwNjBfblVhRU12TER3RTIydHc5b0c3YzdQaVVndEViR3EyOFBfVG9rZW46U2hqYmJzTUdsb09ueHV4elJFRmNZZWl5blFlXzE3NjQxNjU5Njg6MTc2NDE2OTU2OF9WNA)

# 通用区块链 / Universal EVM /Omnichain Smart Contract

### 通用区块链 (Universal Blockchain)

**定位：基础设施层 / 状态机**

这是一个专门为跨链互操作性构建的 Layer 1 区块链。它不仅仅是一个传输信息的“中继器”。它维护着一个全局状态。这个状态包含了所有连接链（比特币、以太坊等）的账户和资产信息。

**核心技术机制：**

-   **去中心化观察者 (Decentralized Observers)：** ZetaChain 的验证节点运行着一种名为“观察者”的客户端。这些客户端实时扫描外部链（如 Bitcoin 内存池或 Ethereum 区块）。它们寻找特定的交易或事件。一旦发现，节点们会通过共识机制确认这个事件确实发生了。这解决了“读”的问题。
    
-   **TSS 门限签名 (Threshold Signature Scheme)：** 这是“写”的核心。区块链没有单一的私钥来控制外部资产。系统生成一个数学上的分布式私钥。这个私钥被切分成碎片。碎片分发给所有验证节点。只有超过 2/3 的节点同时也签署交易，外部链上的 Gateway 才会执行操作。这保证了没有任何单一实体能窃取资金。
    
-   **基于 Cosmos SDK 的共识：** 通用区块链通常基于 Cosmos SDK 构建。这提供了极快的区块确认速度（Instant Finality）。这对于处理高频的跨链交易非常关键。
    

### Universal EVM (通用以太坊虚拟机)

**定位：执行引擎层 / 兼容与扩展**

标准的 EVM 是一个封闭的沙盒。它只能修改自身链上的数据。Universal EVM 打破了这个沙盒。它在标准 EVM 的基础上增加了 **zEVM 模块**。

**核心技术特性：**

-   **ZRC-20 资产映射标准：** 这是 Universal EVM 的核心创新。当比特币或以太坊进入系统时，系统会自动创建一个 ZRC-20 代币合约。这个合约是外部原生资产在 ZetaChain 内部的“影子”。
    
    -   开发者调用 ZRC-20 合约的 `transfer` 函数。
        
    -   Universal EVM 不仅更新内部账本。
        
    -   它还会触发底层的 TSS 模块，在外部链上发起真实转账。
        
-   **同步执行环境：** 在传统跨链中，A 链和 B 链是异步的。你在 A 链发令，不知道 B 链什么时候执行。在 Universal EVM 中，操作是同步的。你在一个区块内就能完成“读取输入 -> 计算逻辑 -> 改变状态”的过程。
    
-   **Gas 费抽象 (Gas Abstraction)：** 在 Universal EVM 中运行合约时，系统会自动计算外部链需要的 Gas 费。用户可以用一种代币（如 ZETA）支付所有费用。系统后台会自动把这些费用兑换成 ETH 或 BTC，用来支付外部矿工费。
    

### Omnichain Smart Contract (全链智能合约)

**定位：业务逻辑层 / 统一编排**

这是部署在 Universal EVM 上的智能合约代码。它实现了“单点部署，全网运行”。

**与传统跨链合约的区别：**

-   **传统模型 (Bridge/Messaging)：** 你需要在以太坊写一个合约。你需要在币安链写一个合约。你还需要在两条链之间架设一个消息中继。这增加了攻击面。状态也是割裂的。
    
-   **全链模型 (Omnichain)：** 你只在 ZetaChain 上写**一个**合约。这个合约拥有“上帝视角”。
    
    -   **输入：** 它可以接收来自比特币网络的 UTXO 作为触发条件。
        
    -   **逻辑：** 它可以执行复杂的 DeFi 逻辑（如 AMM 交易、借贷清算）。
        
    -   **输出：** 它可以把结果资产分散发送到 Polygon 和 Solana。
        
-   **原子性与回滚 (Atomicity & Revert)：** 全链合约需要处理复杂的失败情况。如果外部链（如以太坊）的交易因为滑点过大失败了，全链合约包含了一套回滚机制。这保证了资金要么交易成功，要么原路退回。它不会卡在中间状态。
    

### 总结三者的协作流

1.  **通用区块链** 提供了物理连接和安全共识（眼睛和手）。
    
2.  **Universal EVM** 提供了一个能理解多链资产的计算环境（大脑）。
    
3.  **Omnichain Smart Contract** 告诉系统具体要做什么业务（思维逻辑）。
    

# Q&A

**Q：ZetaChain 决策与 TSS 签名它们两者的核心关系是什么？**

**A：** 它们是大脑**和手**的关系。

-   **ZetaChain 决策**是“大脑”。它负责判断。它决定这笔钱该不该转。
    
-   **TSS 签名**是“手”。它负责执行。它负责把钱实际转出去。 只有大脑发出了指令，手才会动。
    

**Q：在资产“出站”时，第一步发生了什么？**

**A：** 第一步是**决策（共识）**。 当你在 ZetaChain 上发起提现。你会先销毁（Burn）你手里的 ZRC-20 代币。 ZetaChain 的所有验证节点会检查这笔交易。 节点们会确认：“是的，他确实销毁了代币。他的操作是合法的。” 当超过 2/3 的节点都确认了这件事。这就形成了“ZetaChain 决策”。

**Q：TSS 签名是什么时候开始的？**

**A：** TSS 签名紧跟着决策之后开始。 一旦节点们达成了“决策”。它们就会拿出各自保存的私钥碎片。 它们开始进行多方计算（MPC）。 它们共同生成一个签名。这个签名就是给外部链（比如以太坊）看的通行证。

**Q：为什么不能跳过决策，直接签名？**

**A：** 因为没有节点拥有完整的私钥。 私钥被切分成了很多碎片。每个节点只有一片。 如果你想生成有效的签名。你需要大部分节点配合你。 如果网络没有达成“决策”。其他节点就不会配合你进行计算。 你就无法生成签名。你就无法动用 Gateway 里的资金。

**Q：Gateway 怎么知道这个签名是有效的？**

**A：** Gateway 不关心背后的决策过程。 Gateway 只认签名。 ZetaChain 生成的 TSS 签名。在数学上看起来和一个标准的以太坊签名完全一样。 Gateway 看到签名是正确的。它就会认为这是 ZetaChain 官方发来的指令。 它就会打开金库放行资金。

**Q：这一套机制解决了什么安全问题？**

**A：** 它解决了\*\*“内部作恶”\*\*的问题。 在传统的中心化交易所里。管理员一个人就能拿私钥卷款跑路。 在 ZetaChain 里。没有任何一个人、或者一个节点拥有完整的私钥。 想要把钱转走。必须通过全网的“决策”。 这保证了只有合法的交易才能让资金出站。
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->











**学习目标**

-   本地 / 云端完成基础开发环境落地。
    
-   能描述自己接下来 2 周的学习目标。
    

**学习资料**

-   ZetaChain CLI
    
-   [https://github.com/zeta-chain/cli](https://github.com/zeta-chain/cli)
    
-   ZetaChain Node / RPC / Faucet / Explorer / 测试币获取
    
-   [https://www.zetachain.com/docs/reference/](https://www.zetachain.com/docs/reference/)
    
-   Qwen API 参考（含 OpenAI 兼容方式）
    
-   [https://www.alibabacloud.com/help/zh/model-studio/qwen-api-reference](https://www.alibabacloud.com/help/zh/model-studio/qwen-api-reference)
    

**扩展资料（可选）**

-   再快速浏览一遍 ZetaChain Developers
    
-   [https://www.zetachain.com/docs/developers](https://www.zetachain.com/docs/developers)
    

**实践 / 作业**

-   安装或尝试使用 ZetaChain CLI（本地或云端环境均可）。
    
-   了解测试网 RPC、Faucet、Explorer 的入口，记录在自己的笔记中。
    
-   在终端或 Postman 里完成一次 Qwen API 的简单请求（哪怕只是 200 报错也可以，先打通连通性）。
    

# 1.安装ZetaChain CLI

## ZetaChain CLI

ZetaChain CLI：用于构建和交互 [ZetaChain](https://www.zetachain.com/) 通用应用的命令行接口。它是开发者、节点验证者（Validators）和高级用户用来“控制”和“查询”ZetaChain 网络的“遥控器”。由于 ZetaChain 是基于 Cosmos SDK 构建的，其 CLI 的操作逻辑与大多数 Cosmos 生态的链非常相似。

### 核心组件：`zetacored`

ZetaChain 的主要二进制文件通常被称为 `zetacored`。它集成了运行节点和作为客户端交互的所有功能。

### 主要功能模块

ZetaChain CLI 的功能主要分为以下几大类：

A. 钱包与密钥管理 (Key Management)

用于生成和管理账户。

-   **创建账户：** 生成新的助记词和私钥。
    
-   **恢复账户：** 导入助记词。
    
-   **查看地址：** 获取你的 ZetaChain 地址（通常以 `zeta` 开头）。
    
-   _命令示例：_ `zetacored keys add mywallet`
    

B. 查询网络状态 (Querying)

用于从区块链上获取只读信息，不需要消耗 Gas 费。

-   **查余额：** 查看 ZETA 代币余额。
    
-   **查交易：** 追踪跨链交易的状态。
    
-   **查区块信息：** 查看当前区块高度。
    
-   **查跨链信息：** 查询 ZetaChain 观察到的比特币或以太坊网络的状态（这是 ZetaChain 特有的，涉及 `observer` 模块）。
    
-   _命令示例：_ `zetacored query bank balances [地址]`
    

C. 发送交易 (Transactions)

用于向网络写入数据，需要消耗 ZETA 作为 Gas 费。

-   **转账：** 发送 ZETA 代币给其他人。
    
-   **质押 (Staking)：** 将代币质押给验证者以获得奖励。
    
-   **治理 (Governance)：** 对网络提案进行投票。
    
-   **部署合约：** 尽管通常使用 Hardhat/Foundry 部署 EVM 合约，但 CLI 也可用于底层交互。
    
-   _命令示例：_ `zetacored tx bank send [发送者] [接收者] 1000000azeta`
    

D. 节点运营 (Node Operations)

这是验证者使用的功能。

-   **启动节点：** 运行全节点或验证者节点，同步区块数据。
    
-   **初始化：** 配置创世文件 (genesis.json)。
    
-   _命令示例：_ `zetacored start`
    

## 先决条件

-   Node.js ≥ 18
    
-   Git（用于模板克隆）
    
-   （可选）Docker ≥ 24 for`localnet`
    

## 本地运行

```PowerShell
npx zetachain@next new
```

## 全局安装

```PowerShell
npm install -g zetachain@latest
```

## 验证

```PowerShell
zetachain query chains list
// 此命令会打印出当前连接到 ZetaChain 的所有链，即成功
```

![](https://av1ln7qpa6l.feishu.cn/space/api/box/stream/download/asynccode/?code=NjVhODdjZGNjOWM0NmQ1NTJlZDBhMjkwNmJjNGFjNmVfbWwzMWIwV1p5N3dsdm41dkVmbHhkQUNDd3dCbHpCWVJfVG9rZW46TmlNWmJuQm96b2U5TjZ4YVJnV2N1ZmkwbndkXzE3NjQwNjQ2ODQ6MTc2NDA2ODI4NF9WNA)

## Example

创建一个新项目：

```Plain
zetachain new
```

启动localnet：

```Plain
zetachain localnet start
```

![](https://av1ln7qpa6l.feishu.cn/space/api/box/stream/download/asynccode/?code=NDVmMTc3MDRiMDg4Mzc5MWY2OTA5NTE3YmUwNmUyOGVfZFo1UHEwUTVxMHpEVk5TV09RQWZ5MFAzT29DUFdudWpfVG9rZW46U1lGYWJQZHZwb0J6WDR4WGdZT2NJNUowblJnXzE3NjQwNjQ2ODQ6MTc2NDA2ODI4NF9WNA)

查询跨链余额：

```Plain
zetachain query balances --evm <钱包地址>
```

注意：要有钱包（无则创建： npx zetachain accounts create --name <name> ）

![](https://av1ln7qpa6l.feishu.cn/space/api/box/stream/download/asynccode/?code=YmEwOWNmODg0MGQ5NGJlNGE3NDUxNjBhZDA2ZmZkYjNfR0M3OWh0ZkJTS1pqRkZZSTRyMDhTdG5oVU5Dcnh5dzNfVG9rZW46VUx5VmJveWllb3M5NXp4SlYwbWNuM09IbmNoXzE3NjQwNjQ2ODQ6MTc2NDA2ODI4NF9WNA)

## CLI参考

完整指挥文件：

[https://www.zetachain.com/docs/reference/cli/](https://www.zetachain.com/docs/reference/cli/)

```Plain
zetachain docs
```

或者配合任何命令使用：`--help`

```Plain
zetachain accounts --help
```

# 2.项目前置准备

1\. Faucet领取测试币：

[https://www.zetachain.com/docs/reference/faucet/](https://www.zetachain.com/docs/reference/faucet/)

[https://zetachain.faucetme.pro/](https://zetachain.faucetme.pro/)

![](https://av1ln7qpa6l.feishu.cn/space/api/box/stream/download/asynccode/?code=OTY1Yjc5MzVlMzlmYjRhODQ2Y2FjYmE3YWE3M2QyYWFfcmdOdDg2dkI4bDljWHJqVjlDNVZsRnNQbHp1cHE3cktfVG9rZW46VVk3amJzQnEzb0R3MEJ4U0JDQmNiOEtjbmpoXzE3NjQwNjQ2ODQ6MTc2NDA2ODI4NF9WNA)

2.主网和测试网：

[https://www.zetachain.com/docs/developers/chains/list/](https://www.zetachain.com/docs/developers/chains/list/)

3.RPC 节点：

[https://www.zetachain.com/docs/reference/network/api](https://www.zetachain.com/docs/reference/network/api)

使用allthatnode:

[https://www.allthatnode.com/project.dsrv?seq=1c82ed14a14d215be82f31de6e1027359381cdbc](https://www.allthatnode.com/project.dsrv?seq=1c82ed14a14d215be82f31de6e1027359381cdbc)

![](https://av1ln7qpa6l.feishu.cn/space/api/box/stream/download/asynccode/?code=MzRlOTRmMTZkNDU3ZDI1NjkwN2RkZmMzMGZhMDY4MWFfbDA1RkZIang2R3VLc3ZubXR4S2JkbEt6YWdTdld1c2NfVG9rZW46RnZXeGJqQXJtb0trVVF4WHFwbmN6eTN5bjRkXzE3NjQwNjQ2ODQ6MTc2NDA2ODI4NF9WNA)

# 3.回顾一下RPC（**Remote Procedure Call**（远程过程调用））

RPC (Remote Procedure Call，远程过程调用)是一种进程间通信 (IPC)技术。

它允许一台计算机上的程序（客户端）调用另一台计算机（服务器）上的子程序或函数，而程序员无需显式编写远程交互的底层网络代码。

在区块链（特别是 ZetaChain 这种基于 Cosmos SDK 且兼容 EVM 的架构）的上下文中，RPC 的技术定义和作用如下：

### 1\. 技术定义：区块链节点的 API 层

区块链网络由分布式的**\*\*节点 (Nodes)\*\*** 组成。这些节点维护着账本的\*\*状态 (State)\*\*（如账户余额、合约代码、存储槽数据）。节点通常运行在一个隔离的环境中。

RPC 接口是节点软件暴露给外部世界的\*\***标准 API 端点**\*\*。它使得外部客户端（如您的 ZetaChain CLI、DApp 前端、索引器）能够与节点进行通信。

从架构上看，它遵循 **Client-Server 模型**：

-   **Client (Stub):** 您的开发环境。它将本地的方法调用（如 `query balances`）打包（序列化）成网络请求。
    
-   **Transport:** 数据包通过网络协议（通常是 HTTP 或 WebSocket）传输。
    
-   **Server (Node):** 区块链节点接收请求，解包，在本地状态机中执行查询或将交易注入内存池 (Mempool)，然后将结果序列化返回。
    

### 2\. ZetaChain 中的双重 RPC 架构（关键专业知识）

由于 ZetaChain 是基于 Cosmos SDK 构建并兼容 EVM（以太坊虚拟机）的，它的 RPC 架构比普通公链更复杂，通常包含两层：

A. JSON-RPC (EVM 层)

这是以太坊生态的标准协议。当您使用 `npm install -g zetachain` 安装的 JS CLI 开发智能合约时，主要交互的是这一层。

-   **协议标准：** JSON-RPC 2.0 (无状态、轻量级)。
    
-   **数据格式：** JSON。
    
-   **典型端口：** 8545。
    
-   **主要用途：**
    
    -   部署 Solidity 合约 (`eth_sendRawTransaction`)。
        
    -   调用合约方法 (`eth_call`)。
        
    -   获取 EVM 层的区块和交易信息。
        
-   **示例 Payload：**
    
    ```JSON
    {
      "jsonrpc": "2.0",
      "method": "eth_getBalance",
      "params": ["0xYourAddress", "latest"],
      "id": 1
    }
    ```
    

B. gRPC & REST (Cosmos SDK 层)

这是 ZetaChain 底层共识和跨链模块的通信协议。当您涉及质押 (Staking)、治理或底层跨链观察者 (Observer) 数据时，会用到这一层。

-   **gRPC:** 基于 HTTP/2，使用 Protocol Buffers (Protobuf) 进行二进制序列化。性能极高，延迟低。主要用于节点间通信或高性能客户端（如 `zetacored` Go CLI）。
    
    -   _典型端口：9090_
        
-   **REST (LCD):** 将 gRPC 封装为 HTTP 接口，方便网页端调用。
    
    -   _典型端口：1317_
        

### 3\. 开发中的关键概念

在专业开发中，理解 RPC 意味着你需要关注以下指标：

1.  **Endpoint (端点 URL):**
    

1.  这是 RPC 服务的入口地址。
    
    -   _Public RPC:_ 官方或社区提供的免费节点（如您在教程中自动连接的）。通常有\*\*速率限制 (Rate Limit)\*\*，比如每秒只能请求 10 次。
        
    -   _Private RPC:_ 使用 Infura、Alchemy 或 BlockPi 等基础设施服务商提供的专用节点，高并发、更稳定。
        

2.  **WebSockets (WSS) vs HTTP:**
    
    1.  **HTTP:** 它是“拉”模式 (Pull)。您的 CLI 必须不断询问“交易确认了吗？”
        
    2.  **WebSockets:** 它是“推”模式 (Push)。您可以建立长连接，订阅事件（如 `logs`），一旦链上有特定事件发生，节点会主动通过 RPC 推送消息给您的程序。
        
3.  **状态最终性 (Finality):**
    

1.  通过 RPC 查询到的数据取决于节点的同步状态。如果连接的 RPC 节点落后于网络（未同步），您获取的数据就是旧的。
    

### 总结

在您的开发流程中：

**RPC 是将您的业务逻辑（JavaScript/TypeScript 代码）与去中心化网络状态（ZetaChain 区块链数据）连接起来的通信协议和网关。**

当您执行 `zetachain query` 时，您实际上是在构造一个 **HTTP 请求**，发送给远程服务器上的 **JSON-RPC 接口**，服务器查询其本地的 **LevelDB/RocksDB 数据库**，然后将十六进制的结果返回给您并进行解码。

# 完成一次 Qwen API 的简单请求

看到群友“菜鸟Appler-R"的调用，重现了一下hhh

![](https://av1ln7qpa6l.feishu.cn/space/api/box/stream/download/asynccode/?code=OGU4YTRmNGNkMzVkMWJiZWYwMTQxNTg1ZTIyZGM3MGJfQVVnajhMUTdtejF0RklUVDZyeHdEQWlZbTJMN0R6R2lfVG9rZW46Tm5wTmJNNnJLb2V4d014UlV5VmNKNTRObkJjXzE3NjQwNjQ2ODQ6MTc2NDA2ODI4NF9WNA)
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->












# DAY1

**学习资料**

-   ZetaChain 总文档
    
-   [https://www.zetachain.com/docs/](https://www.zetachain.com/docs/)
    
-   ZetaChain Developers（入口总览）
    
-   [https://www.zetachain.com/docs/developers](https://www.zetachain.com/docs/developers)
    
-   Qwen 总文档（Quickstart / Key Concepts）
    
-   [https://qwen.readthedocs.io/zh-cn/latest/](https://qwen.readthedocs.io/zh-cn/latest/)
    

# 什么是ZetaChain

目标：不同链之间互联，能够原生访问任何区块链，让加密货币变得像互联网一样易于访问、多样化和互联

ZetaChain 是一个赋能开发者创建通用，能够无缝交互任何区块链的应用。

官网的developer界面

![](https://av1ln7qpa6l.feishu.cn/space/api/box/stream/download/asynccode/?code=ODYzZjU5M2FjMDgyOGIzZTFmMWU5MDMwYzYyZjZmZTlfNFZydGwwZDF0SEIxSzA0VEpPOWJJOTFxWVR2VnQ3MjNfVG9rZW46WFU2ZmJoQ0xmb05lcUd4OGZoWmN5ano1blFlXzE3NjM5Njk2MzM6MTc2Mzk3MzIzM19WNA)

# 环境搭建

## 先决条件

-   [**Node.js**](https://nodejs.org/en)**（推荐v21或更晚版本）：**必须 运行 ZetaChain CLI 并管理 JavaScript 依赖。(node -v)
    
-   **Yarn或者NPM：**用于安装和 更新项目库。两种方法都可以，随你喜欢。(npm version)
    
-   [**Git**](https://git-scm.com/)**:**管理项目资源必不可少 编程和与他人协作。
    
-   [**JQ**](https://jqlang.org/)**:**一个轻量级命令行JSON处理器， 对于解析Localnet输出和编写shell脚本非常有用。(win bash:winget install jqlang.jq)
    
-   [**Foundry**](https://getfoundry.sh/)**:**一个快速、模块化的以太坊工具包 发展。你会用它来编译契约、管理依赖和运行 Localnet。
    

### Windows

在 Windows 上安装 Foundry 需要一些额外的步骤，因为`foundryup` 目前不支持 PowerShell 或命令提示符 (Cmd)。您需要使用 Windows Subsystem for Linux (WSL) 或 Git BASH 来进行安装。\[[1](https://www.google.com/url?sa=E&q=https%3A%2F%2Fvertexaisearch.cloud.google.com%2Fgrounding-api-redirect%2FAUZIYQF7zCQXvNYyf1lIuwj_SykCNnDx-Fs34VfoYyf3ctjVjTHBXDt9T0oqAn542wsnl088smS_Lj9n5l0cI11E24eFj4qehO9A6AVp_pYN5L5ge9jDRaDvVX27gbPWQ6fqk1C7PraRa8LPv4_aAiap)\]\[[2](https://www.google.com/url?sa=E&q=https%3A%2F%2Fvertexaisearch.cloud.google.com%2Fgrounding-api-redirect%2FAUZIYQGzBjoCSkl_rKeTK2BbapcE-MR8_4zguUZ9YJcyYxxAba1j2J8NNYf7Ngy3woDdVyZklosYbRjydKx1rmO2L3EchMCOQmvlb60R0ca6L4t2ptSATdjLG3pCcg9-kkO_oWYGTzb3fsmBgw9C8qVaXox6pr0W8S2782AXTvGgkg7KxFeIBh6SWdN_l4YBnA%3D%3D)\]

**步骤 1：安装 Windows Subsystem for Linux (WSL)**

1.  打开 PowerShell 或 Windows 命令提示符，并以管理员身份运行。
    
2.  输入以下命令来安装 WSL：
    
3.  codeBash
    

```Plain
wsl --install
```

**步骤 2：在 WSL 中安装 Foundry**

1.  打开您的 WSL 终端 (例如 Ubuntu)。
    
2.  运行与 macOS 和 Linux 相同的 `curl` 命令来安装 `foundryup`：
    

```Plain
curl -L https://foundry.paradigm.xyz | bash
```

1.  按照屏幕上的指示完成安装，然后运行 `foundryup` 来安装 Foundry 工具链：
    

```Plain
foundryup
```

## 环境建设

Install the ZetaChain CLI:

```Shell
npm install -g zetachain
```

Run:

```Shell
zetachain query chains list
```

这个命令可以打印所有当前连接在ZetaChain上的链条

对于本地开发，推荐运行Localnet，一个自给自足的平台。该环境包括ZetaChain、EVM、Solana、Sui和TON。Localnet 是构建和测试通用应用最快的方法，无需依赖公共测试网。

注意：必须要有Foundry才可以运行Localnet

# qwen账号注册和进入控制台

1.进入阿里云百炼，注册，创建API key即可

[https://bailian.console.aliyun.com/?tab=model#/model-market/detail/qwen3-max](https://bailian.console.aliyun.com/?tab=model#/model-market/detail/qwen3-max)
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
