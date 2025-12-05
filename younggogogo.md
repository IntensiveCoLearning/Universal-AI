---
timezone: UTC+8
---

# 曾铭扬

**GitHub ID:** younggogogo

**Telegram:** @fortunallYounnng(微信)

## Self-introduction

区块链新人，想通过本次学习增进对区块链了解

## Notes

<!-- Content_START -->
# 2025-12-05
<!-- DAILY_CHECKIN_2025-12-05_START -->
时间莫名其妙就没有了，今天就简单完成任务，继续鸽技术文档

**学习目标**

\- 串起一条最小通路：自然语言 → Agent → 后端 →（模拟或真实）ZetaChain 调用。

\- 完成一个简版 Demo：

\- 输入一句自然语言；

\- Agent 输出结构化参数；

\- 后端打印计划执行的链上操作（或在测试网上真的发一笔交易）。

结合前两天的千问模板，还有今天的任务，让ai帮我生成了相应代码，我这里只采用了模拟zetachain的调用

\`\`\`python

import os

import json5

from qwen\_agent.agents import Assistant

from qwen\_[agent.tools](http://agent.tools).base import BaseTool, register\_tool

\# ======================

\# 1. 自定义工具：ZETA 转账模拟（保留你的逻辑）

\# ======================

@register\_tool(‘zeta\_transfer\_simulate’)

class ZetaTransferSimulateTool(BaseTool):

description = ‘’'Simulate ZETA token transfer on ZetaChain testnet.

Need to input recipient wallet address (42-bit Ethereum format like 0x…) and transfer amount (ZETA unit, 0.001~0.01 recommended for testnet).

No real transaction will be sent, only print simulation result.‘’’

parameters = \[

{

‘name’: ‘recipient\_address’,

‘type’: ‘string’,

‘description’: ‘Recipient wallet address (must be 42-bit Ethereum address, e.g., 0x1234567890abcdef1234567890abcdef12345678)’,

‘required’: True

},

{

‘name’: ‘amount’,

‘type’: ‘number’,

‘description’: ‘Transfer amount (unit: ZETA, must be greater than 0, testnet recommended 0.001~0.01)’,

‘required’: True

},

{

‘name’: ‘token’,

‘type’: ‘string’,

‘description’: ‘Token type, only ZETA supported now’,

‘default’: ‘ZETA’,

‘required’: False

}

\]

def call(self, params: str, \*\*kwargs) -> str:

try:

params\_dict = json5.loads(params)

recipient = params\_dict.get(‘recipient\_address’, ‘’)

amount = params\_dict.get(‘amount’, 0)

token = params\_dict.get(‘token’, ‘ZETA’)

simulate\_result = {

“状态”: “模拟成功”,

“网络”: “ZetaChain Testnet”,

“代币类型”: token,

“接收地址”: recipient,

“转账金额”: f"{amount} {token}",

“说明”: “仅演示流程，未发送真实链上交易”,

“提示”: “如需真实转账，可对接 Zeta SDK 并配置测试网私钥”

}

\# 返回结构化 JSON，后续直接解析显示

return json5.dumps(simulate\_result, ensure\_ascii=False, indent=2)

except Exception as e:

return json5.dumps({‘状态’: ‘模拟失败’, ‘错误信息’: str(e)}, ensure\_ascii=False)

\# ======================

\# 2. 保留你原有的工具

\# ======================

@register\_tool(‘string\_to\_upper’)

class StringToUpperTool(BaseTool):

description = ‘Convert input string to uppercase, input text content, return uppercase result.’

parameters = \[{

‘name’: ‘text’,

‘type’: ‘string’,

‘description’: ‘Original string that needs to be converted to uppercase’,

‘required’: True

}\]

def call(self, params: str, \*\*kwargs) -> str:

try:

params\_dict = json5.loads(params)

text = params\_dict.get(‘text’, ‘’)

return json5.dumps({‘result’: text.upper()}, ensure\_ascii=False)

except Exception as e:

return json5.dumps({‘error’: str(e)}, ensure\_ascii=False)

@register\_tool(‘add\_two\_numbers’)

class AddTwoNumbersTool(BaseTool):

description = ‘Calculate the sum of two numbers, input two numbers, return their sum.’

parameters = \[

{

‘name’: ‘num1’,

‘type’: ‘number’,

‘description’: ‘First number (integer or float)’,

‘required’: True

},

{

‘name’: ‘num2’,

‘type’: ‘number’,

‘description’: ‘Second number (integer or float)’,

‘required’: True

}

\]

def call(self, params: str, \*\*kwargs) -> str:

try:

params\_dict = json5.loads(params)

num1 = float(params\_dict.get(‘num1’, 0))

num2 = float(params\_dict.get(‘num2’, 0))

sum\_result = num1 + num2

return json5.dumps({‘result’: sum\_result}, ensure\_ascii=False)

except Exception as e:

return json5.dumps({‘error’: str(e)}, ensure\_ascii=False)

\# ======================

\# 3. 配置智能体（移除 stream 参数，适配旧版本）

\# ======================

os.environ\[“DASHSCOPE\_API\_KEY”\] = “xxx”

bot = Assistant(

llm={‘model’: ‘qwen-max’, ‘model\_server’: ‘dashscope’},

\# 简化提示：让 LLM 只返回工具结果，不返回中间垃圾

system\_message=‘’'You are a tool executor:

1\. For transfer requests: Call zeta\_transfer\_simulate, return the tool’s JSON result directly (only the JSON string).

2\. For uppercase requests: Call string\_to\_upper, return the tool’s JSON result directly.

3\. For sum requests: Call add\_two\_numbers, return the tool’s JSON result directly.

4\. Do NOT return function call logs, model info, or extra text. Only return the tool’s JSON output.‘’',

function\_list=\[‘string\_to\_upper’, ‘add\_two\_numbers’, ‘zeta\_transfer\_simulate’\]

\# 移除 stream=False，适配你的 Qwen-Agent 版本

)

\# ======================

\# 4. 交互测试（手动过滤，获取纯净结果）

\# ======================

messages = \[\]

print(“=” \* 60)

print(“ZETA 转账模拟 Demo（输入 ‘exit’ 退出）”)

print(“=” \* 60)

print(“示例输入：帮我往地址 0x7890abcdef1234567890abcdef1234567890abcd 转 0.001 个 ZETA”)

print(“=” \* 60)

while True:

query = input(‘\\n请输入你的需求：’)

if query.lower() == ‘exit’:

print(“\\n退出程序，感谢使用！”)

break

messages.append({‘role’: ‘user’, ‘content’: query})

print(“\\n📋 执行结果：”)

print(“-” \* 40)

try:

\# 执行 Agent，收集所有响应片段

response\_chunks = \[\]

for chunk in [bot.run](http://bot.run)(messages=messages):

response\_chunks.extend(chunk)

\# 从响应中提取工具返回的 JSON 结果

tool\_output = “”

for item in response\_chunks:

if isinstance(item, dict):

\# 提取工具执行后的结果（可能在 content 或 function\_call.arguments 中）

if ‘content’ in item and item\[‘content’\] and item\[‘content’\].strip().startswith(‘{’):

tool\_output = item\[‘content’\].strip()

elif ‘function\_call’ in item and ‘arguments’ in item\[‘function\_call’\]:

arg = item\[‘function\_call’\]\[‘arguments’\].strip()

if arg.startswith(‘{’):

tool\_output = arg

\# 解析并格式化显示结果

if tool\_output:

result = json5.loads(tool\_output)

if ‘网络’ in result: # 转账模拟结果

for key, value in result.items():

print(f"✅ {key}：{value}")

elif ‘result’ in result: # 转大写/求和结果

print(f"✅ 操作成功：{result\[‘result’\]}")

else: # 错误结果

print(f"❌ {result}")

else:

\# 若未提取到，说明 LLM 返回了自然语言，直接显示

natural\_text = “”

for item in response\_chunks:

if isinstance(item, dict) and ‘content’ in item and item\[‘content’\]:

natural\_text += item\[‘content’\]

if natural\_text:

print(f"✅ {natural\_text}")

else:

print(“❌ 未获取到有效结果”)

except Exception as e:

print(f"❌ 执行出错：{str(e)}")

print(“-” \* 40)

messages.extend(response\_chunks)

\`\`\`

结果代码如下：

!\[\[Pasted image 20251205220719.png\]\]

![Pasted image 20251205220719.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/younggogogo/images/2025-12-05-1764944176680-Pasted_image_20251205220719.png)

今天的核心就是把昨天做的中间层和前天完成的agent部分结合起来

执行流程如下：

1\. 用户输入：“帮我往地址 0x7890abcdef1234567890abcdef1234567890abcd 转 0.001 个 ZETA”；

2\. Agent 理解：“用户要转账，需要调用 zeta\_transfer\_simulate，提取参数：地址 = 0x…，金额 = 0.001”；

3\. Agent 调用工具，把参数传给 `zeta_transfer_simulate`；

4\. 工具执行：构造模拟结果（网络 = ZetaChain 测试网、地址 =…、金额 = 0.001 ZETA）；

5\. 我们过滤中间垃圾数据，只显示工具返回的核心结果，用户就能看到清晰的模拟信息。

明天会开始看技术文档吗哈哈哈哈，再不看都要开始黑客松了，今天就这样，明天继续加油
<!-- DAILY_CHECKIN_2025-12-05_END -->

# 2025-12-04
<!-- DAILY_CHECKIN_2025-12-04_START -->

今天的内容和DAY6的swap有关，但我之前我没跑通官方的示例，限制只能先回去折腾第六周的内容。

\---

研究了两个小时发现没啥关系，顺便了解了一下什么是zrc20，他是怎么完成跨链的，有点半只半解，明天的任务才是要求完成这一点，今天只是一个简单承接agent返回来的json就可以，

\---

**学习目标**

\- 把解析出的 DeFi 参数，映射到实际的合约调用接口（哪怕先是伪代码）。

\- 形成一层清晰的“中间层服务”。

\- 写出一个后端伪代码或简单实现：

\- 接收 `parse_swap_intent` 返回值；

\- 根据不同链 / 不同 token 选择具体的合约 / 调用方式；

\- 暂时可以只在控制台打印“准备发起什么交易”。

\---

今天要做的说白了就是一个agent可以调用的tool，明天应该就是把agent和swap样例和你今天的tool结合起来，明确了这一点让ai给我跑了一段代码

\`\`\`PYTHON

\# 1. 基础配置（链信息、合约地址，核心参数不丢）

\# 链配置：key=链ID，value=RPC+默认兑换合约+常用Token

CHAIN\_CONFIG = {

97: { # BSC测试链

"rpc": "[https://data-seed-prebsc-1-s1.binance.org:8545](https://data-seed-prebsc-1-s1.binance.org:8545)",

"swap\_contract": "0x10ED43C718714eb63d5aA57B78B54704E25602", # PancakeSwap测试网合约

"tokens": { # Token符号→地址（简化，精度默认18位）

"BNB": "0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095",

"BUSD": "0xeD24FC36d5Ee211Ea254E43149345926103e988",

"USDT": "0x337610d27c682E347C9cD60BD4b3b107C9d34dD"

}

}

}

\# 2. 中间层核心服务（只保留核心流程）

class DeFiSwapMiddleware:

def **init**(self):

self.chain\_config = CHAIN\_CONFIG # 加载链配置

def process\_swap(self, swap\_intent):

"""

核心方法：处理兑换意图

:param swap\_intent: 解析层返回的字典（简化，不用dataclass）

""" # 步骤1：提取意图参数（直接从字典拿，简洁明了）

chain\_id = swap\_intent\["chain\_id"\]

from\_token = swap\_intent\["from\_token"\]

to\_token = swap\_intent\["to\_token"\]

amount = swap\_intent\["amount"\] # 用户输入的十进制金额（如0.1）

slippage = swap\_intent\["slippage"\] # 滑点（如0.5）

recipient = swap\_intent\["recipient"\] # 接收地址

\# 步骤2：基础校验（只留关键校验，去掉冗余）

if chain\_id not in self.chain\_config:

print(f"❌ 不支持链ID：{chain\_id}")

return

chain\_info = self.chain\_config\[chain\_id\]

if from\_token not in chain\_info\["tokens"\] or to\_token not in chain\_info\["tokens"\]:

print(f"❌ 链{chain\_id}不支持该Token组合：{from\_token}→{to\_token}")

return

\# 步骤3：获取合约和Token地址（根据链选择）

target\_contract = chain\_info\["swap\_contract"\] # 简化：直接用默认合约

from\_token\_addr = chain\_info\["tokens"\]\[from\_token\]

to\_token\_addr = chain\_info\["tokens"\]\[to\_token\]

\# 步骤4：简化计算（金额转wei、滑点计算）

amount\_wei = int(amount _10_\*18) # 默认18位精度，精简掉工具函数

min\_receive\_wei = int(amount\_wei \* (1 - slippage/100)) # 简化滑点逻辑

\# 步骤5：打印交易信息（核心输出，模拟合约调用）

print("="\*50)

print("📥 接收兑换意图：")

print(f" 链ID：{chain\_id} | 兑换方向：{from\_token}→{to\_token} | 金额：{amount}")

print("="\*50)

print("✅ 准备发起交易：")

print(f" 链RPC：{chain\_info\['rpc'\]}")

print(f" 调用合约：{target\_contract}")

print(f" 交易参数：")

print(f" - 输入Token地址：{from\_token\_addr}")

print(f" - 输出Token地址：{to\_token\_addr}")

print(f" - 输入金额（wei）：{amount\_wei}")

print(f" - 最小接收金额（wei）：{min\_receive\_wei}")

print(f" - 接收地址：{recipient}")

print(f" 交易类型：Token兑换（调用合约 swapExactTokensForTokens 方法）")

print("="\*50)

\# 3. 测试调用（模拟解析层返回结果，直接调用中间层）

if **name** == "\_\_main\_\_":

\# 模拟 parse\_swap\_intent 的返回值（简化为字典，无需复杂数据结构）

user\_swap\_intent = {

"chain\_id": 97,

"from\_token": "BNB",

"to\_token": "BUSD",

"amount": 0.1, # 用户想换0.1 BNB

"slippage": 0.5, # 滑点0.5%

"recipient": "0xYourWallet1234567890abcdef" # 用户钱包地址

}

\# 初始化中间层，处理意图

middleware = DeFiSwapMiddleware()

middleware.process\_swap(user\_swap\_intent)

\`\`\`

简单完成今天的需求

输出如下

![Pasted image 20251204220049.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/younggogogo/images/2025-12-04-1764856966991-Pasted_image_20251204220049.png)

!\[\[Pasted image 20251204220049.png\]\]

1\. 链 RPC：和区块链通信的 “官方电话”

2\. 兑换合约地址：链上帮你换币的 “自动柜台” 门牌号

3\. 输入 / 输出 Token 地址：你要换的币（比如 BNB、BUSD）的 “身份证号”（也是代币自身的合约地址）

4\. 金额（wei）：币的 “最小计数单位”（合约只认这个）

5\. 最小接收金额（wei）：能接受的最少换币数量（防汇率波动亏太多）

6\. 接收地址：你的钱包 “账号”（换好的币打这）

上面的名词对应解释

\---

明天真的要搞懂swap了，不然整段拉住，说看的技术文档真得看了！
<!-- DAILY_CHECKIN_2025-12-04_END -->

# 2025-12-03
<!-- DAILY_CHECKIN_2025-12-03_START -->


\### Day 10：DeFi 意图解析（从自然语言到结构化参数）

**学习目标**

\- 让 Agent 能从自然语言中提取 DeFi 操作参数（链名、代币、金额等）。

\- 完成一个最小的“解析层”。

\- 设计一个工具`parse_swap_intent(text)`，返回结构化 JSON，例如：

`{ "chain": "base", "tokenIn": "USDC", "tokenOut": "ETH", "amount": "10" }`

\- 让 Agent 能处理以下输入：

\- “帮我在 Base 上用 10 USDC 换成 ETH”

\- “把我 50 U 兑换成 Polygon 上的 MATIC”

\---

把昨天的跑通的示例丢进去让ai让今天的需求修改秒行，震惊，第一次那么快，然后就让ai按昨天的逻辑给我解释代码

\`\`\`python

import os

import json5

import re

from qwen\_agent.agents import Assistant

from qwen\_[agent.tools](http://agent.tools).base import BaseTool, register\_tool

\# ======================

\# 自定义工具：DeFi意图解析

\# ======================

@register\_tool('parse\_swap\_intent')

class ParseSwapIntentTool(BaseTool):

\# 工具描述

description = 'Parse DeFi swap intent from natural language, extract chain, tokenIn, tokenOut, amount'

\# 参数定义

parameters = \[{

'name': 'text',

'type': 'string',

'description': 'Natural language text containing DeFi swap intent',

'required': True

}\]

\# 链和代币别名映射

CHAIN\_ALIASES = {

'base': \['base', 'base链'\],

'polygon': \['polygon', 'matic链'\],

'ethereum': \['ethereum', 'eth', '以太坊'\],

'bsc': \['bsc', '币安智能链'\],

'arbitrum': \['arbitrum', 'arb'\]

}

TOKEN\_ALIASES = {

'USDC': \['usdc', 'usd coin'\],

'ETH': \['eth', 'ethereum'\],

'MATIC': \['matic', 'polygon'\],

'USDT': \['u', 'usdt', '泰达币', 'usd t'\],

'BTC': \['btc', '比特币'\]

}

\# 工具执行逻辑

def call(self, params: str, \*\*kwargs) -> str:

try:

\# 解析输入参数

params\_dict = json5.loads(params)

text = params\_dict.get('text', '').lower()

\# 初始化结果

result = {

"chain": None,

"tokenIn": None,

"tokenOut": None,

"amount": None

}

\# 提取金额

amount\_match = [re.search](http://re.search)(r'(\\d+(?:\\.\\d+)?)', text)

if amount\_match:

result\["amount"\] = amount\_[match.group](http://match.group)(1)

\# 提取链名

for chain, aliases in self.CHAIN\_ALIASES.items():

if any(alias in text for alias in aliases):

result\["chain"\] = chain

break

\# 提取代币

tokens\_found = \[\]

for token, aliases in self.TOKEN\_ALIASES.items():

if any(alias in text for alias in aliases):

tokens\_found.append(token)

\# 根据上下文确定输入输出代币

if '换成' in text or '兑换成' in text:

split\_text = text.split('换成') if '换成' in text else text.split('兑换成')

if len(tokens\_found) >= 2:

result\["tokenIn"\] = tokens\_found\[0\]

result\["tokenOut"\] = tokens\_found\[1\]

else:

\# 从文本位置判断

if len(split\_text) > 1:

\# 检查输入代币

for token, aliases in self.TOKEN\_ALIASES.items():

if any(alias in split\_text\[0\] for alias in aliases):

result\["tokenIn"\] = token

\# 检查输出代币

for token, aliases in self.TOKEN\_ALIASES.items():

if any(alias in split\_text\[1\] for alias in aliases):

result\["tokenOut"\] = token

\# 特殊处理U->USDT

if result\["tokenIn"\] == 'U':

result\["tokenIn"\] = 'USDT'

if result\["tokenOut"\] == 'U':

result\["tokenOut"\] = 'USDT'

return json5.dumps(result, ensure\_ascii=False)

except Exception as e:

return json5.dumps({'error': str(e)}, ensure\_ascii=False)

\# ======================

\# 配置智能体（加载自定义工具）

\# ======================

os.environ\["DASHSCOPE\_API\_KEY"\] = "6" # 替换为真实Key

bot = Assistant(

llm={'model': 'qwen-max', 'model\_server': 'dashscope'},

\# 系统提示：明确告诉LLM可用工具及用途

system\_message='You are a DeFi intent parsing expert. Use "parse\_swap\_intent" tool to extract swap parameters from user input. Return the structured JSON result directly.',

function\_list=\['parse\_swap\_intent'\] # 加载自定义工具

)

\# ======================

\# 交互测试

\# ======================

messages = \[\]

print("=== DeFi意图解析器 ===")

print("支持的输入示例：")

print("1. 帮我在 Base 上用 10 USDC 换成 ETH")

print("2. 把我 50 U 兑换成 Polygon 上的 MATIC")

print("输入'exit'退出\\n")

while True:

query = input('请输入DeFi需求: ')

if query.lower() == 'exit':

break

messages.append({'role': 'user', 'content': query})

response = \[\]

for chunk in [bot.run](http://bot.run)(messages=messages):

response.extend(chunk)

print('解析结果:', chunk)

messages.extend(response)

\`\`\`

总体框架是：用户输入 → LLM（大脑理解）→ Agent（服务员调度）→ Tool（师傅执行）→ Memory（记忆上下文）→ LLM（结果翻译）→ 用户

在这里是tool部分

\`\`\`python

@register\_tool('parse\_swap\_intent')

class ParseSwapIntentTool(BaseTool):

\# 工具描述

description = 'Parse DeFi swap intent from natural language, extract chain, tokenIn, tokenOut, amount'

\# 参数定义

parameters = \[{

'name': 'text',

'type': 'string',

'description': 'Natural language text containing DeFi swap intent',

'required': True

}\]

\# 链和代币别名映射

CHAIN\_ALIASES = {

'base': \['base', 'base链'\],

'polygon': \['polygon', 'matic链'\],

'ethereum': \['ethereum', 'eth', '以太坊'\],

'bsc': \['bsc', '币安智能链'\],

'arbitrum': \['arbitrum', 'arb'\]

}

TOKEN\_ALIASES = {

'USDC': \['usdc', 'usd coin'\],

'ETH': \['eth', 'ethereum'\],

'MATIC': \['matic', 'polygon'\],

'USDT': \['u', 'usdt', '泰达币', 'usd t'\],

'BTC': \['btc', '比特币'\]

}

def call(self, params: str, \*\*kwargs) -> str:

...

\`\`\`

这个部分是处理defi意图的自定义工具，核心能力是从自然语言中提取链名，代币对，金额,处理名字映射，比如u变成usdt之类的，理解“换成”字眼前后的代币关系，是谁变成谁

\#### **LLM（大脑）：智能决策中心**

python

运行

\`\`\`python

bot = Assistant(

llm={'model': 'qwen-max', 'model\_server': 'dashscope'},

system\_message='You are a DeFi intent parsing expert...',

function\_list=\['parse\_swap\_intent'\]

)

\`\`\`

\- **角色定位**：整个系统的 "大脑"

\- **核心职责**：

\- 理解用户输入的自然语言需求

\- 判断需要调用哪个工具（这里固定为 parse\_swap\_intent）

\- 将用户问题转化为工具能理解的参数格式

\- 把工具返回的结构化数据翻译成友好的用户回答

\- **配置要点**：

\- 使用通义千问大模型（qwen-max）

\- 通过 system\_message 设定角色和工作方式

\#### 3. **Agent（服务员）：协调调度者**

python

运行

\`\`\`python

bot = Assistant(...) # Assistant类就是Agent的具体实现

for chunk in [bot.run](http://bot.run)(messages=messages):

response.extend(chunk)

\`\`\`

\- **角色定位**：连接用户、LLM 和 Tool 的 "服务员"

\- **核心工作流**：

① 接收用户消息 → ② 传给LLM分析 → ③ 根据LLM指令调用对应Tool →

④ 获取Tool结果 → ⑤ 返回给LLM处理 → ⑥ 整理结果输出

\####  **Memory（记忆）：上下文管理器**

python

运行

\`\`\`python

messages = \[\] # 存储对话历史

messages.append({'role': 'user', 'content': query})

messages.extend(response) # 追加机器人回复

\`\`\`

\- **实现方式**：通过列表维护完整对话历史

\- **作用**：

\- 记住之前的交互内容

\- 支持多轮对话的上下文理解

\- 例如：用户先问 "什么是 USDC"，再问 "用它换 ETH"，系统能关联理解

\### 核心工作流程拆解

\#### 示例：用户输入 "帮我在 Base 上用 10 USDC 换成 ETH"

1\. **第一步：用户输入 → Agent 接收**

\`\`\`python

query = input('请输入DeFi需求: ')

messages.append({'role': 'user', 'content': query})

\`\`\`

2\. **第二步：LLM 分析需求**

\- 理解到用户需要进行 DeFi 兑换操作

\- 决定调用 parse\_swap\_intent 工具

\- 提取参数：text="帮我在 Base 上用 10 USDC 换成 ETH"

3\. **第三步：Agent 调用 Tool**

\- Agent 将参数传递给 ParseSwapIntentTool

\- Tool 执行 call () 方法，解析出结构化数据：

json

\`\`\`json

{"chain": "base", "tokenIn": "USDC", "tokenOut": "ETH", "amount": "10"}

\`\`\`

4\. **第四步：结果返回与展示**

\- Tool 结果返回给 LLM

\- LLM 将结构化数据整理成用户友好的格式

\- Agent 输出最终结果

\---

最后的最后今天的结果图

![Pasted image 20251203220805.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/younggogogo/images/2025-12-03-1764771103293-Pasted_image_20251203220805.png)

!\[\[Pasted image 20251203220805.png\]\]
<!-- DAILY_CHECKIN_2025-12-03_END -->

# 2025-12-02
<!-- DAILY_CHECKIN_2025-12-02_START -->



**学习目标**

\- 理解 Qwen-Agent 框架的基本组成（LLM / Agent / Tools / Memory）。

\- 搭建一个最小的 Agent，并挂一个简单 Tool。

**实践 / 作业**

\- 跑通一个 Qwen-Agent 官方示例。

\- 自定义一个简单 Tool，例如：

\- 把字符串转大写；

\- 计算两个数的和。

\- 确认 Agent 能自动调用这个 Tool 并返回结果。

\---

先从理解开始：

agent框架的组成分为四部分：

1\. LLM 相当于这个框架里的大脑，负责理解用户的需求，然后交给把需求和所需要调用的工具交给agent，再把agent带回来的结果翻译给用户

2\. agent 相当于服务员，从llm哪里接受需求后负责喊专门的师傅去干活

3\. tool充当这里面的师傅，专门负责处理需求，比如求和，比如转大写小写

4\. memory像一个大脑，帮助框架记住你之前说了些什么，形成连贯的对话

下面开始：

1\. 先用pip安装qw-agent和dashscope，前者是agent，tool，memory等核心组件的定义和调度，相当于框架；后者是链接llm模型工具，为阿里云提供的调用旗下大模型的工具包，帮助框架可以使用大模型的插件

\`\`\`bash

pip install qwen-agent

pip install dashscope

\`\`\`

随后创建文件输入代码

\`\`\`python

import os

import json5

from qwen\_agent.agents import Assistant

from qwen\_[agent.tools](http://agent.tools).base import BaseTool, register\_tool

\# ======================

\# 1. 自定义工具1：字符串转大写

\# ======================

@register\_tool('string\_to\_upper')

class StringToUpperTool(BaseTool):

\# 工具描述（需清晰，供LLM识别）

description = 'Convert input string to uppercase, input text content, return uppercase result.'

\# 参数定义（严格按官方格式）

parameters = \[{

'name': 'text',

'type': 'string',

'description': 'Original string that needs to be converted to uppercase',

'required': True

}\]

\# 工具执行逻辑（接收JSON字符串参数，返回JSON字符串结果）

def call(self, params: str, \*\*kwargs) -> str:

try:

\# 解析LLM传入的JSON参数

params\_dict = json5.loads(params)

text = params\_dict.get('text', '')

\# 返回JSON格式结果

return json5.dumps({'result': text.upper()}, ensure\_ascii=False)

except Exception as e:

return json5.dumps({'error': str(e)}, ensure\_ascii=False)

\# ======================

\# 2. 自定义工具2：两数求和

\# ======================

@register\_tool('add\_two\_numbers')

class AddTwoNumbersTool(BaseTool):

description = 'Calculate the sum of two numbers, input two numbers, return their sum.'

parameters = \[

{

'name': 'num1',

'type': 'number',

'description': 'First number (integer or float)',

'required': True

},

{

'name': 'num2',

'type': 'number',

'description': 'Second number (integer or float)',

'required': True

}

\]

def call(self, params: str,\*\* kwargs) -> str:

try:

params\_dict = json5.loads(params)

num1 = float(params\_dict.get('num1', 0))

num2 = float(params\_dict.get('num2', 0))

sum\_result = num1 + num2

return json5.dumps({'result': sum\_result}, ensure\_ascii=False)

except Exception as e:

return json5.dumps({'error': str(e)}, ensure\_ascii=False)

\# ======================

\# 3. 配置智能体（加载自定义工具）

\# ======================

os.environ\["DASHSCOPE\_API\_KEY"\] = "sk-bc3f12d9586442e78914edacf6895a9a" # 替换为真实Key

bot = Assistant(

llm={'model': 'qwen-max', 'model\_server': 'dashscope'},

\# 系统提示：明确告诉LLM可用工具及用途

system\_message='You can use "string\_to\_upper" to convert string to uppercase, and "add\_two\_numbers" to calculate sum of two numbers. Return results in natural language.',

function\_list=\['string\_to\_upper', 'add\_two\_numbers'\] # 加载自定义工具

)

\# ======================

\# 4. 交互测试

\# ======================

messages = \[\]

while True:

query = input('\\nuser question (input "exit" to quit): ')

if query.lower() == 'exit':

break

messages.append({'role': 'user', 'content': query})

response = \[\]

for chunk in [bot.run](http://bot.run)(messages=messages):

response.extend(chunk)

print('bot response:', chunk)

messages.extend(response)

\`\`\`

输入hello qwegent返回

!\[\[Pasted image 20251202235054.png\]\]

![Pasted image 20251202235054.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/younggogogo/images/2025-12-02-1764690858371-Pasted_image_20251202235054.png)

算数100+200

!\[\[Pasted image 20251202235240.png\]\]

![Pasted image 20251202235240.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/younggogogo/images/2025-12-02-1764690835191-Pasted_image_20251202235240.png)

简单搞定，半知半解继续努力
<!-- DAILY_CHECKIN_2025-12-02_END -->

# 2025-12-01
<!-- DAILY_CHECKIN_2025-12-01_START -->




\# \*下面是今天的任务

\- 使用自己熟悉的语言完成一次 Qwen API 调用。

\- 熟悉 Qwen 的基础参数、模型选择方式。

\- 写一个最小脚本（Node.js / Python 均可）：

\- 输入一段提示词，请 Qwen 生成一段对 ZetaChain 的介绍。

\- 在终端打印返回内容。

\- 在笔记中记录：你选择了哪个模型？调用参数是怎样的？

\###### 什么是模型？模型是通过海量数据训练出来的参数合集，相当于ai的大脑，通过学习数据里面的规律，具备理解语言和生成内容的能力。

\###### 千问有着很多模型可选，比如qwen-7b，千问21-b等，不同模型擅长的东西不同，我在这里选择了qwen-turbo，一个额度免费的轻量级通用模型，适合今天的基础问答任务

\###### 调用千问api时，需要设置一些参数来控制模型的生成行为，核心参数包括

\- model：指定使用的模型（`qwen-turboqwen-plus`）

\- `prompt/messages`：输入的提示词（prompt/message）（前者是给文本指令，根据文本生成内容，适合一次性问答，后者是像聊天一样，模型会根据之前的问答生成答复，支持多轮交互，模型可以记住之前交互的内容）

\- `temperature`：生成随机性（0~1，值越高越随机）（数值越高模型越容易放飞自我，月能看到意想不到的内容，越低则每次回答都差不多）（数值越高选词的跳脱程度越离谱）

\- `max_tokens`：生成的最大字符数

\- `top_p`：核采样阈值（控制生成多样性）（这个涉及到一个生成词语选择范围，有一个词语选择库，数指越大的话选词的池子越大）

\###### 我调用的参数是

\- `model`: qwen-turbo

\- `temperature`: 0.7（平衡确定性与多样性）

\- `max_tokens`: 500（限制生成长度）

\- `messages`: 用户指令 “请介绍 ZetaChain”

代码 如下 \`\`

\`\`\`python

import requests

\# API配置（需替换为自己的API Key，从阿里云/千问平台获取）

API\_KEY = "your-api-key" API\_URL = "[https://dashscope.aliyuncs.com/compatible-mode/v1/chat/completions](https://dashscope.aliyuncs.com/compatible-mode/v1/chat/completions)"

\# 请求参数

headers = { "Authorization": f"Bearer {API\_KEY}", "Content-Type": "application/json" }

data = { "model": "qwen-turbo", # 选择的模型：轻量版

"messages": \[ {"role": "user", "content": "请介绍ZetaChain"} \], "temperature": 0.7, # 生成随机性

"max\_tokens": 500 # 最大生成长度 }

\# 发送请求并打印结果

response = [requests.post](http://requests.post)(API\_URL, headers=headers, json=data)

result = response.json()\["choices"\]\[0\]\["message"\]\["content"\] print(result)

\`\`\`

!\[\[Pasted image 20251201223329.png\]\]

今天就那么多，本来还想继续看技术文档，根本没时间，好梦！

![Pasted image 20251201223329.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/younggogogo/images/2025-12-01-1764600039468-Pasted_image_20251201223329.png)
<!-- DAILY_CHECKIN_2025-12-01_END -->

# 2025-11-30
<!-- DAILY_CHECKIN_2025-11-30_START -->





昨天受挫后今天开始阅读技术文档，不再借助ai概括，重构对zetachain的认识

先从 _get started_ 第一篇文章开始

\#### zetachain的使命

zetachain的使命是建造一个原生支持链接其他区块链的通用区块链网络，使得加密货币像互联网一样容易获取，开发和互联。是为了解放目前被技术困在信息孤岛里的人和边界，目前的困境不利于承诺的去中心化的未来。

zetachain展望了一个没有边界，区块链技术满足他创始之初的愿望--一个开放无边界的未来。希望通过建造通用区块链，自己能变成一个去中心化世界的入口，为所有人解锁真正的自由和互联.

今天就一段明儿再继续先完成第七天任务

\---

**学习目标**

\- 梳理在 ZetaChain 上能做的通用 DeFi 模式。

\- 为黑客松赛道准备 1–2 个感兴趣的方向。

\- 写出 1–2 个可能的通用 DeFi 项目 idea，包括：

\- 目标用户

\- 想解决的问题

\- 粗略的跨链 / 通用资产使用方式

**zetachain的优势**

1\. 通用资产支持

2\. 原生跨链信息

3\. 单链开发多链条部署

结合这些我让ai给我生成了两个idea：

Idea 1：跨链极简 Swap DApp

1\. 目标用户：需要快速跨链兑换原生资产（如 ETH、BTC、USDT）的普通用户，尤其是不想频繁操作跨链桥的初学者。

2\. 想解决的问题：传统跨链兑换需多次桥接 + 单链兑换，步骤繁琐、手续费高（累计 20+ 美元）、耗时长（30+ 分钟），用户操作门槛高。

3\. 粗略的跨链 / 通用资产使用方式：

\- 用户在 Flask 搭建的前端选择输入链（Ethereum）、输入资产（ETH）、输出链（BSC）、输出资产（USDT），填写 BSC 接收地址；

\- 前端通过 ZetaChain SDK 生成资产转入地址，用户向该地址转入 ETH；

\- ZetaChain 自动将 ETH 包装为 ZRC-20 ETH，触发 Swap 合约（Hardhat 部署在 ZetaChain）；

\- 合约通过 ZetaChain 跨链流动性接口完成 ZRC-20 ETH 与 ZRC-20 USDT 的兑换；

\- 跨链消息将 ZRC-20 USDT 解包为 BSC 原生 USDT，自动转入用户接收地址；

\- Flask 后端实时查询交易状态，在前端展示 “处理中 / 兑换成功 / 失败”。

Idea 2：跨链收益聚合器

1\. 目标用户：持有多链稳定币（USDT、USDC），追求低风险高收益，但缺乏多链理财经验的用户。

2\. 想解决的问题：用户手动在多链配置理财矿池（如 Aave、PancakeSwap），步骤繁琐、收益分散，提现需多次跨链，资金闲置时间长。

3\. 粗略的跨链 / 通用资产使用方式：

\- 用户在前端授权将 Ethereum/BSC 上的 USDT 转入聚合器合约（ZetaChain 上）；

\- 合约通过 ZetaChain 通用资产标准，将不同链的 USDT 统一包装为 ZRC-20 USDT 并归集；

\- 按预设策略（60% 投 Aave 以太坊矿池，40% 投 PancakeSwap BSC 矿池），通过跨链消息将资金分配到对应矿池；

\- 后端每天定时提取各矿池收益，汇总后兑换为 USDT；

\- 用户可在前端选择任意链提现，合约通过跨链消息解包资产并转账。

\---

下面是明天的学习目标，读技术文档，复现几个例子为后面黑客松打下基础
<!-- DAILY_CHECKIN_2025-11-30_END -->

# 2025-11-29
<!-- DAILY_CHECKIN_2025-11-29_START -->






今天不行，两个小时没跑通swap，拿水卡了半天，拿了水安装例子安装了半天，最后各种出问题，明天继续弄！今天失败
<!-- DAILY_CHECKIN_2025-11-29_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->







**学习目标**

\- 理解 ZRC-20、Universal Token / NFT 的基本概念和作用。

\- 明白多链资产在 ZetaChain 上如何被统一表示。

写出

1\. ZRC-20 和普通 ERC-20 的直观区别（从开发者视角）。

2\. 举一个「通用资产」可能的应用场景（比如跨链储蓄、通用 NFT 通行证等）

\---

对于开发者而言

\- ERC20的部署需要在每条链上单独进行，而ZRC20一次部署就可以全链条通用

\- 交互上，如果调用erc20代币的话，只能适配单链条，而zrc20可以适用于全链用户无需额外适配

\- zrc20支持erc20不支持的原生跨链

\- 对于开发者来说，zrc20相当于是所有数据都记录在了中心数据库上（zetachain），所有链条都可以调用

\---

我能想到的是跨链资产借贷，比如我抵押bsc上的zrc20代币，就可以借贷以太坊上的usdt
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->








\- 建立对 “全链应用 / Universal App 合约” 的直观理解。

\- 清楚后面要实现的 Hello World / Demo 会包含哪些模块（合约 + 前端 + RPC）。

\- 在笔记中写出：

\- 自己想做的第一个 Universal App 想实现的“打印 / 记录 / 简单逻辑”是什么。

\- 为后续的 Hello World Demo 决定一种工作流：

\- 使用 CLI + Hardhat / Foundry？

\- 用本地链还是测试网？

\---

今天通过ai大概对全链应用有了一个更深刻的理解，大概说说后面要实现的hello world需要的模块理解

合约相当于在去中心化网络的后端部分，然后前端是方便自己使用的（不需要再去去中心化网络里找合约），然后RPC是链接彼此之间的连线，方便自己前端与合约的交流

我想我第一个通用app想做的一个跨链留言板，比如在以太坊输入一条信息，在其他测试网络能看到这个消息

后续hello world的工作流，ai给我的建议是cli加hardhat 说这种方式会最适合小白，然后用本地链，今天就这样，仓促完成任务明天继续加油！
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->









1\. 开始今天任务前先把昨天没完成的安装foundry继续安装，又试了很多方法，最后是梯子挂了全局代理，秒ok，不知道为什么局部代理就不行

2\. 然后到描述未来两周的学习目标，我想对于来说，未来两周的学习目标就是努力把zatachain的文档看一次，把不懂的知识点查明白，补足基础，然后按照每天学习路径走。

3\. 然后继续安装昨天的 Zetachain CLI，这里才发现自己理解错了，昨天应该安装的CLI前置不应该安装在window，应该把他安装在ubuntu里面，然后又要推翻重来

4\. 安装成功后试图完成昨天的在终端完成一次终端请求

5\. 测试连通性的话一次就过，先去百炼云申请一个api接口，让ai帮你生成一段curl命令，然后替换好api发送即可

\---

然后开始今天的任务

\- 理解 “通用区块链 / Universal EVM / Universal App / Omnichain Smart Contract” 的基本含义。

\- 能用图的方式画出：ZetaChain + 多条公链 + Gateway 的大致结构。

作业为

用自己的话写出：

\- Universal App 是什么？

\- Gateway 大概做什么？

\- 画一张简单的架构图：

ZetaChain 中心 + Bitcoin / Ethereum / Solana 等外围链 + Gateway。

通用区块链相当于一个所有主流链的纽带，比如联通不同银行的支付宝

geteway:不同链与zetachain的专用接口，负责翻译信息，传递资金，验证操作

Universal App：一个可以适用于所有银行app（区块链系统）的软件

Omnichain Smart Contract：一个全链条都通用的智能合约，比如通过zetachain制定，所有链同步的一个规则玩法

Universal EVM：一个万能播放器，比如能让以太坊的的代码应用在其他所有区块链上跑，而不用修改任何东西

\## 架构图##

!\[\[Pasted image 20251126223139.png\]\]

![Pasted image 20251126223139.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/younggogogo/images/2025-11-26-1764167769607-Pasted_image_20251126223139.png)
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->










任务有

\- 安装或尝试使用 ZetaChain CLI（本地或云端环境均可）。

\- 了解测试网 RPC、Faucet、Explorer 的入口，记录在自己的笔记中。

\- 在终端或 Postman 里完成一次 Qwen API 的简单请求（哪怕只是 200 报错也可以，先打通连通性）。

\- 能描述自己接下来 2 周的学习目标。

\---

先从安装CLI的前置开始

分别有

1\. node.js（应用的运行引擎，相当于CLI的操作系统）

2\. npm（软件商店，下载依赖包）

3\. git(管理代码，保存每一次修改，或者方便合作)

4\. jq（数据翻译官，将链上json格式数据变成人类可读格式）

5\. foundry（合约的编译器，测试器）

此我的安装顺序为git，npm，nodejs，jq，foundry

安装npm我出现了报错，需要将nodejs卸载了才能进行，然后用npm去安装对应版本nodejs

jq用的是window包管理器choco进行

foundry的话建议用WSL2(window自带的linux系统安装，原生环境不好用)

PS：太折磨人了，卡在这步俩小时，最后莫名其妙安装上了WSL2,然后就又卡住了，安装foundry超级无敌慢

在等待的时候就做下一个任务

\----

任务二

explore总入口如下

[https://www.zetachain.com/docs/reference/explorers/](https://www.zetachain.com/docs/reference/explorers/)

faucet总入口如下

[https://www.zetachain.com/docs/reference/faucet](https://www.zetachain.com/docs/reference/faucet)

RPC总入口如下

[https://www.zetachain.com/docs/reference/network/api](https://www.zetachain.com/docs/reference/network/api)

\---

任务三

先去阿里云百炼注册，然后再API-KEY管理创建

然后跑回你的Ubuntu窗口发送api请求，我这里让ai帮我生成了一段

curl -X POST [https://dashscope.aliyuncs.com/api/v1/chat/completions](https://dashscope.aliyuncs.com/api/v1/chat/completions) \\ -H "Content-Type: application/json" \\ -H "Authorization: Bearer sk-bc3f12d9586442e78914edacf6895a9a" \\ -d '{ "model": "qwen-turbo", "messages": \[{"role": "user", "content": "Hello Qwen, 测试连通性"}\] }'

但此时临近十二点，发送过去没成功，今天就先这样
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->











任务一：

注册 / 配置好 ZetaChain 开发所需环境（浏览 Docs，确认能访问 Developers 页面）。

简单浏览了zetachain的文档，看不太懂，但能正常打开，方便明天开始环境配置

任务二：

注册Qwen并确认可以进入控制台

注册成功进入控制台

完成

任务三：加入中国开发者社区

完成情况，成功加微信
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
