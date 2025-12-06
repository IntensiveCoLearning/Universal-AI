---
timezone: UTC+8
---

# 张为东

**GitHub ID:** siemens84cn

**Telegram:** @siemens84cn

## Self-introduction

程序员，web3爱好者

## Notes

<!-- Content_START -->
# 2025-12-06
<!-- DAILY_CHECKIN_2025-12-06_START -->
# 2025-12-06学习笔记

1.  项目名称：链上理财宝
    
2.  描述：全链收益优化协议 - 让您的资产自动在所有区块链上寻找最佳收益机会
    
3.  目标用户：
    
    1.  DeFi散户投资者（60%）
        
        -   持有$1,000 - $50,000资产
            
        -   希望获得稳定收益但不想复杂操作
            
        -   痛点：不懂跨链、Gas 费高、收益率监控困难
            
    2.  DeFi老手/"收益农民"（30%）
        
        -   持有$50,000+资产
            
        -   追求最大化收益
            
        -   痛点：手动跨链费时费力、流动性分散、机会成本高
            
    3.  DAO金库管理者（10%）
        
        -   管理数百万美元资产
            
        -   需要安全稳健的收益策略
            
        -   痛点：多链资产管理复杂、审计困难
            
4.  使用场景：
    
    1.  个性化理财助手，这个场景为了解决流动性碎片化问题（流动性在不同链上变得分散，限制了交易、借贷和质押机会。由于流动性在每条链上被隔离，用户面临受限的市场访问，导致流动性水平降低。）
        
    2.  链上转账助手，这个场景为了解决手动跨链的复杂性和成本问题（资产被困在不同的区块链上，无法轻松互动。这导致每个池子变得更小，价格随着每次交易波动更大，导致更高的滑点和更少的交易机会。）
        
    3.  智能收益分析助手，这个场景为了解决收益率追踪困难问题（各链分别在各自平台上实时展示收益率变化，投资者面临在多个平台监控收益率的问题。）
        
5.  核心功能：
    
    -   一键全链收益优化
        

```
用户操作：
1. 连接钱包
2. 存入USDT/USDC/ETH
3. 选择风险偏好（保守/平衡/激进）
4. 点击"开始赚收益"

后台自动：
✅ 扫描 Ethereum、BSC、Polygon、Optimism、Base 等链的收益率
✅ 自动将资产部署到最高收益的协议
✅ 定期再平衡（每日/每周）
✅ 收益自动复投
```

-   智能路由引擎
    

javascript

```
// 伪代码示例
function findBestYield(asset, amount, riskLevel) {
    // 实时扫描所有链的收益协议
    const opportunities = [
        { chain: "Ethereum", protocol: "Aave", apy: 4.5%, risk: "low" },
        { chain: "BSC", protocol: "Venus", apy: 7.8%, risk: "medium" },
        { chain: "Polygon", protocol: "Aave", apy: 6.2%, risk: "low" },
        { chain: "Base", protocol: "Moonwell", apy: 8.5%, risk: "medium" }
    ];
    
    // 考虑 Gas 费和收益率
    const netAPY = calculateNetReturn(opportunity, amount);
    
    // 根据风险偏好筛选
    return selectBestOption(opportunities, riskLevel);
}
```

-   Universal Token 自动再平衡
    

当某条链的收益率变化时，自动跨链转移资产：

```
场景：用户在 Ethereum Aave 存了 10,000 USDT (4.5% APY)
      
第 3 天：Base Moonwell 的 APY 涨到 9.2%

OmniYield 自动：
1. 从 Ethereum Aave 赎回 USDT
2. 通过 ZetaChain 转移到 Base
3. 存入 Base Moonwell
4. 用户收益立即提升到 9.2%
```

6.  技术路径
    

前端层（用户界面） ------ Qwen AI智能层 ------ ZetaChain 智能合约层 ------ 外部链 DeFi 协议层

# 2025-12-05学习笔记

目前实现是仅仅打印出计划执行的链上操作，在测试网的真实交易操作暂没有实现

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/siemens84cn/images/2025-12-06-1765026050462-image.png)
<!-- DAILY_CHECKIN_2025-12-06_END -->

# 2025-12-05
<!-- DAILY_CHECKIN_2025-12-05_START -->

先打个卡，明天再补
<!-- DAILY_CHECKIN_2025-12-05_END -->

# 2025-12-04
<!-- DAILY_CHECKIN_2025-12-04_START -->


# 2025-12-03学习笔记

-   设计一个工具：`parse_swap_intent(text)`，返回结构化 JSON，例如：`{ "chain": "base", "tokenIn": "USDC", "tokenOut": "ETH", "amount": "10" }`
    
-   让 Agent 能处理以下输入：
    
    -   “帮我在 Base 上用 10 USDC 换成 ETH”
        
    -   “把我 50 U 兑换成 Polygon 上的 MATIC”
        

```
#Python
import json5
import requests
from qwen_agent.agents import Assistant
from qwen_agent.tools.base import BaseTool, register_tool

# Qwen 模型配置
llm_cfg = {
    # DashScope 提供的兼容 OpenAI 的模型服务
    'model': 'qwen-plus',
    'model_server': 'https://dashscope.aliyuncs.com/compatible-mode/v1',
    'api_key': 'Bearer QWEN-API-KEY',
}

# 系统提示词：让 Qwen 作为总控助手，必要时调用 parse_swap_intent 工具做结构化解析
system = (
    '你是一个帮助用户进行加密货币代币兑换咨询的智能助手。'
    '当用户给出诸如“帮我在 Base 上用 10 USDC 换成 ETH”这样的自然语言描述时，'
    '你需要调用工具 parse_swap_intent，将用户意图解析为 JSON 结构化数据：'
    '{"chain": "base", "tokenIn": "USDC", "tokenOut": "ETH", "amount": "10"}。'
    '请尽量保持链名为小写，代币符号为大写，amount 使用字符串表示原始数值。'
)


# 自定义工具：parse_swap_intent
@register_tool('parse_swap_intent')
class ParseSwapIntent(BaseTool):
    """
    通过调用 Qwen API，对用户输入的自然语言文本进行意图识别，
    提取链名称、输入代币、输出代币及金额，并返回结构化 JSON。
    """

    description = (
        '解析用户关于代币兑换的自然语言描述，'
        '例如：“帮我在 Base 上用 10 USDC 换成 ETH”，'
        '返回结构化 JSON：{"chain": "base", "tokenIn": "USDC", "tokenOut": "ETH", "amount": "10"}。'
    )

    # 工具入参定义
    parameters = [{
        'name': 'text',
        'type': 'string',
        'description': '用户输入的原始自然语言文本，例如：“帮我在 Base 上用 10 USDC 换成 ETH”。',
        'required': True
    }]

    def _call_qwen(self, text: str) -> dict:
        """
        直接调用 Qwen 的 OpenAI 兼容接口，让大模型只输出目标 JSON 结构。
        """
        url = f"{llm_cfg['model_server'].rstrip('/')}/chat/completions"
        headers = {
            'Authorization': llm_cfg['api_key'],
            'Content-Type': 'application/json',
        }
        system_prompt = (
            '你是一个意图解析工具，只负责从用户的语句中提取代币兑换信息。'
            '请严格按照下面的 JSON 结构输出，且整个回复只能是一个合法的 JSON：'
            '{"chain": "base", "tokenIn": "USDC", "tokenOut": "ETH", "amount": "10"}。'
            '其中：'
            '1. chain：区块链网络名称，全部小写，例如 base、ethereum、arbitrum。'
            '2. tokenIn：用户用来兑换的代币符号，全部大写，例如 USDC、USDT、ETH。'
            '3. tokenOut：用户想要得到的代币符号，全部大写。'
            '4. amount：用户希望兑换的数量，直接使用原始数字文本，作为字符串返回。'
            '如果用户表达不清楚，也要尽量根据语义做出合理猜测。'
        )
        payload = {
            'model': llm_cfg['model'],
            'messages': [
                {'role': 'system', 'content': system_prompt},
                {'role': 'user', 'content': text},
            ],
            'temperature': 0.0,
        }

        resp = requests.post(url, headers=headers, json=payload, timeout=15)
        resp.raise_for_status()
        data = resp.json()

        # 解析 Qwen 返回的 content
        content = data['choices'][0]['message']['content']
        try:
            parsed = json5.loads(content)
        except Exception:
            # 如果解析失败，包一层错误信息返回
            parsed = {
                'error': 'FAILED_TO_PARSE_QWEN_RESPONSE',
                'raw': content,
            }
        return parsed

    def call(self, params: str, **kwargs) -> str:
        """
        Qwen-Agent 调用工具的入口。
        """
        args = json5.loads(params)
        text = args.get('text', '')

        if not text:
            result = {
                'error': 'EMPTY_TEXT',
                'message': '参数 text 不能为空',
            }
            return json.dumps(result, ensure_ascii=False)

        try:
            parsed = self._call_qwen(text)
        except Exception as e:
            parsed = {
                'error': 'QWEN_API_CALL_FAILED',
                'message': str(e),
            }

        return json5.dumps(parsed, ensure_ascii=False)

tools = ['parse_swap_intent', 'code_interpreter']  # code_interpreter 是 Qwen-Agent 内置工具
bot = Assistant(llm=llm_cfg,
                system_message=system,
                function_list=tools)

messages = []
while True:
    query = input('请你提问: ')
    messages.append({'role': 'user', 'content': query})
    responses = list(bot.run(messages=messages))
    if responses:
        print('bot final response:', responses[-1][1].get('content'))
        break;
```

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/siemens84cn/images/2025-12-04-1764854127856-image.png)

# 2025-12-04学习笔记

-   写出一个后端伪代码或简单实现：
    
    -   接收 `parse_swap_intent` 返回值；
        
    -   根据不同链 / 不同 token 选择具体的合约 / 调用方式；
        
    -   暂时可以只在控制台打印“准备发起什么交易”。
        

```
#Python
import json5
import requests
from qwen_agent.agents import Assistant
from qwen_agent.tools.base import BaseTool, register_tool

# Qwen 模型配置
llm_cfg = {
    # DashScope 提供的兼容 OpenAI 的模型服务
    'model': 'qwen-plus',
    'model_server': 'https://dashscope.aliyuncs.com/compatible-mode/v1',
    'api_key': 'Bearer QWEN-API-KEY',
}

# 系统提示词：让 Qwen 作为总控助手，必要时调用 parse_swap_intent 工具做结构化解析
system = (
    '你是一个帮助用户进行加密货币代币兑换咨询的智能助手。'
    '当用户给出诸如“帮我在 Base 上用 10 USDC 换成 ETH”这样的自然语言描述时，'
    '你需要调用工具 parse_swap_intent，将用户意图解析为 JSON 结构化数据：'
    '{"chain": "base", "tokenIn": "USDC", "tokenOut": "ETH", "amount": "10"}。'
    '请尽量保持链名为小写，代币符号为大写，amount 使用字符串表示原始数值。'
)


# 自定义工具：parse_swap_intent
@register_tool('parse_swap_intent')
class ParseSwapIntent(BaseTool):
    """
    通过调用 Qwen API，对用户输入的自然语言文本进行意图识别，
    提取链名称、输入代币、输出代币及金额，并返回结构化 JSON。
    """

    description = (
        '解析用户关于代币兑换的自然语言描述，'
        '例如：“帮我在 Base 上用 10 USDC 换成 ETH”，'
        '返回结构化 JSON：{"chain": "base", "tokenIn": "USDC", "tokenOut": "ETH", "amount": "10"}。'
    )

    # 工具入参定义
    parameters = [{
        'name': 'text',
        'type': 'string',
        'description': '用户输入的原始自然语言文本，例如：“帮我在 Base 上用 10 USDC 换成 ETH”。',
        'required': True
    }]

    def _call_qwen(self, text: str) -> dict:
        """
        直接调用 Qwen 的 OpenAI 兼容接口，让大模型只输出目标 JSON 结构。
        """
        url = f"{llm_cfg['model_server'].rstrip('/')}/chat/completions"
        headers = {
            'Authorization': llm_cfg['api_key'],
            'Content-Type': 'application/json',
        }
        system_prompt = (
            '你是一个意图解析工具，只负责从用户的语句中提取代币兑换信息。'
            '请严格按照下面的 JSON 结构输出，且整个回复只能是一个合法的 JSON：'
            '{"chain": "base", "tokenIn": "USDC", "tokenOut": "ETH", "amount": "10"}。'
            '其中：'
            '1. chain：区块链网络名称，全部小写，例如 base、ethereum、arbitrum。'
            '2. tokenIn：用户用来兑换的代币符号，全部大写，例如 USDC、USDT、ETH。'
            '3. tokenOut：用户想要得到的代币符号，全部大写。'
            '4. amount：用户希望兑换的数量，直接使用原始数字文本，作为字符串返回。'
            '如果用户表达不清楚，也要尽量根据语义做出合理猜测。'
        )
        payload = {
            'model': llm_cfg['model'],
            'messages': [
                {'role': 'system', 'content': system_prompt},
                {'role': 'user', 'content': text},
            ],
            'temperature': 0.0,
        }

        resp = requests.post(url, headers=headers, json=payload, timeout=15)
        resp.raise_for_status()
        data = resp.json()

        # 解析 Qwen 返回的 content
        content = data['choices'][0]['message']['content']
        try:
            parsed = json5.loads(content)
        except Exception:
            # 如果解析失败，包一层错误信息返回
            parsed = {
                'error': 'FAILED_TO_PARSE_QWEN_RESPONSE',
                'raw': content,
            }
        return parsed

    def call(self, params: str, **kwargs) -> str:
        """
        Qwen-Agent 调用工具的入口。
        """
        args = json5.loads(params)
        text = args.get('text', '')

        if not text:
            result = {
                'error': 'EMPTY_TEXT',
                'message': '参数 text 不能为空',
            }
            return json5.dumps(result, ensure_ascii=False)

        try:
            parsed = self._call_qwen(text)
        except Exception as e:
            parsed = {
                'error': 'QWEN_API_CALL_FAILED',
                'message': str(e),
            }

        return json5.dumps(parsed, ensure_ascii=False)

tools = ['parse_swap_intent', 'code_interpreter']  # code_interpreter 是 Qwen-Agent 内置工具
bot = Assistant(llm=llm_cfg,
                system_message=system,
                function_list=tools)

messages = []
while True:
    query = input('请你提问: ')
    messages.append({'role': 'user', 'content': query})
    responses = list(bot.run(messages=messages))

    # 1. 从 Qwen-Agent 的运行结果中，找到 parse_swap_intent 工具的返回值（JSON 字符串）
    swap_intent_raw = None
    for item in responses:
        # 根据 qwen_agent 的输出格式，这里通常是 (idx, message_dict)
        if isinstance(item, (list, tuple)) and len(item) >= 2:
            msg = item[1]
        else:
            msg = item

        if not isinstance(msg, dict):
            continue

        # 工具调用结果通常是 role == 'function'，name 为工具名
        if msg.get('role') == 'function' and msg.get('name') == 'parse_swap_intent':
            swap_intent_raw = msg.get('content')
            break

    if not swap_intent_raw:
        print('未能从模型结果中获取到 parse_swap_intent 的返回值，请检查对话和工具配置。')
        break

    # 2. 解析 JSON 结构体（链、代币、数量等）
    try:
        intent = json5.loads(swap_intent_raw) if isinstance(swap_intent_raw, str) else swap_intent_raw
    except Exception:
        print('parse_swap_intent 返回内容无法解析为 JSON：', swap_intent_raw)
        break

    chain = intent.get('chain', '')
    token_in = intent.get('tokenIn', '')
    token_out = intent.get('tokenOut', '')
    amount = intent.get('amount', '')

    # 2.1 根据不同区块链 / 不同 token 选择具体的合约 / 调用方式（这里只做一个简单路由示例）
    # 实际项目中，你可以根据 ZetaChain 文档中不同链的合约地址与调用方式来完善：
    # https://www.zetachain.com/docs/developers
    route_key = f'{chain.lower()}_{token_in.upper()}_{token_out.upper()}'

    # 非常简化的“路由表”示例：key -> 合约 / 调用信息
    swap_routes = {
        'base_USDC_ETH': {
            'type': 'universal_swap',
            'contract': 'UniversalSwapOnZetaChain',
            'description': '在 Base 链上，从 USDC 兑换为 ETH，通过 ZetaChain 的 Universal Swap 合约发起跨链或本链交易。',
        },
        'ethereum_USDT_ETH': {
            'type': 'universal_swap',
            'contract': 'UniversalSwapOnZetaChain',
            'description': '在 Ethereum 链上，从 USDT 兑换为 ETH，通过 ZetaChain 的 Universal Swap 合约发起跨链或本链交易。',
        },
        'polygon_USDC_MATIC': {
            'type': 'universal_swap',
            'contract': 'UniversalSwapOnZetaChain',
            'description': '在 Polygon 链上，从 USDC 兑换为 MATIC，通过 ZetaChain 的 Universal Swap 合约发起跨链或本链交易。',
        },
        # 可以按需继续扩展其它链和代币组合...
    }

    route = swap_routes.get(route_key, {
        'type': 'unknown_route',
        'contract': 'Unknown',
        'description': '未在本地路由表中找到对应的链 / 代币组合，请在真实环境中补充具体合约调用逻辑。',
    })

    # 3. 在控制台打印“准备发起什么交易”的信息（仅模拟，不真正发交易）
    print('---------------- 准备发起交易 ----------------')
    print(f'源链(chain)        : {chain}')
    print(f'源代币(tokenIn)    : {token_in}')
    print(f'目标链(chain)      : {chain} (示例中暂使用同一条链，可按需扩展多链场景)')
    print(f'目标代币(tokenOut) : {token_out}')
    print(f'数量(amount)       : {amount}')
    print(f'路由类型(type)     : {route["type"]}')
    print(f'合约(contract)     : {route["contract"]}')
    print(f'说明(description)  : {route["description"]}')
    print('------------------------------------------------')

    # 当前 Demo 仅执行一次示例后退出循环
    break
```

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/siemens84cn/images/2025-12-04-1764856478364-image.png)
<!-- DAILY_CHECKIN_2025-12-04_END -->

# 2025-12-03
<!-- DAILY_CHECKIN_2025-12-03_START -->



先打个卡，手上任务有点多，明天合并一起打卡掉，抱歉哈
<!-- DAILY_CHECKIN_2025-12-03_END -->

# 2025-12-02
<!-- DAILY_CHECKIN_2025-12-02_START -->




# 2025-12-01学习笔记

-   写一个小脚本，实现对Qwen API的调用
    

```
#Python
import os
from openai import OpenAI

# ========== 1. 配置信息 ==========
# 请务必先设置环境变量 QWEN_API_KEY
# 在命令行执行：export QWEN_API_KEY="你的API密钥"
API_KEY = os.getenv("QWEN_API_KEY")
if not API_KEY:
    raise ValueError(
        "未找到API密钥。请先设置环境变量！\n"
    )

# Qwen服务的API地址
BASE_URL = "https://dashscope.aliyuncs.com/compatible-mode/v1"
# 指定使用的模型
MODEL_NAME = "qwen-turbo"
# 你的提问
USER_PROMPT = "关于ZetaChain，你能告诉我哪些关键信息？"

# ========== 2. 创建客户端 ==========
client = OpenAI(
    api_key=API_KEY,
    base_url=BASE_URL
)

# ========== 3. 定义API调用函数 ==========
def get_qwen_response(prompt, temperature=0.8):
    """向通义千问发送请求并获取响应"""
    try:
        print("正在调用Qwen API，请稍候...\n")
        completion = client.chat.completions.create(
            model=MODEL_NAME,
            messages=[
                {"role": "user", "content": prompt}
            ],
            temperature=temperature,  # 控制回答的创造性，0.0-1.0
        )
        return completion.choices[0].message.content
    except Exception as e:
        print(f"调用API失败: {e}")
        return None

# ========== 4. 执行调用 ==========
if __name__ == "__main__":
    response = get_qwen_response(USER_PROMPT)
    
    if response:
        print("-" * 50)
        print("Qwen 的回复：")
        print("-" * 50)
        print(response)
        print("-" * 50)
    else:
        print("未能获取到有效回复。")
```

-   选择了qwen-turbo模型，调用参数是prompt（提示词），temperature（创造性），api-key（授权用户）。
    

# 2025-12-02学习笔记

-   跑通一个Qwen-Agent 官方示例；
    

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/siemens84cn/images/2025-12-02-1764682995582-image.png)

-   自定义一个简单tool，实现计算两个数的和；
    

```
# ================= 自定义“工具”：计算两个数的和 =================

def add_numbers(a: float, b: float) -> float:
    """
    计算两个数的和。

    :param a: 第一个数
    :param b: 第二个数
    :return: 两数之和
    """
    return a + b


# 定义一个 OpenAI 风格的 tools 描述，供 LLM 进行函数调用（function calling）。
tools: List[Dict[str, Any]] = [
    {
        "type": "function",
        "function": {
            "name": "add_numbers",
            "description": "计算两个数的和。",
            "parameters": {
                "type": "object",
                "properties": {
                    "a": {
                        "type": "number",
                        "description": "第一个数"
                    },
                    "b": {
                        "type": "number",
                        "description": "第二个数"
                    }
                },
                "required": ["a", "b"]
            }
        },
    }
]

# 把工具名映射到真正的 Python 实现
available_functions = {
    "add_numbers": add_numbers,
}
```

-   确认 Agent 能自动调用这个 Tool 并返回结果；
    

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/siemens84cn/images/2025-12-02-1764683844139-image.png)
<!-- DAILY_CHECKIN_2025-12-02_END -->

# 2025-12-01
<!-- DAILY_CHECKIN_2025-12-01_START -->





先卡个卡，今天有点忙，明天抽空把这两天的任务都补上。
<!-- DAILY_CHECKIN_2025-12-01_END -->

# 2025-11-30
<!-- DAILY_CHECKIN_2025-11-30_START -->






1.  设计一个通用 DeFi 项目 idea，包括：目标用户、想解决的问题、粗略的跨链 / 通用资产使用方式。
    
2.  Idea名称：链上理财宝
    
3.  描述：全链收益优化协议 - 让您的资产自动在所有区块链上寻找最佳收益机会
    
4.  目标用户：
    
    1.  DeFi散户投资者（60%）
        
        -   持有$1,000 - $50,000资产
            
        -   希望获得稳定收益但不想复杂操作
            
        -   痛点：不懂跨链、Gas 费高、收益率监控困难
            
    2.  DeFi老手/"收益农民"（30%）
        
        -   持有$50,000+资产
            
        -   追求最大化收益
            
        -   痛点：手动跨链费时费力、流动性分散、机会成本高
            
    3.  DAO金库管理者（10%）
        
        -   管理数百万美元资产
            
        -   需要安全稳健的收益策略
            
        -   痛点：多链资产管理复杂、审计困难
            
5.  需要解决的问题：
    
    1.  流动性碎片化（流动性在不同链上变得分散，限制了交易、借贷和质押机会。由于流动性在每条链上被隔离，用户面临受限的市场访问，导致流动性水平降低。）
        
    2.  手动跨链的复杂性和成本（资产被困在不同的区块链上，无法轻松互动。这导致每个池子变得更小，价格随着每次交易波动更大，导致更高的滑点和更少的交易机会。）
        
    3.  收益率追踪困难（各链分别在各自平台上实时展示收益率变化，投资者面临在多个平台监控收益率的问题。）
        
6.  核心功能：
    
    -   一键全链收益优化
        

```
用户操作：
1. 连接钱包
2. 存入USDT/USDC/ETH
3. 选择风险偏好（保守/平衡/激进）
4. 点击"开始赚收益"

后台自动：
✅ 扫描 Ethereum、BSC、Polygon、Optimism、Base 等链的收益率
✅ 自动将资产部署到最高收益的协议
✅ 定期再平衡（每日/每周）
✅ 收益自动复投
```

-   智能路由引擎
    

javascript

```javascript
// 伪代码示例
function findBestYield(asset, amount, riskLevel) {
    // 实时扫描所有链的收益协议
    const opportunities = [
        { chain: "Ethereum", protocol: "Aave", apy: 4.5%, risk: "low" },
        { chain: "BSC", protocol: "Venus", apy: 7.8%, risk: "medium" },
        { chain: "Polygon", protocol: "Aave", apy: 6.2%, risk: "low" },
        { chain: "Base", protocol: "Moonwell", apy: 8.5%, risk: "medium" }
    ];
    
    // 考虑 Gas 费和收益率
    const netAPY = calculateNetReturn(opportunity, amount);
    
    // 根据风险偏好筛选
    return selectBestOption(opportunities, riskLevel);
}
```

-   Universal Token 自动再平衡
    

当某条链的收益率变化时，自动跨链转移资产：

```
场景：用户在 Ethereum Aave 存了 10,000 USDT (4.5% APY)
      
第 3 天：Base Moonwell 的 APY 涨到 9.2%

OmniYield 自动：
1. 从 Ethereum Aave 赎回 USDT
2. 通过 ZetaChain 转移到 Base
3. 存入 Base Moonwell
4. 用户收益立即提升到 9.2%
```
<!-- DAILY_CHECKIN_2025-11-30_END -->

# 2025-11-29
<!-- DAILY_CHECKIN_2025-11-29_START -->







-   在测试网跑通官方跨链Demo（Swap）
    

1.  [参考文档](https://www.zetachain.com/docs/developers/tutorials/swap)
    
2.  [源码](https://github.com/zeta-chain/example-contracts/tree/main/examples/swap)
    
3.  部署Swap合约，并发起跨链请求
    

```
npx hardhat run scripts/deploy.ts --network zeta_testnet
npx hardhat run scripts/swap.ts --network sepolia
```

-   用文字说明从哪里发起的调用？最终在ZetaChain上发生了什么？
    

1.  Demo的跨链请求，是从Ethereum Sepolia发起；
    
2.  在ZetaChain的几个关键步骤：
    

第一步：验证跨链请求是否合法。

第二步：调用在ZetaChain Testnet上部署的Swap合约。

第三步：执行Swap合约。

第四步：将目标token发送到我的ZetaChain Testnet地址上，并产生最终交易记录。
<!-- DAILY_CHECKIN_2025-11-29_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->








-   从开发者视角，说明ZRC-20 和普通 ERC-20 的直观区别；
    

| 特性 | ERC-20 | ZRC-20 |
| --- | --- | --- |
| 部署位置 | 单一区块链（如以太坊） | ZetaChain（但代表多链资产） |
| 跨链能力 | ❌ 需要外部桥 | ✅ 原生支持 |
| 代表资产 | 本链代币 | 外部链资产（如 ETH.ETH, BTC.BTC） |
| withdraw 函数 | ❌ 没有 | ✅ 提款到原生链 |
| Gas 费处理 | 只需本链 Gas | 自动处理目标链 Gas |

-   举一个「通用资产」可能的应用场景（比如跨链储蓄、通用 NFT 通行证等）；
    

我想到的是可以在GameFI领域使用「通用资产」，使得游戏角色的装备NFT可以在多个游戏和公链间任意的、自由的流转。

跨链游戏资产的应用场景：

1.  玩家在以太坊上mint游戏装备NFT。
    
2.  利用zatachain的技术，可以无缝转移到：
    

\- Polygon（低费用的游戏内交易）

\- BSC（在 BSC 的 NFT 市场出售）

\- Avalanche（参与其他游戏）

从而保持tokenId不变、元数据一致性不变、以及满足玩家对某链的依赖。
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->









-   自己想做的第一个 Universal App 想实现的“打印 / 记录 / 简单逻辑”是什么?
    

结合ZetaChain的特点，在以太坊上发起一个call（比如说，莎士比亚的一段话），通过gateway和zetaChain链的转发，能够在SUI链上接收到这个消息并记录到链上，具体实现逻辑明天设计一下。

-   为后续的 Hello World Demo 决定一种工作流：使用 CLI + Hardhat / Foundry？用本地链还是测试网？
    

我选择使用CLI+Foundry（个人技术栈熟悉），同时选择本地链（也会尝试在测试网上跑通这个Dapp）。
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->










-   Universal App 是什么？
    

它是部署在ZetaChain上的智能合约，作为一个Dapp（去中心化应用），它能够与包括以太坊、比特币、Solana 等在内的多个区块链无缝交互的应用程序。

-   Gateway 大概做什么？
    

它是一个服务接口，能够连接 任一运行在L1链上（以太坊、Solana等）的智能合约与 ZetaChain 上的通用应用程序之间交互的统一入口点，进而支持它们之间的合约调用和代币转移（交易）。

-   画一张简单的架构图：ZetaChain 中心 + Bitcoin / Ethereum / Solana 等外围链 + Gateway。
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->











-   ZetaChain CLI本地环境安装及使用：
    

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/siemens84cn/images/2025-11-25-1764077584427-image.png)

-   测试网 RPC、Faucet、Explorer 的入口参考下表：
    

| RPC | Faucet | Explorer |
| --- | --- | --- |
| https://www.zetachain.com/docs/reference/network/api | https://www.zetachain.com/docs/reference/faucet | https://www.zetachain.com/docs/reference/explorers |

-   Postman完成一次Qwen API的简单请求，截图参考：
    

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/siemens84cn/images/2025-11-25-1764077097175-image.png)
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->












## 开发环境准备步骤

### 1\. 区块链开发环境

-   安装 MetaMask 钱包
    
-   创建/导入钱包地址
    
-   添加 ZetaChain 网络到 MetaMask
    
-   获取测试网代币
    

### 2\. AI 开发环境

-   注册 Qwen 账号
    
-   申请 API Key
    
-   测试 API 调用
    

### 3\. 代码开发环境

-   安装 Node.js（推荐 v18+）
    
-   安装代码编辑器（VS Code & Cursor）
    
-   安装 Git
    
-   准备开发工具：
    
    -   Hardhat / Foundry（智能合约开发）
        
    -   ethers.js / web3.js（区块链交互）
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
