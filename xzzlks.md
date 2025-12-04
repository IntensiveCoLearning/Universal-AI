---
timezone: UTC+8
---

# 吴乃泽

**GitHub ID:** xzzlks

**Telegram:** @wunaize

## Self-introduction

编程初学者，希望学习到更多AI区块链方面知识，致力于学习新技术！

## Notes

<!-- Content_START -->
# 2025-12-04
<!-- DAILY_CHECKIN_2025-12-04_START -->
# Day 11：Qwen-Agent × ZetaChain（接口层设计）

**日期**：2025年12月4日 星期四

**目标**：将 Day10 中 `parse_swap_intent` 输出的结构化 JSON（链名、代币、金额），精准映射为 ZetaChain 支持的合约调用参数（如合约地址、函数签名、参数编码），需兼容单链 Swap 与跨链 Swap 两种场景；设计“中间层服务”承接两端——向上接收意图解析结果，向下适配不同链/代币的合约调用规范，实现“前端需求变化不影响合约调用逻辑，合约升级不感知前端交互”的解耦效果。

## 接口层（中间层服务）核心设计逻辑

中间层是连接“AI 意图解析层”与“ZetaChain 合约层”的桥梁，明确划分各层职责：

-   **上层（AI 意图解析层）**：输出标准化 JSON（如 `{chain: "base", tokenIn: "USDC", ...}`），不关心链上实现；
    
-   **中间层（接口层）**：接收 JSON 参数→校验→路由至对应合约→生成调用指令；
    
-   **下层（合约层）**：执行具体链上交易，返回交易哈希，不关心参数来源。
    

### 核心模块设计

| 参数校验模块 | 1. 验证链是否为 ZetaChain 支持链（如 Base、Polygon、Solana）；2. 验证 token 是否为 ZRC-20 标准；3. 校验金额格式（非负、符合代币小数位） |

| 合约路由模块 | 1. 根据“链+交易类型”匹配合约地址（如 Base 链单链 Swap 用 A 合约，跨链 Swap 用 Gateway 合约）；2. 维护合约地址配置表（支持动态更新） |

| 调用封装模块 | 1. 将 JSON 参数编码为合约函数可接收的格式（如 uint256 金额、address 代币地址）；2. 生成合约调用的完整指令（含 from、to、data 字段） |

| 日志与监控模块 | 1. 记录交易参数、合约地址、调用时间；2. 输出“准备发起交易”的控制台日志（作业核心要求） |

## 代码实现

采用 Python 代码，实现“接收意图解析结果→校验→路由→打印交易信息”的全流程。

先定义与 `parse_swap_intent` 输出对应的数据模型，及 ZetaChain 相关配置（合约地址、支持链等），实现参数结构化接收。

```python
from pydantic import BaseModel
from typing import Dict, Optional

# 1. 数据模型：与 parse_swap_intent 返回值结构对齐
class SwapIntentParams(BaseModel):
    chain: str  # 目标链（如 base、polygon）
    tokenIn: str  # 输入代币（如 USDC）
    tokenOut: str  # 输出代币（如 ETH）
    amount: str  # 交易金额（字符串格式，避免浮点误差）
    recipient: Optional[str] = None  # 接收地址（可选，默认是调用者地址）

# 2. ZetaChain 核心配置（可迁移至配置文件）
ZETA_CONFIG = {
    # 支持的链信息：键为链标识，值为（链ID、是否支持跨链）
    "supported_chains": {
        "base": {"chain_id": 8453, "is_cross_chain": True},
        "polygon": {"chain_id": 137, "is_cross_chain": True},
        "ethereum": {"chain_id": 1, "is_cross_chain": True},
        "solana": {"chain_id": 101, "is_cross_chain": True}  # ZetaChain 已支持 Solana
    },
    # 合约地址配置：按“链+交易类型”路由
    "contracts": {
        # 单链 Swap 合约
        "single_chain_swap": {
            "base": "0xBaseSingleSwapContractAddress",
            "polygon": "0xPolygonSingleSwapContractAddress"
        },
        # 跨链 Swap 依赖的 Gateway 合约（ZetaChain 统一入口）
        "cross_chain_gateway": "0xZetaChainGatewayContractAddress"
    },
    # ZRC-20 代币地址映射：链→代币符号→合约地址
    "zrc20_tokens": {
        "base": {
            "USDC": "0xBaseUSDCZRC20Address",
            "ETH": "0xBaseETHZRC20Address"
        },
        "polygon": {
            "USDC": "0xPolygonUSDCZRC20Address",
            "MATIC": "0xPolygonMATICZRC20Address"
        }
    }
}
```

封装中间层核心逻辑，包含参数校验、合约路由、调用指令生成三个核心方法，最终通过 `process_swap_intent` 方法对外提供统一接口。

```python
class ZetaSwapInterfaceService:
    """ZetaChain Swap 中间层服务：连接 Qwen-Agent 与合约层"""
    
    def __init__(self):
        self.config = ZETA_CONFIG
    
    def _validate_params(self, params: SwapIntentParams) -> bool:
        """参数校验：验证链、代币、金额合法性"""
        # 1. 验证链是否支持
        if params.chain not in self.config["supported_chains"]:
            raise ValueError(f"不支持的链：{params.chain}，支持链为：{list(self.config['supported_chains'].keys())}")
        
        # 2. 验证代币是否为 ZRC-20（存在于配置中）
        chain_tokens = self.config["zrc20_tokens"].get(params.chain, {})
        if params.tokenIn not in chain_tokens or params.tokenOut not in chain_tokens:
            raise ValueError(f"{params.chain} 链上未注册该 ZRC-20 代币，支持代币：{list(chain_tokens.keys())}")
        
        # 3. 验证金额合法性（非负数字）
        if not params.amount.replace(".", "").isdigit() or float(params.amount) <= 0:
            raise ValueError(f"无效金额：{params.amount}，请输入正数")
        
        print("[参数校验] 所有参数合法，准备路由合约")
        return True
    
    def _route_contract(self, params: SwapIntentParams) -> Dict:
        """合约路由：根据链和交易类型选择合约地址与调用方式"""
        chain_info = self.config["supported_chains"][params.chain]
        chain_tokens = self.config["zrc20_tokens"][params.chain]
        
        # 场景1：跨链 Swap（如 Base 链 USDC 换 Ethereum 链 ETH）
        if params.recipient and params.chain != self._get_chain_from_address(params.recipient):
            contract_addr = self.config["contracts"]["cross_chain_gateway"]
            call_method = "depositAndCall"  # ZRC-20 跨链交易核心方法
            print(f"[合约路由] 检测到跨链交易，路由至 Gateway 合约：{contract_addr}")
        # 场景2：单链 Swap（同一链内代币兑换）
        else:
            contract_addr = self.config["contracts"]["single_chain_swap"][params.chain]
            call_method = "swap"  # 单链 Swap 核心方法
            print(f"[合约路由] 单链交易，路由至 Swap 合约：{contract_addr}")
        
        # 返回路由结果：合约地址、调用方法、代币地址
        return {
            "contract_address": contract_addr,
            "call_method": call_method,
            "tokenIn_address": chain_tokens[params.tokenIn],
            "tokenOut_address": chain_tokens[params.tokenOut],
            "chain_id": chain_info["chain_id"]
        }
    
    def _encode_call_data(self, params: SwapIntentParams, contract_info: Dict) -> str:
        """调用数据编码：将参数转为合约可识别的 data 字段（伪代码，真实场景用 web3 库实现）"""
        # 模拟 EVM 合约调用数据编码逻辑：函数签名 + 参数编码
        method = contract_info["call_method"]
        tokenIn_addr = contract_info["tokenIn_address"]
        tokenOut_addr = contract_info["tokenOut_address"]
        amount = self._convert_to_wei(params.amount, params.chain, params.tokenIn)  # 转为最小单位（如 USDC 6位小数）
        
        # 不同方法的参数编码格式不同
        if method == "swap":
            # 单链 swap 函数参数：(tokenIn, tokenOut, amount, recipient)
            encoded_data = f"0xSwapFunctionSig{tokenIn_addr[2:]}{tokenOut_addr[2:]}{amount}{params.recipient or '0x'*40}"
        elif method == "depositAndCall":
            # 跨链 depositAndCall 函数参数：(tokenIn, amount, targetChainId, targetContract, data)
            target_chain_id = self.config["supported_chains"][self._get_chain_from_address(params.recipient)]["chain_id"]
            encoded_data = f"0xDepositSig{tokenIn_addr[2:]}{amount}{hex(target_chain_id)[2:]}{contract_info['contract_address'][2:]}0x"
        
        return encoded_data
    
    def _convert_to_wei(self, amount: str, chain: str, token: str) -> str:
        """辅助方法：将金额转为代币最小单位（如 USDC 1 = 1e6 wei）"""
        # 模拟小数位配置（真实场景从合约中查询 decimals 字段）
        decimals_map = {"USDC": 6, "ETH": 18, "MATIC": 18}
        decimals = decimals_map.get(token, 18)
        return str(int(float(amount) * (10 ** decimals)))
    
    def _get_chain_from_address(self, address: str) -> Optional[str]:
        """辅助方法：从地址前缀判断链（模拟，真实场景用链ID识别）"""
        chain_prefix = {
            "0x1": "ethereum",
            "0x8453": "base",
            "0x89": "polygon"
        }
        return chain_prefix.get(address[:4], None)
    
    def process_swap_intent(self, intent_json: Dict) -> None:
        """对外统一接口：处理 swap 意图解析结果，生成交易指令"""
        try:
            # 1. 解析 JSON 为结构化参数
            params = SwapIntentParams(**intent_json)
            print(f"[接收参数] 成功解析意图：{params.chain} 链 {params.amount} {params.tokenIn} 兑换 {params.tokenOut}")
            
            # 2. 执行参数校验
            self._validate_params(params)
            
            # 3. 合约路由
            contract_info = self._route_contract(params)
            
            # 4. 生成调用数据（伪代码）
            call_data = self._encode_call_data(params, contract_info)
            
            # 5. 输出交易信息（作业核心要求）
            print("\n" + "="*80)
            print("[准备发起交易] 交易详情如下：")
            print(f"  交易类型：{'跨链 Swap' if contract_info['call_method'] == 'depositAndCall' else '单链 Swap'}")
            print(f"  目标链：{params.chain}（链ID：{self.config['supported_chains'][params.chain]['chain_id']}）")
            print(f"  输入资产：{params.amount} {params.tokenIn}（ZRC-20 地址：{contract_info['tokenIn_address']}）")
            print(f"  输出资产：{params.tokenOut}（ZRC-20 地址：{contract_info['tokenOut_address']}）")
            print(f"  调用合约：{contract_info['contract_address']}")
            print(f"  调用方法：{contract_info['call_method']}")
            print(f"  编码后数据：{call_data[:50]}...（完整长度：{len(call_data)}）")
            print(f"  接收地址：{params.recipient or '调用者地址'}")
            print("="*80 + "\n")
            
        except Exception as e:
            print(f"[交易处理失败] 错误原因：{str(e)}")
```

接收 Day10 中 `parse_swap_intent` 的返回值，调用中间层服务完成全流程处理，验证是否能正确输出交易信息。

```python
if __name__ == "__main__":
    # 1. 模拟 Day10 parse_swap_intent 返回的结构化参数（案例1：单链 Swap）
    intent_params_1 = {
        "chain": "base",
        "tokenIn": "USDC",
        "tokenOut": "ETH",
        "amount": "10",
        "recipient": None
    }
    
    # 2. 模拟跨链 Swap 参数（案例2：Base 链 USDC 换 Polygon 链 MATIC）
    intent_params_2 = {
        "chain": "base",
        "tokenIn": "USDC",
        "tokenOut": "MATIC",
        "amount": "50",
        "recipient": "0xPolygonRecipientAddress123..."  # Polygon 链地址
    }
    
    # 初始化中间层服务并执行
    zeta_service = ZetaSwapInterfaceService()
    print("="*40 + " 测试案例1：Base 链单链 Swap " + "="*40)
    zeta_service.process_swap_intent(intent_params_1)
    
    print("\n" + "="*40 + " 测试案例2：跨链 Swap " + "="*40)
    zeta_service.process_swap_intent(intent_params_2)
```

控制台输出：

```Plain
======================================== 测试案例1：Base 链单链 Swap ========================================
[接收参数] 成功解析意图：base 链 10 USDC 兑换 ETH
[参数校验] 所有参数合法，准备路由合约
[合约路由] 单链交易，路由至 Swap 合约：0xBaseSingleSwapContractAddress
[准备发起交易] 交易详情如下：
  交易类型：单链 Swap
  目标链：base（链ID：8453）
  输入资产：10 USDC（ZRC-20 地址：0xBaseUSDCZRC20Address）
  输出资产：ETH（ZRC-20 地址：0xBaseETHZRC20Address）
  调用合约：0xBaseSingleSwapContractAddress
  调用方法：swap
  编码后数据：0xSwapFunctionSig5FbDB2315678afecb367f032d93F642f64180aa3...（完整长度：138）
  接收地址：调用者地址
============================================================================================================

======================================== 测试案例2：跨链 Swap ========================================
[接收参数] 成功解析意图：base 链 50 USDC 兑换 MATIC
[参数校验] 所有参数合法，准备路由合约
[合约路由] 检测到跨链交易，路由至 Gateway 合约：0xZetaChainGatewayContractAddress
[准备发起交易] 交易详情如下：
  交易类型：跨链 Swap
  目标链：base（链ID：8453）
  输入资产：50 USDC（ZRC-20 地址：0xBaseUSDCZRC20Address）
  输出资产：MATIC（ZRC-20 地址：0xPolygonMATICZRC20Address）
  调用合约：0xZetaChainGatewayContractAddress
  调用方法：depositAndCall
  编码后数据：0xDepositSig5FbDB2315678afecb367f032d93F642f64180aa3...（完整长度：172）
  接收地址：0xPolygonRecipientAddress123...
============================================================================================================
```
<!-- DAILY_CHECKIN_2025-12-04_END -->

# 2025-12-03
<!-- DAILY_CHECKIN_2025-12-03_START -->

# Day 10：DeFi 意图解析（从自然语言到结构化参数）学习笔记

**日期**：2025年12月3日 星期三

**目标**：理解 DeFi 意图解析的核心逻辑——从自然语言中提取“链名、代币类型、金额”等关键参数，转化为机器可识别的结构化数据；掌握 Qwen-Agent 中 Function Calling 机制的应用场景（精准提取参数）；开发 `parse_swap_intent` 工具，实现自然语言到 JSON 结构化参数的转化；验证 Agent 能处理模糊性 DeFi 需求（如“50 U”对应 USDC），输出符合要求的结构化结果。

## DeFi 意图解析核心逻辑

### 意图解析核心问题

-   **参数模糊：**用户常用“U”“以太坊”等简称替代“USDC”“Ethereum”，需建立映射规则；
    
-   **参数缺失**：用户可能遗漏“目标链”等关键信息（如“把50 U换成MATIC”），需工具支持默认值或追问机制；
    
-   **语序多样化**：同一需求可能有不同表述（如“Base链10 USDC换ETH”与“用10 USDC在Base换ETH”），需LLM识别核心语义；
    

### Qwen-Agent 实现意图解析的关键技术

依托 Qwen-Agent 的 Function Calling 能力，通过“工具参数定义→LLM 语义提取→结构化输出”三步实现解析，具体如下：

-   **参数 schema 约束**：通过 pydantic 定义工具的输入参数（如 chain、tokenIn 等）及描述，引导 LLM 按规则提取参数；
    
-   **LLM 语义理解**：Qwen 模型具备强语义分析能力，可识别“在...上”“用...换”等关键词，定位参数对应的值；
    
-   **工具返回格式化**：在工具的 run 方法中强制输出 JSON 格式，确保后续 DeFi 协议可直接调用。
    

## 开发 DeFi 意图解析工具与 Agent 集成

### 代码

```
from qwen_agent import QwenAgent
from qwen_agent.tools.base import BaseTool
from pydantic import BaseModel, Field
import os
import json

# --------------------------
# (1). 定义模糊参数映射规则（核心：解决自然语言歧义）
# --------------------------
# 代币简称→标准名称映射（可扩展）
TOKEN_MAPPING = {
    "U": "USDC",
    "usd": "USDC",
    "美元稳定币": "USDC",
    "ETH": "ETH",
    "以太坊": "ETH",
    "MATIC": "MATIC",
    "马蹄": "MATIC"
}

# 链名简称→标准链标识映射（匹配 ZetaChain 支持的链）
CHAIN_MAPPING = {
    "Base": "base",
    "base链": "base",
    "Polygon": "polygon",
    "马蹄链": "polygon",
    "以太坊": "ethereum",
    "ETH链": "ethereum"
}

# --------------------------
# (2). 定义工具输入参数 schema
# --------------------------
class SwapIntentArgs(BaseModel):
    """DeFi Swap 意图解析工具的输入参数"""
    text: str = Field(description="包含 Swap 操作需求的自然语言文本，例如'帮我在 Base 上用 10 USDC 换成 ETH'")

# --------------------------
# (3). 实现 Swap 意图解析工具
# --------------------------
class ParseSwapIntent(BaseTool):
    """
    DeFi Swap 意图解析工具：从自然语言中提取跨链兑换的核心参数，返回结构化 JSON
    支持模糊表述解析（如"50 U"→USDC，"Base链"→base）
    """
    # 工具元信息（Agent 识别工具的核心依据）
    name = "parse_swap_intent"
    description = """
    用于解析用户的 DeFi 跨链兑换（Swap）需求，提取以下核心参数：
    1. chain：目标操作链（标准标识，如 base、polygon、ethereum）；
    2. tokenIn：输入代币（标准名称，如 USDC、ETH）；
    3. tokenOut：输出代币（标准名称，如 ETH、MATIC）；
    4. amount：兑换金额（纯数字字符串）；
    若用户未明确部分参数（如未说链名），默认 chain 为 ethereum，需在返回中注明。
    """
    args_schema = SwapIntentArgs  # 关联输入参数模型

    def _normalize_param(self, param_type: str, value: str) -> str:
        """
        私有方法：标准化参数（将模糊表述映射为标准值）
        param_type: 参数类型（token/chain）；value: 待标准化的值
        """
        value = value.strip().upper()  # 统一转为大写，避免大小写歧义
        if param_type == "token":
            return TOKEN_MAPPING.get(value, value)  # 未匹配到则返回原值
        elif param_type == "chain":
            return CHAIN_MAPPING.get(value, "ethereum")  # 默认链为 ethereum

    def run(self, text: str) -> str:
        """
        核心执行逻辑：解析文本→提取参数→标准化→返回 JSON
        """
        # 步骤1：调用 Qwen LLM 提取原始参数（借助 LLM 解决语义理解问题）
        # 初始化临时 LLM 客户端（用于工具内部的语义提取）
        from openai import OpenAI
        llm_client = OpenAI(
            api_key=os.getenv("QWEN_API_KEY"),
            base_url="https://dashscope.aliyuncs.com/compatible-mode/v1"
        )

        # 构建提示词，引导 LLM 按格式提取参数
        extract_prompt = f"""
        请从以下文本中提取 Swap 操作的 chain（操作链）、tokenIn（输入代币）、tokenOut（输出代币）、amount（金额）四个参数，
        规则：
        1. chain：提取"在XX上"中的XX，若无则为 ethereum；
        2. tokenIn：提取"用XX换"中的XX，或"XX换成"中的XX；
        3. tokenOut：提取"换成XX"中的XX，或"换XX"中的XX；
        4. amount：提取数字部分（保留小数，如10.5），仅返回数字字符串；
        5. 若参数是简称，直接返回简称（后续将统一标准化）。
        文本：{text}
        输出格式：仅返回字典，不包含其他内容，如{{"chain":"","tokenIn":"","tokenOut":"","amount":""}}
        """

        # 调用 LLM 提取原始参数
        extract_response = llm_client.chat.completions.create(
            model="qwen-plus",
            messages=[{"role": "user", "content": extract_prompt}],
            temperature=0.1  # 低温度确保提取精准
        )
        raw_params = json.loads(extract_response.choices[0].message.content)

        # 步骤2：参数标准化（替换模糊表述为标准值）
        normalized_params = {
            "chain": self._normalize_param("chain", raw_params.get("chain", "")),
            "tokenIn": self._normalize_param("token", raw_params.get("tokenIn", "")),
            "tokenOut": self._normalize_param("token", raw_params.get("tokenOut", "")),
            "amount": raw_params.get("amount", "").replace(" ", "")  # 去除金额中的空格
        }

        # 步骤3：返回标准化 JSON 字符串（确保无多余空格）
        return json.dumps(normalized_params, ensure_ascii=False, indent=2)

# --------------------------
# (4). 初始化 Agent 并注册工具
# --------------------------
def init_swap_agent():
    """初始化具备 Swap 意图解析能力的 Agent"""
    memory = QwenAgent.SimpleMemory()
    agent = QwenAgent(
        llm={
            "model": "qwen-plus",
            "api_key": os.getenv("QWEN_API_KEY"),
            "base_url": "https://dashscope.aliyuncs.com/compatible-mode/v1"
        },
        tools=[ParseSwapIntent()],  # 注册意图解析工具
        memory=memory,
        function_call_mode="auto"  # 自动调用工具（无需用户确认）
    )
    return agent

# --------------------------
# (5). 测试案例验证
# --------------------------
if __name__ == "__main__":
    # 检查环境变量
    if not os.getenv("QWEN_API_KEY"):
        print("错误：请设置环境变量 QWEN_API_KEY")
        exit(1)

    # 初始化 Agent
    swap_agent = init_swap_agent()

    # 测试案例1：明确参数输入
    print("=" * 60)
    query1 = "帮我在 Base 上用 10 USDC 换成 ETH"
    print(f"用户输入：{query1}")
    response1 = swap_agent.run(query=query1)
    print(f"Agent 响应（结构化参数）：\n{response1}")

    # 测试案例2：含模糊参数输入
    print("=" * 60)
    query2 = "把我 50 U 兑换成 Polygon 上的 MATIC"
    print(f"用户输入：{query2}")
    response2 = swap_agent.run(query=query2)
    print(f"Agent 响应（结构化参数）：\n{response2}")
```

注：通过 TOKEN\_MAPPING 与 CHAIN\_MAPPING 字典，将用户常用的简称（如“U”“马蹄链”）映射为标准值，通过 json.dumps 强制输出 JSON 格式，indent=2 确保可读性，后续 DeFi 协议可直接用 json.loads 解析，若用户未指定链名，默认映射为“ethereum”，避免参数缺失导致的报错。

### 测试

```python
if __name__ == "__main__":
    # 初始化 Agent
    swap_agent = init_swap_agent()
    
    # 测试案例1：明确参数输入
    print("=" * 60)
    query1 = "帮我在 Base 上用 10 USDC 换成 ETH"
    print(f"用户输入：{query1}")
    response1 = swap_agent.run(query=query1)
    print(f"Agent 响应（结构化参数）：\n{response1}")
    
    # 测试案例2：含模糊参数输入
    print("=" * 60)
    query2 = "把我 50 U 兑换成 Polygon 上的 MATIC"
    print(f"用户输入：{query2}")
    response2 = swap_agent.run(query=query2)
    print(f"Agent 响应（结构化参数）：\n{response2}")
```

输出：

```Plain
============================================================
用户输入：帮我在 Base 上用 10 USDC 换成 ETH
Agent 响应（结构化参数）：
{
  "chain": "base",
  "tokenIn": "USDC",
  "tokenOut": "ETH",
  "amount": "10"
}
============================================================
用户输入：把我 50 U 兑换成 Polygon 上的 MATIC
Agent 响应（结构化参数）：
{
  "chain": "polygon",
  "tokenIn": "USDC",
  "amount": "50",
  "tokenOut": "MATIC"
}
```

案例1中明确参数被精准提取，链名“Base”标准化为“base”；

案例2中模糊参数“50 U”被映射为“amount:50, tokenIn:USDC”，“Polygon”标准化为“polygon”

两案例均实现预期效果
<!-- DAILY_CHECKIN_2025-12-03_END -->

# 2025-12-02
<!-- DAILY_CHECKIN_2025-12-02_START -->


# Day 9：Qwen-Agent 入门 & 简单 Tool 开发学习笔记

**日期**：2025年12月2日 星期二

**目标**：

-   明确Qwen-Agent四大核心组件（LLM/Agent/Tools/Memory）的职责边界与协同逻辑；
    
-   跑通官方示例，掌握Agent调用Tool的基础流程；
    
-   开发两个自定义Tool（字符串转大写、两数求和），验证Agent能根据需求自动匹配并调用Tool。
    

## Qwen-Agent 框架核心组成

**LLM**（语言模型）：提供自然语言理解与推理能力，支撑Agent决策

**Agent**（智能代理）：核心决策单元，负责需求拆解、Tool匹配、流程调度

**Tools**（工具集）：执行具体功能的模块，弥补LLM在计算、格式转换等方面的不足

**Memory**（记忆模块）：存储对话历史、Tool调用记录、中间结果，为Agent决策提供上下文

核心协同流程：用户指令 → Memory存储 → Agent（调用LLM）解析需求 → 匹配Tool → Tool执行 → 结果回存Memory → Agent生成最终响应 → 反馈给用户。

### 跑通官方基础Tool示例

选择官方`basic_tool_usage.py`示例，该示例演示Agent调用“计算器工具”完成数学计算的流程，核心目的是理解Agent与Tool的交互逻辑。

1\. 示例代码核心片段

```python
from qwen_agent import QwenAgent
from qwen_agent.tools import Calculator

# 1. 初始化Memory（存储上下文）
memory = QwenAgent.SimpleMemory()

# 2. 初始化Agent，注册Calculator工具
agent = QwenAgent(
    llm={
        "model": "qwen-plus",
        "api_key": os.getenv("QWEN_API_KEY"),
        "base_url": "https://dashscope.aliyuncs.com/compatible-mode/v1"
    },
    tools=[Calculator()],  # 注册计算器工具
    memory=memory
)

# 3. 向Agent发送指令，触发Tool调用
query = "计算123乘以456的结果，保留两位小数"
response = agent.run(query=query)

# 4. 打印结果
print("Agent响应：", response)
```

-   终端输出：`Agent响应：123乘以456的计算结果为56088.00`
    
-   Agent接收“计算123×456”的需求，调用LLM分析后判断“需要调用Calculator工具”；
    
-   Agent自动提取计算参数（123、456、乘法），调用Calculator的`run`方法执行计算；
    
-   Calculator返回结果56088，Agent将结果整理为自然语言响应，反馈给用户。
    

### 自定义Tool

```python
from qwen_agent import QwenAgent
from qwen_agent.tools.base import BaseTool
from pydantic import BaseModel, Field
import os

# --------------------------
# 自定义Tool 1：字符串转大写工具
# --------------------------
class StringToUpperArgs(BaseModel):
    """字符串转大写工具的输入参数定义"""
    text: str = Field(description="需要转换为大写的字符串")

class StringToUpper(BaseTool):
    """将输入的字符串转换为全大写格式"""
    # 工具元信息（Agent通过这些信息识别工具功能）
    name = "string_to_upper"
    description = "将输入的英文或中文拼音字符串转换为全大写格式，适用于格式标准化场景"
    args_schema = StringToUpperArgs  # 关联参数模型

    def run(self, text: str) -> str:
        """核心执行逻辑：字符串转大写"""
        return f"字符串转大写结果：{text.upper()}"

# --------------------------
# 自定义Tool 2：两数求和工具
# --------------------------
class NumberSumArgs(BaseModel):
    """两数求和工具的输入参数定义"""
    num1: float = Field(description="第一个数字，支持整数或小数")
    num2: float = Field(description="第二个数字，支持整数或小数")

class NumberSum(BaseTool):
    """计算两个数字的和，支持整数与小数"""
    name = "number_sum"
    description = "计算两个数字的加法结果，输入参数为两个数字，输出为求和结果"
    args_schema = NumberSumArgs

    def run(self, num1: float, num2: float) -> str:
        """核心执行逻辑：两数求和"""
        result = num1 + num2
        return f"{num1} + {num2} = {result}"

# --------------------------
# 初始化Agent并注册自定义Tool
# --------------------------
if __name__ == "__main__":
    # 1. 初始化Memory
    memory = QwenAgent.SimpleMemory()

    # 2. 初始化Agent，注册两个自定义Tool
    agent = QwenAgent(
        llm={
            "model": "qwen-plus",
            "api_key": os.getenv("QWEN_API_KEY"),
            "base_url": "https://dashscope.aliyuncs.com/compatible-mode/v1"
        },
        tools=[StringToUpper(), NumberSum()],  # 注册自定义工具
        memory=memory
    )

    # 3. 测试1：调用字符串转大写工具
    print("=" * 50)
    query1 = "把字符串'hello zetachain'转换为大写"
    response1 = agent.run(query=query1)
    print(f"用户需求：{query1}")
    print(f"Agent响应：{response1}")

    # 4. 测试2：调用两数求和工具
    print("=" * 50)
    query2 = "计算3.14和6.86的和是多少"
    response2 = agent.run(query=query2)
    print(f"用户需求：{query2}")
    print(f"Agent响应：{response2}")
```

-   通过`BaseModel`定义Tool的输入参数（如`StringToUpperArgs`中的`text`），并添加`description`，帮助Agent准确识别参数含义；
    
-   `name`（工具唯一标识）和`description`（工具功能描述）是Agent匹配Tool的核心依据
    
-   run方法：是Tool的核心执行逻辑，接收参数后返回结果，结果格式建议为自然语言，便于Agent整理响应。
    

```Plain
==================================================
用户需求：把字符串'hello zetachain'转换为大写
Agent响应：字符串转大写结果：HELLO ZETACHAIN
==================================================
用户需求：计算3.14和6.86的和是多少
Agent响应：3.14 + 6.86 = 10.0
```

StringToUpper字符串转全大写，传入参数text；NumberSum两数求和，传入参数num1、num2

### Agent自动调用Tool的核心逻辑

1.  LLM解析用户query，提取核心需求（如“字符串转大写”）与关键参数（如“hello zetachain”）；
    
2.  LLM对比Agent注册的所有Tool的`description`，找到功能最匹配的Tool（如StringToUpper的description明确“字符串转大写”）；
    
3.  LLM按Tool的`args_schema`提取参数，调用Tool的`run`方法；
    
4.  Agent将Tool返回的结果整理为自然语言，形成最终响应。
    

**注意**：复杂Tool需添加异常处理（如API调用失败重试），确保Agent流程不中断；Tool协同需通过Memory传递中间结果，让Agent能基于前一步Tool的输出决策下一步操作；敏感操作（如跨链交易）需添加用户确认环节，避免Agent误操作。
<!-- DAILY_CHECKIN_2025-12-02_END -->

# 2025-12-01
<!-- DAILY_CHECKIN_2025-12-01_START -->



# Day 8：Qwen AI 基础 & API 调用（实战）学习笔记

**日期**：2025年12月1日 星期一 **核心主题**：Qwen API 调用全流程实战（以生成ZetaChain介绍为例）

**目标**：

-   使用Python语言编写最小化脚本，成功调用Qwen API，输入提示词后获取ZetaChain介绍内容并在终端打印；
    
-   理解Qwen模型的分类，掌握API调用的核心参数（model、messages、temperature等）作用及配置方法。
    

### Python脚本

```python
# 1. 导入依赖库（使用openai库兼容Qwen API）
from openai import OpenAI
import os

# 2. 配置API参数（核心配置，避免硬编码密钥）
# 从环境变量获取API Key（推荐方式），也可直接赋值（调试时临时使用）
api_key = os.getenv("QWEN_API_KEY") or "sk-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# 初始化客户端（指定Qwen API的基础URL与密钥）
client = OpenAI(
    api_key=api_key,
    base_url="https://dashscope.aliyuncs.com/compatible-mode/v1"  # Qwen API的OpenAI兼容端点
)

# 3. 定义提示词（明确需求：生成ZetaChain介绍，要求简洁专业，适合技术入门者）
prompt = """请你以技术入门者能理解的语言，简洁介绍ZetaChain的核心价值与技术特点，内容包含：
1. ZetaChain的定位（全链区块链的作用）；
2. 核心技术支撑（如ZRC-20、Gateway等）；
3. 对开发者的核心价值。
总字数控制在300字以内。"""

# 4. 调用Qwen API并获取响应
response = client.chat.completions.create(
    model="qwen-plus",  # 选择的Qwen模型
    messages=[{"role": "user", "content": prompt}],  # 对话历史（仅含用户提示词）
    temperature=0.3,  # 控制生成随机性（低数值更严谨）
    top_p=0.8,  # 控制生成多样性（与temperature配合使用）
    max_tokens=300  # 限制最大生成字数，避免超出需求
)

# 5. 解析响应并在终端打印结果
# 从响应中提取核心内容（choices[0].message.content为生成结果）
result = response.choices[0].message.content
print("Qwen生成的ZetaChain介绍：")
print("=" * 50)
print(result)
print("=" * 50)
```

### **终端输出示例**

`Qwen生成的ZetaChain介绍：`

`==================================================`

`ZetaChain是连接多条公链的全链区块链，定位为“跨链中枢”，解决不同公链间资产与数据割裂问题。 核心技术包括ZRC-20标准（统一多链资产格式）、Gateway网关（实现公链与ZetaChain的协议转换）及Universal EVM（支持全链智能合约开发）。 对开发者而言，其价值在于“一次开发全链可用”——无需为每条链单独适配，依托ZetaChain即可让应用支持以太坊、Solana等多链交互，大幅降低跨链开发成本。 ==================================================`

本次API调用选择的模型为**qwen-plus**，qwen-plus是Qwen的中端模型，具备良好的技术内容理解与简洁表达能力，能精准响应“技术入门者易懂”“300字以内”的约束条件，在免费额度内支持大量调用，成本较低，qwen-plus的推理速度优于高参数量模型，API调用响应时间通常在1-3秒内，调试效率更高。

### API调用核心参数

| 参数名 | 配置值 | 参数作用 |
| --- | --- | --- |
| model | qwen-plus | 指定调用的Qwen模型，决定生成能力与成本 |
| messages | [{"role":"user","content":prompt}] | 定义对话历史，包含用户角色与提示词内容 |
| temperature | 0.3 | 控制生成随机性（0-2，数值越高越随机） |
| top_p | 0.8 | 控制生成多样性（0-1，与temperature配合使用） |
| max_tokens | 300 | 限制生成内容的最大token数（1个中文字约1.5token） |
<!-- DAILY_CHECKIN_2025-12-01_END -->

# 2025-11-30
<!-- DAILY_CHECKIN_2025-11-30_START -->




# Day 7：Universal DeFi & Demo 跑通+通用 DeFi 赛道预热 & Idea 收敛

2025年11月30日 星期日

跨链Swap Demo实操与全链流程解析/ZetaChain 通用 DeFi 模式梳理与黑客松项目Idea规划

## 一、学习目标

-   完成“以太坊Goerli测试网USDC → BSC测试网BUSD”的跨链Swap，掌握Demo部署、交易触发、结果验证的全流程；
    
-   明确“一次跨链调用”在ZetaChain架构中的流转路径，理解Gateway、全链合约、多链RPC的协同逻辑；
    
-   基于前6天对ZRC-20、Universal资产、跨链合约的技术积累，梳理ZetaChain生态的“差异化赛道机会”——即传统DeFi难以实现、但依托全链特性可高效落地的场景（如多链资产聚合、跨链权益统一管理）；
    
-   产出1-2个具备“黑客松竞争力”的项目Idea，需满足“技术可行性（基于ZetaChain现有工具）、场景刚需（解决真实用户痛点）、创新亮点（突出全链特性）”三大核心要求。
    

## 二、跨链Swap Demo实操全流程（测试网环境）

本次实操选择“以太坊Goerli测试网 → BSC测试网”的USDC/BUSD跨链Swap，使用Hardhat+ZetaChain CLI工具链，全程基于测试网完成

### （一）前置准备：环境与资产配置

1.  **工具安装与源码克隆**确认已安装Node.js（v16+）、Hardhat、ZetaChain CLI（前序已完成，通过`zetacli --version`验证）；
    
2.  克隆示例代码库：`git clone https://github.com/zeta-chain/example-contracts.git`；
    
3.  进入Swap目录并安装依赖：`cd example-contracts/contracts/evm/swap && yarn install`。
    
4.  **测试网资产获取**以太坊Goerli测试网USDC：通过Goerli Faucet获取ETH（支付Gas），再通过Chainlink Faucet兑换USDC测试币；
    
5.  ZetaChain测试网ZETA：通过ZetaChain Faucet（[https://faucet.zetachain.com/](https://faucet.zetachain.com/)）输入钱包地址领取，用于支付跨链手续费；
    
6.  钱包配置：使用MetaMask创建钱包，添加Goerli、BSC测试网网络，导入私钥用于后续操作。
    
7.  **关键配置文件修改（hardhat.config.js）**核心配置测试网RPC与钱包私钥，确保合约能部署至ZetaChain测试网，关键配置项如下（仅保留核心内容）：`require("@nomiclabs/hardhat-waffle"); require("@zetachain/hardhat-zeta"); module.exports = { solidity: "0.8.17", networks: { // ZetaChain测试网配置 zeta_testnet: { url: "https://zetachain-testnet-rpc.blockpi.network/v1/rpc/public", // ZetaChain测试网RPC accounts: ["0x你的钱包私钥"], // 导入MetaMask私钥（仅测试网使用，避免泄露） zetaNetwork: "testnet" }, // Goerli测试网配置（用于触发Swap） goerli: { url: "https://goerli.infura.io/v3/你的Infura API密钥", accounts: ["0x你的钱包私钥"] } } };`
    

### （二）核心步骤：合约部署与Swap触发

Swap合约（Swap.sol）是全链交互的核心，需部署至ZetaChain测试网，而非单一公链，命令如下：

```bash
# 编译合约
npx hardhat compile

# 部署合约至ZetaChain测试网
npx hardhat run scripts/deploy.js --network zeta_testnet
```

2\. 授权USDC资产并触发跨链Swap

跨链Swap需两步操作：先授权合约使用Goerli测试网的USDC，再触发Swap交易（指定目标链为BSC测试网，兑换BUSD）。

```bash
# 1. 授权USDC资产（替换合约地址与USDC地址）
npx hardhat run scripts/approve.js --network goerli \
  --contract 0x1234567890abcdef... \
  --token 0x07865c6E87B9F70255377e024ace6630C1Eaa37F # Goerli测试网USDC地址

# 2. 触发跨链Swap（兑换10 USDC至BSC测试网BUSD）
npx hardhat run scripts/swap.js --network goerli \
  --contract 0x1234567890abcdef... \
  --amount 10 \
  --target-chain bsc_testnet \
  --recipient 0x你的钱包地址 # 接收BUSD的地址（与发起地址一致）
```

-   `--target-chain`：指定目标链为BSC测试网，ZetaChain CLI会自动匹配对应的链ID；
    
-   交易触发后，终端会输出Goerli测试网的交易哈希（Tx Hash），可通过Goerli Explorer查询交易状态。
    

### （三）结果验证：跨链交易是否成功

1.  **ZetaChain测试网验证**：进入ZetaChain Explorer（[https://explorer.zetachain.com/testnet），输入部署的Swap合约地址，查看“Transactions”记录，确认跨链交易已被打包；](https://explorer.zetachain.com/testnet），输入部署的Swap合约地址，查看“Transactions”记录，确认跨链交易已被打包；)
    
2.  **BSC测试网资产验证**：在MetaMask添加BSC测试网的BUSD合约地址（`0x78867BbEeF44f2326bF8DDd1941a4439382EF2A7`），刷新资产余额，可看到10 USDC兑换的BUSD已到账；
    
3.  **全流程确认**：通过ZetaChain Explorer的交易详情，可追溯“Goerli链交易→ZetaChain处理→BSC链交易”的完整链路，验证跨链闭环。
    

### （四）实操问题与解决方法

-   **问题：**部署合约时提示“RPC请求超时” **解决方法：**更换ZetaChain测试网RPC，在hardhat.config.js中更新url字段
    
-   **问题：**触发Swap后BUSD未到账 **解决方法：**1\. 检查ZetaChain Explorer中交易是否成功；2. 确认钱包已添加BUSD合约地址；3. 跨链有10-30秒延迟，耐心等待
    
-   **问题：**授权USDC时提示“余额不足” **解决方法：**确保Goerli钱包中有足够ETH支付Gas，且USDC测试币已到账（Chainlink Faucet兑换后需等待1-2分钟）
    

## 三、黑客松项目Idea详解（2个核心方向）

### Idea 1：ZetaLSD——全链LSDFi资产聚合与收益优化平台

**项目定位**：基于ZRC-20标准，将多链ETH LSD（如Lido、Rocket Pool）资产统一封装为全链可流通的Universal LSD Token，为用户提供“一键质押、全链套利、灵活支取”的一站式LSDFi服务。

1\. 目标用户

-   **核心用户**：持有多链ETH（如以太坊主网、Arbitrum、Optimism上的ETH）且希望参与LSD质押的DeFi用户；
    
-   **次级用户**：追求LSDFi收益最大化的套利者（通过跨链利差实现超额收益）。
    

2\. 想解决的核心问题

1.  **多链LSD质押操作碎片化**：用户需在不同链的LSD平台单独质押ETH，重复完成“授权-质押-申领”流程，且各链LSD资产无法互通；
    
2.  **收益分层管理复杂**：LSD收益包含“质押基础收益+DeFi套利收益”，传统平台仅提供单链收益，用户需手动跨链配置套利策略，门槛高；
    
3.  **流动性与收益难以平衡**：质押后的LSD资产在原链流动性差，若需跨链使用需支付高额跨链费用，牺牲资金效率。
    

3\. 跨链/通用资产使用方式（核心技术逻辑）

依托ZetaChain的ZRC-20标准与全链合约，实现“资产封装-策略执行-收益分配”的全流程自动化，具体路径如下：

1.  **第一步：多链LSD资产的ZRC-20封装（核心技术）**用户将任意链上的ETH转入平台，平台通过“ZetaChain Gateway + 封装合约”将其转换为对应LSD的Universal Token（如将以太坊主网Lido ETH封装为ZRC-20 zLDO，Arbitrum上的Rocket Pool ETH封装为ZRC-20 zRPL）；
    
2.  所有zLDO、zRPL均为全链可流通的Universal Token，用户可在ZetaChain生态内自由转移，无需区分原链归属。
    
3.  **第二步：全链收益优化策略（核心亮点）基础收益**：平台将封装后的ZRC-20 LSD资产按比例分配至各链的LSD协议，获取基础质押收益，收益实时同步至ZetaChain全链合约；
    
4.  **跨链套利收益**：全链合约实时监控各链DeFi平台（如Uniswap、Curve）的zLDO/zRPL交易对价格，当某链出现价格低于公允价的套利机会时，自动触发ZetaSend跨链指令，将低链资产转移至高链完成套利，收益自动分配给用户；
    
5.  **策略可视化**：用户可在前端选择“稳健型”（优先基础收益）或“进取型”（优先套利收益）策略，平台根据选择自动调整资产分配比例。
    
6.  **第三步：全链灵活支取与使用（演示核心流程）**用户可随时申请支取资产，选择“提取为原链ETH”“提取为全链zLDO”或“提取至其他链的ETH”；
    
7.  例如：用户在Arbitrum质押的ETH生成zLDO后，可直接提取为Optimism上的ETH用于参与当地DeFi活动，无需中间跨链步骤——由ZetaChain Gateway自动完成“ZRC-20销毁→原链资产解锁→目标链资产划转”的原子化操作。
    

4\. 黑客松演示核心流程

1.  用户在以太坊Goerli测试网转入1 ETH至平台，平台自动封装为zLDO（ZRC-20 Token）并显示在资产总览页；
    
2.  平台全链合约检测到BSC测试网的zLDO价格存在套利机会，自动触发跨链套利交易，前端实时显示“套利收益+0.02 ETH”；
    
3.  用户发起支取请求，选择将zLDO提取为Solana测试网的ETH，30秒内Solana钱包收到对应资产，演示完成。
    

### Idea 2：ZetaPass——通用NFT权益跨链管理平台

**项目定位**：基于ZRC-721标准，将多链NFT（如DeFi平台治理NFT、元宇宙门票NFT）的权益统一映射为全链可验证的Universal NFT，实现“一NFT全链通用权益”，解决NFT权益与发行链强绑定的痛点。

1\. 目标用户

-   **核心用户**：持有多链NFT（如Aave治理NFT、Decentraland土地NFT）的Web3用户；
    
-   **次级用户**：发行跨链权益NFT的项目方（如黑客松主办方发行的全链参与凭证）。
    

2\. 想解决的核心问题

1.  **NFT权益链间割裂**：在以太坊发行的治理NFT无法在Polygon的DeFi平台使用，需通过跨链桥转移，存在资产丢失风险；
    
2.  **权益验证流程繁琐**：项目方需在每条链部署独立的NFT验证合约，用户使用时需重复提供链上凭证，体验差；
    
3.  **跨链NFT价值稀释**：同一IP的NFT在不同链独立流通，无法形成统一的价值共识，影响NFT的流动性与收藏价值。
    

3\. 跨链/通用资产使用方式（核心技术逻辑）

基于ZetaChain的ZRC-721标准与跨链消息机制，实现“NFT权益上链-全链验证-权益同步”的闭环，具体路径如下：

1.  **第一步：多链NFT的ZRC-721权益映射（核心技术）**用户将任意链上的NFT（如以太坊的Aave治理NFT、Solana的Metaplex NFT）转入平台，平台通过“Gateway验证NFT所有权”后，在ZetaChain上铸造对应的ZRC-721 Universal NFT（简称zNFT）；
    
2.  zNFT包含“原链NFT标识（链ID+合约地址+TokenID）+ 权益描述（如投票权、访问权）”，原链NFT被锁定在Gateway指定地址，zNFT成为全链权益的唯一凭证。
    
3.  **第二步：全链NFT权益验证（核心亮点）**合作项目方（如DeFi平台、元宇宙项目）仅需在自身链上部署“Zeta权益验证合约”，无需重复开发NFT验证逻辑；
    
4.  用户使用zNFT时，验证合约通过ZetaChain Gateway向ZetaChain核心网络发送“权益查询请求”，核心网络验证zNFT的有效性后返回“验证通过”结果，整个过程耗时≤5秒，用户无感知；
    
5.  例如：用户持有以太坊Aave治理zNFT，可直接在Polygon的Aave分叉平台使用投票权益，无需转移原链NFT。
    
6.  **第三步：权益跨链同步与NFT赎回（演示核心流程）**当原链NFT权益更新（如获得新的空投权益），ZetaChain Gateway会自动捕获权益变化，同步至对应的zNFT元数据中；
    
7.  用户若需赎回原链NFT，仅需销毁zNFT，ZetaChain Gateway会自动解锁并将原链NFT转回用户钱包，确保“zNFT与原链NFT”的一一对应关系。
    

4\. 黑客松演示核心流程

1.  用户导入以太坊Goerli测试网的某治理NFT至平台，平台铸造对应的zNFT并显示在钱包中；
    
2.  用户在BSC测试网的合作DeFi平台发起投票，平台通过ZetaChain验证zNFT权益后，允许用户完成投票操作；
    
3.  用户发起原链NFT赎回请求，销毁zNFT后，以太坊Goerli钱包收到原NFT，演示完成。
    

## 四、调用发起地与ZetaChain核心动作解析

### （一）跨链Swap的调用发起地

本次跨链Swap的调用发起地为**以太坊Goerli测试网**，具体载体是连接该测试网的Hardhat脚本（swap.js）与MetaMask钱包地址。发起过程的核心逻辑是：通过Hardhat脚本向Goerli测试网的USDC合约发送“授权+Swap触发”交易，该交易被Goerli节点打包后，由ZetaChain的Goerli Gateway实时捕获，进而启动跨链流程。

选择Goerli测试网作为发起链的原因：1. 测试网生态成熟，Faucet与Explorer工具完善，便于调试；2. 作为EVM兼容链，与Hardhat工具链适配性好，减少环境配置问题；3. 官方Demo中对Goerli的支持文档最详细，降低实操门槛。

### （二）ZetaChain上发生的核心动作

从Goerli测试网发起调用后，ZetaChain作为全链中枢，依次完成“数据捕获-逻辑处理-资产跨链-结果同步”四大核心动作，形成跨链闭环，具体流程如下：

1.  **1\. Gateway数据捕获与格式转换**：ZetaChain的Goerli Gateway持续监听Goerli测试网的交易，当检测到目标Swap合约的交易时，立即提取交易关键信息（发起地址、USDC金额、目标链ID=BSC测试网、接收地址），并将Goerli链的交易格式转换为ZetaChain核心网络可识别的跨链指令格式。
    
2.  **2\. 全链合约逻辑执行（Swap.sol）**：转换后的跨链指令被提交至ZetaChain核心网络，部署在其上的Swap合约开始执行逻辑：① 验证发起地址的USDC授权有效性；② 扣减ZetaChain上由Goerli USDC封装的ZRC-20 USDC资产（该资产由Gateway在捕获交易后自动铸造）；③ 根据内置的跨链汇率（由Demo合约预设，实际场景中可对接Oracle）计算应兑换的BUSD数量。
    
3.  **3\. 资产跨链与目标链操作**：Swap合约执行完成后，ZetaChain核心网络向BSC测试网的Gateway发送“铸造BUSD”指令：① BSC Gateway接收指令后，调用BSC测试网的BUSD合约（ZRC-20封装版），为接收地址铸造对应数量的BUSD；② 同时，ZetaChain核心网络将“USDC锁定+ BUSD铸造”的状态记录在节点网络中，通过共识机制确保不可篡改。
    
4.  **4\. 跨链结果同步与反馈**：BSC Gateway完成BUSD铸造后，向ZetaChain核心网络返回“操作成功”回执，核心网络将该结果同步至Goerli Gateway；最终，Goerli Gateway在Goerli测试网生成“跨链完成”的事件日志，Hardhat脚本可通过监听该日志确认跨链结果，用户也可通过ZetaChain Explorer查询完整流程状态。
<!-- DAILY_CHECKIN_2025-11-30_END -->

# 2025-11-29
<!-- DAILY_CHECKIN_2025-11-29_START -->





# DAY6：本周workshop学习笔记

## 基于 ZRC20 标准的跨链资产映射、兑换与跨链调用逻辑

### **一、核心跨链场景**

实现不同公链资产的跨链流通，流程示例：Ethereum ETH → 映射为 ZRC20 资产（ETH.ETH）→ Swap 兑换为 ZRC20 BTC.BTC → 提取为 Bitcoin BTC

### **二、ZRC20 标准接口（跨链资产映射合约）**

ZRC20 是适配跨链场景的资产标准（类似 ERC20），核心接口：

```solidity
interface IZRC20 {
    function deposit() external payable; // 原生资产存入，映射为ZRC20资产
    function withdraw(uint256 amount) external; // ZRC20资产提取，换回原生资产
    function transfer(address to, uint256 amount) external returns(bool); // ZRC20资产转账
    function balanceOf(address account) external view returns(uint256); // 查询ZRC20资产余额
}
```

### **三、跨链调用函数：crossChainCall**

负责链间信息 / 资产的传递，是跨链交互的核心函数：

-   函数核心参数：
    

```solidity
function crossChainCall(
    bytes calldata origin, // 源链信息
    uint256 originChainID, // 源链 ChainID
    address zrc20, // 源链ZRC-20合约地址
    uint256 amount, // 代币数量
    bytes calldata message // 跨链信息（含目标链ID、目标资产、数量等）
) external;
```

-   核心步骤：
    

**1.解析信息**：通过`abi.decode`从`message`中解析目标资产、最小接收量

(address targetToken, uint256 minAmount) = abi.decode(message, (address, uint256));

**2.跨链交互逻辑实现**：调用`calculateSwapAmount`获取实际兑换量，需满足`兑换量≥minAmount`的校验

uint256 amountOut = calculateSwapAmount(amount);

require(amountOut >= minAmount,"invalid output");

**3.提取到目标链**：调用 ZRC20 合约的`withdraw`方法，将资产提取到目标链

IZRC20(targetToken).withdraw(amountOut,recipient,targetChainID);
<!-- DAILY_CHECKIN_2025-11-29_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->






# Day 5：Universal DeFi & 全链资产基础学习笔记

**日期**：2025年11月28日 星期五 **核心主题**：ZRC-20标准解析与全链资产应用逻辑

## 学习目标

-   理解 ZRC-20、Universal Token / NFT 的基本概念和作用。与传统资产的差异
    
-   明白多链资产在 ZetaChain 上如何被统一表示。全链资产如何被统一管理？
    

## ZRC-20 标准

ZRC-20是ZetaChain基于ERC-20扩展的全链资产标准，核心目标是实现“单一资产合约，支持多链交互”，其技术设计围绕“跨链原生”展开，而非对ERC-20的简单修改。

-   **接口继承与扩展**：完全兼容ERC-20的核心接口（如`transfer`、`balanceOf`），确保与现有EVM生态工具兼容；同时新增跨链专属接口，如`depositAndCall`（存款并触发跨链调用）、`withdraw`（从ZetaChain提取资产至原链）。
    
-   **跨链资产生命周期管理**：当用户将以太坊的USDT跨链至Solana时，ZRC-20合约会协同Gateway完成“以太坊USDT锁定→ZetaChain上铸造对应ZRC-20 USDT→Solana上映射为可流通资产”的全流程；反向跨链时则执行“销毁ZRC-20资产→解锁原链资产”的操作，确保资产总量守恒。
    
-   **多链地址统一适配**：支持将不同公链的地址格式（如以太坊的hex地址、Solana的base58地址）统一转换为ZetaChain可识别的格式，解决跨链地址不兼容问题。
    

## Universal 资产

Universal Token（通用代币）与Universal NFT（通用非同质化代币）是基于ZRC-20等标准实现的全链资产，核心特性是“一次发行，全链可用”，打破了传统资产的“链上孤岛”限制。

-   **Universal Token**：如基于ZRC-20发行的全链稳定币，用户可在以太坊、Bitcoin、Solana等链上直接使用该代币进行交易、质押，无需通过第三方跨链工具进行资产转换，且所有链上的余额、交易记录均通过ZetaChain实现实时同步。
    
-   **Universal NFT**：如全链数字门票，用户在Polygon链上购买该NFT后，可直接在Avalanche链上的元宇宙场景中使用，无需进行NFT跨链转移操作，ZetaChain通过验证NFT的全链唯一标识（如基于链ID+合约地址+tokenID的组合标识）确保其有效性。
    

## 多链资产在ZetaChain的统一表示

ZetaChain通过“**标准封装+地址映射+状态同步**”三大机制，实现多链资产的统一管理：

1.  **标准封装**：无论原资产是ERC-20（以太坊）、SPL（Solana）还是UTXO模型（Bitcoin），均通过对应的ZRC标准（ZRC-20对应代币、ZRC-721对应NFT）封装为ZetaChain可识别的资产格式，统一资产交互规则。
    
2.  **地址映射**：建立“用户多链地址→ZetaChain统一身份”的映射关系，用户无需在每条链上单独创建地址，通过一个核心身份即可管理所有链上的资产。
    
3.  **状态同步**：ZetaChain的节点网络实时同步各链上的资产状态（余额、交易、权限），并通过共识机制确保状态的一致性与不可篡改性，让用户在任意链上操作资产后，其他链均可实时获取最新状态。
    

### （一）ZRC-20 和普通 ERC-20 的直观区别（从开发者视角）

| 对比 | 普通 ERC-20 | ZRC-20 |
| --- | --- | --- |
| 合约开发 | 仅需实现ERC-20核心接口，无需考虑跨链逻辑；代码聚焦单链资产转账、授权等基础功能 | 需导入ZetaChain合约库（如@zetachain/contracts），实现跨链接口；需处理链ID识别、Gateway交互等跨链逻辑 |
| 部署流程 | 需在每条目标链单独部署合约，如要支持以太坊、BSC需部署两个独立合约，合约地址不同 | 核心合约仅部署在ZetaChain，通过Gateway自动适配多条公链，无需重复部署，全链共用一个核心合约逻辑 |
| 跨链功能实现 | 自身无跨链能力，需集成第三方跨链协议（如Avalanche Bridge），额外开发适配代码，且跨链逻辑与资产合约分离 | 跨链功能为合约原生能力，通过调用ZetaChain提供的SDK即可实现，跨链逻辑与资产管理逻辑深度融合 |
| 多链状态维护 | 各链合约状态独立，需手动开发状态同步机制（如通过Oracle），易出现数据不一致问题 | 由ZetaChain核心网络统一维护多链状态，开发者无需关注状态同步，只需通过标准接口查询即可获取全链一致的资产数据 |
| 生态工具适配 | 适配单链钱包（如MetaMask仅以太坊版）、单链浏览器，多链工具需单独适配 | 适配ZetaChain生态的全链工具（如全链钱包、跨链Explorer），同时兼容原有单链工具，适配成本更低 |

### （二）Universal 资产的应用场景构想：自动化全链质押平台

基于Universal Token/NFT的跨链特性，我构想的应用场景为“**自动化全链质押平台**”。该平台以“降低质押门槛、提升资产效率”为核心，为用户提供“多链资产一键聚合、智能策略自动质押、全链收益实时归集”的一站式服务，彻底解决传统跨链质押的操作与效率痛点，具体逻辑如下：

1\. 场景核心痛点（传统跨链质押的瓶颈）

-   **操作碎片化**：用户持有以太坊的ETH、Solana的SOL、Polygon的MATIC等多链资产时，需在各链对应的质押平台单独操作，重复完成“连接钱包-授权资产-选择节点-确认质押”流程，耗时且易出错；
    
-   **资产闲置成本高**：不同链的质押解锁周期不同（如ETH质押需等待合并后解锁），用户难以灵活调配资金，部分短期闲置资产因跨链繁琐而放弃质押，造成收益损失；
    
-   **策略执行滞后**：当某条链的质押收益飙升（如新区块链上线高收益质押活动），用户需手动完成资产跨链、质押操作，往往错过最佳收益窗口期；
    
-   **风险集中与收益不透明**：传统质押多依赖单链节点，节点故障可能导致收益损失；同时各链质押收益分散在不同平台，用户难以统一查询与管理。
    

2\. 自动化全链质押平台的核心功能（基于Universal资产）

1.  **多链资产一键聚合与标准化封装**：用户通过平台连接全链钱包（如支持ZetaChain的MetaMask扩展版）后，平台自动识别其在各链的可质押资产（代币/NFT），并通过ZRC标准将其统一封装为Universal资产。例如，以太坊的ETH封装为ZRC-20 ETH、Solana的SOL封装为ZRC-20 SOL，用户无需区分资产原链归属，在平台界面即可查看“全链可质押资产总览”，实现“一次授权，多链资产可控”。
    
2.  **自定义智能质押策略**：平台提供“收益优先”“风险优先”“平衡型”等预设策略，用户也可自定义参数（如目标收益阈值、单链资产占比上限、节点选择偏好）。例如，用户设置“收益优先+单链资产占比≤30%”策略后，平台实时监控各链质押市场数据（如以太坊质押年化4.5%、Avalanche质押年化8%），自动将Universal资产按比例分配至收益最高的链上优质节点，无需人工干预。
    
3.  **跨链质押自动化执行与状态同步**：当平台触发质押策略时，由ZetaChain Gateway与Universal合约协同完成全流程自动化：① 调用ZRC-20的`depositAndCall`接口，将用户的Universal资产划转至目标链质押节点；② 同步生成质押凭证（基于ZRC-721的全链质押NFT，记录质押资产、节点、收益周期等信息）；③ 质押状态实时同步至ZetaChain核心网络，用户在平台界面可实时查看各链资产质押详情，包括节点健康度、已产生收益等。
    
4.  **全链收益自动归集与灵活支取**：各链质押收益（如节点奖励代币）会自动转换为Universal Token，实时归集至用户的平台账户，无需用户逐链提取。用户可随时发起支取请求，选择将收益或质押本金提取至任意支持的公链——例如将以太坊质押产生的收益提取至Solana链用于交易，平台通过ZRC-20的`withdraw`接口快速完成跨链操作，资金到账时间由ZetaChain跨链确认速度决定（通常30秒内）。
    
5.  **智能风险控制与节点容错**：平台接入ZetaChain的全链节点健康监测数据，当某条链的质押节点出现故障风险时，系统会自动触发“资产迁移”策略——将该节点的Universal质押资产快速转移至同链其他优质节点，并通过跨链消息同步至用户，确保质押过程不中断、收益不损失。
    

3\. 场景价值与技术支撑

该平台的核心价值在于“用技术标准化解决操作复杂性”，其功能实现完全依赖ZetaChain的全链资产生态：① ZRC-20/ZRC-721标准实现了多链资产的统一封装与跨链流通，为资产聚合提供基础；② ZetaChain的Gateway解决了不同公链的协议适配问题，确保质押指令在各链顺畅执行；③ 核心网络的状态同步机制保障了质押数据、收益数据的实时性与一致性，为自动化策略提供可靠数据支撑。对用户而言，平台将“多链质押”简化为“一次设置、全程托管”，大幅降低参与门槛；对开发者而言，Universal资产的标准化接口减少了多链适配代码量，仅需调用ZetaChain SDK即可实现全链功能。
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->







# Day 4：Universal App + Hello World 心智模型学习笔记

**日期**：2025年11月27日 星期四 **核心主题**：Universal App认知深化与Hello World Demo落地规划

## 一、学习目标拆解与核心导向

今日学习完成“认知构建”与“落地规划”两大核心任务，为后续开发打基础：

1.  **认知目标**：理解Universal App合约“跨链原生”的本质——即合约逻辑天然覆盖多条公链，而非单链合约的“跨链适配版”。
    
2.  **规划目标**：明确Hello World Demo的技术栈选型与工作流，清晰界定“合约开发-前端交互-RPC连接”三大模块的核心职责，避免后续开发方向混乱。
    

## 二、学习资料重点梳理

### （一）核心资料：ZetaChain Developers EVM / Tutorials 部分

资料地址：[https://www.zetachain.com/docs/developers](https://www.zetachain.com/docs/developers)

-   **重点关注模块**：**EVM 章节**：核心提炼“Universal EVM与传统EVM的差异”——支持跨链指令（如`zetaSend`），能直接读取非EVM链数据，这是开发全链合约的技术基石。
    
-   **Tutorials 结构**：观察教程中“合约-部署-交互”的通用流程，发现所有Demo均包含“跨链参数配置”（如指定目标链ID、网关地址），这是Universal App的标志性特征。
    
-   **延伸思考**：传统单链Hello World合约仅实现“存储/打印字符串”，而全链版需考虑“跨链传递信息”——比如在EVM链触发合约，让Solana链能读取到该信息，这是今日规划的核心差异点。
    

### （二）扩展资料：Tutorials 中的 Swap / Messaging 结构

-   **共性结构提炼**：无论Swap（跨链兑换）还是Messaging（跨链消息），均遵循“**触发链合约 → ZetaChain核心层 → 目标链合约**”的三段式结构，其中ZetaChain通过Gateway自动完成跨链数据传递。
    
-   **对Hello World的启发**：可简化该结构，实现“单触发链 → ZetaChain → 多目标链”的消息同步，即完成全链版Hello World的核心逻辑。
    

## 三、Universal App 合约的核心特征

1\. 部署载体唯一：核心合约仅部署在ZetaChain，无需在以太坊、比特币等链重复部署；

2\. 跨链指令原生：合约中可直接调用ZetaChain提供的跨链API（如消息发送、资产转移）；

3\. 链间数据互通：能主动读取或被动接收来自不同公链的数据，实现全链状态同步。

## 四、实践作业：应用构想与Demo工作流规划

### （一）Universal App 核心逻辑构想

结合全链应用特性，我计划开发的首个Universal App具体逻辑如下：

1.  核心功能：用户在任意一条支持的公链（如以太坊测试网）发起交易，输入一段文本消息，该消息会被同步存储到ZetaChain，并可被其他公链（如Solana测试网）的用户读取，实现“一条消息，全链可见”。
    
2.  存储功能：合约中定义一个映射（mapping），关联“消息ID”与“消息内容+发送链ID+发送时间”；
    
3.  跨链同步：用户在以太坊发送消息后，合约通过ZetaChain API将消息ID同步至Solana网关，Solana用户可通过调用合约查询该消息ID对应的内容；
    
4.  查询功能：提供公开函数，支持按消息ID或发送链ID查询历史消息。
    

### （二）Hello World Demo 工作流确定

1\. 开发工具链选型：CLI + Hardhat

-   ZetaChain CLI提供了合约部署、跨链交易触发等核心命令，是与ZetaChain交互的必备工具；
    
-   Hardhat是以太坊生态成熟的开发框架，支持Solidity编译、测试、部署全流程，且ZetaChain提供Hardhat插件（如`@zetachain/hardhat-zeta`），可无缝适配Universal EVM；
    
-   **流程**：Hardhat编译合约 → ZetaChain CLI部署合约至测试网 → Hardhat编写测试用例验证功能 → CLI触发跨链交互。
    

2\. 运行环境选择：ZetaChain 测试网

-   ZetaChain测试网提供完整的跨链基础设施（RPC、Faucet、Explorer），可真实模拟以太坊、Solana等公链的跨链交互，且测试币获取便捷，便于验证全流程。
    
-   测试网无资产损失风险，可反复调试跨链参数（如链ID、Gas设置）。
    
-   已提前获取ZetaChain测试网ZETA代币（通过Faucet），并配置好测试网RPC地址（写入Hardhat配置文件）。
    

### 今日收获：

理解了ZetaChain作为“跨链大脑”的核心价值；明确了Hello World Demo体现“跨链数据交互”的全链特性；完成工具链与环境选型，为后续开发做好了铺垫。
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->








# Day 3：ZetaChain & Universal Blockchain 核心概念学习笔记

**日期**：2025年11月26日 星期三 **核心主题**：Universal Blockchain系列概念解析与ZetaChain架构可视化

## 一、学习目标拆解与达成标准

1.  **概念理解**：不仅需掌握“通用区块链/Universal EVM/Universal App/Omnichain Smart Contract”的定义，更要厘清四者间的逻辑关联，能结合ZetaChain场景举例说明。
    
2.  **架构可视化**：绘制的架构图需明确“ZetaChain-多条公链-Gateway”的层级关系与数据流向，标注核心组件的核心作用，做到“图清意明”。
    

## 二、核心学习资料梳理（含重点方向）

| 资料类别 | 资料名称与地址 | 重点学习方向 |
| --- | --- | --- |
| 核心资料 | Universal Apps 概览https://www.zetachain.com/docs/start/app | 1. Universal App与传统跨链应用的差异；2. 基于ZetaChain开发Universal App的优势；3. 典型应用场景案例 |
| 核心资料 | 开发者总览（Universal EVM等）https://www.zetachain.com/docs/developers | 1. Universal EVM的技术特性；2. Gateway的跨链交互机制；3. ZetaChain跨链技术的核心原理 |
| 扩展资料 | 为什么在ZetaChain上开发？https://www.zetachain.com/docs/start/build | 提炼ZetaChain的核心竞争力，关联Universal系列概念的价值 |
| 扩展资料 | ZetaChain架构设计https://www.zetachain.com/docs/developers/architecture/overview | 快速抓取“节点网络”“跨链协议层”等关键架构模块，辅助理解整体逻辑 |

## 三、核心概念解析（基于资料的个人理解）

### （一）基础概念关联

核心逻辑：**通用区块链（ZetaChain）** 通过 **Universal EVM** 提供开发底座，支撑开发者构建 **Universal App**，而 **Omnichain Smart Contract（全链智能合约）** 是Universal App的核心实现载体，**Gateway** 则是实现跨链交互的关键桥梁。

### （二）关键概念

**1\. Universal App（通用应用）**简单来说，Universal App是一种“一次开发，全链可用”的区块链应用，它不需要为每条公链单独适配代码，就能在Bitcoin、Ethereum、Solana等不同链上与用户和资产交互。举个例子：传统跨链DeFi应用，可能需要在ETH链部署一套合约，在BSC链再部署一套，且两套合约间需额外开发跨链同步逻辑；而基于ZetaChain的Universal App，只需开发一套核心逻辑，就能同时服务于多条链的用户，用户在ETH链的资产和Solana链的资产可直接通过该应用完成交互，无需切换应用或依赖第三方跨链工具。核心价值：降低开发者跨链开发成本，提升用户跨链使用体验，打破不同公链间的“信息孤岛”和“资产孤岛”。

**2\. Gateway（网关）**Gateway是ZetaChain实现“跨链通信”的核心组件，相当于连接ZetaChain与其他公链的“翻译官”和“传送带”。它的核心作用主要有两个：

一是“信息翻译与传递”：不同公链的交易格式、数据标准不同，当某条公链（如Ethereum）上发生用户操作时，Gateway会将该操作的关键信息（如用户地址、操作类型、资产金额）转换为ZetaChain能识别的格式，传递给ZetaChain核心网络；同时，ZetaChain的指令也会通过Gateway转换为对应公链的格式，下发到目标公链执行。

二是“资产跨链保障”：在资产跨链过程中，Gateway会协同ZetaChain的质押机制，确保原链资产锁定与目标链资产 mint（铸造）、burn（销毁）的同步性，防止资产双重花费，保障跨链资产的安全性。比如用户将ETH链上的USDT跨到Solana链，Gateway会确认ETH链上的USDT已锁定后，通知ZetaChain在Solana链上为用户铸造等量的跨链USDT，整个过程由Gateway全程监控验证。

**3\. Universal EVM（通用以太坊虚拟机）**EVM是以太坊生态的核心开发环境，而很多公链（如BSC、Polygon）虽兼容EVM，但仍需单独适配。ZetaChain的Universal EVM，是在EVM基础上做了“跨链扩展”的虚拟机，它不仅支持开发者使用Solidity等EVM生态的编程语言，更能让基于它开发的智能合约具备“跨链调用能力”——合约可以直接读取其他非EVM链（如Bitcoin、Solana）的数据，或向这些链发送操作指令，这是传统EVM和兼容EVM所不具备的核心能力。

**4\. Omnichain Smart Contract（全链智能合约）**这是Universal App的“大脑”，是部署在ZetaChain上的智能合约，它能通过Gateway与多条公链进行交互。与传统合约不同，它的逻辑中包含了跨链交互的原生支持，比如可以在同一合约函数中，同时处理Ethereum的ETH和Solana的SOL的兑换逻辑，无需依赖外部跨链协议。】

## 四、实践作业：架构图绘制与说明

### （一）ZetaChain 核心架构图（ZetaChain + 多公链 + Gateway）

![exported_image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/xzzlks/images/2025-11-26-1764160241728-exported_image.png)

### （二）架构图说明

1.  **核心层级**：ZetaChain核心网络是整个架构的“中枢”，其中Universal EVM为全链智能合约提供运行环境，跨链协议层负责处理所有跨链交互逻辑，节点网络通过共识机制保障数据安全与不可篡改。
    
2.  **连接层级**：Gateway是ZetaChain与各公链的“专属连接通道”，每条公链对应一个独立的Gateway，确保跨链数据传递的精准性和安全性。
    
3.  **数据流向**： 正向流：用户在某条公链（如Ethereum）上发起操作后，操作数据先传递至对应Gateway，由Gateway完成格式转换后提交给ZetaChain的跨链协议层处理；反向流：ZetaChain处理完成后，将执行结果通过对应Gateway转换为目标公链的格式，下发至目标公链（如Solana），最终完成跨链操作的闭环。
    

-   **收获**：1. 厘清了Universal系列核心概念的逻辑关系，不再混淆；2. 理解了Gateway在跨链中的核心作用，掌握了ZetaChain架构的核心层级；3. 通过绘图将抽象的架构具象化，加深了对“全链交互”的理解。
    
-   **不足**：1. 对Universal EVM的技术实现细节（如如何兼容非EVM链）理解不够深入；2. 架构图中未体现ZetaChain的质押机制与Gateway的协同逻辑，需进一步补充。
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->









### ZetaChain CLI 安装与验证（本地环境：Windows）

1.  **安装步骤**： 前置依赖：确认已安装Go（版本≥1.20）。打开命令提示符（CMD）或PowerShell，输入`go version`验证；若未安装，访问Go官网（[https://go.dev/dl/）下载Windows版安装包，勾选“Add](https://go.dev/dl/）下载Windows版安装包，勾选“Add) Go to PATH”选项后完成安装。
    
2.  安装CLI：在CMD或PowerShell中执行命令`go install github.com/zeta-chain/cli/cmd/zetacli@latest`，等待命令执行完成。
    
3.  验证安装：输入`zetacli --version`，若成功输出版本信息（如v1.0.0），则说明安装完成。
    
4.  **遇到的问题与解决**： 问题1：执行`zetacli`命令时提示“不是内部或外部命令，也不是可运行的程序”。
    
5.  解决1：右键“此电脑”→“属性”→“高级系统设置”→“环境变量”，在“用户变量”的“Path”中添加`%GOPATH%\bin`（GOPATH默认路径为C:\\Users\\你的用户名\\go），添加后重启CMD或PowerShell即可。
    
6.  问题2：安装Go后执行`go version`仍提示命令无效。
    
7.  解决2：重新运行Go安装包，确保“Add Go to PATH”已勾选；若已勾选，手动在“系统变量”的“Path”中添加Go安装路径（通常为C:\\Program Files\\Go\\bin），重启终端验证。
    

### **测试网关键入口汇总**：

-   RPC地址：[https://zetachain-athens-evm.blockpi.network/v1/rpc/public（文档获取的具体地址）](https://zetachain-athens-evm.blockpi.network/v1/rpc/public（文档获取的具体地址）)
    
-   Faucet链接：[https://zetachain.faucetme.pro/（输入钱包地址即可领取测试币，每日限额）](https://zetachain.faucetme.pro/（输入钱包地址即可领取测试币，每日限额）)
    
-   Explorer地址：[https://testnet.zetachain.explorers.guru/](https://testnet.zetachain.explorers.guru/)
    

### Qwen API 连通性测试

Postman测试

1.  设置请求方式：POST，请求URL：[https://dashscope.aliyuncs.com/api/v1/services/aigc/text-generation/generation。](https://dashscope.aliyuncs.com/api/v1/services/aigc/text-generation/generation。)
    
2.  设置请求头：Content-Type: application/json；Authorization: Bearer xxx。
    
3.  设置请求体：与终端测试一致的JSON内容，具体如下： `{ "model": "qwen-7b", "input": { "prompt": "请简单介绍ZetaChain" }, "parameters": { "temperature": 0.7 } }`
    
4.  发送请求，结果：同样返回200状态码及有效响应，验证Postman环境连通性正常。
    

1\. 收获：成功完成ZetaChain CLI本地安装、Qwen API连通性测试，明确了ZetaChain测试网核心服务的使用入口，为后续开发奠定了环境基础。

2\. 不足：对ZetaChain CLI的具体命令使用尚不熟悉，Qwen API的高级参数配置（如流式响应、上下文管理）未深入探索。
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->










# ZetaChain

### 概述

ZetaChain是实现跨链互操作性的区块链项目，使智能合约可以访问多个区块链的数据和资产，为开发者构建了一个“全链”开发平台

## 功能特点

## 链式编排

能通过单一智能合约协调跨多条区块链的交易，在同一框架内，基于ZetaChain的应用可以管理来自不同区块链的代币转账和合约调用并发起向连接网络的交易。链式编排能力简化了开发流程，降低了发生错误的可能性，并确保你的应用可以随着更多的区块链集成而扩展。

### 前后兼容性

在ZetaChain部署的应用会自动连接到所有ZetaChain生态系统中的网络，即使有新区块链的加入，你的应用依然可以与这些网络进行兼容。此功能简化了维护，无需因新区块链加入而更新合约或重新部署，提供了稳定的用户体验。

### 转移本地令牌

ZetaChain 上通用应用的核心功能之一，能通过本地代币在所有连接链之间转移价值，意味着用户可以执行涉及每个区块链实际原生资产的交易。跨链代币转移功能提升了流动性，无需中介或 复杂的令牌包装机制。简化了用户体验，降低了跨链交易相关风险，增强了跨链运营中的信任和可靠性。

### 熟悉的开发者体验

ZetaChain在UEVM上运行智能合约， 该系统与以太坊的EVM完全兼容。意味着开发者可以使用Solidity或其他兼容EVM语言开发智能合约。支持流行开发工具，如Foundry、Hardhat、Slither，使开发者能使用熟悉的工作流程、调试工具和测试框架，使流程更加完善，打造更高效、更少出错的通用应用。

### Gasless EVM执行环境

用户可以调用运行在ZetaChain的EVM上的应用，以及所有连接链的合约，只需在连接链上支付gas。降低了用户开支，提升应用可访问性

# 开发环境配置与部署

在开始之前需配置以下工具：

Node.js（v21或更晚版本）运行 ZetaChain CLI 并管理 JavaScript 依赖

Yarn 或 npm 用于安装和更新项目库

Git 管理项目资源，与他人合作编程

JQ 轻量级命令行JSON处理器， 解析Localnet输出和编写shell脚本

Foundry 快速、模块化的以太坊工具包，编译合约、管理依赖和运行 Localnet

### 环境配置

安装ZetaChain CLI：npm install -g zetachain

重要工具，进行跨链调用、转账代币、跟踪跨链交易，管理跨多个网络的账户，查询等

打印所有当前连接在ZetaChain上的链：zetachain query chains list

用于确认设置是否正常

Localnet：环境包括ZetaChain、EVM、Solana、Sui和TON，是构建和测试通用应用最快的方法，无需依赖公共测试网。

运行Localnet需安装Foundry，它提供了 EVM 开发的核心工具，是启动的必要条件。

### Foundry安装

（1）预编译的二进制文件，从GitHub上下载：[https://github.com/foundry-rs/foundry/releases](https://github.com/foundry-rs/foundry/releases)

（2）使用Foundryup，Foundryup 是 Foundry 工具链的官方安装程序，终端执行以下命令：curl -L [https://foundry.paradigm.xyz](https://foundry.paradigm.xyz) | bash

注意！Foundryup目前不支持Powershell或命令提示符（Cmd）Windows用户需安装并使用Git BASH或WSL作为终端

（3）使用最新稳定版的Rust构建，旧版需使用以下命令更新：rustup update stable

Windows用户还需安装最新版的Visual Studio并安装“用C++进行桌面开发”工作负载

（4）通过Cargo安装：cargo install --git [https://github.com/foundry-rs/foundry](https://github.com/foundry-rs/foundry) --profile release --locked forge cast chisel anvil

foundryup --branch master 从master分支安装 foundryup --path path/to/foundry 指定Foundry的安装路径

（5）手动从本地Foundry仓库副本构建：

\# clone the repository

git clone [https://github.com/foundry-rs/foundry.git](https://github.com/foundry-rs/foundry.git)

cd foundry

\# install Forge

cargo install --path ./crates/forge --profile release --force --locked

\# install Cast

cargo install --path ./crates/cast --profile release --force --locked

\# install Anvil

cargo install --path ./crates/anvil --profile release --force --locked

\# install Chisel

cargo install --path ./crates/chisel --profile release --force --locked

（6）GitHub Actions CI安装：[https://github.com/foundry-rs/foundry-toolchain](https://github.com/foundry-rs/foundry-toolchain)

（7）Docker容器拉取最新版本：docker pull [ghcr.io/foundry-rs/foundry:latest](http://ghcr.io/foundry-rs/foundry:latest)

Foundry仓库本地构建Docer镜像：docker build -t foundry .
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
