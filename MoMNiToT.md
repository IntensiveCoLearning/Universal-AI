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
# 2025-12-07
<!-- DAILY_CHECKIN_2025-12-07_START -->
### **一、团队分工 & 黑客松每日里程碑**

**1\. 团队分工（按 5 人团队配置）**

| 角色 | 负责模块 | 核心职责 |
| --- | --- | --- |
| 合约开发 | 跨链合约层 | 复用 ZetaChain Swap 合约模板，适配多链资产映射，部署合约并提供调用接口 |
| 后端开发 | 接口集成 & 数据层 | 对接 ZetaChain API（资产查询、CCTX 状态、手续费计算），封装 Agent 调用工具 |
| Agent 开发 | Qwen-Agent 适配 | 意图识别、自然语言解析、工具调用逻辑、用户流程引导、交易状态反馈 |
| 前端开发 | 用户交互层 | 极简 Web 界面（需求输入框、参数确认页、交易进度展示），对接 Agent 和后端接口 |
| Pitch 负责人 | 演示设计 & 项目讲解 | 梳理 Demo 流程、准备话术、配合前端调试演示效果、黑客松最终路演 |

**2\. 黑客松 3 日里程碑（Day14-Day16）**

| 日期 | 核心里程碑 |
| --- | --- |
| Day14（搭建期） | 1. 合约：完成 ZetaChain Swap 合约复用与部署，验证跨链资产兑换基础功能；2. Agent：注册 ZetaChain 查询工具，实现需求解析→参数提取核心逻辑；3. 前端：完成极简界面原型（输入框、确认页、状态页） |
| Day15（集成期） | 1. 端到端打通：AI 解析→后端查询→合约调用全流程联调；2. 测试验证：完成 1-2 组跨链测试（如 ETH→Solana、BSC→Avalanche），修复数据同步、参数传递 bug；3. 状态反馈：实现交易进度实时查询与 AI 推送 |
| Day16（优化期） | 1. 体验优化：简化操作流程，修复界面卡顿、参数展示不清晰等问题；2. Demo 彩排：完整走通演示流程，控制时间在 3 分钟内；3. Pitch 准备：梳理项目价值、技术亮点，配合演示话术 |

### **二、1-3 分钟 Demo 脚本草稿**

**开场（5 秒）**

“大家好！我是 ZetaAI Swap 的产品负责人 —— 我们解决的核心问题是：**跨链兑换太复杂，新手望而却步**。而今天，我们让跨链兑换像聊天一样简单。”

**步骤 1：自然语言输入需求（30 秒）**

-   （屏幕展示：前端界面，左侧是输入框，右侧是 AI 对话区）
    
-   演示者：“假设我是一个 DeFi 新手，我想把以太坊上的 0.1 ETH 换成 Solana 的 USDT，但我根本不懂什么是跨链桥、ZRC-20。没关系，我直接在输入框里说我的需求：‘把以太坊上的 0.1 ETH 换成 Solana 的 USDT’。”
    
-   （点击 “提交”，AI 快速回复）
    
-   标注：【Qwen-Agent 发力点】—— 实时解析自然语言，提取核心参数（源链：Ethereum、资产：ETH、金额：0.1、目标链：Solana、资产：USDT），无需用户手动填写复杂表单。
    

**步骤 2：AI 生成最优方案，用户确认（40 秒）**

-   （屏幕展示：AI 回复界面，清晰列出兑换详情）
    
-   演示者：“看！AI 立刻帮我整理了所有关键信息：源资产是以太坊 ETH，目标资产是 Solana USDT，实时兑换汇率是 1 ETH = 1800 USDT，跨链手续费仅 0.002 ETH，预计到账时间 3 分钟。这些数据都来自 ZetaChain 的官方接口，安全可信。”
    
-   （点击 “确认兑换” 按钮）
    
-   标注：【ZetaChain 发力点】—— AI 调用 ZetaChain API，查询支持的 ZRC-20 资产映射（ETH.ETH、USDT.SOL）、 liquidity 池状态、手续费（withdrawGasFee），确保兑换路径可行且最优；【Qwen-Agent 发力点】—— 用通俗语言呈现专业数据，降低用户决策门槛。
    

**步骤 3：一键执行，实时查看进度（30 秒）**

-   （屏幕展示：交易进度页，显示 “资产已锁定→兑换中→已到账”）
    
-   演示者：“点击确认后，不需要我做任何额外操作 ——ZetaChain 会自动完成‘以太坊资产上链→ZRC-20 Swap 兑换→Solana 链下账’全流程。现在看进度条，已经显示‘已到账’，我打开 Solana 钱包，确实收到了 180 USDT！”
    
-   标注：【ZetaChain 发力点】—— 基于 Deposit-and-Call 机制，触发跨链合约自动执行，无需用户手动调用多步合约；通过 CCTX（Cross-Chain Transaction）实时反馈交易状态，确保流程透明；【Qwen-Agent 发力点】—— 用自然语言推送进度（“你的资产已锁定，正在跨链传输～”“兑换成功！USDT 已到账 Solana 钱包”），提升用户体验。
    

**收尾（15 秒）**

“这就是 ZetaAI Swap 的核心价值：Qwen-Agent 解决‘懂用户’的问题，ZetaChain 解决‘跨链行’的问题，两者结合，让任何人都能轻松实现跨链资产兑换。谢谢大家！”
<!-- DAILY_CHECKIN_2025-12-07_END -->

# 2025-12-06
<!-- DAILY_CHECKIN_2025-12-06_START -->

# **项目概要：跨链智能资产兑换助手（Cross-Chain AI Swap Assistant）**

## **一、项目名称**

跨链智能资产兑换助手（暂定名：ZetaAI Swap）

## **二、目标用户 / 场景**

### **目标用户**

-   跨链 DeFi 初学者：不熟悉多链资产映射规则、跨链手续费计算的新手用户；
    
-   高频跨链交易者：需要在不同公链（如 Ethereum、Solana、Bitcoin）间快速切换资产的用户；
    
-   低门槛资产配置者：希望通过简单操作实现多链资产分散配置的普通用户。
    

### **核心场景**

-   用户持有 A 链资产（如 Ethereum 上的 USDC），希望兑换为 B 链资产（如 Bitcoin 上的 BTC），无需手动处理跨链桥、 liquidity 池选择、手续费换算等复杂操作；
    
-   用户不确定不同链上资产的兑换汇率、手续费成本，需要 AI 实时计算最优兑换路径；
    
-   非技术用户希望通过自然语言指令（如 “把 0.5 个 ETH 换成 Solana 上的 USDT”）完成跨链兑换，无需编写代码或操作合约。
    

## **三、关键功能（3 点核心）**

1.  自然语言交互解析：用户通过文字描述跨链兑换需求（如 “从 BSC 转 100 USDT 到 Avalanche”），AI 自动提取关键信息（源链、目标链、资产类型、金额），生成标准化兑换参数；
    
2.  智能兑换路径规划：基于 ZetaChain 的 ZRC-20 资产映射规则，自动查询支持的跨链资产、liquidity 池状态，计算最优兑换路径（含手续费预估、兑换汇率），并提示用户确认；
    
3.  一键跨链执行：集成 ZetaChain Swap 合约逻辑，用户确认后自动触发跨链兑换交易（含资产 Deposit、ZRC-20swap、目标链 Withdraw 全流程），无需手动调用多步合约。
    

## **四、技术路线（ZetaChain + Qwen 配合）**

### **核心技术栈**

-   跨链底层：ZetaChain（ZRC-20 资产映射、跨链 Swap 合约、Gateway 网关）；
    
-   AI 能力：Qwen-Agent（自然语言解析、用户意图识别、兑换参数生成、流程引导）；
    
-   合约层：复用 ZetaChain Swap 合约模板，适配多链资产兑换逻辑；
    
-   前端交互：简化版 Web 界面（聚焦兑换需求输入、参数确认、交易状态查询）。
    

### **协作逻辑**

1.  用户层：通过自然语言向 Qwen-Agent 输入跨链兑换需求（如 “把以太坊的 USDC 换成 Solana 的 SOL”）；
    
2.  AI 处理层：Qwen-Agent 调用自定义工具（基于 ZetaChain API），完成 3 件事：
    
    -   解析需求：提取源链、目标链、资产类型、金额等关键参数；
        
    -   验证可行性：查询 ZetaChain 支持的 ZRC-20 资产列表（如 USDC.ETH、SOL.SOL 的映射地址）、liquidity 池容量；
        
    -   计算成本：调用 ZetaChain 接口获取跨链手续费（withdrawGasFee）、实时兑换汇率；
        
3.  跨链执行层：Qwen-Agent 将标准化参数（源资产 ZRC-20 地址、目标资产 ZRC-20 地址、 recipient 地址等）传入 ZetaChain Swap 合约，触发 Deposit-and-Call 流程，自动完成 “资产上链→swap→下链” 全流程；
    
4.  反馈层：AI 实时查询跨链交易状态（CCTX），向用户推送进度（如 “资产已锁定→兑换中→已到账”）。
    

## **五、计划复用的 Demo / 模板**

1.  ZetaChain Swap 合约模板：复用官方跨链兑换合约逻辑（onCall 函数、ZRC-20 swap、Gateway withdraw 流程），减少合约开发工作量；
    
2.  ZetaChain 资产映射表：直接使用官方支持的 ZRC-20 资产列表（如 ETH.ETH、USDC.SOL 等），无需额外适配多链资产；
    
3.  Qwen-Agent 工具扩展模板：复用自定义工具注册逻辑（如示例中的 my\_image\_gen），开发 “ZetaChain 跨链查询工具”（查询支持资产、手续费、交易状态）；
    
4.  官方跨链交易 Demo：参考 Base→Ethereum、Solana→Ethereum 的兑换示例，适配多链间的交易参数传递逻辑。
<!-- DAILY_CHECKIN_2025-12-06_END -->

# 2025-12-05
<!-- DAILY_CHECKIN_2025-12-05_START -->


正在重新跑通Demo，但是并未成功

![屏幕截图 2025-12-04 211444.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/MoMNiToT/images/2025-12-05-1764945128448-_____2025-12-04_211444.png)
<!-- DAILY_CHECKIN_2025-12-05_END -->

# 2025-12-03
<!-- DAILY_CHECKIN_2025-12-03_START -->



# **Day10：DeFi 意图解析**

## **函数调用**

**函数调用是什么？**

-   让大语言模型（如 Qwen3）能够调用外部函数/工具的能力
    
-   解决 LLM 的局限性：缺乏实时信息、数学计算不精确等
    
-   建立 LLM 与外部系统之间的标准化交互协议
    

**工作流程**

1.  **提供函数说明**：应用程序向模型描述可用的函数及其参数
    
2.  **模型决策**：模型分析用户查询，决定是否/如何调用函数
    
3.  **执行函数**：应用程序执行模型选择的函数
    
4.  **返回结果**：将执行结果返回给模型
    
5.  **生成最终回复**：模型基于结果生成最终答案
    

**两种实现方式**

1.  **Qwen-Agent**（高级框架）
    

-   封装了函数调用逻辑
    
-   支持思考模式（reasoning）和非思考模式
    
-   返回结构化的函数调用信息
    

2.  **vLLM**（推理部署库）
    

-   通过 OpenAI 兼容 API 实现
    
-   自动解析工具调用
    
-   需要手动处理函数调用和结果的关联
    

## **Qwen-Agent**

```
 import json5
 from qwen_agent.agents import Assistant
 from qwen_agent.tools.base import BaseTool, register_tool
 from qwen_agent.utils.output_beautify import typewriter_print
 from typing import Dict, Any, Optional
 ​
 @register_tool('parse_swap_intent')
 class DefiSwapIntentParser(BaseTool):
     description = '从用户自然语言中解析DeFi代币交换意图，提取链名、输入代币、输出代币和金额等参数'
     parameters = [{
         'name':'user_input',
         'type':'string',
         'description':'用户输入的自然语言文本，包含代币交换意图',
         'required': True
     }]
     def call(self,params:str,**kwargs) -> str:
         params_dict = json5.loads(params)
         user_input = params_dict['user_input']
         result = self._parse_intent_with_rules(user_input)
         return json5.dumps(
            {
                 'success': True,
                 'parsed_intent': result,
                 'message': '成功解析DeFi意图'
             },
             ensure_ascii=False)
     def _parse_intent_with_rules(self, text: str) -> Dict[str, Any]:
         """
         使用规则匹配解析意图
         """
         import re
         
         # 初始化结果
         result = {
             'chain': 'ethereum',  # 默认链
             'token_in': '',
             'token_out': '',
             'amount': '',
             'amount_type': 'exact',
             'operation': 'swap'
         }
         
         # 链名映射
         chain_keywords = {
             'base': 'base',
             'polygon': 'polygon',
             '以太坊': 'ethereum',
             'ethereum': 'ethereum',
             'bsc': 'bsc',
             'arbitrum': 'arbitrum',
             'optimism': 'optimism'
         }
         
         # 代币映射
         token_mapping = {
             'u': 'USDT',
             'usdt': 'USDT',
             'usdc': 'USDC',
             'eth': 'ETH',
             'matic': 'MATIC',
             'weth': 'WETH',
             'bnb': 'BNB'
         }
         
         text_lower = text.lower()
 ​
         # 解析链名
         for keyword, chain in chain_keywords.items():
             if keyword in text_lower:
                 result['chain'] = chain
                 break
         
         # 解析金额 - 使用正则表达式匹配数字
         amount_match = re.search(r'(\d+(?:\.\d+)?)\s*', text)
         if amount_match:
             result['amount'] = amount_match.group(1)
         
         # 检查"全部"、"所有"等关键词
         if '全部' in text or '所有' in text or 'all' in text_lower:
             result['amount_type'] = 'all'
         
         # 解析代币 - 查找代币关键词
         tokens_found = []
         for token_keyword, token_symbol in token_mapping.items():
             if token_keyword in text_lower:
                 tokens_found.append(token_symbol)
 ​
         # 简单逻辑：假设第一个找到的代币是输入，第二个是输出
         # 或者根据"换成"、"兑换成"等关键词判断
         if '换成' in text or '兑换成' in text or '兑换为' in text:
             # 找到"换成"前后的代币
             tokens_in_context = re.split(r'换成|兑换成|兑换为', text_lower)
             if len(tokens_in_context) >= 2:
                 # 在第一部分找输入代币
                 for token_keyword, token_symbol in token_mapping.items():
                     if token_keyword in tokens_in_context[0]:
                         result['token_in'] = token_symbol
                         break
                 
                 # 在第二部分找输出代币
                 for token_keyword, token_symbol in token_mapping.items():
                     if token_keyword in tokens_in_context[1]:
                         result['token_out'] = token_symbol
                         break
         
         # 如果上面的逻辑没找到，尝试其他模式
         if not result['token_in'] or not result['token_out']:
             # 尝试"用...换..."模式
             if '用' in text and '换' in text:
                 match = re.search(r'用\s*(\d+)?\s*([a-zA-Z]+)\s*换\s*([a-zA-Z]+)', text_lower)
                 if match:
                     if not result['amount'] and match.group(1):
                         result['amount'] = match.group(1)
                     
                     # 处理输入代币
                     input_token = match.group(2).upper()
                     result['token_in'] = token_mapping.get(input_token.lower(), input_token)
                     
                     # 处理输出代币
                     output_token = match.group(3).upper()
                     result['token_out'] = token_mapping.get(output_token.lower(), output_token)
         
         # 最后的回退逻辑：使用找到的代币列表
         if not result['token_in'] and len(tokens_found) >= 1:
             result['token_in'] = tokens_found[0]
         if not result['token_out'] and len(tokens_found) >= 2:
             result['token_out'] = tokens_found[1]
         
         return result
 ​
 ​
 llm_cfg = {
     'model':'qwen-max-latest',
     'model_type':'qwen_dashscope',
     'generate_cfg':{
         'top_p':0.8
     }
 }
 ​
 system_instruction = '''你是一个DeFi（去中心化金融）智能助手，专门帮助用户解析代币交换意图。
 ​
 你的任务是：
 1. 分析用户输入的文本，理解用户的DeFi操作意图
 2. 调用 parse_swap_intent 工具来解析用户的意图
 3. 将解析结果以清晰的结构化格式展示给用户
 4. 根据解析结果，提供下一步操作建议
 ​
 常见的DeFi操作包括：
 - 代币交换（swap）：将一种代币换成另一种代币
 - 流动性提供（liquidity provision）
 - 质押（staking）
 ​
 请用中文回复用户，并以友好的方式展示解析结果。'''
 ​
 # 可用工具列表
 tools = ['parse_swap_intent']
 ​
 bot = Assistant(
     llm=llm_cfg,
     system_message=system_instruction,
     function_list=tools
 )
 ​
 messages=[]
 while True:
     query = input('\n用户请求：')
     messages.append({'role': 'user', 'content': query})
     response = []
     response_plain_text = ''
     print('\nAgent Anwer：')
     for response in bot.run(messages=messages):
         response_plain_text = typewriter_print(response, response_plain_text)
     messages.extend(response)
```

生成结果：

```
 用户请求：把我 50 U 兑换成 Polygon 上的 MATIC
 ​
 Agent Anwer：
 2025-12-03 16:25:54,605 - base.py - 780 - INFO - ALL tokens: 272, Available tokens: 57855
 [TOOL_CALL] parse_swap_intent
 {"user_input": "把我 50 U 兑换成 Polygon 上的 MATIC"}
 [TOOL_RESPONSE] parse_swap_intent
 {success: true, parsed_intent: {chain: "polygon", token_in: "USDT", token_out: "MATIC", amount: "50", amount_type: "exact", operation: "swap"}, message: "成功解析DeFi意图"}2025-12-03 16:25:58,002 - base.py - 780 - INFO - ALL tokens: 357, Available tokens: 57855
 ​
 [ANSWER]
 我已成功解析了您的DeFi操作意图，以下是详细信息：
 ​
 - **操作类型**: 代币交换 (swap)
 - **区块链网络**: Polygon
 - **输入代币**: USDT (假设您指的是 USDT，通常简称为 "U")
 - **输出代币**: MATIC
 - **交易金额**: 50 USDT
 ​
 ### 下一步建议
 1. 确保您的钱包已连接到 Polygon 网络。
 2. 检查钱包中是否有足够的 USDT（至少 50 USDT）。
 3. 使用支持 Polygon 的去中心化交易所（如 QuickSwap 或其他 DEX）进行代币交换。
 4. 在确认交易前，请留意当前的汇率和 Gas 费用。
 ​
 如果您需要更多帮助，请随时告诉我！ 😊
```
<!-- DAILY_CHECKIN_2025-12-03_END -->

# 2025-12-02
<!-- DAILY_CHECKIN_2025-12-02_START -->




# **Day9：Qwen-Agent 入门 & 简单 Tool**

**_\##_ import**

```
 import pprint  # 美观打印数据结构
 import urllib.parse  # URL编码解码
 import json5  # 增强版JSON解析（支持注释等）
 from qwen_agent.agents import Assistant  # 核心的助手智能体类
 from qwen_agent.tools.base import BaseTool, register_tool  # 工具基类和注册装饰器
 from qwen_agent.utils.output_beautify import typewriter_print  # 打字机效果输出
```

## **Tool**

```
 @register_tool('my_image_gen')  # 将工具注册到系统中
 class MyImageGen(BaseTool):
     # 工具描述 - 告诉LLM这个工具是做什么的
     description = 'AI 绘画（图像生成）服务，输入文本描述，返回基于文本信息绘制的图像 URL。'
     
     # 参数定义 - 告诉LLM需要什么参数
     parameters = [{
         'name': 'prompt',  # 参数名
         'type': 'string',  # 参数类型
         'description': '期望的图像内容的详细描述',  # 参数说明
         'required': True  # 是否必需
     }]
 ​
     def call(self, params: str, **kwargs) -> str:
         # 实际执行工具的方法
         prompt = json5.loads(params)['prompt']  # 解析LLM传来的参数
         prompt = urllib.parse.quote(prompt)  # URL编码（处理中文等特殊字符）
         # 返回图像URL
         return json5.dumps(
             {'image_url': f'https://image.pollinations.ai/prompt/{prompt}'},
             ensure_ascii=False)
```

作用：调用`ImageGen`，利用`plooinations`生成图像

## **LLM**

```
 llm_cfg = {
     # 配置1：使用阿里云DashScope服务
     'model': 'qwen-max-latest',
     'model_type': 'qwen_dashscope',
     # 'api_key': 'YOUR_DASHSCOPE_API_KEY',  # 可在此设置或使用环境变量
     
     # 配置2：使用本地部署的模型（如vLLM、Ollama）
     # 'model': 'Qwen2.5-7B-Instruct',
     # 'model_server': 'http://localhost:8000/v1',
     # 'api_key': 'EMPTY',
     
     # 可选：生成参数配置
     'generate_cfg': {
         'top_p': 0.8  # 核采样参数，控制生成多样性
     }
 }
```

## **Agent**

```
 system_instruction = '''在收到用户的请求后，你应该：
 - 首先绘制一幅图像，得到图像的url，
 - 然后运行代码`request.get`以下载该图像的url，
 - 最后从给定的文档中选择一个图像操作进行图像处理。
 用 `plt.show()` 展示图像。
 你总是用中文回复用户。'''
 ​
 tools = ['my_image_gen', 'code_interpreter']  # 可用工具列表
 files = ['./examples/resource/doc.pdf']  # 智能体可读取的文档
 ​
 # 创建助手智能体实例
 bot = Assistant(llm=llm_cfg,
                 system_message=system_instruction,
                 function_list=tools,
                 files=files)
```

## **Diagoue**

```
 messages = []  # 存储对话历史
 while True:
     query = input('\n用户请求: ')  # 获取用户输入
     messages.append({'role': 'user', 'content': query})  # 添加到历史
     
     response = []
     response_plain_text = ''
     print('机器人回应:')
     
     # 流式处理响应（逐步输出）
     for response in bot.run(messages=messages):
         response_plain_text = typewriter_print(response, response_plain_text)
     
     messages.extend(response)  # 将助手响应加入历史
```

## **测试**

这里测试的是官方的`assistant_audio`

```
 # Copyright 2023 The Qwen team, Alibaba Group. All rights reserved.
 # 
 # Licensed under the Apache License, Version 2.0 (the "License");
 # you may not use this file except in compliance with the License.
 # You may obtain a copy of the License at
 # 
 #    http://www.apache.org/licenses/LICENSE-2.0
 # 
 # Unless required by applicable law or agreed to in writing, software
 # distributed under the License is distributed on an "AS IS" BASIS,
 # WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 # See the License for the specific language governing permissions and
 # limitations under the License.
 ​
 from qwen_agent.agents import Assistant
 from qwen_agent.gui import WebUI
 ​
 ​
 def test():
     bot = Assistant(llm={'model_type': 'qwenaudio_dashscope', 'model': 'qwen-audio-turbo-latest'})
     messages = [{
         'role':
             'user',
         'content': [{
             'audio': 'https://dashscope.oss-cn-beijing.aliyuncs.com/audios/welcome.mp3'
         }, {
             'text': '这段音频在说什么?'
         }]
     }]
     for rsp in bot.run(messages):
         print(rsp)
 ​
 ​
 def app_gui():
     # Define the agent
     bot = Assistant(llm={'model': 'qwen-audio-turbo-latest'})
     WebUI(bot).run()
 ​
 ​
 if __name__ == '__main__':
     # test()
     app_gui()
 ​
```

结果：

![image-20251202152020903](file:///C:/Users/%E7%8E%8B%E6%80%9D%E5%93%B2/Documents/MOCODE/Universal%20BlockChain/Day9%EF%BC%9AQwen%20Agent.assets/image-20251202152020903.png?lastModify=1764661918)

## **小案例：替换为大写**

```
 import pprint  # 美观打印数据结构
 import urllib.parse  # URL编码解码
 import json5  # 增强版JSON解析（支持注释等）
 from qwen_agent.agents import Assistant  # 核心的助手智能体类
 from qwen_agent.tools.base import BaseTool, register_tool  # 工具基类和注册装饰器
 from qwen_agent.utils.output_beautify import typewriter_print  # 打字机效果输出
 ​
 @register_tool('string_to_upper')
 class StringToUpper(BaseTool):
     description = '将输入的字符串转换为大写形式'
     parameters = [{
         'name':'input_text',
         'type':'string',
         'description':'需要转换为大写的文本',
         'required': True
     }]
     def call(self,params:str,**kwargs) -> str:
         params_dict = json5.loads(params)
         input_text = params_dict['input_text']
         result = input_text.upper()
         return json5.dumps(
             {'uppercase_text':result},
             ensure_ascii=False)
 ​
 llm_cfg = {
     'model':'qwen-max-latest',
     'model_type':'qwen_dashscope',
     'generate_cfg':{
         'top_p':0.8
     }
 }
 ​
 system_instruction = '''你是一个文本处理助手。当用户要求将文本转换为大写时，你应该：
 1. 识别用户提供的需要转换的文本
 2. 调用 string_to_upper 工具将文本转换为大写
 3. 将转换结果返回给用户
 ​
 请用中文回复用户。'''
 tools = ['string_to_upper']
 ​
 bot = Assistant(llm=llm_cfg,
                 system_message=system_instruction,
                 function_list=tools)
 ​
 messages=[]
 while True:
     query = input('\n用户请求：')
     messages.append({'role': 'user', 'content': query})
     response = []
     response_plain_text = ''
     print('\nAgent Anwer：')
     for response in bot.run(messages=messages):
         response_plain_text = typewriter_print(response, response_plain_text)
     messages.extend(response)
```

输出内容：

```
 用户请求：把hello world转换成大写  
 ​
 Agent Anwer：
 2025-12-02 15:48:01,428 - base.py - 780 - INFO - ALL tokens: 7, Available tokens: 57935        
 [TOOL_CALL] string_to_upper
 {"input_text": "hello world"}
 [TOOL_RESPONSE] string_to_upper
 {uppercase_text: "HELLO WORLD"}2025-12-02 15:48:09,588 - base.py - 780 - INFO - ALL tokens: 36, Available tokens: 57935
 ​
 [ANSWER]
 "hello world"转换为大写后是"HELLO WORLD"。
```
<!-- DAILY_CHECKIN_2025-12-02_END -->

# 2025-12-01
<!-- DAILY_CHECKIN_2025-12-01_START -->





# **Day8：Qwen AI 基础 & API 调用**

## **地址与 base\_url**

SDK 调用配置的`base_url`：`https://dashscope.aliyuncs.com/compatible-mode/v1`

HTTP 请求地址：`POST https://dashscope.aliyuncs.com/compatible-mode/v1/chat/completions`

## **请求体**

### **model**

!\[image-20251201221429713\](file:///C:/Users/%E7%8E%8B%E6%80%9D%E5%93%B2/Documents/MOCODE/Universal%20BlockChain/Day8%EF%BC%9AQwen%20AI%20%E5%9F%BA%E7%A1%80%20&%20API%20%E8%B0%83%E7%94%A8.assets/image-20251201221429713.png?lastModify=1764600118)

### **messages**

\*\*System Message：\*\*系统消息，用于设定大模型的角色、语气、任务目标或约束条件等。

`{"role": "system", "content": "You are a helpful assistant."}`

\*\*User Message：\*\*用户消息，用于向模型传递问题、指令或上下文等。

`{"role": "user", "content": "你是谁？"}`

\*\*Assistant Message：\*\*模型的回复。通常用于在多轮对话中作为上下文回传给模型。

```
 {
         "role": "assistant",
         "content": "def calculate_fibonacci(n):\n    if n <= 1:\n        return n\n    else:\n",
         "partial": true
 }
```

`partial` 是布尔型变量，以`content`为必须的起始内容，往后生成

\*\*Tool Message：\*\*工具的输出信息。

### **stream**

是否以流式输出方式回复。

可选值：

-   `false`：模型生成全部内容后一次性返回；
    
-   `true`：边生成边输出，每生成一部分内容即返回一个数据块（chunk）。需实时逐个读取这些块以拼接完整回复。
    

### **temperature 和 top\_p**

采样温度，控制模型生成文本的多样性。

temperature越高，生成的文本更多样，反之，生成的文本更确定。

取值范围： \[0, 2)

### **presence\_penalty**

控制模型生成文本时的内容重复度。

取值范围：\[-2.0, 2.0\]。正值降低重复度，负值增加重复度。

在创意写作或头脑风暴等需要多样性、趣味性或创造力的场景中，建议调高该值；在技术文档或正式文本等强调一致性与术语准确性的场景中，建议调低该值。

### **response\_format**

返回内容的格式。可选值：

-   `{"type": "text"}`：输出文字回复；
    
-   `{"type": "json_object"}`：输出标准格式的JSON字符串。
    

## **Python API调用尝试**

调用：用了官网最简单的示例逻辑，选择的是**Qwen-Plus**

```
 import os
 from openai import OpenAI
 ​
 ​
 client = OpenAI(
     api_key=os.getenv("DASHSCOPE_API_KEY"),
     base_url="https://dashscope.aliyuncs.com/compatible-mode/v1",  
 )
 ​
 completion = client.chat.completions.create(
     model="qwen-plus",
     messages=[
         {"role": "system", "content": "You are a helpful assistant."},
         {"role": "user", "content": "介绍一下zetachain"},
     ],
 )
 print(completion.model_dump_json())
```

输出：

```
 {"id":"chatcmpl-684efcab-cfdf-47c9-8990-f1b4456172b1","choices":[{"finish_reason":"stop","index":0,"logprobs":null,"message":{"content":"ZetaChain 是一个专注于**跨链互操作性**（cross-chain interoperability）的区块链项目，旨在实现不同区块链之间的无缝通信与价值转移，而无需依赖传统的桥接合约或封装资产。它的核心目标是让开发者能够构建真正的“全链”（omnichain）应用，即可以在多个区块链上运行并交互的应用程序。\n\n以下是 ZetaChain 的主要特点和关键技术：\n\n---\n\n### 1. **统一的全链智能合约**\nZetaChain 允许开发者编写部署在 ZetaChain 上的智能合约，这些合约可以直接与**任何连接的区块链**进行交互，包括比特币 、以太坊、BNB Chain、Solana 等，即使这些链本身不支持智能合约（如比特币）。\n\n- 比如：你可以写一个智能合约，监听比特币网络上的交易，并在以太坊上触发相应的操作。\n- 这种能力被称为“观察链状态 + 跨链执行”。\n\n---\n\n### 2. **无需封装资产（Native Asset Support）**\n传统跨链方案通常需要将资产“封装”成另一种形式（如 wBTC），这增加了复杂性和风险。而 ZetaChain 支持**原生资产的跨链调用**，用户可以直接使用 BTC、ETH、DOT 等原生代币参与全链应用，无需换币或锁定/铸造机制。\n\n---\n\n### 3. **去中心化的中继网络**\nZetaChain 使用一组去中心化的验证节点（由 PoS 共识保护）来监听多条区块链的状态变化，并将消息和事件 传递到 ZetaChain 主链。这个机制称为 **Observer Network**。\n\n- 验证节点持续监控各链上的特定事件（如存款、转账等）。\n- 一旦检测到事件，就会在 ZetaChain 上触发相应的智能合约逻辑。\n\n---\n\n### 4. **支持 EVM 和非 EVM 区块链**\nZetaChain 不仅支持 Ethereum 虚拟机（EVM）生态的链，还支持非 EVM 链（如比特币），这是其跨链能力的关键优势之一。\n\n---\n\n### 5. **ZETA 代币**\nZETA 是 ZetaChain 的原生代币，用于：\n- 支付交易手续费\n- 质押参与网络安全（PoS）\n- 治理投票\n\n---\n\n### 6. **开发工具友好**\nZetaChain 提供了类似 Ethereum 的开发环境，支持 Solidity 编程语言和常见的 Web3 工具（如 Hardhat、MetaMask），降低了开发者进入门槛。\n\n---\n\n### 实际应用场景\n- **全链 DeFi 应用**：用户可以用 BTC 抵押借出 ETH，直接跨链操作。\n- **跨链游戏**：游戏角色或资产可在不同链间自由流动。\n- **跨链身份系统**：基于 多链活动的身份验证。\n- **自动化跨链交易机器人**：根据某条链的价格变动，在另一条链上执行交易。\n\n---\n\n### 当前进展（截至 2024 年）\n- 已上线主网（Mainnet）。\n- 支持 Ethereum、BNB Chain、Bitcoin、Polygon、Arbitrum、Avalanche、Solana 等主流链。\n- 生态中已有多个全链 DApp 上线，如 ZetaSwap（全链 DEX）、Maverick Protocol 等。\n\n---\n\n### 总结\nZetaChain 的愿景是成为“**跨链层的基础设施**”，就像互联网中的 TCP/IP 协议一样，连接所有区块链。它通过创新的观察者网络和全链智能合约模型，解决了传统跨链桥的安全隐患和碎片化问题，为真正的去中心化全链经济铺平道路。\n\n> 官网：[https://www.zetachain.com](https://www.zetachain.com)  \n> 文档：[https://docs.zetachain.com](https://docs.zetachain.com)\n\n如果你对某个具体技术细节或应用场景感兴趣，我可以进一步深入讲解。","refusal":null,"role":"assistant","annotations":null,"audio":null,"function_call":null,"tool_calls":null}}],"created":1764599844,"model":"qwen-plus","object":"chat.completion","service_tier":null,"system_fingerprint":null,"usage":{"completion_tokens":860,"prompt_tokens":23,"total_tokens":883,"completion_tokens_details":null,"prompt_tokens_details":{"audio_tokens":null,"cached_tokens":0}}}
```

![屏幕截图 2025-12-01 224224.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/MoMNiToT/images/2025-12-01-1764600158181-_____2025-12-01_224224.png)
<!-- DAILY_CHECKIN_2025-12-01_END -->

# 2025-11-30
<!-- DAILY_CHECKIN_2025-11-30_START -->






# **Day6&7：Demo！**

`Swap` 合约是一个部署在 ZetaChain 上的通用应用。它使用单个跨链调用，使用户能够在不同区块链之间进行代币兑换。代币以 ZRC-20 的形式接收，可选择使用 Uniswap v2 流动性进行兑换，并提回连接的链上。

## **前提：用户操作**

用户在 ETH 链发起跨链调用，把 1 ETH 转到 ZetaChain 的 Swap 合约，同时附带「ABI 编码的参数」（包含 3 个关键信息）：

-   `zrc20`：用户输入的ZRC-20代币地址。
    
-   `amount`：输入代币的数量。
    
-   `message`：ABI编码的负载，包含：
    
    -   `targetToken`：目标链需兑换的ZRC-20代币地址。
        
    -   `recipient`：目标链接收地址（字节形式，兼容EVM、Solana等）。
        
    -   `withdrawFlag`：是否跨链提回（`true`）或ZetaChain本地转移（`false`）。
        

调用入口：

```
 function onCall(
     MessageContext calldata context,
     address zrc20,
     uint256 amount,
     bytes calldata message
 ) external onlyGateway
```

## **参数解码**

```
 (address targetToken, bytes memory recipient, bool withdrawFlag) =
     abi.decode(message, (address, bytes, bool));
```

## **查询目标链 gas 费用**

目标链的提款 Gas 报价：

```
 (address gasZRC20, uint256 gasFee) = IZRC20(targetToken).withdrawGasFee();
```

-   `gasZRC20` 是代表目标链的燃料代币的 ZRC-20。
    
-   `gasFee` 是在目标链上执行所需的金额。
    

## **验证输入够不够用**

用 Uniswap v2 计算「最少需要多少输入代币（ZRC-20 ETH），才能覆盖 gas 费 + 兑换目标代币」。如果用户转的 1 ETH（ZRC-20）不够，直接报错。

流程：

1.  通过 `withdrawGasFee()` 引用目标气体需求。
    
2.  使用 DEX 报价验证输入是否涵盖：
    
    ```
     uint256 minInput = quoteMinInput(inputToken, targetToken);
     if (amount < minInput) revert InsufficientAmount(...);
    ```
    

## **兑换目标链 gas**

如果用户输入的代币（ZRC-20 ETH）和 gas 代币（ZRC-20 BTC）不一样，就用 Uniswap 换「刚好够 gasFee 的 ZRC-20 BTC」（比如换 0.001 BTC）。

```
 inputForGas = SwapHelperLib.swapTokensForExactTokens(
   uniswapRouter, inputToken, gasFee, gasZRC20, amount
 );
```

## **兑换剩下的目标代币**

把剩下的 ZRC-20 ETH（1 ETH - 换 gas 用的部分），通过 Uniswap 换成 ZRC-20 BTC（比如换了 0.05 BTC）。

```
 out = SwapHelperLib.swapExactTokensForTokens(
   uniswapRouter, inputToken, amount - inputForGas, targetToken, 0
 );
```

## **提回目标链**

调用 `gateway.withdraw()`，让网关做两件事：

-   在 ZetaChain 上销毁这 0.051 BTC 的 ZRC-20；
    
-   在 BTC 链上，给用户的收款地址释放 0.05 BTC 原生代币（gas 费 0.001 BTC 被目标链消耗）。
    

```
 IZRC20(gasZRC20).approve(address(gateway), gasFee);
 IZRC20(params.target).approve(address(gateway), out);
  
 gateway.withdraw(
   abi.encodePacked(params.to), // chain-agnostic recipient (bytes)
   out,                         // amount of target token
   params.target,               // ZRC-20 to withdraw
   revertOptions                // failure handling
 );
```

## **使用** `RevertOptions` **和** `onRevert` **撤销**

```
 function onRevert(RevertContext calldata context) external onlyGateway {
     (bytes memory sender, address zrc20) =
         abi.decode(context.revertMessage, (bytes, address));
  
     (uint256 out,,) = handleGasAndSwap(
         context.asset, context.amount, zrc20, true
     );
  
     gateway.withdraw(
         sender, // chain-agnostic refund address
         out,
         zrc20,
         RevertOptions({
             revertAddress: address(bytes20(sender)), // best-effort for EVM
             callOnRevert: false,
             abortAddress: address(0),
             revertMessage: "",
             onRevertGasLimit: gasLimit
         })
     );
 }
```

## **项目（AI写的）**

### **项目 Idea 1: 跨链流动性保险池 (Cross-Chain Liquidity Insurance Pool)**

**目标用户**:

-   多链资产持有者（如同时持有以太坊、Solana、Cosmos 链资产的用户）。
    
-   DeFi 协议方（希望降低跨链流动性迁移风险的借贷或交易协议）。
    

**想解决的问题**: 当前跨链流动性转移存在以下痛点：

1.  **流动性碎片化**：用户资产分散在不同链，难以统一管理风险。
    
2.  **跨链延迟与失败风险**：资产转移过程中可能因网络拥堵、桥接故障或黑客攻击导致资金损失。
    
3.  **缺乏风险对冲工具**：用户无法为跨链操作投保，若出现故障需自行承担损失。
    

**跨链/通用资产使用方式**:

-   **通用资产抵押**: 用户用跨链稳定币（如 USDC、DAI）或原生链资产（如 ETH、SOL）存入流动性池，通过跨链桥（如 Chainlink CCIP 或 Wormhole）实现多链资产同步。
    
-   **智能合约保险机制**: 当用户发起跨链转账时，协议自动生成保险单，保费从流动性池中按比例扣除。若转账失败或超时，保险池按预设规则自动补偿用户损失。
    
-   **风险共担模型**: 保险池资金由多链流动性提供者共同维护，通过预言机监控链上事件（如桥接状态、链下攻击信号），动态调整保费费率和赔付阈值。
    

* * *

### **项目 Idea 2: 通用资产跨链收益聚合器 (Universal Asset Cross-Chain Yield Aggregator)**

**目标用户**:

-   普通 DeFi 用户（希望最大化资产收益但缺乏跨链操作经验的散户）。
    
-   机构投资者（需高效配置多链资产的量化策略团队）。
    

**想解决的问题**: 现有收益聚合器局限性：

1.  **链孤岛效应**：收益机会仅限于单一链，无法跨链比价和自动迁移。
    
2.  **高操作成本**：手动跨链需支付高额 Gas 费和时间成本。
    
3.  **收益波动风险**: 不同链的市场环境（如 TVL、利率）变化快，手动调整策略滞后。
    

**跨链/通用资产使用方式**:

-   **资产标准化**: 支持主流跨链资产（如 stETH、wbtc、多链 USDC）作为通用抵押品，通过跨链桥实现一键部署至最优收益链（如以太坊上的 Aave、Solana 上的 Jupiter）。
    
-   **自动化策略引擎**: 基于链上数据（如利率、TVL、Gas 费）和链下市场分析（如攻击事件预警），AI 驱动的算法实时推荐跨链收益策略，并通过智能合约自动执行资产迁移。
    
-   **收益再平衡**: 用户可设定风险偏好（如保守型/激进型），系统自动将收益按比例分配至不同链的流动性池或质押协议，同时通过跨链预言机同步更新资产状态，避免因链间延迟导致的套利失效。
<!-- DAILY_CHECKIN_2025-11-30_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->







# **Day5：Universal DeFi & 全链资产**

**💫通用资产：通用合约和连接合约**

通用合约：在 ZetaChain 上铸造资产、将资产从 ZetaChain 转移到连接的链、处理从连接的链到 ZetaChain 的资产转入

连接合约：在连接的链上铸造资产、将资产转移到另一个连接的链或 ZetaChain、处理来自 ZetaChain 或其他已连接链的资产转账

通用资产部署：

1.  在 ZetaChain 上部署通用合约。这是必需的步骤，因为 ZetaChain 作为所有跨链转账的中心枢纽，即使是在已连接的 EVM 链之间也是如此。
    
2.  在支持的 EVM 链（例如 Ethereum、Base、Polygon、BNB）上部署一个连接合约。
    
3.  在 ZetaChain 上的通用合约上运行 `setConnected(zrc20, connectedAddress)` ，其中：
    
    -   `zrc20` 是目标 EVM 链的燃料代币的 ZRC-20 合约。它充当链标识符。
        
    -   `connectedAddress` 是 EVM 链上连接合约的地址（来自步骤 2）。
        

4.  在连接的 EVM 链上运行 `ConnectedAsset.setUniversal(universalAddress)` ，其中 `universalAddress` 是 ZetaChain 上通用合约的地址（来自步骤 1）。
    

回滚处理：从ZetaChain中把资产退还，防止原链的高成本操作

ZRC20 和 ERC20：

ZRC20：可表示任何链上的资产，实现 "全链资产" 概念

ERC20：以太坊生态系统的 "通用语言"，几乎所有 DeFi 应用都支持

应用场景：

跨链的游戏通行证，能够使得玩家在一个链游上的NFT宠物等能够应用于其他游戏。在经济上，一方面资产能够增值，稀有产品的价值更加充分；另一方面，能够提升开发者的收益，特权的定制，增加用户留存和付费意愿。
<!-- DAILY_CHECKIN_2025-11-28_END -->

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
