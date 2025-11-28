---
timezone: UTC+8
---

# Jacky

**GitHub ID:** jackysense

**Telegram:** @jacky_luxue

## Self-introduction

Web3 builder

## Notes

<!-- Content_START -->
# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->
zrc-20 是对其他链代币的映射zeta链代币资产，可以直接交换其他链的映射，用于跨链交换，erc20以evm链的代币

一个例子：一个银行存钱取钱的，兑换外汇，还可以抵押贷
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->

1.  通用app，打印出输入记录，输出，tx记录，错误信息，输出信息到对话输入框，通过ai转成参数，调用记录。
    
2.  使用 CLI + Foundry
    
3.  优先使用测试网 ，初步是base + zeta测试能通
    

**Qwen agent:**

```python

import os
import json5
from qwen_agent.tools.base import BaseTool, register_tool
from qwen_agent.agents import Assistant


# 步骤 1：配置您所使用的 LLM
llm_cfg = {
    # 使用 DashScope 提供的模型服务：
    'model': 'qwen-max',
    'model_server': 'dashscope',
    'api_key': os.getenv('DASHSCOPE_API_KEY'),
    # 如果这里没有设置 'api_key'，它将读取 `DASHSCOPE_API_KEY` 环境变量。

    # 使用与 OpenAI API 兼容的模型服务，例如 vLLM 或 Ollama：
    # 'model': 'Qwen2-7B-Chat',
    # 'model_server': 'http://localhost:8000/v1',  # base_url，也称为 api_base
    # 'api_key': 'EMPTY',
    # （可选） LLM 的超参数：
    'generate_cfg': {
        'top_p': 0.8
    }
}

# 步骤 2：创建一个智能体。这里我们以 `Assistant` 智能体为例，它能够使用工具并读取文件。
system_instruction = '你是一位区块链专家，根据你的经验来精准的回答用户提出的问题,根据用户的意图，适当调用工具，返回json格式'


@register_tool('mul_tool')
class MulTool(BaseTool):
    description = 'Multiply two numbers: a * b'
    parameters = [{
        'name': 'a',
        'type': 'float',
        'description': 'first  parameter',
        'required': True,
    },
    {
        'name': 'b',
        'type': 'float',
        'description': 'second  parameter',
        'required': True,
    }
    ]
    def call(self, params: str, **kwargs) -> str:
        print('params', params)  # {"a": 3.5, "b": 4}
        a = json5.loads(params)['a']
        b = json5.loads(params)['b']
        result = a * b  # 乘法运算
        print('call mul_tool tool', a, b, 'result:', result)
        # 返回字符串格式的结果
        return str(result)

@register_tool('upper_case')
class upper_case(BaseTool):
    description = 'upper case string'
    parameters = [{
        'name': 'text',
        'type': 'string',
        'description': 'first  parameter',
        'required': True,
    }
   
    ]
    def call(self, params: str, **kwargs) -> str:
        print('params', params)  # {"a": 3.5, "b": 4}
        text = json5.loads(params)['text']    
        result =  text.upper()   # 乘法运算
        print('call upper_case tool','result:', result)
        # 返回字符串格式的结果
        return str(result)

tools = ['mul_tool','upper_case']  # 使用已注册的工具名称字符串

bot = Assistant(llm=llm_cfg,
                system_message=system_instruction,
                function_list=tools)

# 步骤 3：作为聊天机器人运行智能体。
messages = []  # 这里储存聊天历史。
while True:
    query = input('\n用户请求: ')
    if query == '-1':
        break
    # 将用户请求添加到聊天历史。
    messages.append({'role': 'user', 'content': query})
    response = []
    current_index = 0
    
    for response in bot.run(messages=messages):
        # 调试：如果需要查看完整响应结构，取消下面的注释
        #print('\n[DEBUG] response:', json.dumps(response, indent=2, ensure_ascii=False))
        
        if response and len(response) > 0:          
            
            # 流式输出最后一个 assistant 消息的内容（最终回复）
            for msg in reversed(response):  # 从后往前找最后一个 assistant 消息
                if msg.get('role') == 'assistant' and 'content' in msg and msg['content']:
                    content = msg['content']
                    # 只输出新增的内容部分
                    if len(content) > current_index:
                        current_response = content[current_index:]
                        current_index = len(content)
                        print(current_response, end='', flush=True)
                    break  # 找到最后一个就退出
       
    # 输出换行，确保结果清晰
    print()
    # 将机器人的回应添加到聊天历史。
    messages.extend(response)
```

![638bc56d-4337-4cb2-8023-fbe768e8d6fb.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/jackysense/images/2025-11-27-1764226236959-638bc56d-4337-4cb2-8023-fbe768e8d6fb.png)
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->



通用应用

可以接收来自连接链上用户和合约的合约调用和代币

可以触发合约调用和代币转移到连接的链

可以自动处理跨链交易的 gas 费用

完全兼容 EVM 链、 Cosmos（通过 IBC）和其他链的支持

网关

Gateway 是一个接口，它作为 ZetaChain 上连接链上的合约与通用应用程序之间交互的统一入口点。

将原生 Gas 代币存入 ZetaChain 上的通用应用程序或账户

将支持的 ERC-20 代币（包括 ZETA 代币）存入 ZetaChain 上的通用应用程序或账户

存入原生 gas 代币并向通用应用程序发出合约调用（可传递任意数据）

存入受支持的 ERC-20 代币，并向通用应用程序发出合约调用（可传递任意数据）。

向通用应用程序发出合约调用（传递任意数据）

![ba5351a69bfb4ae8af46fe18eb2235f2.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/jackysense/images/2025-11-26-1764119951580-ba5351a69bfb4ae8af46fe18eb2235f2.jpg)
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->




**浏览器、水龙头**

explorers: [https://www.zetachain.com/docs/reference/explorers](https://www.zetachain.com/docs/reference/explorers)

faucet: [https://www.zetachain.com/docs/reference/faucet](https://www.zetachain.com/docs/reference/faucet)

network: [https://www.zetachain.com/docs/reference/network/details](https://www.zetachain.com/docs/reference/network/details)

RPC: [https://www.zetachain.com/docs/reference/network/api](https://www.zetachain.com/docs/reference/network/api)

contracts address: [https://www.zetachain.com/docs/reference/network/contracts](https://www.zetachain.com/docs/reference/network/contracts)

**1.安装cli**

npm install -g zetachain@latest

**2.创建helloworld项目**

zetachain new 选hello

**3.启动本地网络**

zetachain localnet start

![dd7034a1-7c89-4d1d-87d8-a46f418395de.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/jackysense/images/2025-11-25-1764077716227-dd7034a1-7c89-4d1d-87d8-a46f418395de.png)

**4.创建账户**

![15d259c8-a7cd-4590-9234-ea85224f53a0.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/jackysense/images/2025-11-25-1764077962082-15d259c8-a7cd-4590-9234-ea85224f53a0.png)

**5.forge build 编译通用合约、获取本地私钥、部署通用合约**

![cdc8d7c3-3af4-4573-bbca-b34783ed81b3.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/jackysense/images/2025-11-25-1764079280288-cdc8d7c3-3af4-4573-bbca-b34783ed81b3.png)

6 调用oncall

![ab540164-ca35-45bc-9b0b-b62e76c36611.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/jackysense/images/2025-11-25-1764080729822-ab540164-ca35-45bc-9b0b-b62e76c36611.png)

7.查看调用tx记录和事件，看到context来自 evm 的chainId，sender，zrc20为ETH.ETH地址

![b8030609-42e1-4ccc-a2f6-c3cd1e2ef058.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/jackysense/images/2025-11-25-1764080814961-b8030609-42e1-4ccc-a2f6-c3cd1e2ef058.png)

**Qwen API 的简单请求：**

![3a8fbf91b0d9dfe9c2c27ef268b0ad46.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/jackysense/images/2025-11-25-1764077332232-3a8fbf91b0d9dfe9c2c27ef268b0ad46.png)
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->








zeta 通用AI原理 :ZetaChain 有个Gateway 协议作为桥梁：从通用应用程序向连接链上的合约调用和提取代币。

**应用实例解析**‌  
以问题中描述的ETH兑换BNB流程为例：

‌**步骤分解：**‌

1.  ‌**接收阶段**‌：从以太坊接收6 ETH和"我想要BNB"的消息数据
    
2.  ‌**交换阶段**‌：在ZetaChain上通过去中心化交易所将6 ZRC-20 ETH兑换为ZRC-20 BNB
    
3.  ‌**跨链执行**‌：调用网关合约撤回ZRC-20 BNB，并在BNB链上执行合约调用
    

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/jackysense/images/2025-11-24-1763991106936-image.png)

**通用应用：**

-   可以接收来自连接链上用户和合约的合约调用和代币
    
-   可以触发合约调用和代币转移到连接的链
    
-   可以自动处理跨链交易的 gas 费用
    
-   完全兼容 EVM 链（如以太坊、BNB 和 Polygon）、比特币、Solana、TON 和 Sui。对 Cosmos（通过 IBC）和其他链的支持即将推出
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
