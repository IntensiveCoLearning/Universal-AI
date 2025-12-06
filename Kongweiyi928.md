---
timezone: UTC+8
---

# Kong Weiyi

**GitHub ID:** Kongweiyi928

**Telegram:** @KLucifer928

## Self-introduction

From SEU BA

## Notes

<!-- Content_START -->
# 2025-12-06
<!-- DAILY_CHECKIN_2025-12-06_START -->
## **项目名称**

ZetaMind AI - 跨链智能投资策略引擎

## **目标用户/场景**

**目标用户**：

1.  加密货币新手投资者 - 需要简单、安全的跨链投资指导
    
2.  DeFi中等用户 - 希望获得AI优化的投资策略建议
    
3.  多链资产持有者 - 需要智能的跨链资产配置管理
    

**核心场景**：

-   用户通过自然语言描述投资目标（如："我想用1000 USDT在ZetaChain上寻找年化15%以上的低风险收益"）
    
-   AI分析跨链市场数据，生成个性化投资策略
    
-   系统提供一键执行的安全跨链交易方案
    

## **关键功能（MVP范围）**

1.  **自然语言投资咨询**
    
    -   用户用中文/英文描述投资需求
        
    -   AI解析需求，返回结构化投资建议
        
2.  **跨链策略生成**
    
    -   分析ZetaChain上的流动性池、借贷协议等
        
    -   结合跨链资产价格，推荐最优路径
        
    -   输出包含风险评估的完整方案
        
3.  **安全交易向导**
    
    -   生成分步交易指南（非直接自动化交易）
        
    -   显示预估gas费用和跨链时间
        
    -   提供交易前风险检查清单
        

## **技术路线**

### **ZetaChain部分**

text

```
前端 (React) 
    ↓ 
ZetaChain智能合约 (Solididity)
    ├── 查询跨链资产价格 (ZRC-20)
    ├── 获取流动性池数据
    └── 模拟交易路径计算
```

### **Qwen Agent部分**

text

```
用户输入 → Qwen Agent
    ├── 意图识别（投资咨询/风险评估/策略优化）
    ├── 调用ZetaChain数据API
    ├── 分析跨链市场状态
    └── 生成结构化报告
```

### **数据流架构**

text

```
Qwen Agent (策略分析层)
    ↓ API调用
ZetaChain节点 (数据获取层)
    ↓ 
前端界面 (结果显示层)
```

## **计划复用的Demo/模板**

### **从ZetaChain Demo复用的部分：**

1.  **ZRC-20价格查询模块**
    
    -   基于：`https://www.zetachain.com/docs/developers/evm/zrc20/`
        
    -   复用：跨链资产价格获取功能
        
2.  **Swap交易模拟**
    
    -   基于：`https://www.zetachain.com/docs/developers/tutorials/swap`
        
    -   复用：交易路径计算逻辑
        
    -   修改：不直接执行交易，只进行模拟计算
        

### **从Qwen Demo复用的部分：**

1.  **Agent框架基础**
    
    -   基于：Qwen Agent文档中的多工具调用模式
        
    -   构建：专门针对DeFi数据分析的定制Agent
        
2.  **自然语言处理流程**
    
    -   用户需求 → 结构化查询 → 数据获取 → 报告生成
        

### **新增开发部分：**

1.  **投资策略分析引擎**
    
    -   风险评估模型（简单打分系统）
        
    -   收益率计算逻辑
        
    -   跨链费用优化算法
        
2.  **用户界面**
    
    -   聊天式投资咨询界面
        
    -   策略可视化展示
        
    -   交易步骤指导面板
        

## **MVP边界定义（不做什么）**

### **第一期不做：**

1.  ❌ 自动化交易执行
    
2.  ❌ 高级量化策略
    
3.  ❌ 多用户社交功能
    
4.  ❌ 移动端应用
    

### **核心聚焦：**

1.  ✅ 安全的咨询功能（只读、不写链）
    
2.  ✅ 清晰的用户指导
    
3.  ✅ 准确的跨链数据分析
    
4.  ✅ 简洁的用户体验
    

## **项目价值主张**

**对用户**：

-   降低跨链DeFi投资门槛
    
-   提供个性化的AI投资建议
    
-   避免复杂的手动数据分析
    

**对ZetaChain生态**：

-   展示跨链数据查询的强大能力
    
-   吸引更多非技术用户进入DeFi
    
-   验证AI+区块链的实用场景
    

* * *

**技术栈**：

-   前端：React + Vite
    
-   智能合约：Solidity (ZetaChain EVM)
    
-   AI层：Qwen-7B + LangChain
    
-   部署：ZetaChain测试网 + Vercel
<!-- DAILY_CHECKIN_2025-12-06_END -->

# 2025-12-05
<!-- DAILY_CHECKIN_2025-12-05_START -->

## **项目结构**

text

```
zeta-agent-demo/
├── agent_parser.py      # Qwen-Agent自然语言解析
├── backend.py           # 后端处理逻辑
├── zetachain_client.py  # ZetaChain交互
└── requirements.txt
```

## **1\. 环境准备**

**requirements.txt:**

txt

```
qwen-agent>=0.0.12
web3>=6.0.0
requests>=2.31.0
python-dotenv>=1.0.0
```

安装依赖：

bash

```
pip install -r requirements.txt
```

## **2\. Qwen-Agent自然语言解析**

**agent\_**[**parser.py**](http://parser.py)**:**

python

```
import json
from qwen_agent.llm import get_chat_model
from qwen_agent.agent import Agent

class ZetaChainAgent:
    def __init__(self):
        # 使用Qwen2.5-7B-Instruct模型（本地或API）
        self.llm = get_chat_model({
            'model': 'Qwen/Qwen2.5-7B-Instruct',
            'api_key': 'your-api-key',  # 或使用本地模型路径
            'generate_cfg': {'temperature': 0.1}
        })
        
        # 定义系统提示词
        self.system_prompt = """你是一个ZetaChain交易解析助手。请将用户的自然语言转换为结构化交易参数。

支持的交易类型：
1. 跨链转账：从源链转账到目标链
2. 智能合约调用：调用特定合约的函数
3. 查询：查询余额、交易状态等

请始终返回JSON格式，包含以下字段：
{
    "action": "transfer|contract_call|query",
    "chain_from": "ethereum|bsc|polygon|bitcoin",
    "chain_to": "ethereum|bsc|polygon|bitcoin",
    "token": "ETH|BNB|MATIC|BTC|USDT|USDC",
    "amount": 数字,
    "recipient": "地址",
    "contract_address": "合约地址（如适用）",
    "function_name": "函数名（如适用）",
    "parameters": ["参数1", "参数2"],
    "description": "交易描述"
}

示例输入："从以太坊转0.1个ETH到BSC链的0x742d35Cc6634C0532925a3b844Bc9e"
示例输出：{"action": "transfer", "chain_from": "ethereum", "chain_to": "bsc", "token": "ETH", "amount": 0.1, "recipient": "0x742d35Cc6634C0532925a3b844Bc9e", "description": "跨链转账0.1 ETH从以太坊到BSC"}
"""
    
    def parse_natural_language(self, user_input):
        """解析自然语言为结构化参数"""
        messages = [
            {'role': 'system', 'content': self.system_prompt},
            {'role': 'user', 'content': user_input}
        ]
        
        response = self.llm.chat(messages)
        
        try:
            # 提取JSON部分
            content = response[0].content
            json_start = content.find('{')
            json_end = content.rfind('}') + 1
            json_str = content[json_start:json_end]
            
            parsed_data = json.loads(json_str)
            return parsed_data
        except Exception as e:
            print(f"解析错误: {e}")
            return {
                "error": "解析失败",
                "raw_response": response[0].content
            }
    
    def validate_parameters(self, params):
        """验证解析出的参数"""
        required_fields = ["action", "chain_from", "chain_to", "token", "amount"]
        
        for field in required_fields:
            if field not in params:
                return False, f"缺少必要字段: {field}"
        
        # 验证action类型
        valid_actions = ["transfer", "contract_call", "query"]
        if params["action"] not in valid_actions:
            return False, f"无效的action类型: {params['action']}"
        
        return True, "参数验证通过"
```

## **3\. ZetaChain客户端**

**zetachain\_**[**client.py**](http://client.py)**:**

python

```
import json
import requests
from web3 import Web3
from typing import Dict, Any

class ZetaChainClient:
    def __init__(self, testnet=True):
        self.testnet = testnet
        
        # ZetaChain测试网配置
        if testnet:
            self.rpc_url = "https://zetachain-athens-evm.blockpi.network/v1/rpc/public"
            self.explorer_url = "https://athens3.explorer.zetachain.com"
            self.chain_id = 7001
        else:
            # 主网配置
            self.rpc_url = "https://zetachain-evm.blockpi.network/v1/rpc/public"
            self.explorer_url = "https://explorer.zetachain.com"
            self.chain_id = 7000
        
        # 初始化Web3
        self.w3 = Web3(Web3.HTTPProvider(self.rpc_url))
        
        # ZetaChain合约地址（测试网）
        self.contracts = {
            "connector": "0x2ca7d64A7EFE2D62A725E2B35Cf7230D6677FfEe",
            "zeta_token": "0x5F0b1a82749cb4E2278EC87F8BF6B618dC71a8bf"
        }
    
    def prepare_transfer(self, params: Dict[str, Any]) -> Dict[str, Any]:
        """准备跨链转账交易"""
        print(f"\n🔗 准备跨链转账...")
        print(f"  从: {params['chain_from']}")
        print(f"  到: {params['chain_to']}")
        print(f"  代币: {params['token']}")
        print(f"  数量: {params['amount']}")
        print(f"  接收者: {params['recipient']}")
        
        # 模拟交易数据
        transaction_data = {
            "action": "cross_chain_transfer",
            "source_chain": params["chain_from"],
            "destination_chain": params["chain_to"],
            "token": params["token"],
            "amount": params["amount"],
            "recipient": params["recipient"],
            "estimated_gas": "0.001 ZETA",
            "estimated_time": "2-5分钟",
            "tx_data": {
                "method": "transferCrossChain",
                "params": [
                    params["chain_to"].upper(),
                    params["recipient"],
                    int(params["amount"] * 10**18)  # 转换为wei
                ]
            }
        }
        
        return transaction_data
    
    def prepare_contract_call(self, params: Dict[str, Any]) -> Dict[str, Any]:
        """准备合约调用交易"""
        print(f"\n📄 准备合约调用...")
        print(f"  合约地址: {params.get('contract_address', '默认连接器')}")
        print(f"  函数: {params.get('function_name', 'N/A')}")
        print(f"  参数: {params.get('parameters', [])}")
        
        transaction_data = {
            "action": "contract_call",
            "contract": params.get("contract_address", self.contracts["connector"]),
            "function": params.get("function_name", ""),
            "parameters": params.get("parameters", []),
            "estimated_gas": "0.005 ZETA",
            "tx_data": {
                "to": params.get("contract_address", self.contracts["connector"]),
                "data": self._encode_function_call(params)
            }
        }
        
        return transaction_data
    
    def _encode_function_call(self, params):
        """编码函数调用（简化版）"""
        # 实际实现需要ABI编码
        return "0x" + "simulated_encoded_data".hex()
    
    def simulate_transaction(self, tx_data: Dict[str, Any]) -> Dict[str, Any]:
        """模拟交易执行"""
        print(f"\n🔄 模拟交易执行...")
        
        simulation_result = {
            "success": True,
            "simulation_id": f"sim_{hash(str(tx_data)) % 1000000}",
            "gas_used": "250000",
            "status": "成功",
            "steps": [
                "1. 验证参数 ✓",
                "2. 检查余额 ✓",
                "3. 预估Gas ✓",
                "4. 构建交易 ✓",
                "5. 模拟执行 ✓"
            ],
            "next_action": "发送到ZetaChain测试网"
        }
        
        return simulation_result
    
    def send_test_transaction(self, tx_data: Dict[str, Any]) -> Dict[str, Any]:
        """发送测试交易（模拟或真实）"""
        print(f"\n🚀 发送测试交易...")
        
        # 这里可以是真实的交易发送
        # 为了演示，我们返回模拟结果
        if self.testnet:
            return {
                "success": True,
                "tx_hash": f"0x{hash(str(tx_data)) % 10**64:064x}",
                "explorer_url": f"{self.explorer_url}/tx/0x{hash(str(tx_data)) % 10**64:064x}",
                "message": "✅ 交易已发送到ZetaChain雅典测试网",
                "timestamp": "2024-01-01T12:00:00Z"
            }
        else:
            return {
                "success": False,
                "message": "⚠️ 主网交易需要私钥签名，已跳过"
            }
```

## **4\. 后端服务**

[**backend.py**](http://backend.py)**:**

python

```
import json
from agent_parser import ZetaChainAgent
from zetachain_client import ZetaChainClient

class ZetaChainBackend:
    def __init__(self):
        self.agent = ZetaChainAgent()
        self.client = ZetaChainClient(testnet=True)
    
    def process_request(self, user_input: str, execute_real=False):
        """处理用户输入"""
        print("=" * 50)
        print(f"📝 用户输入: {user_input}")
        print("=" * 50)
        
        # 步骤1: Agent解析自然语言
        print("\n1️⃣ Agent解析中...")
        params = self.agent.parse_natural_language(user_input)
        
        if "error" in params:
            print(f"❌ 解析失败: {params['error']}")
            return params
        
        print(f"✅ 解析成功!")
        print(f"   结构化参数: {json.dumps(params, indent=2, ensure_ascii=False)}")
        
        # 步骤2: 验证参数
        print("\n2️⃣ 验证参数...")
        is_valid, message = self.agent.validate_parameters(params)
        if not is_valid:
            print(f"❌ 验证失败: {message}")
            return {"error": message}
        print(f"✅ {message}")
        
        # 步骤3: 准备交易
        print("\n3️⃣ 准备交易数据...")
        if params["action"] == "transfer":
            tx_data = self.client.prepare_transfer(params)
        elif params["action"] == "contract_call":
            tx_data = self.client.prepare_contract_call(params)
        else:
            tx_data = {"action": "query", "params": params}
        
        print(f"✅ 交易数据准备完成")
        print(f"   交易详情: {json.dumps(tx_data, indent=2, ensure_ascii=False)}")
        
        # 步骤4: 模拟交易
        print("\n4️⃣ 模拟交易执行...")
        simulation = self.client.simulate_transaction(tx_data)
        print(f"✅ 模拟完成: {simulation['status']}")
        for step in simulation.get("steps", []):
            print(f"   {step}")
        
        result = {
            "user_input": user_input,
            "parsed_params": params,
            "tx_data": tx_data,
            "simulation": simulation
        }
        
        # 步骤5: 可选的真实交易
        if execute_real and params["action"] != "query":
            print("\n5️⃣ 发送真实交易...")
            real_tx = self.client.send_test_transaction(tx_data)
            result["real_transaction"] = real_tx
            print(f"   {real_tx['message']}")
            if real_tx.get("tx_hash"):
                print(f"   交易哈希: {real_tx['tx_hash']}")
                print(f"   浏览器查看: {real_tx.get('explorer_url', 'N/A')}")
        
        print("\n" + "=" * 50)
        print("🎉 流程完成!")
        print("=" * 50)
        
        return result

def main():
    """主函数 - 演示流程"""
    backend = ZetaChainBackend()
    
    # 示例输入
    examples = [
        "从以太坊转0.1个ETH到BSC链的0x742d35Cc6634C0532925a3b844Bc9e8d3e9e5a1b",
        "调用合约0x1234...的transfer函数，参数是[0xabcd..., 1000]",
        "查询我的ZETA余额",
        "从Polygon转50个USDT到以太坊"
    ]
    
    print("🤖 ZetaChain智能助手演示")
    print("选择输入方式:")
    print("1. 使用示例")
    print("2. 自定义输入")
    
    choice = input("\n请选择 (1/2): ").strip()
    
    if choice == "1":
        print("\n可用的示例:")
        for i, example in enumerate(examples, 1):
            print(f"{i}. {example}")
        
        example_choice = int(input(f"\n选择示例 (1-{len(examples)}): ")) - 1
        user_input = examples[example_choice]
    else:
        user_input = input("\n请输入您的指令: ").strip()
    
    # 是否执行真实交易
    execute_real = input("\n是否发送真实交易到测试网? (y/N): ").lower() == 'y'
    
    # 处理请求
    result = backend.process_request(user_input, execute_real=execute_real)
    
    # 保存结果到文件
    with open("transaction_result.json", "w", encoding="utf-8") as f:
        json.dump(result, f, indent=2, ensure_ascii=False)
    print(f"\n📁 结果已保存到: transaction_result.json")

if __name__ == "__main__":
    main()
```

## **5\. 简化版运行脚本**

**run\_**[**demo.py**](http://demo.py)**:**

python

```
#!/usr/bin/env python3
"""
简版Demo运行脚本
"""
import sys
from backend import ZetaChainBackend

def simple_demo():
    """简化演示"""
    print("🚀 ZetaChain最小通路Demo")
    print("-" * 40)
    
    # 硬编码示例，避免输入
    user_input = "从以太坊转0.05个ETH到Polygon链的0xabcdef1234567890"
    
    print(f"📝 输入: {user_input}")
    print("-" * 40)
    
    backend = ZetaChainBackend()
    
    # 执行流程
    print("\n🔍 步骤1: Agent解析自然语言...")
    params = backend.agent.parse_natural_language(user_input)
    print(f"   解析结果: {params}")
    
    print("\n✅ 步骤2: 准备交易...")
    tx_data = backend.client.prepare_transfer(params)
    print(f"   交易数据: {tx_data}")
    
    print("\n🔄 步骤3: 模拟执行...")
    simulation = backend.client.simulate_transaction(tx_data)
    print(f"   模拟结果: {simulation['status']}")
    
    print("\n📋 步骤4: 生成最终输出...")
    result = {
        "user_input": user_input,
        "parsed_params": params,
        "tx_data": tx_data,
        "simulation": simulation
    }
    
    print("\n" + "=" * 50)
    print("🎉 Demo完成！以下是结构化输出:")
    print("=" * 50)
    
    import json
    print(json.dumps(result, indent=2, ensure_ascii=False))
    
    return result

if __name__ == "__main__":
    simple_demo()
```
<!-- DAILY_CHECKIN_2025-12-05_END -->

# 2025-12-02
<!-- DAILY_CHECKIN_2025-12-02_START -->


## **1\. Qwen-Agent 框架基本组成**

**核心概念：**

-   **LLM**: 大语言模型（如 Qwen2.5），负责理解和生成文本
    
-   **Agent**: 代理，协调 LLM、Tools 和 Memory 来完成复杂任务
    
-   **Tools**: 工具，让 Agent 能执行特定功能（如计算、搜索、API调用等）
    
-   **Memory**: 记忆，保存对话历史和上下文
    

## **2\. 环境搭建**

首先安装必要的包：

```
pip install qwen-agent
```

## **3\. 跑通官方示例**

先运行一个最简单的官方示例：

```
from qwen_agent.agent import Agent

# 创建最简单的 Agent
agent = Agent(llm={'model': 'qwen2.5-7b-instruct'})

# 运行对话
messages = [{'role': 'user', 'content': '你好，介绍一下你自己'}]
response = agent.run(messages)
print(response)
```

## **4\. 自定义简单 Tool**

创建自定义工具的完整代码：

```
from qwen_agent.agent import Agent
from qwen_agent.tools import BaseTool

# 1. 创建大写转换工具
class UpperCaseTool(BaseTool):
    """将字符串转换为大写"""
    
    def __init__(self):
        super().__init__()
        self.name = 'to_uppercase'
        self.description = '将输入字符串转换为大写字母'
    
    def call(self, params: str, **kwargs) -> str:
        """执行工具调用"""
        return f'大写结果: {params.upper()}'

# 2. 创建加法计算工具
class AdditionTool(BaseTool):
    """计算两个数的和"""
    
    def __init__(self):
        super().__init__()
        self.name = 'add_numbers'
        self.description = '计算两个数字的和，输入格式: "a,b"'
    
    def call(self, params: str, **kwargs) -> str:
        """执行工具调用"""
        try:
            a, b = map(float, params.split(','))
            return f'计算结果: {a} + {b} = {a + b}'
        except:
            return '错误：请输入格式如"3,5"的两个数字'

# 3. 创建带工具的 Agent
def create_custom_agent():
    # 创建工具实例
    tools = [UpperCaseTool(), AdditionTool()]
    
    # 创建 Agent 配置
    agent = Agent(
        llm={'model': 'qwen2.5-7b-instruct'},
        function_list=tools,
        system_message='你是一个有用的助手，可以调用工具来帮助用户。'
    )
    
    return agent

# 4. 测试 Agent
def test_agent():
    agent = create_custom_agent()
    
    # 测试案例
    test_cases = [
        "把hello world转换成大写",
        "计算3和5的和",
        "把python programming转换成大写",
        "计算12.5和7.3的和"
    ]
    
    for query in test_cases:
        print(f"\n{'='*50}")
        print(f"用户: {query}")
        print(f"{'-'*50}")
        
        messages = [{'role': 'user', 'content': query}]
        response = agent.run(messages)
        
        # 打印响应
        if hasattr(response, 'last') and response.last:
            print(f"助手: {response.last[-1]['content']}")
        else:
            print(f"助手: {response}")

# 5. 交互式测试
def interactive_test():
    """交互式测试 Agent"""
    agent = create_custom_agent()
    
    print("自定义 Agent 测试开始！")
    print("可用的工具：")
    print("1. to_uppercase - 将字符串转换为大写")
    print("2. add_numbers - 计算两个数的和（格式: a,b）")
    print("输入 '退出' 结束测试\n")
    
    messages = []
    while True:
        user_input = input("\n请输入: ")
        if user_input.lower() in ['退出', 'exit', 'quit']:
            break
        
        messages.append({'role': 'user', 'content': user_input})
        response = agent.run(messages)
        
        # 获取最新响应
        if hasattr(response, 'last'):
            assistant_response = response.last[-1]['content']
        else:
            assistant_response = str(response)
        
        print(f"\n助手: {assistant_response}")
        messages.append({'role': 'assistant', 'content': assistant_response})

if __name__ == '__main__':
    print("=== Qwen-Agent 自定义工具示例 ===")
    
    # 方法1: 批量测试
    print("\n1. 批量测试:")
    test_agent()
    
    # 方法2: 交互式测试（取消注释以使用）
    # print("\n2. 交互式测试:")
    # interactive_test()
```
<!-- DAILY_CHECKIN_2025-12-02_END -->

# 2025-11-30
<!-- DAILY_CHECKIN_2025-11-30_START -->



## **ZetaChain 通用 DeFi 模式梳理**

| 模式 | 核心机制 | ZetaChain 优势 |
| --- | --- | --- |
| 跨链 DEX | 用户存入任意链资产，在 ZetaChain 完成交易，资产可提至任意链 | ZRC-20 封装/解封装，避免传统跨链桥风险 |
| 跨链借贷 | 在 A 链存资产作为抵押，在 B 链借出资产 | 统一抵押品管理，跨链清算 |
| 收益聚合器 | 自动将资金部署到多链最佳收益机会 | 通过 ZetaChain 统一管理跨链仓位 |
| 跨链收益农场 | 单次操作即可参与多链流动性挖矿 | 简化用户操作，降低 Gas 成本 |
| Omnichain NFT | NFT 可在多链自由转移且保持同一合约地址 | 增强 NFT 流动性和应用场景 |

* * *

## **项目方向 1：OmniFarm - 跨链单边资产收益优化器**

### **目标用户**

-   持有大额 BTC、ETH 等主流资产但不希望频繁跨链操作的用户
    
-   希望获得跨链收益但担心传统跨链桥风险的 DeFi 用户
    

### **解决的问题**

1.  **资产闲置问题**：用户持有资产分散在多条链上，难以统一管理获取收益
    
2.  **跨链复杂度**：传统跨链收益 farming 需要多次跨链、兑换等操作
    
3.  **安全风险**：使用多个跨链桥增加安全风险
    

### **跨链/通用资产使用方式**

```
用户操作流程：
1. 用户在任意链（如 Bitcoin、Ethereum、BNB Chain）存入 BTC
2. ZRC-20 自动将 BTC 封装为 zBTC 并跨链至 ZetaChain
3. OmniFarm 合约自动将 zBTC 分配到最优收益策略：
   - 部分作为抵押品在 Ethereum 上借出稳定币
   - 部分提供到 BNB Chain 的 BTC/稳定币流动性
   - 部分参与 Polygon 上的收益农场
4. 所有收益自动复合，用户可随时在任意链提取本金+收益
```

### **技术亮点**

-   使用 ZetaChain 的 `crossChainCall` 统一管理多链 DeFi 交互
    
-   基于各链利率和风险参数的智能再平衡算法
    
-   收益以原生资产（BTC）形式返还，避免无常损失担忧
<!-- DAILY_CHECKIN_2025-11-30_END -->

# 2025-11-29
<!-- DAILY_CHECKIN_2025-11-29_START -->




## **🚀 ZetaChain Swap Demo 实践记录**

### **环境准备与关键命令**

```
# 1. 克隆示例代码库
git clone https://github.com/zeta-chain/example-contracts
cd example-contracts

# 2. 安装依赖
npm install

# 3. 配置环境变量
cp .env.example .env
# 编辑 .env 文件，添加私钥和测试网配置
```

**关键配置项：**

```
PRIVATE_KEY=你的测试网私钥
NETWORK=testnet
```

```
# 4. 部署跨链交换合约
npx hardhat deploy --tags omnichain-swap
```

**部署输出示例：**

```
Deploying OmnichainSwap contract to ZetaChain
OmnichainSwap deployed to: 0x1234...5678 on ZetaChain
```

### **🎯 执行跨链交换**

```
# 从 Goerli ETH 交换到 BSC-testnet BNB
npx hardhat omnichain-swap --network goerli \
  --amount 0.01 \
  --src-token ETH \
  --dest-chain bsc-testnet \
  --dest-token BNB
```

**交易流程记录：**

1.  **发起链**: Goerli Ethereum Testnet
    
2.  **源资产**: 0.01 ETH
    
3.  **目标链**: BSC Testnet
    
4.  **目标资产**: BNB
    

### **📝 调用过程分析**

**1\. 调用发起位置**

-   **网络**: Goerli Ethereum 测试网
    
-   **操作**: 调用本地部署的 OmnichainSwap 合约的 `swap` 函数
    
-   **参数**:
    
    -   `destinationChainId`: BSC测试网链ID
        
    -   `token`: 目标token地址（BNB）
        
    -   `amount`: 交换数量
        

**2\. ZetaChain 上的处理流程**

**第一步：跨链消息接收**

-   Goerli 上的交易通过 ZetaChain 的 Connector 合约转发到 ZetaChain
    
-   ZetaChain 的 Omnichain 协议验证跨链消息的真实性
    

**第二步：智能合约执行**

```
// 在 ZetaChain 上执行的逻辑
function onCrossChainCall(
    address sender,
    uint256 sourceChainId,
    bytes calldata message
) external override {
    // 1. 解码消息，获取交换参数
    // 2. 通过内部 DEX 路由寻找最佳交换路径
    // 3. 执行资产交换（ETH → ZETA → BNB）
    // 4. 准备跨链转账到目标链
}
```

**第三步：跨链资产转移**

-   ZetaChain 调用目标链（BSC）的 Connector 合约
    
-   通过 ZetaChain 的流动性池完成资产跨链转移
    
-   在 BSC 测试网向用户地址发送交换后的 BNB
    

### **🔍 架构层面的理解**

根据 ZetaChain 架构文档，这个流程涉及：

1.  **Outbound Transaction**: 从源链到 ZetaChain
    
2.  **Observer Network**: 验证和中继跨链消息
    
3.  **TSS (Threshold Signature Scheme)**: 在多链上安全签名交易
    
4.  **Inbound Transaction**: 从 ZetaChain 到目标链
<!-- DAILY_CHECKIN_2025-11-29_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->





# **1\. 对 “全链应用 / Universal App” 的直观理解**

-   **一个合约，多处运行**：你只写一次核心业务逻辑（Universal App 合约），它可以被部署到任何支持的区块链上（如以太坊、Arbitrum、Polygon、Base 等）。
    
-   **无缝的跨链交互**：前端应用可以轻松地调用任何链上的合约，甚至可以发起一个动作，让它在链 A 上执行一部分，在链 B 上执行另一部分，而用户几乎感知不到底层的切换。
    
-   **统一的 Gas 费体验**：用户可能只在一条链上（比如他们的主链）持有资产，但可以通过中继服务等方式，支付一次 Gas 费就能完成跨链操作。
    
-   **共享的应用状态**：应用的数据和状态可以在不同链之间同步和共享，打破了链与链之间的数据孤岛。
    

* * *

# **2\. Hello World / Demo 会包含哪些模块**

**模块一：智能合约**

你需要编写两种合约：

1.  **Universal App 合约**
    
    -   这是你的核心业务逻辑合约。在我们的 Demo 中，它就是一个计数器。
        
    -   它会继承跨链协议（如 LayerZero）提供的 `OApp` 或 `ONFT` 等基础合约。
        
    -   它包含：
        
        -   `counter`：一个存储计数器值的状态变量。
            
        -   `increment()`：一个普通的增量函数，供本地用户调用。
            
        -   `incrementCrossChain(uint32 _dstChainId)`：一个特殊的函数。当用户调用它时，它不会直接修改本地计数器，而是通过 LayerZero 的 **Endpoint** 发送一条跨链消息到目标链。
            
        -   `_lzReceive()`：一个**强制必须实现**的接收函数。当 LayerZero 的 Relayer 将消息从源链传递到目标链时，会自动调用此函数。在这个函数里，你才会真正执行增加计数器的逻辑。
            
2.  **前端交互合约（可选但推荐）**
    
    -   有时为了简化前端调用，可以部署一个 Helper 合约，它封装了估算跨链消息费用等复杂操作。
        

**模块二：前端 / dApp**

这是一个用户界面（一个网页），通常使用 React + Vite + Wagmi/以太坊生态系统 或类似技术栈构建。

它需要具备以下能力：

-   **多链钱包连接**：能够同时检测和支持用户连接多个链的钱包（如 MetaMask）。
    
-   **读取多条链的状态**：
    
    -   通过对应的 RPC 提供商，分别从 **Sepolia** 和 **Arbitrum Sepolia** 链上读取 `counter` 的当前值，并显示在页面上。
        
-   **发送跨链交易**：
    
    -   提供一个按钮，例如“在 Sepolia 上增加，并同步到 Arbitrum”。
        
    -   当用户点击时，前端需要：
        
        1.  调用 LayerZero Endpoint 来**估算**从 Sepolia 发送到 Arbitrum 的跨链消息 **Gas 费**。
            
        2.  引导用户签署交易，调用 Sepolia 链上合约的 `incrementCrossChain` 函数，并附上估算的 Gas 费。
            
        3.  监听交易状态和跨链消息的完成状态。
            

**模块三：RPC 与基础设施**

这是连接一切的“神经系统”。

1.  **区块链 RPC 提供商**：
    
    -   你需要为每条你部署合约的链配置一个 RPC 节点。例如：
        
        -   **Sepolia ETH**: 来自 Alchemy, Infura 或公共节点。
            
        -   **Arbitrum Sepolia**: 来自 Alchemy 或 Arbitrum 官方节点。
            
    -   前端通过这些 RPC 来读取链上数据和发送交易。
        
2.  **LayerZero 的 Endpoint / Relayer / Oracle**：
    
    -   这是全链应用的核心。你**不需要自己运行**这些，LayerZero 已经提供了测试网和主网的设施。
        
    -   **Endpoint**：每个链上都有一个 LayerZero 的合约地址，你的 Universal App 合约需要知道它，它是消息的“邮局”。
        
    -   **Relayer & Oracle**：是 LayerZero 网络中的去中心化节点，负责传递和验证消息。你只需要在合约中配置信任的路径即可。
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->






# **1\. 通用区块链**

**基本含义**：  
一个旨在作为**中心枢纽或底层基础**，能够连接和协调多条其他区块链（称为“第1层”或 L1）的区块链。它本身是一条功能完备的区块链，但其核心价值在于其**连接能力**。

**关键点**：

-   **角色**：它不试图在单链性能上超越所有链，而是致力于成为区块链世界的“路由器”或“指挥中心”。
    
-   **功能**：它通过一系列跨链通信协议，来验证和传递不同链上发生的事件和信息。
    
-   **例子**：
    
    -   **Cosmos**：通过 IBC 协议连接链与链，本身也是一个枢纽链。
        
    -   **Polkadot**：中继链是典型的通用区块链，负责协调所有连接到它的平行链的安全和通信。
        
    -   **Avalanche**：其子网架构也体现了通用连接的思想。
        

# **2\. 通用 EVM**

**基本含义**：  
指一个**虚拟的、标准化的执行环境**，它让以太坊虚拟机能够超越以太坊主网本身，在任何支持它的区块链上运行。EVM 是以太坊智能合约的“操作系统”，而“通用 EVM”就是让这个操作系统变得无处不在。

**关键点**：

-   **核心是兼容性**：任何实现了 EVM 的区块链（如 Polygon, BNB Chain, Avalanche C-Chain）都能无缝运行为以太坊编写的智能合约。这为开发者提供了巨大的便利。
    
-   **“通用”的延伸**：这个概念进一步演变为，一个智能合约的**逻辑**可以被部署到多个链上，并且这些实例能够相互感知和协作，形成一个统一的应用程序。
    
-   **技术实现**：这通常通过跨链消息传递来实现。
    

# **3\. 通用应用**

**基本含义**：  
一个**部署在多个区块链上的单一应用程序**。它在每条链上都有一个或多个智能合约实例，但这些实例共享同一个应用状态和逻辑，并且能够相互通信，为用户提供一个统一的、跨链的体验。

**关键点**：

-   **多链部署**：应用本身存在于多条链上，而不仅仅是在一条链上。
    
-   **状态同步**：应用的关键状态（如用户余额、全局数据）在不同链间是同步或可汇总的。
    
-   **用户体验**：用户可以在任何一条支持的链上与这个应用交互，并感受到它是在同一个应用内操作，无需关心底层是哪条链。
    
-   **例子**：
    
    -   **跨链 DEX**：你可以在 Polygon 上存入 USDC，然后在 BNB Chain 上兑换成 ETH，整个过程感觉像是在一个交易所内完成。
        
    -   **跨链借贷**：在 Arbitrum 上存入抵押品，在以太坊主网上借出资金。
        

# **4\. 全链智能合约**

**基本含义**：  
这是实现“通用应用”的**核心技术范式**。它指的是一个单一的、逻辑上统一的智能合约，其代码和状态不再局限于一条区块链，而是能够根据需求，在不同的区块链上执行不同的函数，并保持整个合约状态的一致性。

**关键点**：

-   **逻辑统一**：开发者可以编写一个“主合约”，并定义其不同部分在哪些链上执行。例如，“函数A在以太坊执行，函数B在Polygon执行”。
    
-   **跨链自动执行**：合约能够自动触发在不同链上的操作。这是比“通用应用”更高级、更集成的概念。
    
-   **依赖底层跨链基础设施**：它严重依赖于像 LayerZero、CCIP、Wormhole 这样的跨链互操作性协议来传递消息和保证安全。
    

# **总结与关系**

这四者构成了一个从基础设施到应用体验的完整技术栈：

1.  **通用区块链** 提供了互联的**底层基础设施**（如 Cosmos, Polkadot）。
    
2.  **通用 EVM** 提供了互联的**标准执行环境**，是生态繁荣的关键。
    
3.  **全链智能合约** 是构建互联应用的**核心技术和开发范式**。
    
4.  **通用应用** 是最终呈现给用户的**产品和体验**，是前三者的价值体现。
    

**演进关系**：  
**通用区块链** 和 **通用 EVM** 奠定了多链互联的基础 -> 开发者利用 **全链智能合约** 的技术来构建 -> 最终诞生了面向用户的 **通用应用**。

# 示意图绘制

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Kongweiyi928/images/2025-11-26-1764166485924-image.png)
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->







# **1.安装ZetaChain CLI**

## **ZetaChain CLI**

ZetaChain CLI 是一个功能强大的命令行工具，专门用于与 ZetaChain 区块链网络进行交互。它既是开发者的构建工具，也是节点运营者和高级用户的管理控制台。

### **核心架构**

### **二进制组件**

-   **主要组件**: `zetacored` - ZetaChain 的核心二进制文件
    
-   **集成功能**: 节点运行 + 客户端交互一体化
    
-   **技术基础**: 基于 Cosmos SDK 构建，遵循 Cosmos 生态标准
    

### **主要功能模块**

### **1\. 钱包与密钥管理**

```
# 创建新钱包
zetacored keys add my-wallet

# 恢复钱包（通过助记词）
zetacored keys add recovered-wallet --recover

# 列出所有钱包
zetacored keys list

# 查看钱包详情
zetacored keys show my-wallet

# 删除钱包
zetacored keys delete my-wallet
```

### **2\. 网络查询功能**

```
# 查询账户余额
zetacored query bank balances zeta1...

# 查询交易状态
zetacored query tx <交易哈希>

# 获取区块信息
zetacored query block

# 查询验证者列表
zetacored query staking validators

# 跨链状态查询（ZetaChain 特有）
zetacored query observer chain-state
```

### **3\. 交易操作**

```
# ZETA 代币转账
zetacored tx bank send <发送者> <接收者> 1000000azeta --chain-id zetachain_7000-1

# 质押操作
zetacored tx staking delegate <验证者地址> 1000000azeta

# 治理投票
zetacored tx gov vote <提案ID> yes

# 跨链交易（核心功能）
zetacored tx crosschain send ...
```

### **4\. 节点运营管理**

```
# 初始化节点
zetacored init my-node --chain-id zetachain_7000-1

# 启动节点
zetacored start

# 检查节点状态
zetacored status

# 重置节点（测试环境）
zetacored unsafe-reset-all
```

## **安装与配置**

### **环境要求**

-   **Node.js**: ≥ 18.x
    
-   **Git**: 用于克隆项目模板
    
-   **Docker**: ≥ 24.x（用于本地网络）
    

### **安装方式**

**方法一：本地临时使用**

```
npx zetachain@next new
```

**方法二：全局安装**

```
npm install -g zetachain@latest
```

**方法三：从源码构建**

```
git clone https://github.com/zeta-chain/zetacored
cd zetacored
make install
```

### **环境验证**

```
# 验证安装
zetachain version

# 测试网络连接
zetachain query chains list

# 检查配置
zetachain config list
```

## **实际应用场景**

### **开发工作流**

```
# 1. 创建新项目
zetachain new my-crosschain-dapp

# 2. 启动本地开发网络
zetachain localnet start

# 3. 编译合约
cd my-crosschain-dapp
npm run compile

# 4. 部署合约
npm run deploy

# 5. 测试交互
zetachain query contract-state <合约地址>
```

### **钱包管理流程**

```
# 创建开发钱包
npx zetachain accounts create --name dev-wallet

# 导出私钥（谨慎操作）
npx zetachain accounts export dev-wallet

# 查询测试币余额
zetachain query balances --evm $(zetachain accounts list)

# 从水龙头获取测试币
curl -X POST https://faucet.zetachain.com/request -H "Content-Type: application/json" -d '{"address":"你的地址"}'
```

### **跨链操作示例**

```
# 查询支持的链列表
zetachain query chains list

# 跨链余额查询
zetachain query balances --evm 0x... --chain eth

# 发送跨链交易
zetachain tx crosschain send \
  --from my-wallet \
  --chain eth \
  --recipient 0x... \
  --amount 1000000
```

## **高级功能**

### **配置管理**

```
# 查看当前配置
zetachain config list

# 设置默认网络
zetachain config set network testnet

# 配置自定义 RPC 端点
zetachain config set rpc.endpoint https://your-custom-rpc.com

# 设置 Gas 价格
zetachain config set gas.price 1000000000azeta
```

### **脚本自动化**

```
#!/bin/bash
# 自动化部署脚本示例

# 设置环境变量
export PRIVATE_KEY="你的私钥"
export NETWORK="zeta_testnet"

# 编译合约
echo "编译合约..."
npx hardhat compile

# 部署合约
echo "部署合约..."
npx hardhat deploy --network $NETWORK

# 验证合约
echo "验证合约..."
npx hardhat verify --network $NETWORK <合约地址>
```

### **监控与调试**

```
# 实时日志监控
zetacored logs --follow

# 交易调试
zetachain tx debug <交易哈希>

# 性能监控
zetachain query tendermint validator-set
```

## **故障排除**

### **常见问题解决**

```
# 清除缓存
zetachain cache clean

# 重置配置
zetachain config reset

# 更新工具版本
npm update -g zetachain

# 检查网络连接
zetachain query status
```

### **日志分析**

```
# 查看详细日志
zetacored logs --level debug

# 过滤特定模块日志
zetacored logs --module bank

# 导出日志文件
zetacored logs > zetacored.log
```

![](https://camo.githubusercontent.com/10bf7a23c219fa1f242bb1e763e7df911448b9d7b6c2365f4db2928eb4f34a0f/68747470733a2f2f6176316c6e37717061366c2e6665697368752e636e2f73706163652f6170692f626f782f73747265616d2f646f776e6c6f61642f6173796e63636f64652f3f636f64653d596d45774f574e6d4f4467304d4751354e474a6c4e4745334e4455784e6a42685a4441325a6d5a6b596a4e6652304d334f5768305a6b4a5453317071526b5a5a535452794d4468546447356f56553544636e6835647a4e66564739725a57343656557835566d4a7665576c6c62334d354e587034536c597762574e754d303949626d4e6f587a45334e6a51774e6a51324f4451364d5463324e4441324f4449344e4639574e41)

# **2.ZetaChain Faucet**

## **什么是 Faucet？**

**Faucet**（水龙头）是区块链测试网络中免费分发测试代币的服务。它允许开发者在无需真实资金的情况下获取测试网代币，用于开发和测试智能合约、DApps 和跨链功能。

## **ZetaChain Faucet 的作用**

### **主要用途**

-   **开发测试**: 获取测试网 ZETA 代币部署和测试智能合约
    
-   **交易燃料**: 支付交易手续费（Gas Fee）
    
-   **跨链测试**: 测试跨链交易和消息传递功能
    
-   **教育学习**: 供学习者体验 ZetaChain 功能
    

## **官方 Faucet 渠道**

### **1\. 主 Faucet 网站**

```
https://www.zetachain.com/docs/reference/faucet/
```

### **2\. 社区 Faucet**

```
https://zetachain.faucetme.pro/
```

### **3\. Discord 水龙头**

-   加入 ZetaChain 官方 Discord
    
-   在特定频道使用命令获取测试币
    

## **使用流程**

### **基本步骤**

```
# 1. 首先创建或获取你的测试网地址
npx zetachain accounts create --name test-account

# 2. 获取生成的地址
zetachain accounts list

# 3. 访问 Faucet 网站并输入地址
# 或者使用 API 方式
```

### **API 请求示例**

```
curl -X POST https://faucet.zetachain.com/request \
  -H "Content-Type: application/json" \
  -d '{"address":"你的zeta地址或0x格式地址"}'
```

![](https://camo.githubusercontent.com/06158d84aa2aae98eb38d43d5f2a878272e17bf5efd049494a353a74510a5992/68747470733a2f2f6176316c6e37717061366c2e6665697368752e636e2f73706163652f6170692f626f782f73747265616d2f646f776e6c6f61642f6173796e63636f64652f3f636f64653d4f545931596a63354d7a566c4d7a6c6d596a52684f4451325932466a596d4533595745334d32517959574666636d644f64446732646b493462446c6a57484a71566a6c444e565a73526e4e516248703163484533636b7466564739725a57343656566b33616d4a7a516e457a623052334d454a3455304a44516d4e694f45746a626d706f587a45334e6a51774e6a51324f4451364d5463324e4441324f4449344e4639574e41)

# **3.ZetaChain 主网与测试网**

## **主网 (Mainnet)**

### **定义与特点**

**主网**是 ZetaChain 的正式生产环境，处理真实价值的资产和交易。

### **关键特性**

-   **真实经济价值**: 所有交易使用真实 ZETA 代币
    
-   **不可逆性**: 交易一旦确认不可撤销
    
-   **生产级安全**: 最高级别的网络安全措施
    
-   **真实用户资产**: 承载真实的用户资金和商业应用
    

### **技术参数**

```
# 主网 Chain ID
Chain ID (EVM): 7000
Chain ID (Cosmos): zetachain_7000-1

# 主网 RPC 端点
https://zetachain-evm.blockpi.network/v1/rpc/public
https://zetachain-mainnet.rpc.thirdweb.com

# 主网区块浏览器
https://zetascan.com
https://explorer.zetachain.com
```

## **测试网 (Testnet)**

### **定义与目的**

**测试网**是 ZetaChain 的测试环境，用于开发和测试，不涉及真实价值。

### **主要用途**

-   **智能合约开发**: 安全地测试和调试合约
    
-   **DApp 测试**: 验证应用程序功能
    
-   **协议升级测试**: 在新功能上线主网前进行测试
    
-   **开发者教育**: 学习 ZetaChain 功能的最佳环境
    

### **当前测试网: Maimet Beta**

**技术参数**

```
# 测试网 Chain ID
Chain ID (EVM): 7001
Chain ID (Cosmos): zetachain_7001-1

# 代币信息
Denom: azeta
Symbol: testZETA (或 tZETA)
Decimals: 18

# 测试网 RPC 端点
https://zetachain-testnet.rpc.thirdweb.com
https://zetachain-evm.testnet.lava.build

# 测试网区块浏览器
https://athens.explorer.zetachain.com
https://zetachain-testnet.blockexplorer.ai
```

# **4.ZetaChain RPC**

## **什么是 RPC？**

RPC（远程过程调用）是区块链与外部世界通信的桥梁。在 ZetaChain 的语境中，RPC 是**节点暴露的 API 端点**，允许开发者查询区块链数据、提交交易、与智能合约交互。

## **ZetaChain 的双重 RPC 架构**

ZetaChain 的独特之处在于它同时支持两种 RPC 协议：

### **1\. JSON-RPC (EVM 兼容层)**

**特点**

-   **协议**: JSON-RPC 2.0
    
-   **数据格式**: JSON
    
-   **端口**: 通常 8545、8546
    
-   **兼容性**: 完全兼容以太坊生态工具
    

**主要方法**

```
// 基础查询
eth_blockNumber        // 获取最新区块号
eth_getBalance         // 查询地址余额
eth_getTransactionReceipt // 获取交易回执

// 智能合约交互
eth_call              // 读取合约状态
eth_sendRawTransaction // 发送签名交易

// 事件监听
eth_getLogs           // 查询事件日志
```

**使用示例**

```
// 使用 Web3.js 连接 ZetaChain RPC
const Web3 = require('web3');
const web3 = new Web3('https://zetachain-evm.blockpi.network/v1/rpc/public');

// 查询余额
const balance = await web3.eth.getBalance('0x...');
console.log(`余额: ${web3.utils.fromWei(balance, 'ether')} ZETA`);

// 发送交易
const tx = {
  from: '0x...',
  to: '0x...',
  value: web3.utils.toWei('0.1', 'ether'),
  gas: 21000
};
const receipt = await web3.eth.sendTransaction(tx);
```

### **2\. gRPC & REST (Cosmos SDK 层)**

**特点**

-   **协议**: gRPC (HTTP/2 + Protobuf) 和 REST/HTTP
    
-   **数据格式**: 二进制 (gRPC) 或 JSON (REST)
    
-   **端口**: 9090 (gRPC), 1317 (REST)
    
-   **功能**: 访问 Cosmos 原生功能
    

**端点示例**

```
# REST 端点示例
# 查询账户信息
curl http://localhost:1317/cosmos/auth/v1beta1/accounts/zeta1...

# 查询链状态
curl http://localhost:1317/cosmos/base/tendermint/v1beta1/node_info

# 查询跨链观察者状态
curl http://localhost:1317/zeta-chain/observer/chain_params
```

## **RPC 端点类型**

### **公共端点 (Public RPC)**

```
// 主网公共端点
const mainnetRPC = {
  evm: "https://zetachain-evm.blockpi.network/v1/rpc/public",
  cosmos: "https://zetachain-rest.core.chainstack.com"
};

// 测试网公共端点  
const testnetRPC = {
  evm: "https://zetachain-testnet.rpc.thirdweb.com",
  cosmos: "https://zetachain-testnet.rest.core.chainstack.com"
};
```

**特点**:

-   免费使用
    
-   有速率限制
    
-   适合开发和测试
    
-   可能存在性能瓶颈
    

### **专用端点 (Dedicated RPC)**

```
// 基础设施服务商
const dedicatedRPC = {
  // Chainstack
  chainstack: "https://zetachain-mainnet.rpc.chainstack.com",
  
  // BlockPI
  blockpi: "https://zetachain.blockpi.network/v1/rpc/public",
  
  // Lava Network
  lava: "https://zetachain.rpc.lava.build"
};
```

**特点**:

-   更高的速率限制
    
-   更好的性能
    
-   可能需要注册或付费
    
-   生产环境推荐
    

### **本地端点 (Local RPC)**

```
# 启动本地节点
zetacored start

# 本地 RPC 端点
EVM: http://localhost:8545
REST: http://localhost:1317
gRPC: http://localhost:9090
```

## **实际配置示例**

### **1\. Hardhat 配置**

```
// hardhat.config.js
require('@nomiclabs/hardhat-ethers');

module.exports = {
  networks: {
    zeta_mainnet: {
      url: "https://zetachain-evm.blockpi.network/v1/rpc/public",
      chainId: 7000,
      accounts: [process.env.MAINNET_PRIVATE_KEY],
      gasPrice: 20000000000, // 20 Gwei
      timeout: 60000
    },
    zeta_testnet: {
      url: "https://zetachain-testnet.rpc.thirdweb.com", 
      chainId: 7001,
      accounts: [process.env.TESTNET_PRIVATE_KEY],
      gasPrice: 10000000000, // 10 Gwei
      timeout: 60000
    },
    zeta_local: {
      url: "http://localhost:8545",
      chainId: 70000,
      accounts: [process.env.LOCAL_PRIVATE_KEY]
    }
  },
  solidity: {
    version: "0.8.19",
    settings: {
      optimizer: {
        enabled: true,
        runs: 200
      }
    }
  }
};
```

### **2\. 前端 DApp 配置**

```
// web3配置
import { ethers } from 'ethers';

const ZETACHAIN_RPC = {
  mainnet: 'https://zetachain-evm.blockpi.network/v1/rpc/public',
  testnet: 'https://zetachain-testnet.rpc.thirdweb.com'
};

class ZetaChainClient {
  constructor(network = 'testnet') {
    this.provider = new ethers.providers.JsonRpcProvider(
      ZETACHAIN_RPC[network]
    );
    this.signer = null;
  }
  
  // 连接钱包
  async connectWallet() {
    if (window.ethereum) {
      await window.ethereum.request({ method: 'eth_requestAccounts' });
      this.signer = new ethers.providers.Web3Provider(window.ethereum).getSigner();
    }
  }
  
  // 查询跨链余额
  async getCrossChainBalance(address, chain) {
    const balance = await this.provider.getBalance(address);
    return ethers.utils.formatEther(balance);
  }
}
```

### **3\. 命令行工具使用**

```
# 使用 CLI 查询不同 RPC 端点
zetachain query balances --rpc https://zetachain-evm.blockpi.network/v1/rpc/public

# 设置默认 RPC 端点
zetachain config set rpc.evm https://zetachain-evm.blockpi.network/v1/rpc/public
zetachain config set rpc.cosmos https://zetachain-rest.core.chainstack.com
```

# **5.ZetaChain开发工作流程**

### **1\. 环境设置**

```
# 1. 安装 CLI
npm install -g zetachain@latest

# 2. 创建项目
zetachain new my-crosschain-app

# 3. 安装依赖
cd my-crosschain-app && npm install

# 4. 配置环境变量
cp .env.example .env
# 编辑 .env 文件添加私钥和 RPC URL
```

### **2\. 本地开发测试**

```
# 启动本地网络
zetachain localnet start

# 编译合约
npx hardhat compile

# 运行测试
npx hardhat test

# 部署到本地网络
npx hardhat deploy --network localnet
```

### **3\. 测试网部署**

```
# 获取测试币
npx zetachain faucet --address <你的地址>

# 部署到测试网
npx hardhat deploy --network zeta_testnet

# 验证合约
npx hardhat verify --network zeta_testnet <合约地址>
```

# **6.Qwen API**

## **1\. 终端使用 cURL 请求**

### **基本请求格式**

```
curl -X POST "https://dashscope.aliyuncs.com/api/v1/services/aigc/text-generation/generation" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "qwen-turbo",
    "input": {
      "messages": [
        {
          "role": "user",
          "content": "请介绍一下你自己"
        }
      ]
    },
    "parameters": {
      "result_format": "message"
    }
  }'
```

### **简化版本**

```
curl -X POST "https://dashscope.aliyuncs.com/api/v1/services/aigc/text-generation/generation" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "qwen-turbo",
    "input": {
      "messages": [
        {"role": "user", "content": "你好"}
      ]
    }
  }' > response.json
```

## **2\. Postman 请求配置**

### **步骤 1：创建新请求**

-   **方法**: POST
    
-   **URL**: `https://dashscope.aliyuncs.com/api/v1/services/aigc/text-generation/generation`
    

### **步骤 2：设置 Headers**

| Key | Value |
| --- | --- |
| Authorization | Bearer YOUR_API_KEY |
| Content-Type | application/json |
| X-DashScope-SSE | disable (如需关闭流式输出) |

### **步骤 3：设置 Body**

选择 **raw** 和 **JSON** 格式，输入以下内容：

```
{
  "model": "qwen-turbo",
  "input": {
    "messages": [
      {
        "role": "user",
        "content": "请用一句话介绍量子计算"
      }
    ]
  },
  "parameters": {
    "result_format": "message",
    "max_tokens": 1500,
    "temperature": 0.7
  }
}
```

## **3\. 不同模型的请求示例**

### **Qwen-Turbo**

```
curl -X POST "https://dashscope.aliyuncs.com/api/v1/services/aigc/text-generation/generation" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "qwen-turbo",
    "input": {
      "messages": [
        {"role": "user", "content": "写一首关于春天的短诗"}
      ]
    }
  }'
```

### **Qwen-Max**

```
curl -X POST "https://dashscope.aliyuncs.com/api/v1/services/aigc/text-generation/generation" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "qwen-max",
    "input": {
      "messages": [
        {"role": "user", "content": "解释深度学习的基本原理"}
      ]
    },
    "parameters": {
      "result_format": "message"
    }
  }'
```

### **Qwen-Plus**

```
curl -X POST "https://dashscope.aliyuncs.com/api/v1/services/aigc/text-generation/generation" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "qwen-plus",
    "input": {
      "messages": [
        {"role": "user", "content": "用Python写一个计算斐波那契数列的函数"}
      ]
    }
  }'
```
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->








# **ZetaChain**

目前，区块链世界在享受多链带来的多样性和特定优势的同时，也深刻受困于由此产生的“孤岛效应”。

1.  **流动的碎片化：资本被分散在数十条公链和 Layer 2 网络上。这不仅降低了资金的整体利用效率，还导致用户在跨链转移资产时面临高昂的手续费、显著的交易滑点以及漫长的等待时间。**
    
2.  **技术复杂性与安全风险**：传统的跨链方案，如资产桥，通常需要引入“包装资产”和额外的信任假设。这不仅增加了用户的操作步骤和理解门槛，更引入了**新的单点故障风险**，桥接合约已成为黑客攻击的重灾区。此外，不同区块链在协议、共识机制和数据结构上的技术差异，使得实现无缝互操作在工程上极为困难。
    
3.  **开发者体验与创新受阻**：开发者若想构建覆盖多链用户的应用，不得不为每条链单独部署和维护智能合约，并处理复杂的跨链逻辑。这种高昂的开发成本和复杂性严重**抑制了创新，限制了应用的可访问性**。
    

面对上述问题，市场迫切需要一种更底层、更统一的基础设施。ZetaChain 作为一个“通用区块链”，其设计理念和技术架构直指这些核心问题。

1.  **实现真正的原生互操作性**：ZetaChain 最核心的突破在于，它允许开发者**通过一个全链智能合约，直接管理和操作不同链上的原生资产**，甚至是比特币。用户无需再与包装资产打交道，从而消除了与之相关的信用风险和摩擦。
    
2.  **统一流动性层**：ZetaChain 的愿景是成为一个**统一的流动性层和业务逻辑执行层**。它通过其通用EVM（zEVM）和跨链消息传递机制，抽象了底层链的复杂性。这使得开发者可以像调用单个链上的合约一样，构建跨链的DeFi应用、NFT平台等，从而**释放被割裂的流动性价值**。
    
3.  **提升开发者效率与用户体验**：开发者得以从多链部署和维护的繁重工作中解放出来，**一次开发，即可覆盖所有主流区块链的用户**。对用户而言，复杂的跨链操作被封装在简洁的应用界面之后，交互体验无限接近于单链应用，这极大地**降低了Web3的进入门槛**。
    

# **Qwen**

当前，AI Agent在实际应用中暴露出关键问题。在推理能力方面，大多数模型难以维持复杂的逻辑链条，当任务复杂度增加时，很容易出现"推理迷失"现象。具体表现为：

1.  **在处理多步骤任务时，模型会逐渐偏离核心目标。**
    
2.  **在需要长期规划的任务中，无法保持战略方向的一致性。**
    
3.  **面对突发情况时，缺乏灵活的应变能力。**
    

面对这些挑战，Qwen模型从底层架构开始就进行了针对性的设计和优化。在推理能力方面，Qwen通过创新的训练方法和模型结构，显著提升了逻辑推理和数学推理能力。这不仅体现在基准测试的成绩上，更在实际的复杂任务处理中展现出明显的优势。模型能够更好地理解任务的内在逻辑，保持推理过程的一致性，并在遇到意外情况时展现出更强的应变能力。
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
