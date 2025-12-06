---
timezone: UTC+8
---

# XiaoHai67890

**GitHub ID:** XiaoHai67890

**Telegram:** @XiaoHai489

## Self-introduction

嘿嘿

## Notes

<!-- Content_START -->
# 2025-12-06
<!-- DAILY_CHECKIN_2025-12-06_START -->
### 学习笔记 Day 13:

## 一、今天在学什么？

1.  **在 DeFi / AI 两大方向中，先选定一个清晰赛道和题目。**
    
2.  **为这个题目划出一个真正可落地的 MVP（最小可行产品）范围，只做 1–3 个关键功能。**
    

今天不是再学新概念，而是把前面 Module 0–5 的内容「收口」：

-   DeFi 侧：基于 ZetaChain 的 **跨链 / 全链应用**，比如用 ZRC-20 代币标准、跨链 Swap 等。
    
-   AI 侧：基于 Qwen-Agent 搭建 **智能 Agent / 助手 / 工具调用型应用**。
    

## 二、关键技术要点回顾

### 1\. ZetaChain & ZRC-20：为什么适合做「跨链 DeFi」

-   **ZRC-20 是 ZetaChain 上的通用代币标准**，兼容 ERC-20，但每个连接链（ETH、BTC 等）的原生资产都会在 ZetaChain 上对应一个 ZRC-20 版本。这样开发者可以像在「单链」上一样，统一处理多链资产。
    
-   这意味着你可以在一个合约里做：
    
    -   跨链 DEX：用户在一个界面，用一个交易，就能完成「A 链的代币 ➜ B 链的代币」。
        
    -   跨链借贷 / 质押 / 组合管理等 omnichain DeFi 协议。
        

**Swap 教程**的重点是：

-   如何写一个「Universal App」，接收来自不同链的调用，然后在 ZetaChain 上完成 Swap 逻辑，再在目标链完成资产兑现。
    

我的启发：

> 如果做 DeFi 方向，核心价值不在「再造一个 AMM」，而是利用 ZetaChain 的跨链能力和 ZRC-20，把「多链复杂操作」压缩成「一次交互」。

### 2\. Qwen-Agent：为什么它适合作为「AI 后端脑」

-   Qwen-Agent 是一个面向 LLM 应用的 Agent 框架，支持：指令跟随、工具调用、规划、记忆等能力，可以快速搭建浏览器助手、代码助手、任务自动化工具等。
    
-   框架本身已经提供了一些 **示例 Agent**，比如：
    
    -   Browser Assistant（会查网页）
        
    -   Code Interpreter（会跑代码）
        
    -   Custom Assistant（自定义工作流）
        

结合 Qwen2.5 的整体能力：

-   文本理解和推理不错
    
-   有专门的 Coder / Math 版本可选
    

启发：

> 如果做 AI 方向，不一定非要做「花哨的界面」，而是重点思考：
> 
> -   Agent 要帮用户 **自动做哪一件具体事？**
>     
> -   它需要调用哪些工具（合约、价格预言机、钱包、HTTP API…）？
>     

## 三、MVP 思维：我今天真正理解到的几件事

1.  **题目越大，越需要「砍功能」。**
    
    -   「全链资产管理平台」听上去很酷，但 MVP 阶段只要做到「一种典型用户、一个核心场景、1–3 个关键功能」就够了。
        
2.  **MVP 是可 demo 的，而不是可想象的。**
    
    -   评委/用户能在 3–5 分钟内完成一个闭环：
        
        -   连接钱包 / 输入需求
            
        -   看到系统计算/调用的过程
            
        -   获得一个对他们「有实际用处」的结果（例如完成一笔跨链 Swap，或者给出一套可执行的操作计划）。
            
3.  **技术路线必须反推自场景，而不是反过来。**
    
    -   不应该是：「我想用 ZRC-20 + Qwen + xxx」→ 硬凑题目
        
    -   而应该是：「某类用户有一个具体痛点」→ 这个痛点刚好适合用 ZetaChain + Qwen 的组合去解决
        

## 四、思考：

> 说明：下面是我根据今天要求写的一版「练习用概要」，可以把它当作模板，后续根据真正确定的 idea 替换内容即可。

### 1\. 项目名称（？？？不知道）

**OmniGuard：全链 DeFi 风险提醒 & 一键操作助手**

### 2\. 目标用户 / 使用场景

-   **目标用户**
    
    -   有一定 DeFi 经验、持有多链资产（ETH、BTC、EVM 链代币等）的普通用户。
        
    -   没有时间每天盯盘、比较不同链之间收益和风险。
        
-   **核心场景**
    
    > 用户关心的问题：  
    > 「我在这么多链上的资产，现在有没有明显的风险 / 更好的收益机会？可不可以一键帮我调整？」
    
    在这个场景中，用户只需要连接钱包，AI 就会：
    
    1.  读取用户在不同链上的主要资产分布（通过 ZetaChain / indexer 接口获取）
        
    2.  用 Qwen-Agent 分析：
        
        -   是否存在过度集中某单一资产/链
            
        -   当前某些资产收益率过低
            
        -   是否存在明显的「无意义小额碎片资产」
            
    3.  给出一套「推荐操作方案」，并支持一键发起跨链 Swap 进行调整。
        

### 3\. 关键功能

**功能 1：多链资产一览（基础视图）**

-   从用户钱包和 ZetaChain 侧获取资产信息（包括对应的 ZRC-20 映射），聚合到一个面板上。
    
-   按链、按代币类型（稳定币 / 波动资产）分类展示。
    

**功能 2：AI 风险体检 + 配置建议（Qwen-Agent）**

-   Qwen-Agent 拉取资产分布、部分市场数据（预言机 / API）。
    
-   内部 prompt / tool-calling 流程示例：
    
    1.  计算当前多链仓位集中度（例如单资产超过 60% 提示风险）。
        
    2.  检查是否有长期闲置小额资产，可以统一换成主力资产或稳定币。
        
    3.  给出简单的目标配置方案（比如「60% 主流资产 + 30% 稳定币 + 10% 其他」）。
        

**功能 3：一键跨链调整（基于 ZetaChain Swap 模板）**

-   用户勾选「接受推荐配置」，系统基于 ZetaChain 的跨链 Swap 逻辑，为用户生成一组交易。
    
-   尽量复用官方 Universal Swap 教程里的调用模式（从某链发起、在 ZetaChain 执行逻辑、在目标链完成兑换）。
    

> MVP 的边界：
> 
> -   只支持 **少量主流链 + 2–3 种主流资产**（比如 ETH、BTC 的 ZRC-20 版本 + USDC）。
>     
> -   不做复杂的收益策略，不做自动再平衡，仅提供「一次性建议 + 一次性执行」。
>     

### 4\. 技术路线（ZetaChain + Qwen 如何配合）

**前端 / 接入层**

-   用户通过网页连接钱包（EVM 钱包为主）。
    
-   前端调用后台 API：
    
    -   获取多链资产数据
        
    -   发起风险分析请求（转发给 Qwen-Agent）
        
    -   生成 / 发送交易到 ZetaChain
        

**链上部分（ZetaChain 合约）**

1.  **资产表示**：
    
    -   使用 ZRC-20 作为各链资产的统一表示层，方便在 ZetaChain 上跨链调度。
        
2.  **跨链 Swap 逻辑**：
    
    -   参考官方 Swap 教程实现一个简化版 Universal App：
        
        -   根据 AI 给出的目标配置，合约执行多次跨链 Swap（或尽量合并）。
            

**AI Agent 部分（Qwen-Agent）**

1.  基于 Qwen-Agent 框架定义一个自定义 Agent：
    
    -   具备工具：
        
        -   `get_portfolio()`：调用后端服务读取链上资产。
            
        -   `get_market_data()`：获取简单行情 / 波动数据。
            
        -   `suggest_rebalance_plan()`：内部工具，基于简单规则生成配置方案。
            
2.  Agent 负责：
    
    -   解析用户意图（「帮我检查风险」/「帮我优化配置」）。
        
    -   决定是否需要跨链操作，并生成具体代币对、数量建议。
        
    -   把「建议」转化成前端可展示的自然语言解释 + 结构化计划（给合约使用）。
        

### 5\. 计划复用的 Demo / 模板

1.  **ZetaChain 侧**
    
    -   参考官方 **Swap 教程** 完成跨链兑换核心流程：
        
        -   Universal App 的结构
            
        -   `onCrossChainCall` / `onCall` 的处理逻辑
            
    -   如有需要，可参考 Universal Token / Universal DEX 文档进一步扩展：
        
2.  **Qwen-Agent 侧**
    
    -   复用 Qwen-Agent 的 **Custom Assistant / Tool Calling 示例**，重点是：
        
        -   如何定义工具 schema
            
        -   如何在对话中多步调用工具、规划执行顺序
            
3.  **整体架构**
    
    -   前端 + 后端 + 合约通信，可参考 ZetaChain Docs 中的 Universal App 示例结构。
        

## 五、我的收获与黑客松想法

**收获：**

1.  原来 ZetaChain 的「全链」能力，本质上是用 ZRC-20 把多链资产统一成一个操作面，再通过 Universal App 实现跨链逻辑，这比自己搭桥、自己搞多链消息要简单很多。
    
2.  Qwen-Agent 已经帮我解决了很多「Agent 工程」问题，我更多要想的是：
    
    -   场景是什么？
        
    -   需要什么工具？
        
    -   怎么把链上的数据和 Qwen 的推理能力结合起来？
        
3.  Hackathon 项目不需要「覆盖所有功能」，只要能清楚回答三个问题就足够有说服力：
    
    -   为谁解决什么具体问题？
        
    -   通过哪些关键功能完成这个「闭环」？
        
    -   为什么一定要用「ZetaChain + Qwen」，而不是普通 L2 + 普通 LLM？
        

。。。。。。似乎有一点想法，明天写出来吧
<!-- DAILY_CHECKIN_2025-12-06_END -->

# 2025-12-05
<!-- DAILY_CHECKIN_2025-12-05_START -->

# 学习笔记 Day 12：

**学习目标**

-   **核心目标**：串起一条最小通路：自然语言 → Agent → 后端 →（模拟或真实）ZetaChain 调用。
    
-   **技术要点**：
    
    1.  理解 Qwen-Agent 的 `Function Calling` (工具调用) 机制。
        
    2.  掌握使用 `web3.py` 连接 ZetaChain 测试网并发送交易。
        
    3.  完成“意图识别”到“链上执行”的闭环。
        

**学习资料**

-   **ZetaChain Developers** (官方文档，查阅 RPC 和网络参数)
    
    -   [https://www.zetachain.com/docs/developers](https://www.zetachain.com/docs/developers)
        
-   **Qwen-Agent Tools** (框架文档，查阅如何注册工具)
    
    -   [https://qwen.readthedocs.io/en/v2.5/framework/qwen\_agent.html](https://qwen.readthedocs.io/en/v2.5/framework/qwen_agent.html)
        
-   **Web3.py Documentation** (Python 交互库)
    
    -   [https://web3py.readthedocs.io/en/stable/](https://web3py.readthedocs.io/en/stable/)
        

**扩展资料**

在开发 Demo 前，请先在代码中配置以下 ZetaChain Athens Testnet 参数 1：

| 参数项 | 值 | 备注 |
| Network | Athens-3 Testnet |   |
| Chain ID | 7001 | 用于防止跨链重放攻击 |
| Currency | ZETA | 测试币用于支付 Gas |
| RPC URL | https://zetachain-athens-evm.blockpi.network/v1/rpc/public | 公共节点，注意限频 |
| Explorer | https://athens.explorer.zetachain.com | 用于验证交易结果 |

**实践 / 作业指南**

本次作业的核心是将 AI 的“思考”转化为区块链的“动作”。以下是完成简版 Demo 的步骤与代码骨架。

### 1\. 环境准备

安装必要的 Python 依赖库：

```
pip install qwen-agent web3 python-dotenv
```

_提示：请务必使用_ `.env` _文件管理你的_ `ZETA_PRIVATE_KEY`_，严禁上传私钥到代码库。_

### 2\. 后端实现：定义“手”

需要编写一个工具，让 Agent 能够执行转账。注意 ZetaChain 基于 Tendermint，使用 `web3.py` 时建议注入 `geth_poa_middleware` 3。

Python

```
from qwen_agent.tools.base import BaseTool, register_tool
from web3 import Web3
from web3.middleware import geth_poa_middleware
import json
import os

# 初始化连接
w3 = Web3(Web3.HTTPProvider("https://zetachain-athens-evm.blockpi.network/v1/rpc/public"))
w3.middleware_onion.inject(geth_poa_middleware, layer=0)

@register_tool('zeta_transfer')
class ZetaTransfer(BaseTool):
    # description 非常重要，这是给 AI 看的“说明书”
    description = '在 ZetaChain 测试网上发送 ZETA 代币。输入参数为接收地址和金额。'
    parameters = {
        'name': 'zeta_transfer',
        'type': 'object',
        'properties': {
            'recipient': {'type': 'string', 'description': '接收代币的 0x 开头钱包地址'},
            'amount': {'type': 'number', 'description': '转账金额（单位：ZETA）'}
        },
        'required': ['recipient', 'amount']
    }

    def call(self, params: str, **kwargs) -> str:
        # 1. 解析参数
        params = json.loads(params)
        recipient = w3.to_checksum_address(params['recipient'])
        amount_ether = params['amount']
        
        # 2. 获取私钥和账户 (从环境变量)
        private_key = os.getenv("ZETA_PRIVATE_KEY")
        account = w3.eth.account.from_key(private_key)
        
        # 3. 构建交易 (模拟/真实)
        # 真正发交易前，建议先打印日志，确认参数被正确提取
        print(f"\n[后端日志] 正在构建交易: 向 {recipient} 发送 {amount_ether} ZETA...")
        
        try:
            nonce = w3.eth.get_transaction_count(account.address)
            tx = {
                'nonce': nonce,
                'to': recipient,
                'value': w3.to_wei(amount_ether, 'ether'), # 注意单位转换
                'gas': 210000,
                'gasPrice': w3.eth.gas_price,
                'chainId': 7001
            }
            
            # 4. 签名并广播
            signed_tx = w3.eth.account.sign_transaction(tx, private_key)
            tx_hash = w3.eth.send_raw_transaction(signed_tx.rawTransaction)
            
            return json.dumps({
                "status": "success", 
                "tx_hash": w3.to_hex(tx_hash),
                "explorer": f"https://athens.explorer.zetachain.com/tx/{w3.to_hex(tx_hash)}"
            })
        except Exception as e:
            return json.dumps({"status": "error", "message": str(e)})
```

### 3\. Agent 串联：定义“脑”

配置 Qwen-Agent，使其能够根据自然语言自动调用上述工具。

Python

```
from qwen_agent.agents import Assistant

def main():
    # 配置 Agent
    bot = Assistant(
        llm={'model': 'qwen-max', 'api_key': os.getenv("DASHSCOPE_API_KEY")},
        name='ZetaChain Assistant',
        system_message='你是一个区块链助手，可以帮助用户在 ZetaChain 上进行转账。转账成功后，必须返回交易哈希链接。',
        function_list=['zeta_transfer'] # 注册刚才写的工具
    )

    # 模拟用户输入
    query = "帮我给地址 0x123... (换成你的测试号) 转 0.001 个 ZETA"
    
    print(f"用户指令: {query}")
    
    # 运行 Agent
    messages = [{'role': 'user', 'content': query}]
    for response in bot.run(messages=messages):
        # 流式输出最后一条消息
        if response:
            print(f"Agent 回复: {response[-1]['content']}")

if __name__ == "__main__":
    main()
```

### 4\. 作业

1.  **准备账号**：确保你的 `.env` 中配置的账号在测试网有水（可在 Discord 领水）。
    
2.  **录屏步骤**：
    
    -   展示终端运行 `python demo.py`。
        
    -   输入自然语言指令（例如：“转 0.01 个币给 <你的另一个地址>”）。
        
    -   **关键点**：展示终端打印出的“\[后端日志\]”，证明参数是由 Agent 自动提取的。
        
    -   **高光时刻**：点击 Agent 返回的浏览器链接，展示链上交易成功的页面。
        

**常见问题 (FAQ)**

-   **Q: 报错** `insufficient funds`**？**
    
    -   A: 检查你的 Gas Price 设置，或者去水龙头再领一点测试币。
        
-   **Q: Agent 总是只聊天不调用工具？**
    
    -   A: 检查 `system_message` 是否足够明确，或者尝试在 prompt 中强制要求使用工具。
        
-   **Q: 报错** `extraData` **长度错误？**
    
    -   A: 确保在 `web3` 初始化时注入了 `geth_poa_middleware`。
<!-- DAILY_CHECKIN_2025-12-05_END -->

# 2025-12-04
<!-- DAILY_CHECKIN_2025-12-04_START -->


### 学习笔记 Day 11:

## 一、今天我又双叒叕要干啥？

**主题：Qwen-Agent × ZetaChain 的接口层设计**

-   把 `parse_swap_intent` 解析出的自然语言 Swap 意图，**落到具体的合约调用接口**（哪怕只是伪代码）。
    
-   在 Qwen-Agent 和链上合约之间，**设计一层清晰的“中间层服务（interface layer）”**，负责路由、映射、校验。
    

## 二、ZetaChain 开发模型快速复盘

### 1\. Universal App + Gateway

在 ZetaChain 上，典型的跨链 DeFi 应用会写成一个 **Universal App**，部署在 ZetaChain 上，外部链用户通过各自链上的 Gateway `depositAndCall` 把资产 + 消息一起送进来。Swap 示例就是这种模式：

-   Swap 合约部署在 ZetaChain，遵循 `UniversalContract` 接口。
    
-   所有跨链调用都通过 Gateway 进入，只暴露一个 `onCall` 入口。
    
-   这让你可以从任意连接链上用一次交易完成「充值 + 指令」的操作，而合约统一在 ZetaChain 内部处理逻辑。
    

### 2\. Messaging / Contract Calls

除了通用 Swap 合约，ZetaChain 还提供：

-   **Messaging 教程**：教你如何在多个 EVM 链之间做跨链消息传递。
    
-   **Calls to/from EVM / Solana / Sui**：底层是相同的消息和资产模型，只是链类型不同。
    

对今天的接口层设计来说，这意味着：

> 你的中间层服务只需要决定「我要调用哪个 Universal 合约 / Gateway / 消息格式」，真正的跨链细节交给 ZetaChain 处理。

## 三、ZRC-20 标准再理解：

### 1\. ZRC-20 是什么？

ZRC-20 是在 ZetaChain 上的 **ERC-20 兼容代币标准**，用来表示「任意连接链上的原生资产」。

-   例如：链上真实的 BTC / ETH / SOL 等，都可以在 ZetaChain 上被映射成对应的 ZRC-20。
    
-   对开发者来说，你只跟 ZRC-20 打交道，就能在 ZetaChain 上统一管理多链资产、做 DeFi 逻辑。
    

### 2\. 在 Swap 合约中的角色

官方 Swap 教程里的 Universal Swap 合约，大致流程是：

1.  从某条链（如 Base Sepolia）`depositAndCall` 到 ZetaChain。
    
2.  Gateway 把资产包装成对应的 ZRC-20，连同 `message` 交给 Swap 合约的 `onCall`。
    
3.  合约解析 payload，拿到：
    
    -   目标 ZRC-20 token 地址（`targetToken`）
        
    -   目标链上的接收人地址（bytes 格式）
        
    -   是否需要从 ZetaChain 再 withdraw 到目标链（`withdrawFlag`）
        
4.  查询 `withdrawGasFee`，用一部分资产换目标链的 gas ZRC-20，剩余部分换成目标 token。
    
5.  如果需要跨链 withdraw，则通过 `gateway.withdraw` 把目标 ZRC-20 burn 掉，释放回目标链上的原生资产。
    

> 接口层要做的，就是帮合约准备好这些“意图喂给 onCall”的参数，比如 `targetToken`、`recipient(bytes)`、`withdrawFlag` 等。

## 四、接口层 / 中间层服务：

把 Qwen-Agent 看成「上游」，ZetaChain 合约看成「下游」，中间层服务的职责可以拆成几块：

1.  **统一意图结构**
    
    -   把 `parse_swap_intent` 返回的 JSON 标准化为一个内部的 `SwapIntent` 数据结构。
        
2.  **链 & Token 映射**
    
    -   根据 `source_chain` / `dest_chain`、`from_token` / `to_token`，查表找到：
        
        -   源链 chainId / Gateway 地址
            
        -   目标 ZRC-20 token 地址（用于 payload 里的 `targetToken`）
            
3.  **策略 & 合约选择**
    
    -   目前默认使用官方的 Universal Swap 合约地址 `UNIVERSAL_SWAP_ADDRESS`。
        
    -   将来可以根据业务类型（普通 Swap / 带消息的 Swap / 自定义 DEX）选用不同合约。
        
4.  **构造调用参数**
    
    -   构建 `deposit-and-call` 或 Gateway `depositAndCall` 的参数：
        
        -   `--chain-id`（源链）
            
        -   `--receiver`（universal 合约地址）
            
        -   `--types / --values`（ABI 编码的 payload）
            
5.  **输出计划 / 执行调用**
    
    -   在当前作业阶段，只需要把「准备发起什么交易」打印到控制台。
        
    -   未来可以真正发起链上调用，或返回给前端 / 调用钱包 SDK。
        

## 五、假设的 `parse_swap_intent` 输出结构

今天主要是「接口层建模」练习，可以先假定 `parse_swap_intent` 返回的数据结构大概长这样：

```
{
  "source_chain": "base-sepolia",
  "dest_chain": "ethereum-sepolia",
  "from_token": "ETH",
  "to_token": "ETH",
  "amount": "0.001",
  "slippage_bps": 50,
  "receiver": "0x92ae647a9D8d09D58514037d6535ab93a2A8138f",
  "withdraw": true
}
```

在代码里可以抽象成一个类型：

```
type ParsedSwapIntent = {
  source_chain: string;   // eg. "base-sepolia"
  dest_chain: string;     // eg. "ethereum-sepolia"
  from_token: string;     // eg. "ETH"
  to_token: string;       // eg. "ETH"
  amount: string;         // 原样字符串，方便后续 BigNumber 处理
  slippage_bps?: number;  // 可选
  receiver: string;       // 用户填写/推断出的收款地址（EVM 格式）
  withdraw: boolean;      // true: 最终要 withdraw 到目标链
};
```

> 实际项目里，你可以把 `parse_swap_intent` 原始返回结构做一次 `transform`，变成这个统一的 `ParsedSwapIntent`。

## 六、链 & Token 映射配置：

### 1\. 链配置：chainId + Gateway

示例：

```
const CHAIN_CONFIG = {
  "base-sepolia": {
    name: "Base Sepolia",
    chainId: 84532,
    gateway: "0xGatewayOnBase...", // EVM Gateway 地址（真实地址查 docs 或 protocol-contracts）:contentReference[oaicite:7]{index=7}
  },
  "ethereum-sepolia": {
    name: "Ethereum Sepolia",
    chainId: 11155111,
    gateway: "0xGatewayOnSepolia...",
  },
  // ... 其他链
} as const;
```

将来可以扩展：Solana / Bitcoin / Sui 等，在 ZetaChain 的体系里也是通过相应的 Gateway 或程序实现的。[zetachain.com](http://zetachain.com)[+1](https://www.zetachain.com/docs/developers?utm_source=chatgpt.com)

### 2\. Token 映射：业务 token → ZRC-20

ZetaChain 的命令行可以查询某个 token 的 ZRC-20 地址，例如：

```
zetachain q tokens show --symbol sETH.SEPOLIA -f zrc20
```

返回的是该 token 在 ZetaChain 上的 ZRC-20 合约地址。

中间层里，我们预先写一张映射表：

```
type TokenConfig = {
  symbol: string;        // 业务层的 token 名，如 "ETH"
  chain: string;         // 属于哪条链，如 "ethereum-sepolia"
  zrc20Symbol: string;   // ZetaChain token symbol，如 "sETH.SEPOLIA"
  zrc20Address: string;  // ZRC-20 合约地址
};

// key 采用 `${symbol}:${chainKey}`
const TOKEN_CONFIG: Record<string, TokenConfig> = {
  "ETH:base-sepolia": {
    symbol: "ETH",
    chain: "base-sepolia",
    zrc20Symbol: "sETH.BASE_SEPOLIA",
    zrc20Address: "0x....",
  },
  "ETH:ethereum-sepolia": {
    symbol: "ETH",
    chain: "ethereum-sepolia",
    zrc20Symbol: "sETH.SEPOLIA",
    zrc20Address: "0x....",
  },
  // ...
};
```

> 这张表就是接口层的「知识库」：AI 只说“从 Base 把 0.001 ETH 换到 Ethereum”，中间层根据它帮你找到正确的 ZRC-20 合约地址和链参数。

## 七、作业：

下面是一个偏 Node.js / TypeScript 风格的代码，实现作业：

1.  接收 `parse_swap_intent` 返回值。
    
2.  根据不同链 / 不同 token 选择具体的合约 / 调用方式。
    
3.  目前只在控制台打印「准备发起什么交易」，不真的上链。
    

### 1\. 定义意图类型和配置

```
// 1. 意图类型（上一节提到的）
type ParsedSwapIntent = {
  source_chain: string;
  dest_chain: string;
  from_token: string;
  to_token: string;
  amount: string;
  slippage_bps?: number;
  receiver: string;
  withdraw: boolean;
};

// 2. 链 & token 配置
type ChainConfig = {
  name: string;
  chainId: number;
  gateway: string;
};

const CHAIN_CONFIG: Record<string, ChainConfig> = {
  "base-sepolia": {
    name: "Base Sepolia",
    chainId: 84532,
    gateway: "0xGatewayOnBase...",
  },
  "ethereum-sepolia": {
    name: "Ethereum Sepolia",
    chainId: 11155111,
    gateway: "0xGatewayOnSepolia...",
  },
};

type TokenConfig = {
  symbol: string;
  chain: string;
  zrc20Symbol: string;
  zrc20Address: string;
};

const TOKEN_CONFIG: Record<string, TokenConfig> = {
  "ETH:base-sepolia": {
    symbol: "ETH",
    chain: "base-sepolia",
    zrc20Symbol: "sETH.BASE_SEPOLIA",
    zrc20Address: "0xBaseETHZRC20...",
  },
  "ETH:ethereum-sepolia": {
    symbol: "ETH",
    chain: "ethereum-sepolia",
    zrc20Symbol: "sETH.SEPOLIA",
    zrc20Address: "0xSepoliaETHZRC20...",
  },
};
```

### 2\. 中间层核心函数：

```
// Universal Swap 合约地址（部署在 ZetaChain）
const UNIVERSAL_SWAP_ADDRESS = "0xUniversalSwapOnZeta...";

function buildSwapPlan(intent: ParsedSwapIntent) {
  const srcChain = CHAIN_CONFIG[intent.source_chain];
  const dstChain = CHAIN_CONFIG[intent.dest_chain];

  if (!srcChain || !dstChain) {
    throw new Error(`未知链: ${intent.source_chain} / ${intent.dest_chain}`);
  }

  const fromTokenKey = `${intent.from_token}:${intent.source_chain}`;
  const toTokenKey = `${intent.to_token}:${intent.dest_chain}`;

  const fromToken = TOKEN_CONFIG[fromTokenKey];
  const toToken = TOKEN_CONFIG[toTokenKey];

  if (!fromToken || !toToken) {
    throw new Error(`找不到 Token 映射: ${fromTokenKey} 或 ${toTokenKey}`);
  }

  // 构造给 Swap Universal App 的 message payload（概念层面）
  // 实际合约 onCall 中会用 abi.decode(message, (address, bytes, bool))
  const payload = {
    targetToken: toToken.zrc20Address, // ZRC-20 地址
    recipientBytes: `bytes(${intent.receiver})`, // 真实实现需要按链类型做编码
    withdrawFlag: intent.withdraw,
  };

  return {
    srcChain,
    dstChain,
    fromToken,
    toToken,
    payload,
    // 表示我们计划调用的链上方法（概念）
    call: {
      type: "depositAndCall",
      gateway: srcChain.gateway,
      universal: UNIVERSAL_SWAP_ADDRESS,
      amount: intent.amount,
      // CLI 对应：--types address bytes bool
      //          --values targetToken recipientBytes withdrawFlag
    },
  };
}
```

### 3\. 实际入口：接收 `parse_swap_intent` 的输出并打印

```
function handleParsedSwapIntent(intent: ParsedSwapIntent) {
  try {
    const plan = buildSwapPlan(intent);

    console.log("=== [Qwen-Agent × ZetaChain] 交易计划 ===");
    console.log(`来源链: ${plan.srcChain.name} (chainId=${plan.srcChain.chainId})`);
    console.log(`目标链: ${plan.dstChain.name} (chainId=${plan.dstChain.chainId})`);
    console.log(
      `资产: 从 ${plan.fromToken.symbol} @ ${plan.fromToken.chain} ` +
      `=> ${plan.toToken.symbol} @ ${plan.toToken.chain}`
    );
    console.log(`数量: ${intent.amount}`);
    if (intent.slippage_bps !== undefined) {
      console.log(`滑点: ${intent.slippage_bps} bps`);
    }
    console.log(`接收地址: ${intent.receiver}`);
    console.log(`是否需要跨链 withdraw: ${intent.withdraw}`);

    console.log("\n将调用链上方法（概念化表示）：");
    console.log(`- Gateway 合约: ${plan.call.gateway}`);
    console.log(`- Universal Swap 合约: ${plan.call.universal}`);
    console.log(`- 调用类型: ${plan.call.type}`);
    console.log("- 携带的 payload 参数：");
    console.log(`  * targetToken (ZRC-20): ${plan.payload.targetToken}`);
    console.log(`  * recipient (bytes): ${plan.payload.recipientBytes}`);
    console.log(`  * withdrawFlag: ${plan.payload.withdrawFlag}`);

    console.log("\n（当前阶段仅打印计划，不真正发起交易）");
  } catch (e) {
    console.error("[错误] 构建交易计划失败:", (e as Error).message);
  }
}
```

### 4\. 一个 HTTP 后端路由

```
// 接收来自 Qwen-Agent 的请求
app.post("/swap-intent", (req, res) => {
  const intent: ParsedSwapIntent = req.body; // 这里假设 Agent 把 parse_swap_intent 结果直接 POST 过来
  handleParsedSwapIntent(intent);
  res.json({ ok: true });
});
```

> **作业的核心：**
> 
> -   把自然语言解析结果 → 映射到链 & token → 映射到合约调用接口（哪怕只是打印）。
>     
> -   整个过程只在“中间层服务”里完成，Agent 不需要关心任何链上细节。
>     

## 头疼。。。。T\_T
<!-- DAILY_CHECKIN_2025-12-04_END -->

# 2025-12-03
<!-- DAILY_CHECKIN_2025-12-03_START -->



### Day 10 学习笔记：

## 1\. 今天干了啥

目标：  
把「自然语言 DeFi 需求」变成「结构化参数」，为之后真正发起 swap 交易打基础。

-   用户说：
    
    > 帮我在 Base 上用 10 USDC 换成 ETH  
    > 我们希望 Agent 产出类似：
    
    ```
    { "chain": "base", "tokenIn": "USDC", "tokenOut": "ETH", "amount": "10" }
    ```
    
-   技术路线：利用 **Function Calling / Tool Calling**，让 LLM 按我们定义好的 JSON Schema 自动填参数。Qwen / 百炼已经内置了这套机制。
    

今天的核心是：**设计一个 parse\_swap\_intent 工具 + 相应的参数 schema**，完成一个最小可用的意图解析层。

## 2\. Function Calling 快速回顾

### 2.1 工具（tools）数组的结构

在百炼 / Qwen 的 OpenAI 兼容接口中，调用聊天接口时可以传入 `tools` 数组，每个元素代表一个可用的工具（函数）：

```
{
  "tools": [
    {
      "type": "function",
      "function": {
        "name": "your_tool_name",
        "description": "工具做什么，用什么场景",
        "parameters": {
          "type": "object",
          "properties": {
            "...": { "type": "string" }
          },
          "required": ["..."]
        }
      }
    }
  ]
}
```

要点：

-   `type` 固定为 `"function"`。
    
-   `parameters` 字段使用 **JSON Schema** 描述入参；`type: "object"` + `properties` + `required`。
    
-   模型在生成工具调用时，会按照这个 schema 自动补齐 `arguments` 字段（字符串形式的 JSON）。
    

### 2.2 调用流程

百炼文档中给的是一个标准流程（以天气查询为例）：

1.  **第一次模型调用**：传入 `messages` + `tools`。
    
2.  模型如果认为需要用工具，返回 `tool_calls`，里面包含：
    
    -   要调用的函数名 `name`
        
    -   JSON 字符串形式的 `arguments`
        
3.  应用侧根据 `tool_calls` 去执行真实工具（API / 数据库 / on-chain 等）。
    
4.  把工具的返回结果作为 `role: "tool"` 的 message 再喂给模型，发起第二次调用。
    
5.  模型基于工具结果 + 原始问题，给出最终自然语言回答。
    

在 DeFi 场景里，可以有两种玩法：

-   **玩法 A：parse\_swap\_intent 是“假工具”**  
    只用它的参数 schema 来让模型「吐出 JSON」，后端并不真的执行这个函数，而是直接把 `arguments` 当做解析结果。
    
-   **玩法 B：parse\_swap\_intent 是“真工具”**  
    由后端通过规则 / 正则 / 传统 NLP 来解析文本，把结果 JSON 返回给模型做后续推理。
    

今天练习偏向 **玩法 A（schema-only 工具）**：  
——让 LLM 自己负责参数填充，我们只消费它的 `arguments`。

## 3\. 针对 DeFi Swap 设计 parse\_swap\_intent

### 3.1 工具设计目标

工具名：`parse_swap_intent`

需求：

-   输入：**用户的自然语言需求**，其实可以直接来自上一轮 User message，不一定要再传 `text` 字段。
    
-   输出（工具 arguments 结构）：
    
    -   `chain`：在哪条链上 swap（可选）
        
    -   `tokenIn`：卖出代币符号
        
    -   `tokenOut`：买入代币符号
        
    -   `amount`：数量，使用字符串形式表示十进制数
        

我们把这些字段放在 `parameters` 里，让模型帮我们填。

### 3.2 tools 中的 schema 设计

示意（省略无关字段）：

```
{
  "tools": [
    {
      "type": "function",
      "function": {
        "name": "parse_swap_intent",
        "description": "解析用户的 DeFi 换币意图，抽取链、代币和金额等参数，仅做解析，不实际发起交易。",
        "parameters": {
          "type": "object",
          "properties": {
            "chain": {
              "type": "string",
              "description": "目标链名称，例如 base、polygon、ethereum。如果用户没提，可以留空或填 null。"
            },
            "tokenIn": {
              "type": "string",
              "description": "用户支付的代币符号，如 USDC、USDT、ETH。"
            },
            "tokenOut": {
              "type": "string",
              "description": "用户想要收到的代币符号，如 ETH、MATIC。"
            },
            "amount": {
              "type": "string",
              "description": "支付代币 tokenIn 的数量，十进制字符串，不带单位。"
            }
          },
          "required": ["tokenIn", "tokenOut"],
          "additionalProperties": false
        }
      }
    }
  ]
}
```

说明：

-   这里我们刻意 **不把** `text` **作为参数**，因为用户原始文本已经在 messages 里；这个工具纯粹用来“产出结构化参数”。
    
-   `amount` 允许留空：如果用户只说“把我所有的 USDC 换成 ETH”，模型可以不填或自定义约定（比如 `"all"`）。
    
-   `chain` 也可以是可选字段，由后端决定默认链（例如默认 Ethereum）。
    

## 4\. 在 Qwen API 中如何使用

基于 Qwen 的 OpenAI 兼容接口，调用时大致是这样（伪代码）：

```
from openai import OpenAI

client = OpenAI(
    api_key="YOUR_KEY",
    base_url="https://dashscope.aliyuncs.com/compatible-mode/v1"
)

tools = [...]  # 上面定义好的 parse_swap_intent schema

resp = client.chat.completions.create(
    model="qwen-turbo",
    messages=[
        {
            "role": "system",
            "content": (
                "你是一个 DeFi 换币意图解析器。"
                "无论用户怎么说，只需要通过 parse_swap_intent 工具给出结构化参数，"
                "不要直接用自然语言回答。"
            )
        },
        {
            "role": "user",
            "content": "帮我在 Base 上用 10 USDC 换成 ETH"
        }
    ],
    tools=tools,
    tool_choice="auto"
)

# 解析 resp.choices[0].message.tool_calls[0].function.arguments
```

后端只需拿到：

```
{"chain": "base", "tokenIn": "USDC", "tokenOut": "ETH", "amount": "10"}
```

就完成了本次 MVP 的工作。

## 5\. 手工演练：两个输入如何被解析？

### 5.1 “帮我在 Base 上用 10 USDC 换成 ETH”

语义拆解：

-   链：Base
    
-   金额：10
    
-   输入代币：USDC
    
-   输出代币：ETH
    

期望工具调用（核心部分）：

```
{
  "tool_calls": [
    {
      "type": "function",
      "function": {
        "name": "parse_swap_intent",
        "arguments": "{\"chain\": \"base\", \"tokenIn\": \"USDC\", \"tokenOut\": \"ETH\", \"amount\": \"10\"}"
      }
    }
  ]
}
```

解 JSON 字符串后的结构：

```
{
  "chain": "base",
  "tokenIn": "USDC",
  "tokenOut": "ETH",
  "amount": "10"
}
```

### 5.2 “把我 50 U 兑换成 Polygon 上的 MATIC”

语义拆解：

-   「Polygon 上的 MATIC」→ `chain = "polygon"`, `tokenOut = "MATIC"`
    
-   「50 U」→ `amount = "50"`, `tokenIn` 需要一个约定：
    
    -   简单版本：把 **U 统一映射为 USDT**（或 USDC），由业务侧统一定义。
        
    -   严谨版本：让 Agent 先问一句澄清（现实系统里可以这么做）。
        

在本练习中，假设：

> 约定：`"U"` → `"USDT"`

则期望解析结果：

```
{
  "chain": "polygon",
  "tokenIn": "USDT",
  "tokenOut": "MATIC",
  "amount": "50"
}
```

## 6\. 个人的思考：通用 DeFi 项目里还要哪些字段？

结合未来想做的通用 DeFi 项目，可以在 `parameters.properties` 里逐步引入更多字段，例如：

-   `slippageBps`：滑点（bp），如 `30` 代表 0.3%；
    
-   `recipient`：接收地址（默认用当前用户地址）；
    
-   `exactIn` / `exactOut`：区分「精确输入」还是「精确输出」模式；
    
-   `deadline`：交易过期时间（Unix 时间戳或相对秒数）；
    
-   `tokenInAddress` / `tokenOutAddress`：在指定链上的合约地址（后端可根据 symbol + chain 解析）；
    
-   `referrer` / `affiliateCode`：返佣 / 邀请码；
    
-   `useAggregator`：是否使用聚合器（如 1inch、0x、Odyssey 等）；
    
-   `gasToken`：支付 gas 的币种（在 multi-gas 模式链上有用）。
    

这些都可以通过 **不断扩展 JSON Schema + few-shot 示例** 的方式逐步训练 Agent 填得更准，而解析层本质保持不变。

## 7\. 小结

1.  **Function Calling / Tool Calling** 是把 LLM 变成「能填 JSON、能调 API 的 Agent」的核心机制。
    
2.  在 Qwen / 百炼 中，通过 `tools` + `parameters(JSON Schema)` 定义一套结构，模型就会按这个结构帮你生成参数。
    
3.  对 DeFi 来说，**parse\_swap\_intent 可以只做“解析工具”**：
    
    -   不直接发交易，只负责把自然语言 → `{ chain, tokenIn, tokenOut, amount }`。
        
4.  因此今天的作业本质是：
    
    -   设计好 `parse_swap_intent` 的 JSON Schema；
        
    -   理解 Qwen Function Calling 的参数格式和调用流程；
        
    -   手工演练几个输入，确保你的 schema 足够表达这些需求。
        

官方文档优化啊。。。。😣
<!-- DAILY_CHECKIN_2025-12-03_END -->

# 2025-12-02
<!-- DAILY_CHECKIN_2025-12-02_START -->




### 学习笔记 Day 9:

## 一、Qwen-Agent 是什么？

Qwen-Agent 是阿里巴巴 Qwen 团队开源的智能体（Agent）开发框架，用来把 Qwen 系列大模型变成“会用工具、会规划、有记忆”的 Agent。它主要基于四种能力：

-   指令跟随（Instruction Following）
    
-   工具使用（Function / Tool Calling）
    
-   任务规划（Planning）
    
-   记忆管理（Memory）
    

官方已经用它做了 Browser Assistant、Code Interpreter、定制助手等应用，现在 Qwen Chat 的后端也由 Qwen-Agent 驱动。

## 二、核心组件：LLM / Agent / Tools / Memory

### 1\. LLM（大模型）

-   Qwen-Agent 自身不“带模型”，它只是一个框架。
    
-   你可以：
    
    -   用 DashScope 的托管模型；
        
    -   或用 vLLM / Ollama 等把开源 Qwen 模型以 OpenAI 兼容接口服务起来。
        

LLM 在 Qwen-Agent 中通常通过一个 `llm_cfg` 字典配置，例如：

```
llm_cfg = {
    'model': 'Qwen/Qwen3-7B-Instruct',        # 或者 DashScope 模型名
    'model_server': 'http://localhost:8000/v1',  # OpenAI 兼容服务地址
    'api_key': 'EMPTY',                       # 自托管一般写个占位
    'generate_cfg': {
        'top_p': 0.8,
    }
}
```

> 记住：Qwen-Agent 会利用 Qwen 模型内置的函数调用能力，无需你自己去写复杂的 function call 模板。

### 2\. Agent（智能体）

-   框架里已经封装了多种 Agent 类，比如 `Assistant`、`FnCallAgent`、`ReActChat` 等。
    
-   这些 Agent 会负责：
    
    -   把用户消息、历史对话、工具列表组织成 prompt；
        
    -   调用底层 LLM；
        
    -   解析 LLM 给出的“工具调用指令”；
        
    -   把工具执行结果再喂回模型，直到得到最终回答。
        

对你来说，最常用的是 `Assistant`：

```
from qwen_agent.agents import Assistant

bot = Assistant(
    llm=llm_cfg,
    function_list=[],      # 后面我们会把自定义 Tool 填进来
)
```

### 3\. Tools（工具）

工具是 Agent 可以调用的“外部能力”，比如：

-   内置工具：`code_interpreter`、浏览器、RAG 等；
    
-   MCP 工具：通过 MCP 协议挂接各种外部服务；
    
-   自定义工具：用 Python 写一个类继承 `BaseTool` 即可。
    

当工具被注册后，只要你把它的名字放到 `function_list` 里，Agent 就可以在合适的时候“自动决定”是否调用它。

### 4\. Memory（记忆）

Qwen-Agent 的记忆主要体现在两层：

1.  **工作记忆（Working Memory）**
    
    -   在一次多轮对话 / 长流程中，Agent 会记录：
        
        -   历史消息；
            
        -   已调用过的工具及返回结果。
            
    -   后续每一步决策都会基于这份记忆。
        
2.  **规划 + 多步推理**
    
    -   Agent 可以在内部规划“先干什么、再干什么”，例如：
        
        -   先用某个工具检索信息；
            
        -   再调用计算工具处理数据；
            
        -   最后生成一段自然语言回答。
            

对 Day 9 来说，你只需要知道：**你维护一个** `messages` **列表，Qwen-Agent 会帮你把这份“上下文 + 工具结果”的记忆用好。**

## 三、搭建一个最小可运行的 Agent

### 1\. 安装 Qwen-Agent

```
# 推荐安装带 GUI / RAG / 代码执行 / MCP 的完整版本
pip install -U "qwen-agent[gui,rag,code_interpreter,mcp]"

# 或者只装最小依赖
# pip install -U qwen-agent
```

这是官方推荐的安装方式。

### 2\. 准备模型服务（二选一）

1）使用 DashScope 托管服务（最简单）

-   在环境变量中设置 `DASHSCOPE_API_KEY`；
    
-   在 `llm_cfg` 中写：
    

```
llm_cfg = {
    'model': 'qwen-max-latest',
    'model_type': 'qwen_dashscope',
}
```

2）自托管 Qwen 模型（本地 / 服务器）

-   按 Qwen 官方文档用 vLLM 或 Ollama 启一个 OpenAI 兼容服务；
    
-   然后在 `llm_cfg` 中改为：
    

```
llm_cfg = {
    'model': 'Qwen/Qwen3-7B-Instruct',
    'model_server': 'http://localhost:8000/v1',
    'api_key': 'EMPTY',
}
```

### 3\. 最小 Agent 示例（无工具版）

```
from qwen_agent.agents import Assistant

# 1. LLM 配置（按你的实际环境改）
llm_cfg = {
    'model': 'Qwen/Qwen3-7B-Instruct',
    'model_server': 'http://localhost:8000/v1',
    'api_key': 'EMPTY',
}

# 2. 创建 Agent（暂时不挂任何工具）
bot = Assistant(
    llm=llm_cfg,
    function_list=[],            # 这里先留空
)

# 3. 发送一次简单对话
messages = [{'role': 'user', 'content': '用一句话介绍一下你自己'}]

for responses in bot.run(messages=messages):
    # 官方示例中 bot.run 会以“流”的形式不断返回最新结果列表:contentReference[oaicite:10]{index=10}
    pass

print(responses)   # responses 通常是一个包含 assistant 回复的列表
```

只要你的模型服务和 `llm_cfg` 配好了，这段代码跑通，你就完成了：

> ✅ “搭建一个最小的 Agent”

## 四、实现两个自定义 Tool：大写转换 & 加法

下面我们用官方推荐的方式（继承 `BaseTool` + `@register_tool` 装饰器）各写一个 Tool。

### 1\. 通用工具模板

Qwen-Agent 里自定义 Tool 的基础写法是这样的（简化版）：

```
from qwen_agent.tools.base import BaseTool, register_tool
import json

@register_tool('tool_name')
class MyTool(BaseTool):
    # 向 Agent 描述这个工具是干什么的
    description = '简单说明工具用途'
    # 告诉 Agent 这个工具需要哪些参数，以及参数类型
    parameters = [
        {
            'name': 'param1',
            'type': 'string',          # 或 number / integer / boolean ...
            'description': '参数说明',
            'required': True,
        }
    ]

    def call(self, params: str, **kwargs) -> str:
        # params 是 LLM 生成的一段 JSON 字符串
        data = json.loads(params)
        # 根据参数执行实际逻辑
        # ...
        # 返回字符串（可以是纯文本，也可以是 JSON 串）
        return 'some result'
```

> 只要类用 `@register_tool('xxx')` 装饰，字符串 `'xxx'` 就可以作为工具名出现在 `function_list` 里。

### 2\. 字符串转大写 Tool

```
from qwen_agent.tools.base import BaseTool, register_tool
import json

@register_tool('to_upper')
class ToUpper(BaseTool):
    description = '把输入字符串转换为大写形式'
    parameters = [
        {
            'name': 'text',
            'type': 'string',
            'description': '要转换的大写字符串',
            'required': True,
        }
    ]

    def call(self, params: str, **kwargs) -> str:
        data = json.loads(params)
        text = data['text']
        result = text.upper()
        # 返回简单 JSON 结果，方便 Agent 解析
        return json.dumps({'result': result}, ensure_ascii=False)
```

### 3\. 两数求和 Tool

```
from qwen_agent.tools.base import BaseTool, register_tool
import json

@register_tool('add')
class Add(BaseTool):
    description = '计算两个数字的和'
    parameters = [
        {
            'name': 'a',
            'type': 'number',
            'description': '第一个数字',
            'required': True,
        },
        {
            'name': 'b',
            'type': 'number',
            'description': '第二个数字',
            'required': True,
        }
    ]

    def call(self, params: str, **kwargs) -> str:
        data = json.loads(params)
        a = data['a']
        b = data['b']
        s = a + b
        return json.dumps({'result': s}, ensure_ascii=False)
```

### 4\. 把 Tool 挂到 Agent 上

只要保证上述两个类在你脚本里被 import / 定义过，`@register_tool` 就已经生效了，你只需要：

```
from qwen_agent.agents import Assistant

# 延续前面的 llm_cfg
bot = Assistant(
    llm=llm_cfg,
    function_list=[
        'to_upper',   # 自定义工具
        'add',        # 自定义工具
        # 你也可以在这里加内置的 'code_interpreter' 等
    ],
)

# 测试调用：字符串转大写
messages = [{
    'role': 'user',
    'content': '请把字符串 "hello qwen-agent" 变成大写。'
}]

for responses in bot.run(messages=messages):
    pass

print(responses)
```

如果模型配置正确、工具定义无误，你会在返回结果中看到 Agent 已经自动选择调用 `to_upper` 工具，并把工具返回的结果整合进自然语言回答。

同理，你可以再发一条：

> “再帮我算一下 3 + 5。”

Agent 就应该调用 `add` 工具并给出计算结果。

## 五、Qwen-Agent 中的工具调用与对话流程

结合官方文档与社区解读，可以把一次工具调用的流程想象成这样：

1.  **收集上下文**
    
    -   把当前用户消息 + 历史对话 + 系统指令 + 可用工具列表打包成 prompt。
        
2.  **模型判断是否需要工具**
    
    -   Qwen 模型根据 prompt 决定：
        
        -   直接回答；
            
        -   还是输出“函数调用格式”的 JSON（类似 OpenAI function calling）。
            
3.  **框架解析工具调用 JSON**
    
    -   Qwen-Agent 内部的解析器读取工具名和参数；
        
    -   调用对应的 Python Tool 类的 `call` 方法。
        
4.  **执行工具并写回记忆**
    
    -   Tool 返回的字符串会被作为新的“消息”加入到对话记忆中。
        
5.  **模型基于工具结果生成最终回答**
    
    -   再次调用 LLM；
        
    -   这一次 prompt 中已经包含工具输出，模型可以基于这些结果组织自然语言回答。
        

这个流程，你不需要自己写任何“解析 JSON、执行函数”的胶水代码——Qwen-Agent 都已经封装好了，你只管写 **工具逻辑** 和 **业务提示词**。
<!-- DAILY_CHECKIN_2025-12-02_END -->

# 2025-12-01
<!-- DAILY_CHECKIN_2025-12-01_START -->





### 学习笔记 Day8：

## 一、今日学习内容

-   **目标 1**：搞清楚通义千问（Qwen）是什么、有哪些模型，适合什么场景。
    
-   **目标 2**：理解 Qwen 的 **OpenAI 兼容 API 调用方式**（chat/completions）。
    
-   **目标 3**：用 Python 写一个最小脚本：
    
    -   输入提示词
        
    -   调用 Qwen 生成一段 **ZetaChain 公链介绍**
        
    -   在终端打印结果
        
-   **记录**：本次示例选择的模型和调用参数。
    

## 二、通义千问 / Qwen 模型快速认知

1.  **通义千问是什么？**  
    通义千问是阿里云自研的大语言模型家族，能处理自然语言、多模态数据（文本、图片、音频、视频等），支持创作、对话、代码、翻译等多种任务。
    
2.  **在阿里云 Model Studio 里的主力商用模型（北京/新加坡）**
    
    | 产品名（中文） | 典型模型名（API） | 特点 |
    | --- | --- | --- |
    | 通义千问 Max | qwen-max / qwen3-max 等 | 能力最强，适合复杂推理、大型应用 |
    | 通义千问 Plus | qwen-plus / qwen-plus-latest | 效果、速度、成本均衡，本次示例用它 |
    | 通义千问 Flash | qwen-flash | 简单任务、速度优先、成本低 |
    
3.  **为什么示例选择 qwen-plus？**
    

-   官方定位是“**效果/速度/成本均衡**”的旗舰模型，适合作为默认通用模型。
    
-   在 API 文档和 OpenAI 兼容示例中大量以 `qwen-plus` 作为示例模型，资料完善。
    

> **记一笔**：以后自己的习惯可以是——**默认用** `qwen-plus` **做通用对话/文案，遇到特别重的推理或高价值任务再切到** `qwen-max`**。**

## 三、Qwen API：OpenAI 兼容调用方式梳理

### 3.1 调用入口

官方推荐使用 **OpenAI 兼容接口** 调用 Qwen 模型。

-   **Base URL**
    
    -   中国大陆（北京）：`https://dashscope.aliyuncs.com/compatible-mode/v1`
        
    -   国际（新加坡）：`https://dashscope-intl.aliyuncs.com/compatible-mode/v1`
        
-   **HTTP 路径（统一）**：  
    `POST {base_url}/chat/completions`  
    比如北京地域就是：  
    `POST https://dashscope.aliyuncs.com/compatible-mode/v1/chat/completions`
    

### 3.2 鉴权 & SDK

1.  在阿里云百炼控制台申请 **API Key**。
    
2.  推荐把 Key 配置到环境变量，例如：
    

```
export DASHSCOPE_API_KEY="sk-xxxxxxxx"
```

3.  Python 使用官方 OpenAI SDK：
    

```
pip install --upgrade openai
```

4.  创建客户端（Python）：
    

```
from openai import OpenAI
import os

client = OpenAI(
    api_key=os.getenv("DASHSCOPE_API_KEY"),
    base_url="https://dashscope.aliyuncs.com/compatible-mode/v1",
)
```

> 注意：**如果忘记设置** `base_url`**，SDK 会连到 OpenAI 官方而不是通义千问。**

### 3.3 请求体基本结构

Qwen 的 OpenAI 兼容接口遵循标准的 `chat.completions` 规范：

核心字段：

```
{
  "model": "qwen-plus",
  "messages": [
    {"role": "system", "content": "You are a helpful assistant."},
    {"role": "user", "content": "你是谁？"}
  ],
  // 下列参数可选
  "temperature": 0.7,
  "top_p": 0.9,
  "max_tokens": 512,
  "stream": false,
  "tools": [...],
  "response_format": {...},
  // 非标准 OpenAI 参数通过 extra_body 扩展
  "extra_body": {
    "enable_search": true
  }
}
```

### 3.4 返回结构要点

典型返回结构（简化）：

-   `id`: 本次调用 ID
    
-   `model`: 使用的模型名
    
-   `choices`: 数组，每个 choice 是一个候选回复
    
    -   `choices[0].message.content`: 助手生成的文本
        
-   `usage`: token 统计
    
    -   `prompt_tokens`
        
    -   `completion_tokens`
        
    -   `total_tokens`
        

对于简单应用，只要记住：**拿** `choices[0].message.content` **当作答案** 即可。

## 四、常用调用参数理解小抄

（基于官方文档 + Qwen3 推荐配置整理）

1.  `model`（必填）
    
    -   字符串：如 `"qwen-plus"`, `"qwen-max"`, `"qwen-flash"`, `"qwen3-max"` 等。
        
    -   具体可用模型列表在 Model Studio “模型列表”页面和 OpenAI 兼容文档中有完整表格。
        
2.  `messages`（必填）
    
    -   聊天历史数组，每条消息包含：
        
        -   `role`: `"system" | "user" | "assistant"`
            
        -   `content`: 文本或多模态内容（图像输入时 content 是数组）。
            
    -   常见模式：
        
        -   只用 `system + user`：结构简单
            
        -   带多轮 `assistant` 内容：保持上下文、做连贯对话
            
3.  `temperature`（0~2，默认 ~0.7）
    
    -   控制“发散度”：越大越有创意，但不稳定。
        
    -   Qwen3 文档中对部分模型示例推荐：
        
        -   非思考模式：`temperature≈0.7, top_p≈0.8`
            
        -   思考模式：`temperature≈0.6, top_p≈0.95`
            
4.  `top_p`
    
    -   nucleus sampling 截断阈值，和 `temperature` 一起影响随机性。
        
    -   一般：`temperature=0.7, top_p=0.8~0.9` 是比较稳妥的组合。
        
5.  `max_tokens`
    
    -   最大生成 token 数。
        
    -   越大越容易生成长文案，但也消耗更多费用和时间。
        
    -   通常根据任务预估一个上限，如 256 / 512 / 1024。
        
6.  `stream` & `stream_options`
    
    -   `stream=True`：启用流式输出，逐块返回内容。
        
    -   `stream_options={"include_usage": True}`：在流最后一块里附带 `usage` 统计。
        
7.  `tools`（函数/工具调用）
    
    -   支持 OpenAI 风格的 tool / function calling，用于让模型调用外部函数（比如查天气、查时间等）。
        
8.  `extra_body.enable_search`（Qwen 特有）
    
    -   是否开启**联网搜索**：
        
    -   用法示例：
        
        ```
        extra_body={"enable_search": True}
        ```
        
    -   部分模型不支持该能力（会返回 `This model does not support enable_search` 错误）。
        

## 五、Python 最小脚本 —— 生成 ZetaChain 介绍

### 5.1 任务描述

-   使用 `qwen-plus` 模型
    
-   输入提示词：请模型用**面向区块链新手**的方式介绍一下 **ZetaChain 公链**
    
-   在终端打印生成的介绍文本
    

> 背景补充：ZetaChain 是一个面向“跨链互操作”的 Layer1 公链，支持所谓的“Omnichain Smart Contracts”，即在一条链上的合约可以直接管理多条链上的资产和数据，包括比特币等非智能合约链，这使它成为“通用跨链计算层”的一种实现。

### 5.2 完整 Python 脚本

```
import os
from openai import OpenAI

# 1. 创建客户端，指向阿里云百炼的 OpenAI 兼容接口
client = OpenAI(
    api_key=os.getenv("DASHSCOPE_API_KEY"),  # 在环境变量中配置 API Key
    base_url="https://dashscope.aliyuncs.com/compatible-mode/v1",
)

def main():
    # 2. 要求模型完成的任务：介绍 ZetaChain
    user_prompt = (
        "请用通俗易懂、面向区块链新手的方式，用大约 200 字介绍一下 ZetaChain 公链："
        "包括它解决什么问题、核心特点，以及适合哪些应用场景。"
    )

    # 3. 发送 chat.completions 请求
    completion = client.chat.completions.create(
        model="qwen-plus",  # 本次示例选的模型
        messages=[
            {
                "role": "system",
                "content": "You are a helpful Chinese assistant good at explaining blockchain concepts simply."
            },
            {
                "role": "user",
                "content": user_prompt
            },
        ],
        temperature=0.7,   # 适度发散，保证内容有点“说人话”
        top_p=0.9,
        max_tokens=512,    # 足够容纳 ~200 字中文介绍
    )

    # 4. 打印模型回复内容
    print(completion.choices[0].message.content)

if __name__ == "__main__":
    main()
```

这段代码结构基本和官方文档示例一致，只是**换成了自己的提示词和参数**。

### 5.3 本次调用：模型与参数记录

> **模型选择**

-   `model = "qwen-plus"`
    
    -   原因：官方推荐的通用旗舰模型，效果 & 价格均衡；文档示例丰富。
        

> **关键参数一览**

| 参数名 | 本次示例值 | 作用 & 备注 |
| --- | --- | --- |
| model | "qwen-plus" | 通义千问 Plus 通用大模型 |
| messages | system + user | system 设定“讲解风格”，user 给具体任务 |
| temperature | 0.7 | 增加一点多样性，让介绍更自然，不那么死板 |
| top_p | 0.9 | 配合 temperature 控制采样，多样但不太发散 |
| max_tokens | 512 | 限制生成长度，避免一次生成过长文案 |
| stream | 未设置（默认 False） | 本次不需要流式，简单拿最终结果即可 |
| extra_body | 未设置 | 如果要联网搜索，可以加 {"enable_search": True} |

> 以后可以按任务微调：
> 
> -   精准、可控：降低 `temperature`（比如 0.3~0.5），甚至减小 `top_p`
>     
> -   更有创意：提高 `temperature`，但可能会跑题
>     

### 5.4 运行效果

运行脚本后，终端会输出一段类似这样的中文说明（示意）：

> “ZetaChain 是一条专注跨链互操作的 Layer1 公链，你可以把它理解成连接各种区块链的‘总线’。  
> 它通过 Omnichain Smart Contracts 让开发者只在 ZetaChain 上写一份合约，就能同时管理以太坊、比特币等多条链上的资产和数据，从而简化跨链桥、跨链 DEX、统一钱包等场景的开发。对于普通用户来说，体验就像在一条链上操作，却能无感地完成多条链之间的资产流转和交互。”

具体措辞会由 Qwen 动态生成，但总体会围绕“**跨链互操作 + Omnichain 智能合约 + 简化用户体验**”展开。

## 六、今天的收获 & 小坑提示

1.  **Qwen 模型层面**
    
    -   知道了 Qwen/通义千问是一整个模型家族，有 Max / Plus / Flash 等不同定位。
        
    -   明确了可以按任务复杂度与成本，在不同模型之间切换。
        
2.  **API 调用层面**
    
    -   掌握了 OpenAI 兼容模式的基本使用方式：
        
        -   `base_url` 使用阿里云 DashScope
            
        -   `chat.completions.create` + `model` + `messages`
            
    -   学会通过 Python SDK 快速调用，并打印结果。
        
3.  **参数理解**
    
    -   温度、top\_p、max\_tokens 等参数的直觉用法；
        
    -   了解 Qwen 特有的 `extra_body.enable_search`，以后可以给 ZetaChain 这类“有实时动态”的项目配合联网搜索使用。
        
4.  **容易踩的坑**
    
    -   忘记设置 `base_url`：会误连到 OpenAI 官方；
        
    -   没有设置环境变量 `DASHSCOPE_API_KEY`：会报鉴权错误；
        
    -   给不支持联网搜索的模型传 `enable_search`：会报不支持该参数的错误。
        

补充一句：《风再起时》确实好听啊 哈哈哈
<!-- DAILY_CHECKIN_2025-12-01_END -->

# 2025-11-30
<!-- DAILY_CHECKIN_2025-11-30_START -->






## 学习笔记 Day6：  
一、回顾：ZetaChain 带来的「通用性」到底是什么？

### 1.1 Omnichain Smart Contracts & Universal Apps

ZetaChain 是一个专门为 **跨链 / 通用应用（Universal Apps）** 设计的 L1，它的节点可以直接读写外部链上的状态、管理外链资产。开发者只需要在 ZetaChain 上写一份合约，就可以统一编排多条链上的资产和逻辑，包括比特币这类非智能合约链。

官方提供了两种主要模式：

1.  **Omnichain Smart Contracts（全链合约）**
    
    -   合约部署在 ZetaChain EVM 上。
        
    -   通过协议本身的跨链读写能力 + ZRC-20，直接操作外链资产。
        
    -   好处：所有状态和安全边界集中在一份合约里，开发和审计成本更低。
        
2.  **Cross-Chain Messaging（跨链消息）**
    
    -   逻辑主要部署在各个外链（例如两个 EVM 链上的合约），ZetaChain 做消息路由。
        
    -   适合“每条链各自有逻辑，只需要同步少量数据”的场景。
        

**直观理解：**

-   需要强资产编排、统一风控、统一清算：用 Omnichain Smart Contract。
    
-   只是传个状态、通知、做简单触发：可以用 Messaging。
    

### 1.2 ZRC-20：外链资产在 ZetaChain 上的“影子”

ZRC-20 是 ZetaChain 定义的 **“跨链可控的 ERC-20 等价物”**：

-   外链上的原生资产（BTC、ETH、BNB、各链的 ERC-20 等）通过存入 TSS 地址 / 托管合约，  
    → 在 ZetaChain 上铸造对应的 ZRC-20。
    
-   从 ZetaChain 提现时：  
    → 在 ZetaChain 上销毁 ZRC-20，协议从外链托管地址把原生资产打给用户。
    
-   **只有协议本身可以铸造 ZRC-20**，确保 1:1 对应外链真实资产。
    

特别的一点是：

> 同一个“表面上相同”的资产，在不同链会对应不同的 ZRC-20。  
> 例如：

-   USDT(ETH) → ZRC-20 USDT from Ethereum
    
-   USDT(BSC) → ZRC-20 USDT from BSC  
    它们在 ZetaChain 看是两种资产，不过可以在 ZetaChain 内部 swap，从而实现“跨链转账”的效果（存入 A 链 → ZRC-20 swap → 提现到 B 链）。
    

这使得 **“统一看管 + 各链可编排”** 成为通用 DeFi 的基础。

### 1.3 Universal Token & Universal NFT：真正“链无关”的资产

在 ZRC-20 之外，ZetaChain 又提出了 **Universal Token / Universal NFT** 标准，用于直接发行「多链原生」资产：

1.  **Universal Token（通用 ERC-20）**
    
    -   是一套升级后的 ERC-20 合约（基于 OpenZeppelin + UUPS 升级模式）。
        
    -   同一个 Token 的合约部署在 ZetaChain + 若干 EVM 链上，通过 `setConnected` / `setUniversal` 互相绑定。
        
    -   供应和 metadata 在多链间保持统一，不需要 wrap / bridge。
        
    -   适合：稳定币、治理代币、LP 份额等需要「一份身份、多链通行」的资产。
        
2.  **Universal NFT（通用 ERC-721）**
    
    -   NFT 可以在任意连接链上铸造、转移，同一个 `tokenId` 在所有链上保持一致，metadata 也统一。
        
    -   适合：跨链游戏道具、跨链身份、NFT 市场等。
        

**对 DeFi 的意义：**

-   ZRC-20 更偏「外链资产的影子」。
    
-   Universal Token 更偏「多链原生资产本体」。
    

在通用 DeFi 场景里，可以考虑：用 ZRC-20 编排底层真实资产，用 Universal Token 表示协议中的「份额票据」。

### 1.4 Swap & Messaging：实现通用 DeFi 的“操作指令集”

**Swap（跨链兑换）**

官方教程里有一个通用 Swap 应用：用户可以在一个链上发起交易，在另一个链收到完全不同的资产，比如 **“以太坊上的 USDC → 比特币上的 BTC”，一步完成**。

大致流程：

1.  用户在源链把资产发给 Gateway；
    
2.  协议在 ZetaChain 铸造对应 ZRC-20；
    
3.  全链合约根据路由在 ZetaChain 或外链 DEX 上完成兑换；
    
4.  在目标链销毁 ZRC-20 并释放原生资产给用户。
    

**Messaging（跨链消息）**

Messaging 教程展示了如何只通过消息在两条 EVM 链的合约之间传数据，由 ZetaChain 做中继，无需在 ZetaChain 上部署合约。

通用 DeFi 可以把这两者组合：

-   用 Swap 处理“资产流转”；
    
-   用 Messaging 处理“策略指令 / 状态同步”。
    

### 1.5 把所有能力拼在一起

ZetaChain 的文档强调：**把 Omnichain Smart Contracts + Messaging 组合起来，可以把多步跨链交易包装成一次对用户友好的操作**，包括资产移动、清算、收益分发等等。

再结合前面那篇 “Cross-chain DEX, Restaking, BTC Staking” 博文，可以确认：

-   像 THORChain 这种原本需要单独一条链的跨链 DEX，可以直接用 ZetaChain 做成一个 dApp；
    
-   EigenLayer / BabylonChain 这样的 Restaking / BTC Staking 逻辑，也能被“搬”到 ZetaChain，全链化为一个 Omnichain dApp。
    

## 二、在 ZetaChain 上可以玩出的通用 DeFi 模式

结合官方案例，我把通用 DeFi 模式粗略整理成几个 archetype，后面做 idea 时可以当积木自由组合：

1.  **跨链 DEX / 聚合器**
    
    -   类似 THORChain / Eddy Finance：
        
        -   池里是各种 ZRC-20（BTC、ETH、USDT from 多链）。
            
        -   用户在任意链直接换到其他链的原生资产。
            
    -   再往上做一层：路由不同链上、不同 DEX 的流动性，做“通用聚合器”。
        
2.  **Omnichain 借贷 / 抵押管理**
    
    -   把多链资产（包括 BTC）作为抵押品统一管理。
        
    -   风险引擎和清算逻辑集中在 ZetaChain，上层只暴露一个统一借贷入口。
        
    -   借出的可以是某种 Universal stablecoin，在任意链使用。
        
3.  **LSDFi & Restaking 聚合**
    
    -   参考官方关于“把 EigenLayer 做成 Omnichain dApp”的思路：
        
        -   把 ETH、BTC 甚至其他链的资产都纳入同一套 Restaking 市场。
            
        -   对不同 AVS / 模块做统一的参数、收益分配和 Slash 规则管理。
            
4.  **通用收益管理 / Vault（金库）**
    
    -   参考 Amana 这类 Universal Yield Aggregator：
        
        -   在一个入口存入资产，由协议决定跨链去哪里挖矿 / 借贷 / 做策略，
            
        -   用户只关心一个统一的收益 Token。
            
5.  **通用衍生品 & 杠杆工具**
    
    -   如 SubstanceX 这样的 Universal Perps：用 native BTC 做抵押，在多链上开合约头寸。
        
6.  **跨链支付 / 身份 / 账户抽象**
    
    -   例如：用户只用 BTC 钱包，就能直接发起向以太坊上发送 USDC 的操作。
        
    -   对 DeFi 来说，可以做“任意链支付保证金 / 还款 / 清算”等统一入口。
        

**总结一句：**  
ZRC-20 负责“把多链资产拉到一个可编排的控制面”，  
Universal Token / NFT 负责“让协议内部票据、份额、身份在多链上流动”，  
Swap + Messaging + Omnichain Contracts 则是「通用 DeFi 的操作系统」。

## 三、Idea 收敛：2 个可用于黑客松的通用 DeFi 项目

下面是按作业整理的 2 个 idea，每个都包含：目标用户 / 要解决的问题 / 粗略跨链与通用资产设计。

### Idea 1：Omnichain LSD 收益 & Restaking 路由器

> 简单描述：  
> 一个「多链 LSD 收益 & Restaking 聚合器」，让用户在任意链存入 LSD / 质押资产，由协议自动跨链选择最佳策略（包括 EigenLayer 风格 restaking、BTC 收益策略等），并用一个 Universal Token 表示用户份额。

1）目标用户

-   持有 LSD / 质押资产的用户（例如 stETH、rETH、各链的 LST、甚至 BTC）；
    
-   想参与 LSDFi / Restaking，但不想自己：
    
    -   研究各个链的策略、
        
    -   承担复杂的跨链、gas 操作、
        
    -   手动再平衡头寸。
        

2）想解决的问题

-   LSDFi 协议分散在多条链上，收益、风险、锁仓期都不同；
    
-   用户要在不同协议 / 链之间频繁迁移；
    
-   Restaking（类似 EigenLayer）进一步提高复杂度：
    
    -   需要理解 AVS 风险；
        
    -   需要管理 Slash 风险与收益权拆分。
        

**本协议的价值：**

-   提供一个「统一入口 + 风险偏好选择」；
    
-   协议自动跨链路由到各个 LSD / Restaking 策略；
    
-   用户只持有一个代表份额的 Universal Token（比如 `uLSD`）。
    

3）ZetaChain 上的通用资产 & 跨链设计

大致组件设计：

1.  **底层资产接入（ZRC-20 层）**
    
    -   用户在各个链把 LSD（如 stETH on Ethereum、LST on L2）或 BTC 等资产存入对应链上的 Gateway / Connector；
        
    -   ZetaChain 协议为这些资产铸造对应的 ZRC-20（如 ZRC-20 stETH、ZRC-20 BTC）。
        
2.  **Omnichain 策略合约（ZetaChain EVM）**
    
    -   合约持有一篮子 LSD 类 ZRC-20；
        
    -   根据预设策略 / 管理员策略 / DAO 策略：
        
        -   通过跨链调用把一部分 ZRC-20 发送到指定链，
            
        -   在该链对应的 Restaking / 借贷 / LP 协议中做存款、复投等操作（可通过 messaging + 该链上的 helper 合约实现）。
            
    -   可以参考官方关于“在 ZetaChain 上实现 EigenLayer 式 Restaking”的思路：Omnichain 合约统一负责质押、再质押、奖励分发，不同链用户通过各自入口参与。
        
3.  **协议份额表示（Universal Token）**
    
    -   在 ZetaChain 定义一个 Universal Token，比如 `uLSD`，表示整个策略金库的份额。
        
    -   用户在任意链存入资产后，ZetaChain 计算折算值并增发 `uLSD`，通过 Universal Token 标准同步到对应链上。
        
    -   用户可以在任意连接链上持有和转移 `uLSD`，甚至在其他 DeFi 里做抵押、LP 等。
        
4.  **收益与退出**
    
    -   收益来源包括：
        
        -   LSD 原始收益；
            
        -   Restaking 额外奖励；
            
        -   可能的激励代币 / farming 收益。
            
    -   周期性在 ZetaChain 合约中进行结算，更新 `uLSD` 的资产支持。
        
    -   用户在任意链销毁 `uLSD`（Universal Token 标准支持多链销毁 /转移），触发：
        
        -   在 ZetaChain 侧减少其份额、
            
        -   通过跨链消息执行对应链上的提现和资产回流，
            
        -   最终把指定资产打到用户选定的目标链地址。
            

**扩展空间：**

-   根据风险偏好切成多个池（保守 / 平衡 / 激进）；
    
-   加入 BTC-LSD 或原生 BTC 收益策略（官方博客已经展示了如何用 ZetaChain 做 BTC dApp / staking）。
    

### Idea 2：Omnichain 抵押借贷 & 统一清算引擎

> 简单描述：  
> 一个「多链抵押 → 多链借款 → 统一清算」的借贷协议：用户在任何链抵押多种资产（包括 BTC），在自己想要的链上借出资产；协议在 ZetaChain 上集中计算抵押率和清算，真正实现“跨链版 Aave / Maker”。

1）目标用户

-   持有多链资产的 DeFi 用户（BTC、ETH、稳定币、某些长尾资产），想要：
    
    -   在不桥资产的前提下获得流动性；
        
    -   把多链资产当作一个统一资产池来管理。
        
-   做量化 / 做市 / 套利的专业用户：
    
    -   需要“多链统一授信额度”，方便快速在某条链开仓。
        

2）想解决的问题

-   现有借贷大多是「单链视角」：
    
    -   在 ETH 抵押的资产，只能在 ETH 里借；
        
    -   在 L2 或其他链的资产不能与 ETH 侧资产统一计入抵押能力。
        
-   用户多链资产分散，综合利用率不高；
    
-   多协议多链清算机制割裂，风险管理困难。
    

**本协议的价值：**

-   引入一个「跨链统一信用账户」的概念；
    
-   用户可以用多个链、多个资产作抵押，统一计算健康度；
    
-   随时选择在任意支持的链上借出资产（例如通用稳定币）。
    

3）ZetaChain 上的通用资产 & 跨链设计

组件设计：

1.  **多链抵押接入（ZRC-20 + Connector）**
    
    -   用户在任意链把可抵押资产（BTC、ETH、主流稳定币等）存入该链的借贷入口合约；
        
    -   ZetaChain 协议为其铸造对应 ZRC-20，并记录“用户 → 资产 → 抵押数额”。
        
2.  **统一风控 & 账户系统（Omnichain Lending 合约）**
    
    -   在 ZetaChain 上维护一个全局「信用账户」：
        
        -   每个用户的所有 ZRC-20 抵押资产都在这里计价；
            
        -   按资产种类定义 LTV / 清算阈值；
            
        -   使用跨链预言机价格（这里需要额外的喂价模块）。
            
    -   这种模式非常适合用 Omnichain Smart Contracts 来集中管理逻辑和状态。
        
3.  **借出资产的形态：Universal Stablecoin**
    
    -   在 ZetaChain 上发行一个 Universal Token 形式的稳定币（例如 `uUSD`），作为协议内部贷款资产；
        
    -   用户在任意链发起“借款”：
        
        -   ZetaChain 合约检查其全局健康度；
            
        -   若通过，则在 ZetaChain 铸造对应数量的 `uUSD`，并通过 Universal Token 标准把这部分 `uUSD` 转移到目标链的用户地址。
            
4.  **清算逻辑（统一触发，多链执行）**
    
    -   当用户健康度低于阈值：
        
        -   ZetaChain 合约触发清算流程：在内部挑选某些抵押资产（优先高流动性高 LTV 的资产），
            
        -   通过 Swap 逻辑把相应的 ZRC-20 换成 `uUSD` 或其他结算资产，
            
        -   或者通过 Messaging 对外链清算合约发送指令，在外链局部实现卖出/偿还。
            
    -   清算后的多余价值，可以返还给用户；不足部分则标记为坏账（可引入保险池 / 风险基金）。
        
5.  **用户体验层面**
    
    -   前端只需要给用户展示一个「全链统一抵押仓位面板」；
        
    -   用户看到的是：
        
        -   总抵押价值（折合 USD）；
            
        -   已借出总额；
            
        -   全局 LTV / Health Factor；
            
    -   用户在任何链操作，底层都是 ZetaChain 的 Omnichain 合约在维护同一套账户数据。
        

**扩展方向：**

-   在 `uUSD` 之上进一步支持杠杆挖矿 / 跨链 LP；
    
-   引入 Portfolio Margin：对冲仓位降低风险权重；
    
-   引入 DAO 决定各资产 LTV、利率曲线与激励发放。
    

## 四、小结：

1.  **概念层面已基本清晰：**
    
    -   ZRC-20 是外链资产的可编排影子；
        
    -   Universal Token / NFT 是协议“份额 / 票据 / 权益”的链无关表达；
        
    -   Omnichain 合约 + Swap + Messaging 是实现通用 DeFi 的“控制平面”。
        
2.  **黑客松方向：**
    
    -   一个偏 **LSDFi + Restaking 聚合** 的通用收益协议；
        
    -   一个偏 **多链抵押借贷 + 统一清算** 的信用账户协议。
        
3.  **下一步？**
    
    -   选定其中一个 idea，画出简单架构图：
        
        -   组件：ZetaChain 合约、各链 Connector、ZRC-20、Universal Token；
            
        -   流程：存入、策略执行、收益结算、清算、提现。
            
    -   对照官方 example-contracts 仓库里现成的跨链 Swap、ERC-20 / NFT 例子，找出可以直接复用或改造的模板。
<!-- DAILY_CHECKIN_2025-11-30_END -->

# 2025-11-29
<!-- DAILY_CHECKIN_2025-11-29_START -->







**Day 6 学习笔记：**

## 一、一次调用完成跨链 DeFi

这一天的核心是：**从一个链发起一次交易，让 ZetaChain 在背后帮我们完成跨链 Swap**。

官方 Swap 教程里的场景是：  
在 Base Sepolia 上发起一笔交易，把 ETH 打进 ZetaChain 上的一个 Universal App（`Swap` 合约），它在链上帮我们：

1.  接收从 Base 过来的资产（作为 ZRC-20 代币）；
    
2.  在 ZetaChain 上用 Uniswap v2 池做兑换；
    
3.  把兑换后的资产再次“提”到另一条链（比如 Ethereum Sepolia），在那边以原生资产的形式到账。
    

这一整套流程对用户来说只是一笔交易，但实际上经历了：**Base → ZetaChain → Ethereum** 三条链的联动。

## 二、我选择的 Demo：官方 Swap Universal App

这次实践我选择了官方文档里的 **Swap 教程**，而不是 Messaging：

-   Swap 教程本身就展示了一个典型的 **Universal DeFi 动作**：跨链兑换 + 出金。
    
-   它使用的是官方的 `UniversalContract` 接口，代码可以在 `example-contracts` 仓库的 `examples` 目录中找到对应示例。
    

Swap 合约的几个关键特性：

-   合约部署在 **ZetaChain** 上，属于 Universal App。
    
-   所有从其他链过来的资产，在 ZetaChain 上都表现为 **ZRC-20**。
    
-   对外暴露一个统一入口函数 `onCall(MessageContext context, address zrc20, uint256 amount, bytes message)`，只能由 Gateway 调用。
    

## 三、环境准备 & 关键命令记录

### 1\. 初始化项目

根据官方教程，我用 ZetaChain CLI 创建了一个 Swap 项目：

```
# 1. 初始化 Swap 项目
zetachain new --project swap

cd swap

# 2. 安装 JS 依赖
yarn

# 3. 拉取 Solidity 依赖并编译
forge soldeer update
forge build
```

这几步会：

-   生成一个包含 Universal App 模板代码的项目；
    
-   帮你配置好 Foundry、ZetaChain CLI 的基础结构；
    
-   编译出可以部署到 ZetaChain Testnet 的 Swap 合约。
    

### 2\. 部署到 ZetaChain Testnet

官方提供了现成的 `commands deploy` 脚本，我按教程用一个测试私钥来部署：

```
# 事先设置一个 EVM 测试私钥
export PRIVATE_KEY=0x...  # 只用于测试网

# 部署 Universal Swap 合约到 ZetaChain Testnet
UNIVERSAL=$(
  npx tsx commands deploy --private-key $PRIVATE_KEY \
  | jq -r .contractAddress
)
echo "Universal app address on ZetaChain: $UNIVERSAL"
```

这里的关键配置项：

-   `PRIVATE_KEY`：
    
    -   EVM 测试私钥，对应的地址会作为部署者和之后的 `RECIPIENT` 地址来源。
        
-   `UNIVERSAL`：
    
    -   部署到 ZetaChain Testnet 的 **Swap Universal App 地址**，后面所有跨链调用都会把资产和 payload 发给它。
        

### 3\. 查询目标资产的 ZRC-20 地址 & 发送方地址

Swap 合约需要知道“我要最终在目标链上拿到什么代币”，在教程中用的是 **Ethereum Sepolia 上的 ETH**，对应 ZetaChain 上的 ZRC-20：

```
# 从私钥推导出 EVM 地址，作为最终在 Ethereum Sepolia 收款的地址
RECIPIENT=$(cast wallet address $PRIVATE_KEY)
echo "Recipient on Ethereum Sepolia: $RECIPIENT"

# 查询 ZetaChain 上代表 Ethereum Sepolia ETH 的 ZRC-20 地址
ZRC20_ETHEREUM_ETH=$(
  zetachain q tokens show --symbol sETH.SEPOLIA -f zrc20
)
echo "ZRC-20 for ETH on Ethereum Sepolia: $ZRC20_ETHEREUM_ETH"
```

## 四、从调用视角看整个跨链 Swap 流程

### 1\. 调用是从哪里发起的？

这次我按照教程里 **“Swap from Base to Ethereum”** 的路径来跑：

> **发起点：本地终端 + Zetachain CLI，对 Base Sepolia（chain id 84532）的 Gateway 合约发起** `depositAndCall`**。**

命令如下：

```
npx zetachain evm deposit-and-call \
  --chain-id 84532 \
  --amount 0.001 \
  --types address bytes bool \
  --receiver $UNIVERSAL \
  --values $ZRC20_ETHEREUM_ETH $RECIPIENT true
```

这条命令做了几件事：

-   `--chain-id 84532`：指定在 **Base Sepolia** 上发交易；
    
-   `--amount 0.001`：从 Base Sepolia 发 0.001 ETH 作为输入资产；
    
-   `--receiver $UNIVERSAL`：ZetaChain 上 Swap 合约地址，资产会在 ZetaChain 上打给它；
    
-   `--types address bytes bool` & `--values ...`：  
    用 ABI 编码生成 `message`，对应 Swap 合约里 `onCall` 的第三个参数：
    
    -   `address targetToken` → `$ZRC20_ETHEREUM_ETH`：我要最终得到的目标 ZRC-20（Ethereum Sepolia ETH）；
        
    -   `bytes recipient` → `$RECIPIENT` 的 bytes 形式：最终在 Ethereum Sepolia 收款的地址；
        
    -   `bool withdrawFlag` → `true`：告诉合约这次要 **提到外部链**，而不是留在 ZetaChain。
        

所以，从工程师视角，这次调用是：

> **从本地命令行，使用** `npx zetachain evm deposit-and-call`**，向 Base Sepolia 上的 GatewayEVM 发起一次 “存入 + 调用” 的跨链操作**。

## 五、在 ZetaChain 上到底发生了什么？

可以分为 **“进来”** 和 **“出去”** 两部分来看。

### 1\. 进来：Base → ZetaChain（Incoming Transaction）

1.  我在 Base Sepolia 上调用 GatewayEVM 的 `depositAndCall`，转入 0.001 ETH，并附带 payload。
    
2.  Base 上的 GatewayEVM 合约：
    
    -   锁定 / 管理这 0.001 ETH；
        
    -   发出一个事件，描述这次跨链请求。
        
3.  ZetaChain 的 Observer-Signer validators 监听到这个事件，收集交易数据并在 ZetaChain 上对这次跨链请求进行投票（CrossChain 模块维护 CCTX 状态）。
    
4.  多数投票通过后，ZetaChain：
    
    -   记录一个新的 CCTX；
        
    -   在 ZetaChain 上 **铸造等值的 ZRC-20 Base ETH**（代表这 0.001 ETH）；
        
    -   调用我部署的 `Swap` Universal App 的 `onCall` 函数，并把：
        
        -   `zrc20` 设置为 ZRC-20 Base ETH 合约地址；
            
        -   `amount` 设置为 0.001 ETH 对应的 ZRC-20 数量；
            
        -   `message` 设置成前面通过 `--types/--values` 编码的 payload。
            

### 2\. 合约内部：Swap Universal App 的逻辑

在 `onCall` 里，大致发生了这些事：

1.  **解码 message payload**：
    
    ```
    (address targetToken, bytes memory recipient, bool withdrawFlag) =
        abi.decode(message, (address, bytes, bool));
    ```
    
    对应我们传入的：`targetToken = ZRC20_ETHEREUM_ETH`，`recipient = RECIPIENT`，`withdrawFlag = true`。
    
2.  **查询目标链提取需要的 gas**：
    
    ```
    (address gasZRC20, uint256 gasFee) = IZRC20(targetToken).withdrawGasFee();
    ```
    
    -   `gasZRC20`：在 ZetaChain 上代表 **目标链 gas 代币** 的 ZRC-20（例如 Ethereum Sepolia 的 gas 代币）；
        
    -   `gasFee`：把资产从 ZetaChain 提到目标链一次所需的 gas 数量。
        
3.  **检查用户输入是否够支付 gas + 兑换需要**：
    
    ```
    uint256 minInput = quoteMinInput(inputToken, targetToken);
    if (amount < minInput) revert InsufficientAmount(...);
    ```
    
    合约利用 Uniswap v2 的报价判断：当前的输入金额是否足够支付目标链 gas + 兑换损耗。
    
4.  **从输入资产中“切一块出来”换 gas**：
    
    如果当前的输入资产 `inputToken` 不是 `gasZRC20`，就先在 ZetaChain 用 Uniswap v2 换出足够的 gas 代币：
    
    ```
    inputForGas = SwapHelperLib.swapTokensForExactTokens(
      uniswapRouter, inputToken, gasFee, gasZRC20, amount
    );
    ```
    
5.  **剩余部分全部兑换成目标资产**：
    
    ```
    out = SwapHelperLib.swapExactTokensForTokens(
      uniswapRouter,
      inputToken,
      amount - inputForGas,
      targetToken,
      0
    );
    ```
    
    这里 `out` 就是这次 Swap 在 ZetaChain 上得到的目标 ZRC-20 数量。
    
6.  **授权 GatewayZEVM，发起 withdraw**：
    
    接下来合约会：
    
    -   先 `approve` 网关合约可以花费：
        
        -   `gasZRC20` 的 `gasFee`；
            
        -   `targetToken` 的 `out` 数量（或者两者合并授权，如果 gas 代币和目标代币是同一个）。
            
    -   再调用 ZetaChain 上的 `GatewayZEVM.withdraw`，把这些 ZRC-20 **“燃烧”掉，并指定目标链和收款地址**：
        
        ```
        gateway.withdraw(
          abi.encodePacked(recipient),  // 通用 bytes 地址
          out,                          // 提取的目标资产数量
          targetToken,                  // 要提取的 ZRC-20
          revertOptions                 // 出错时的回退策略
        );
        ```
        

### 3\. 出去：ZetaChain → Ethereum（Outgoing Transaction）

当 `GatewayZEVM.withdraw` 被调用后，ZetaChain 又会走一遍“出站”流程：

1.  **在 ZetaChain 内部创建 Outbound 请求**：  
    CrossChain 模块更新 CCTX 状态，从 `PendingInbound` 变为 `PendingOutbound`；记录目标链（Ethereum Sepolia）、接收地址、资产数量等。
    
2.  **Validator 通过 TSS 共同签名出站交易**：  
    一部分观察者兼签名者验证参数无误后，用 Threshold Signature Scheme 生成一笔在 Ethereum Sepolia 上的链外交易签名。
    
3.  **把交易广播到 Ethereum Sepolia**：  
    出站交易在 Ethereum Sepolia 上被打包、执行：
    
    -   对应的托管合约释放/铸造 ETH；
        
    -   把 ETH 发送到 `RECIPIENT` 地址。
        
4.  **更新 CCTX 状态为** `OutboundMined`：  
    最终，CCTX 被标记为完成，这一整次跨链 Swap 结束。  
    在文档示例中，可以通过 `zetachain query cctx --hash ...` 查看从 Base → ZetaChain → Ethereum 的完整 CCTX 轨迹。
    

如果出站过程中失败（比如目标链交易失败），`onRevert` 和 `RevertOptions` 会触发回退逻辑，按合约的预设方式 refund。

## 六、作业

### 1\. 你是从哪里发起的调用？

-   **调用入口**：本地终端（命令行）。
    
-   **使用工具**：`npx zetachain evm deposit-and-call`。
    
-   **真正被调用的链上对象**：
    
    -   位于 **Base Sepolia（chain id 84532）** 的 `GatewayEVM` 合约的 `depositAndCall` 函数。
        

换句话说，我在本地敲了一条 CLI 命令，但那条命令实质上 **在 Base Sepolia 上发起了一笔跨链请求**。

### 2\. 最终在 ZetaChain 上发生了什么？

概括一下，在 ZetaChain 上的关键步骤是：

1.  **铸造 ZRC-20**：  
    ZetaChain 检测到 Base 上的存入事件后，为这 0.001 ETH 铸造了 ZRC-20 版的 Base ETH。
    
2.  **调用 Universal Swap 合约的** `onCall`：  
    把铸造的 ZRC-20 + payload 传给 Swap 合约，触发跨链 Swap 的业务逻辑。
    
3.  **在 ZetaChain 上完成兑换 & Gas 处理**：
    
    -   用一部分输入资产换出目标链的 gas 代币；
        
    -   用剩余部分在 ZetaChain 的 Uniswap v2 池中兑换成目标 ZRC-20（Ethereum Sepolia ETH）。
        
4.  **调用 GatewayZEVM 发起 withdraw**：  
    授权并调用 `GatewayZEVM.withdraw`，把目标 ZRC-20 燃烧，并携带收款地址（`RECIPIENT`）发起出站交易。
    
5.  **跨链出站完成后更新 CCTX 状态**：  
    Validator 使用 TSS 在 Ethereum Sepolia 上执行转账，成功后 CCTX 被标记为完成，一个“Base → ZetaChain → Ethereum”的跨链 Swap 结束。
    

## 七、个人理解与收获

1.  **Universal App 把复杂度聚合到 ZetaChain**  
    作为开发者，我只需要在 ZetaChain 上写一个 `Swap` 合约，实现一次 Swap 的业务逻辑；所有跨链的存入 / 提取、资产映射、签名、安全，都由协议层完成。
    
2.  **资产统一视图：ZRC-20 是关键抽象**  
    无论资产来自 Base、Ethereum 还是 Solana，只要到了 ZetaChain 上，就统一变成 ZRC-20，合约只需要和 ZRC-20 + Gateway 协作，就能同时支持多条链。
    
3.  **一次交易 ≈ 多链协同的封装**  
    对用户来说，只是一次在 Base 上的交易；  
    对协议来说，是一整条 CCTX 的生命周期，从 **Incoming → 在 ZetaChain 执行 → Outgoing** 的完整过程。这种抽象正是 “Universal DeFi” 的体验基础。
<!-- DAILY_CHECKIN_2025-11-29_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->








### Day 5 学习笔记：

## 一、今天要搞清楚的三个核心概念

1.  **ZRC-20**：
    
    -   ZetaChain 上的一种 _ERC-20 兼容_ 代币标准。
        
    -   用来在 ZetaChain 上“镜像”各条链上的原生资产（BTC、ETH、USDT 等），让合约可以在一个地方统一编排多链资产。
        
2.  **Universal Assets（通用资产）**：
    
    -   包括 **Universal Token（通用 ERC-20）** 和 **Universal NFT（通用 ERC-721）**。
        
    -   核心：一种“真正跨链”的代币 / NFT 标准，本身就可以在任意连接链之间 _铸造 & 转移_，而不是被“桥接成包装资产（wrapped）”。
        
3.  **多链资产在 ZetaChain 上的统一表示**：
    
    -   外部链上的原生资产 → 存入 ZetaChain 时被锁在 TSS 地址 / 托管合约里 → 在 ZetaChain 上铸造出对应的 ZRC-20。
        
    -   对 ZetaChain 上的合约来说，它们只需要操作本地的 ZRC-20，就像操作普通 ERC-20 一样，就能“间接控制”外部链上的真实资产。
        

## 二、ZRC-20 是什么？如何工作的？

### 1\. 高层定义

一句话概括：

> ZRC-20 是集成进 ZetaChain 全链智能合约平台的代币标准，使 dApp 可以在一个地方编排任意连接链上的原生资产，就好像这些资产都在同一条链上一样。

可以代表的资产包括：

-   各链的 **原生 Gas 资产**（如 BTC、ETH）
    
-   **白名单 ERC-20**（USDT、USDC 等）
    

### 2\. 存入（Deposit）流程

当用户把某条链上的资产存入 ZetaChain 时：

1.  用户在连接链（例如 Ethereum）上发送资产到 **TSS 地址 / ERC-20 托管合约**。
    
2.  这些资产在该链上被锁定。
    
3.  ZetaChain 协议在 ZetaChain 上 **铸造等量的 ZRC-20 代币**，发给指定地址。
    

> 结果：ZRC-20 变成了“这笔外部资产在 ZetaChain 上的代表”。

### 3\. 提取（Withdraw）流程

当用户要从 ZetaChain 提回资产到原链时：

1.  在 ZetaChain 上 **销毁（burn）对应数量的 ZRC-20**。
    
2.  协议从 TSS 地址 / 托管合约中，把等量的原生资产转给目标链上的接收地址。
    

### 4\. 一些关键细节

-   **只允许协议铸造**：  
    ZRC-20 只能由 ZetaChain 协议铸造；  
    普通开发者在 ZetaChain 上部署的 ERC-20，_即使接口一样，也不具备 ZRC-20 的跨链属性_，不能从 ZetaChain 提到其它链。
    
-   **同一 ERC-20 在不同链上会对应不同的 ZRC-20**：  
    例如：
    
    -   Ethereum 上的 USDT → ZRC-20 USDT(ETH)
        
    -   BSC 上的 USDT → ZRC-20 USDT(BSC)  
        在 ZetaChain 看来这两类 ZRC-20 是不同资产，但可以在 ZetaChain 内部通过兑换实现“跨链换链”。
        
-   **从开发者视角**：
    
    -   On-chain 接口：仍是熟悉的 `balanceOf`, `transfer` 等 ERC-20 接口。
        
    -   Off-chain 语义：多了“背后锁着某条链的真实资产”的语义。
        

## 三、ZRC-20 vs ERC-20

### 1\. 直观思维模型

-   **普通 ERC-20**：
    
    > “只属于这一条链的记账单位”
    
-   **ZRC-20**：
    
    > “某条外部链原生资产，在 ZetaChain 上的镜像凭证”
    

### 2\. 维度对比表

| 维度 | 普通 ERC-20 | ZRC-20 |
| --- | --- | --- |
| 部署位置 | 某一条 EVM 链（以太坊、L2…） | 仅部署在 ZetaChain 上 |
| 发行主体 | 合约拥有 mint 权限的账户（项目方） | 仅限 ZetaChain 协议 自身，根据跨链存取逻辑铸造 / 销毁 |
| 资产来源 | 该链的“内部资产”，不会自然指向外部某条链 | 映射某条连接链上的 原生 Gas 或 ERC-20 资产（资产真实锁在 TSS / 托管合约中） |
| 跨链能力 | 原生不支持，需要桥 / 消息通道，自行设计 | 内建跨链语义：通过 deposit / withdraw 把外部链资产“搬进 / 搬出” ZetaChain，配合 Gateway 可以进一步跨链调用合约 |
| 接口兼容性 | 标准 ERC-20 接口 | 接口上是 ERC-20 扩展，在 ZetaChain 文档中也被描述为 ERC-20 的扩展标准 |
| 使用场景 | 单链 DeFi、DAO、支付等 | Omnichain DEX、跨链借贷、跨链资产组合管理等，需要统一编排多条链的原生资产 |

### 3\. 开发视角的“手感”差异

-   写普通 ERC-20：
    
    -   只考虑本链的 `totalSupply`，转账就是同一条链上的账户间记账变化。
        
-   写使用 ZRC-20 的合约：
    
    -   同样调用 `transfer`/`balanceOf`，但是你要意识到：
        
        -   `balanceOf` 背后对应的是“被锁在其他链 TSS 地址中的真实资产”。
            
        -   一次 `withdraw` 最终会触发 **外部链上真实资产的转账**，而不是简单的本链记账。
            

## 四、Universal Assets（Universal Token / Universal NFT）

> 关键词：**资产本身就是跨链的**，而不是靠额外桥接。

### 1\. 总体概念

-   Universal NFT + Universal Token 这两个标准的共同点是：
    
    -   允许 **在任意连接链上铸造**
        
    -   允许在链与链之间 **无包装（no wrapping）、无桥代币（no wrapped token）地直接转移**
        
-   文档里把它们统称为 **“assets”**，方便统一讨论。
    

跨链转移的逻辑：

1.  在源链上 **销毁（burn）** 该资产；
    
2.  将资产的元数据 + 信息通过消息发给目标链上的资产合约；
    
3.  在目标链 **铸造（mint）** 对应资产。
    

> 这是一种“单一逻辑、多条链实体”的设计。

### 2\. Universal / Connected 合约结构

每个 Universal 资产项目由两类合约组成：

1.  **Universal 合约（部署在 ZetaChain）**
    
    -   在 ZetaChain 上铸造资产
        
    -   负责 ZetaChain ↔ 各连接链的资产转移
        
    -   处理“链 A → 链 B”的中枢逻辑
        
2.  **Connected 合约（部署在一条或多条 EVM 链上）**
    
    -   在对应 EVM 链上铸造资产
        
    -   处理从该链发出的跨链转移请求
        
    -   处理来自 ZetaChain 或其他连接链的资产转入
        

> 整体拓扑：**ZetaChain 作为 Hub，各 EVM 链作为 Spoke**。  
> 例如 Ethereum → ZetaChain → BNB，两步跨链，但对用户来说时延 / 费用上被抽象得比较平滑。

### 3\. Universal Token（通用 ERC-20）

-   定义：
    
    > 一种 **完全互操作的 ERC-20**，可以在任意连接链之间铸造和转移，无需 wrapped token / 传统桥。
    
-   特性：
    
    -   **供应 & 元数据在多链间保持一致**（chain-agnostic fungibility）。
        
    -   基于标准 OpenZeppelin ERC-20 + UUPS 可升级代理模式，方便未来升级逻辑。
        
-   对开发者来说：
    
    -   基本接口仍是 ERC-20；
        
    -   通过官方 `@zetachain/standard-contracts` 继承 UniversalTokenCore，就可以给现有 ERC-20 增加跨链能力。
        

### 4\. Universal NFT（通用 ERC-721）

-   定义：
    
    > 一种 **可在任意连接链铸造 & 转移的 NFT 标准（ERC-721）**，同样无需 wrapped NFT。
    
-   核心特性：
    
    -   每个 NFT 拥有一个 **跨链唯一且持久的 token ID**，在各链间转移不会改变这个 ID。
        
    -   元数据在跨链过程中会被完整保留。
        
-   场景：
    
    -   跨链游戏资产
        
    -   跨链身份 / 徽章 / 会员 NFT
        
    -   多链市场（同一 NFT 可在不同链的市场自由流动）
        

## 五、ZetaChain 上“多链资产统一表示”的整体视图

从“资产视角”来看，ZetaChain 提供了两层抽象：

### 1\. ZRC-20：外部原生资产的“镜像层”

-   任何连接链的 **原生 Gas / ERC-20** → 通过存入流程变成 ZRC-20。
    
-   合约只需要处理 ZRC-20，就能统一编排多个链上的真实资产。
    
-   适合的理解是：
    
    > “我有一堆真实资产散落在各条链上，ZRC-20 帮我在 ZetaChain 上建立了统一的余额视图，并提供跨链出入金能力。”
    

### 2\. Universal Token / NFT：真正意义上的“全链资产层”

-   这时资产本身就是跨链存在的：
    
    -   Token / NFT 可以在任何支持的链被铸造。
        
    -   持有人把它从 A 链转到 B 链，本质是 **burn + mint**，逻辑上仍然是“同一份资产”。
        
-   它解决的问题：
    
    > 不同链上出现一堆“有点像同一个东西”的 wrapped 资产（liquid-staked token、bridged token），容易导致流动性碎片、价格偏离。Universal 资产把这些合为“一个逻辑资产，多链存在”。
    

## 六、通用资产的应用场景示例

### 场景 1：跨链储蓄 / 收益聚合器（基于 Universal Token）

**设想：**

-   产品发行一个 **Universal Token：uSAVE**，代表用户在“全链储蓄池”中的份额。
    
-   用户可以在任意支持链（比如 Ethereum、Base、Polygon）存入 USDC：
    
    -   存入后，在对应链上铸造 / 增发 uSAVE（Universal Token）。
        
    -   uSAVE 总供应 & 元数据跨链统一。
        

**在后台：**

-   ZetaChain 上的 Universal App 调度策略：
    
    -   一部分资金部署到以太坊上的借贷协议；
        
    -   一部分资金部署到 L2 上更高收益的仓位；
        
    -   甚至可以通过 ZRC-20 控制非智能合约链的资产（如 BTC）参与某些收益策略。
        

**对用户体验：**

-   无需关心资金最后在哪条链上耕作；
    
-   只要持有 uSAVE，就共享整个多链策略池的收益；
    
-   换链时，只需把 uSAVE 从 A 链转到 B 链（Universal Token 的跨链转移），整个过程无 wrapped 资产。
    

### 场景 2：通用 NFT 通行证（Universal NFT）

**设想一个“Web3 会员 / 身份 NFT Pass”：**

-   项目发一个 **Universal NFT** 系列：`UniversalPass`。
    
-   用户可以在任意链铸造这张 NFT（例如在 Polygon 铸造，Gas 便宜）。
    

**使用方式：**

-   以后如果某个高级 DeFi 协议只在 Ethereum 上部署，  
    用户可以把 `UniversalPass` 从 Polygon 转到 Ethereum，而 NFT 的 `tokenId` 和元数据都保持不变。
    
-   不同链的前端 / 协议只要认同 `UniversalPass` 合约，即可统一识别用户的会员身份，无需为每条链单独发一个“类似但不完全一样”的 NFT。
    

**价值：**

-   用户身份与权益真正跨链，而不是“这条链一张会员卡，那条链再发一张近似的卡”。
    
-   项目方运营也更简单：
    
    -   所有统计、空投、治理都围绕“单一 NFT 系列”进行，只是它的持有人分布在不同链上。
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->









## 笔记Day4

**目标：**

1.  搞清楚什么是 **Universal App / 全链应用合约**。
    
2.  搭好后面 Hello World Demo 的心智模型：
    
    -   合约在 ZetaChain 上
        
    -   前端在浏览器
        
    -   调用通过连接链上的 Gateway + RPC 完成（例如 Arbitrum / Base 等测试网）。
        

可以把它想象成：

> 「所有链（ETH、BNB、BTC…）都只是**客户端**，真正的业务逻辑集中在 ZetaChain 上那一个 Universal App 合约。」

* * *

## 2\. 什么是 Universal App（全链应用）？

### 2.1 官方定义（翻译 + 压缩版）

-   **Universal App = 部署在 ZetaChain 的智能合约**，但它**天然连着别的链**（Ethereum、BNB、Bitcoin 等）。
    
-   和普通合约不同，它可以：
    
    -   接收来自任意连接链的：**合约调用 + 消息 + 代币转账**
        
    -   反过来再发起：**跨链合约调用 + 代币转账**。
        
-   Universal App 部署在 **Universal EVM** 上，这个 EVM 在普通 EVM 之上扩展了跨链互操作能力。
    

### 2.2 关键组件

1.  **onCall 函数：跨链入口**
    
    智能合约：
    
    ```
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
    
    -   所有从其它链（例如 Base Sepolia）经由 Gateway 过来的调用，最终都会进入 `onCall`。
        
    -   `context` 告诉你：源链 ID、调用者地址等。
        
    -   `zrc20` + `amount` 代表从源链带来的 Token（映射为 ZRC-20）。
        
    -   `message` 是任意 ABI 编码的数据（Hello 例子里是一个字符串）。
        
2.  **ZRC-20：统一表示跨链资产**
    
    -   每个连接链上的原生 gas / ERC-20，在 ZetaChain 上都有一个对应的 **ZRC-20**。
        
    -   例如：
        
        -   Ethereum 上的 ETH ↔ ZetaChain 上的 `ZRC-20 ETH`
            
        -   这些 ZRC-20 代币可以随时“提现”回原链（ZRC-20 ETH → 以太坊上的 ETH）。
            
3.  **Gateway：所有链进入 Universal App 的“门”**
    
    -   每条连接链上有一个 **Gateway 合约**，专门负责：
        
        -   接收用户 / 合约的调用、消息、代币
            
        -   转发到 ZetaChain 上指定的 Universal App。
            
    -   调用路径是：
        
        > 源链 EVM 合约 / 用户 → 源链 Gateway → ZetaChain Universal App →（可选）再往其它链发射调用
        
4.  **对 Bitcoin 的支持（很重要的卖点）**
    
    -   BTC 用户可以直接往 Gateway 地址 **发送一笔 BTC + data**，就完成对 Universal App 的调用。
        
    -   只需要**一笔 Bitcoin 交易**，无需在其他链开钱包、准备 gas（由 Universal App 帮忙处理 gas，称为 gas 抽象）。
        

### 2.3 心智模型一句话总结

> **Universal App = 一个部署在 ZetaChain 的“总控合约”。  
> 所有其他链只是入口（Gateway），资产都映射成 ZRC-20，业务逻辑集中在 onCall 里。**

* * *

## 3\. Hello World Universal App 教程拆解

### 3.1 效果

官方的 _First Universal Contract_ 教程里，你会完成：

-   编写一个 Universal 合约，当收到跨链调用时：
    
    -   从 `message` 里解码一个字符串
        
    -   `emit HelloEvent("Hello: ", name)`
        
-   在 **Localnet 或 Testnet** 上部署该合约。
    
-   从一个连接链（本地 EVM / Base Sepolia）通过 Gateway 发起跨链调用，看到 ZetaChain 上的事件日志。
    

### 3.2 环境与工具

1.  **ZetaChain CLI + Foundry 是官方推荐组合**
    
    从教程和 Intro 文档：
    
    -   CLI：创建项目、启动 Localnet、发起跨链调用、查询 CCTX。
        
    -   Foundry：`forge build` 编译、`forge create` 部署、`cast` 管理私钥等。
        
    
    初始化 Hello 项目：
    
    ```
    npx zetachain@latest new --project hello
    cd hello
    yarn
    forge soldeer update
    ```
    
2.  **Localnet：本地多链实验室**
    
    -   `npx zetachain localnet start` 可以在本机拉起：ZetaChain + 多条链（EVM、Solana、Sui、TON），以及预部署好的 Gateway、ZRC-20、Uniswap 池等。
        
    -   可以理解为 “ZetaChain on ”：
        
        -   快速调试跨链逻辑
            
        -   不花真实测试币
            
        -   一条命令就能重置环境
            
3.  **Testnet：对接真实公共测试网**
    
    -   教程中使用 ZetaChain testnet（Athens）+ Base Sepolia 等测试网。
        
    -   通过公共 RPC + Gateway 合约地址完成跨链调用，整体流程更接近真实生产环境。
        

### 3.3 合约侧的逻辑

核心逻辑：

-   `Universal` 合约实现 `UniversalContract` 接口，必须实现 `onCall`。
    
-   `onCall` 收到参数：
    
    -   `context.chainID`：源链 ID
        
    -   `context.sender`：源链调用 Gateway 的地址
        
    -   `zrc20`：源链资产映射
        
    -   `amount`：资产数量
        
    -   `message`：ABI 编码的业务参数
        
-   Hello 示例里只做一件事：
    
    -   `string name = abi.decode(message, (string));`
        
    -   `emit HelloEvent("Hello: ", name);`
        

> 心智模型：**这是一个“跨链 printf”** —— 从任何链发一个字符串，它在 ZetaChain 上打印出 `Hello: XXX` 事件。

### 3.4 调用

大致步骤：

1.  启动 Localnet
    
    ```
    npx zetachain localnet start
    ```
    
2.  `forge build` 编译合约。
    
3.  从本地 EVM（Anvil）拿一个预置私钥 + 测试币。
    
4.  使用 `forge create` 把 Universal 合约部署到 ZetaChain。
    
5.  找到连接链（如本地 ETH）的 Gateway 地址。
    
6.  用 CLI 从连接链调用 Universal：
    
    ```
    npx zetachain evm call \
      --rpc http://localhost:8545 \
      --gateway $GATEWAY_EVM \
      --receiver $UNIVERSAL \
      --private-key $PRIVATE_KEY \
      --types string \
      --values hello
    ```
    
7.  在 Localnet 终端里看到 `[ZetaChain]: Event from onCall` 的日志（说明 Universal App 收到了跨链消息）。
    

Testnet 版本则是类似的 `npx zetachain evm call --chain-id 84532 ...`，只是把 RPC、链 ID 换成公共测试网。

* * *

## 4\. 前端 + RPC 心智模型

在 **Build a Web App** 教程里，会在 Hello 合约之上加一层 React 前端：

### 4.1 前端

1.  连接一个 EVM 测试网钱包（例如 Arbitrum Sepolia）。
    
2.  使用 ZetaChain Toolkit 的 `evmCall` 发送跨链调用到 Gateway。
    
3.  记录源链的交易 hash。
    
4.  轮询 ZetaChain 的 CCTX API，拿到 ZetaChain 上执行的交易 hash。
    
5.  在 UI 中展示：
    
    -   源链 explorer 链接
        
    -   ZetaChain explorer 链接
        

> 于是你有了一个**完整的跨链调用闭环**：用户在前端点一下按钮 → 源链发送交易 → ZetaChain 执行 → 用户看到两个链上的结果。

### 4.2 RPC / 网络角色拆分

可以简单地把“后端 + RPC”想象为三类东西：

1.  **源链 RPC**（例如 Arbitrum Sepolia 的 RPC）
    
    -   负责发送用户签名的交易（调用 Gateway）。
        
2.  **ZetaChain RPC / API**
    
    -   负责查询 CCTX 状态、ZetaChain 上的交易执行情况。
        
3.  **Gateway 合约地址 / 协议 Contracts**
    
    -   写死在前端配置中，或通过 CLI / Docs 查到
        
    -   统一入口，前端不会直接连 ZetaChain 的 Universal 合约，而是先打到 Gateway，再由协议转发到 Universal App。
        

* * *

## 5\. 为后续 Swap / Messaging 铺路：Universal App 可以做什么？

在 Hello 之后，官方 Tutorials 会带你做：Swap / Messaging 等更复杂例子。它们本质上只是 **onCall 里面的逻辑变复杂** 而已：

1.  **跨链留言本 / 事件记录**
    
    -   Hello 已经是最小版本：跨链字符串 → 事件。
        
2.  **跨链 Swap（示例思路）**
    
    -   从 Ethereum 接收 ZRC-20 ETH + “我想要 BNB” 的 message
        
    -   在 ZetaChain 上用 DEX 把 ZRC-20 ETH 换成 ZRC-20 BNB
        
    -   再通过 Gateway 把 ZRC-20 BNB 提现到 BNB Chain 用户地址。
        
3.  **跨链 NFT / Game / Social**
    
    -   所有状态与规则写在 ZetaChain 的 Universal App 中。
        
    -   不同链的用户只需通过「本链的钱包 + Gateway」，就能玩同一套逻辑。
        

> 心智模型：**你只写一份“主脑合约”，不同链的用户排队来问它问题 / 给它资产。**

* * *

## 6\. 工具 & 工作流选择建议（CLI + Hardhat / Foundry？Localnet 还是 Testnet？）

我觉得需要思考这个问题：

后面的 Hello World / Demo，打算用哪一套工具 + 哪个网络？

来做一一个对比 + 建议吧

### 6.1 开发工具：Hardhat vs Foundry

ZetaChain 官方说明：平台原生支持 Foundry、Hardhat、Slither、Ethers.js 等主流工具，你可以继续用自己熟悉的工作流。

**Hardhat：**

-   优点：
    
    -   JS / TS 生态友好，插件多（与前端、脚本集成方便）。
        
    -   自带 Hardhat Network，本地调试 EVM 很顺手。
        
-   适合：
    
    -   你更熟悉 JS / TS，喜欢用 Hardhat 脚本部署。
        
    -   想和前端工程整合得很紧（比如一套 monorepo）。
        

**Foundry：**

-   优点：
    
    -   更偏 **Solidity 原生**，`forge build / forge test` 很快。
        
    -   CLI 体验统一，和 ZetaChain 官方教程深度绑定（包括 Localnet、`forge create` 等）。
        

### 6.2 网络：Localnet vs Testnet

**Localnet（个人比较推荐）：**

-   优点：
    
    -   本地跑在 Docker / 本机，多链一起启动，极快迭代。
        
    -   不依赖测试网 RPC 稳定性，不用到处找 faucet。
        
    -   可以自由重置环境，适合“乱试”。
        
-   缺点：
    
    -   与真实用户环境有一点差距（没有真实的测试网 Explorer UX）。
        
    -   需要本地环境配置（Docker、Foundry 等）。
        

**Testnet：**

-   优点：
    
    -   完全贴近真实环境：公共 RPC、浏览器（Blockscout / Basescan）、真实延迟。
        
    -   前端 Demo 给别人看时更“像真的”。
        
-   缺点：
    
    -   依赖测试网状态（RPC 慢 / 挂）。
        
    -   流程稍长，跨链确认需要等待时间。
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->










# 学习笔记 Day 3：ZetaChain & Universal Blockchain 核心概念

## 1\. 整体认识：什么是 “Universal Blockchain / Universal EVM”？

-   **ZetaChain 是什么？**  
    ZetaChain 是一个通用的 Layer 1 区块链（Universal L1），用 PoS 共识 + Cosmos SDK + Cosmos EVM 搭起来，本身就是一条可编程的链，但它把“跨链能力”写进了底层共识和 EVM 里。
    
-   它的目标是：
    
    > 让开发者在一条链（ZetaChain）上写合约，就能原生地管理 Bitcoin、以太坊、Solana 等多条链上的资产和状态——也就是 **Universal Blockchain** 的含义。
    

**几个核心术语在 ZetaChain 里的对应：**

-   **Universal Blockchain**：  
    一条“覆盖所有链”的基础设施链。它自己是 L1，但可以与 EVM 链、非 EVM 链、甚至无智能合约的链（如比特币）互通，在这条链上执行的逻辑可以“伸手”操作其他链。
    
-   **Universal EVM（ZetaChain 的 Universal EVM）**：  
    兼容普通 EVM，同时在 EVM 层面直接内置跨链能力（跨链消息、资产映射 ZRC-20 等），专门用来开发 **Universal Apps**。
    
-   **Omnichain Smart Contract / Universal Contract**：  
    部署在 ZetaChain Universal EVM 上的智能合约，一份合约就可以：
    
    -   接收来自任意已连接区块链的消息 / 代币；
        
    -   在任意连接链上发起合约调用或资产转移；
        
    -   像“跨链总控室”一样协调多链操作。
        

* * *

## 2\. 用自己的话：Universal App 是什么？

> **“一份合约，管所有链的事。”**

官方的定义：Universal App 是部署在 ZetaChain 的 Universal EVM 上、原生连接任意其他区块链（包含比特币、EVM 链、非 EVM 链等）的智能合约应用。

**我自己的理解：**

1.  **部署位置**：
    
    -   只部署在 **ZetaChain 上**（Universal EVM）。
        
    -   外围链上不需要布置一堆“同构合约副本”，最多只需要 Gateway 等网关合约。
        
2.  **能干什么**：
    
    -   接收来自任意连接链的：
        
        -   合约调用 / 消息；
            
        -   代币存入（例如 BTC、ETH、USDC 等）。
            
    -   再从 ZetaChain 这边发起：
        
        -   对任意链上合约的调用；
            
        -   跨链转账、跨链 swap 等。
            
3.  **体验上的效果**：
    
    -   一个比特币用户，只在 BTC 钱包里签一笔交易，就能通过某个 universal app 把 BTC 换成以太坊上的 USDC，自己甚至不需要有以太坊地址里的 gas。
        
4.  **和传统多链 dApp 的差别：**
    
    | 传统多链 dApp | Universal App（在 ZetaChain 上） |
    | --- | --- |
    | 每条链部署一套合约，逻辑分散、状态分裂 | 一份合约在 ZetaChain，统一持久状态 |
    | 跨链 usually 依赖桥 + 外部消息网络 | 跨链能力写在链里，由 ZetaChain 共识保证 |
    | 复杂的桥配置、跨链安全面多点、开发维护重 | 开发者只维护 ZetaChain 这套逻辑，跨链由基础设施兜底 |
    

**一句话记忆：**

> Universal App 就是“部署在 ZetaChain 上的全链 dApp”，它有多链读写能力和多链资产总控能力，用户可以在任意连接链上用自己熟悉的钱包和资产，与它交互。

* * *

## 3\. 自己的理解：Gateway 大概做什么？

官方文档：

> Gateway 是一个接口，为连接链上的合约和 ZetaChain 上的 universal apps 提供统一的入口（unified entry point）。

**我自己的理解：**

可以把 Gateway 想成 **“每条外部公链上的 ZetaChain 总服务台 / 前台”**，它主要负责：

1.  **统一入口（Entry Point）**
    
    -   在每条连接链上只有一个 Gateway 合约，所有“想跟 ZetaChain 通话”的用户或合约，都要通过它。
        
    -   用户在以太坊、BSC、Polygon、甚至比特币网络上，都只需要和本链的 Gateway 交互，不需要直接“找上”ZetaChain。
        
2.  **消息 + 资产的“打包、转交、拆包”**
    
    -   当用户在某条链上调用 Gateway 时，可以同时附带：
        
        -   代币（比如 1 ETH、100 USDC）；
            
        -   数据 payload（比如 “帮我把这个 USDC 转到 BNB 链上的某个地址”）。
            
    -   Gateway 负责把这两者封装成一条跨链“任务”，交给 ZetaChain。
        
    -   等 ZetaChain 上的 Universal App 执行完逻辑后，提现 / 释放资产时，也会通过目标链的 Gateway 把结果落地到对应链上。
        
3.  **对开发者的意义**
    
    -   开发者只需要面对一个统一接口：
        
        -   “从本链 → ZetaChain → 其他链” 都是同一套调用模式。
            
    -   安全性、跨链路由、观察者 + TSS 签名这些麻烦事，交给 ZetaChain 底层和 Gateway 处理。
        

**一句话记忆：**

> Gateway 是每条外部链通往 ZetaChain 的“跨链网关合约”，负责统一入口 + 代币托管 + 消息转发，是 Universal App 对外的门面。

* * *

## 4\. ZetaChain & Universal App & Gateway 的关系

从开发视角看，可以这样抽象：

1.  **底层链：ZetaChain（Universal L1 + Universal EVM）**
    
    -   负责共识、跨链消息、安全、执行 universal contracts。
        
2.  **应用层：Universal Apps**
    
    -   部署在 Universal EVM 上，写一次合约，面向所有连接链用户。
        
3.  **接入层：Gateway（每条外部链一个）**
    
    -   外部链的所有调用，都要先进 Gateway，再由 ZetaChain 节点网络观察、共识，最后路由到 Universal App。
        

可以对比成一个“总行 + 各地支行”的结构：

-   ZetaChain = 总行，总账 + 风控 + 产品逻辑；
    
-   Universal App = 总行的具体业务系统；
    
-   各链 Gateway = 各地支行的业务窗口，负责接待用户、收取现金（代币）、把指令送回总行，执行完以后再给用户出账。
    

* * *

## 5\. 简易架构图

作业：**ZetaChain 中心 + Bitcoin / Ethereum / Solana 等外围链 + Gateway**：  

```
                [ Bitcoin 链 ]
                      |
                  (BTC Gateway)
                      |
    [ Ethereum 链 ] --+           
          |           |           
   (ETH Gateway)      |
                      v
         +-------------------------+
         |       ZetaChain         |
         |   (Universal Blockchain |
         |        + Universal EVM) |
         +-------------------------+
                      ^
                      |
            [ Universal Apps ]
   (Omnichain / Universal Contracts)
         |         |          |
         |         |          |
   跨链 DEX   跨链 NFT   跨链支付等


                [ Solana 链 ]
                      |
                 (Solana Gateway)
                      |
                      +----→ 连接到 ZetaChain
```

第二张图（感觉更能体现和任意链家湖？）：

```
         Any Chain A        Any Chain B        Any Chain C
          (BTC/ETH/...)     (Solana/TON/...)   (L2/其他)
               \                 |                /
                \   [ Gateways on each chain ]  /
                 \           |                 /
                  +----------v----------------+
                  |        ZetaChain         |
                  |  Universal EVM + Apps    |
                  +--------------------------+
```
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->











Day 2 学习笔记：环境与工具实战（ZetaChain + Qwen）

——————————————  
0\. 今日学习目标 & Checklist  
——————————————

今日目标：

1.  在本地或云端把 ZetaChain 开发环境（CLI 为主）跑起来（安装、简单命令）。
    
2.  搞清楚 ZetaChain 测试网的 RPC / Faucet / Explorer 入口并记录。
    
3.  在终端或 Postman 里完成一次 Qwen API 请求（成功返回或报错都可以，打通连通性最重要）。
    

——————————————

1.内容

1.1 技术栈范围

链：ZetaChain（Localnet + Testnet）  
工具：ZetaChain CLI、ZetaChain Explorer、Faucet 等  
模型：Qwen 系列（通过 OpenAI 兼容接口调用）

1.2 能力目标

（1）环境与基础

-   能在本地或云端搭好 ZetaChain CLI 环境。
    
-   能连接测试网，执行基础查询 / 交互命令。
    

（2）跨链与 Universal Apps 概念

-   理解 ZetaChain 是一个基于 Cosmos SDK、兼容 EVM 的 L1 区块链。
    
-   理解 Universal EVM 的基本概念（跨链交易记录、状态更新等）。
    

（3）Qwen API 使用

-   熟练用 OpenAI 兼容方式调用 Qwen（HTTP / Python SDK / Postman）。
    
-   知道如何选择不同的 Qwen 模型（如通用对话、代码模型、长文档模型等）。
    

——————————————  
2\. ZetaChain 快速回顾  
——————————————

2.1 ZetaChain 是什么？

ZetaChain 是一个基于 Cosmos SDK 的 L1 区块链，同时兼容 EVM。  
核心卖点是跨链 / 通用（universal / omnichain）：  
通过 Universal EVM，合约可以原生处理来自多个链的资产与消息，管理跨链交易状态。

2.2 开发者入口

官方文档仓库：[https://github.com/zeta-chain/docs](https://github.com/zeta-chain/docs)  
在线文档入口：[https://www.zetachain.com/docs/](https://www.zetachain.com/docs/) （包含 developers、reference、教程等）

——————————————  
3\. ZetaChain CLI：安装与基础使用  
——————————————

3.1 CLI 简介

“zetachain” 是官方提供的 CLI 工具（npm 包名同为 zetachain）。  
主要能力包括：

-   创建和管理 ZetaChain universal app 项目；
    
-   与 ZetaChain 网络交互（查询链、启动本地环境等）；
    
-   在多链（EVM / Solana / Bitcoin / Sui / TON 等）上进行操作（依场景与版本而定）。
    

可以把它看作 ZetaChain 开发者的开路利器。

3.2 前置依赖（本地）

常见依赖（以 Node 版 CLI 为主）：

-   Node.js（建议 18 及以上）
    
-   npm 或 yarn
    
-   Git
    
-   可选：  
    · Docker（用于 Localnet 等多链环境）；  
    · Foundry / 其他 EVM 开发工具。
    

3.3 安装方式

3.3.1 临时使用（npx），适合快速试用：

npx zetachain@latest --help  
或  
npx zetachain@latest new

无需全局安装，适合一次性试验。

3.3.2 全局安装：

npm install -g zetachain

安装完成后可以通过以下命令验证：

zetachain --help  
zetachain --version

3.4 基础命令示例

实际命令以当前 CLI 帮助信息 “zetachain --help” 为准，这里只写出典型交互思路。

查看 CLI 帮助：

zetachain --help

（如果支持）列出可用链：

zetachain query chains list

创建新项目（示例）：

zetachain new my-universal-app

建议你在笔记中记录自己实际运行的命令和输出摘要：

命令：zetachain query chains list  
输出摘要：

-   ...  
    结论：CLI 已正常连接网络 / 有报错信息 ...
    

——————————————  
4\. ZetaChain 测试网：RPC / Faucet / Explorer  
——————————————

4.1 测试网网络参数（Network Details）

ZetaChain Testnet（EVM 侧）常用参数：

Chain ID (EVM)：7000  
Chain ID (Cosmos)：zetachain\_7000-1  
Denom：azeta（链上最小单位）  
Symbol：ZETA  
Decimals：18  
HD Path：m/44'/60'/0'/0/0  
Bech32 prefix：zeta  
Explorer：[https://zetascan.com](https://zetascan.com)  
EVM RPC：[https://zetachain-evm.blockpi.network/v1/rpc/public](https://zetachain-evm.blockpi.network/v1/rpc/public)

你可以在钱包（如 MetaMask）里这样配置测试网：

Network Name: ZetaChain Testnet  
RPC URL: [https://zetachain-evm.blockpi.network/v1/rpc/public](https://zetachain-evm.blockpi.network/v1/rpc/public)  
Chain ID: 7000  
Currency: ZETA  
Block Explorer:[https://zetascan.com](https://zetascan.com)

4.2 测试网 RPC / API

公共 EVM RPC 和其他公共端点会在官方 RPC/API Endpoints 文档中列出。  
公共 RPC 更适合钱包或前端使用，高频 / 后端场景建议使用专用私有节点。

4.3 Faucet（测试币）

ZETA 用途：在 ZetaChain 上作为 gas，用于部署和调用合约、发起交易。  
测试网 ZETA 无任何货币价值，仅用于测试开发。

常见获取方式（ZETA）：

-   Google Cloud Web3 Faucet
    
-   FaucetMe：[https://zetachain.faucetme.pro](https://zetachain.faucetme.pro)
    

4.4 Explorer：ZetaScan

官方推荐的区块浏览器：[https://zetascan.com](https://zetascan.com)

功能：

-   查看区块、交易、合约、地址余额等；
    
-   支持主网 / 测试网切换。
    

——————————————  
5\. Qwen API：OpenAI 兼容调用实战  
——————————————

5.1 整体认知

Qwen（通义千问）是阿里云提供的大模型系列，可在 Model Studio / 阿里云百炼 中使用。

官方提供两种主要调用协议：

-   OpenAI 兼容协议
    
-   DashScope 原生协议
    

Day 2 重点放在 OpenAI 兼容：

只要替换 API Key、base\_url 和 model，基本可以无缝迁移原本的 OpenAI 代码。

5.2 获取 API Key 与环境变量

官方推荐步骤：

-   在阿里云百炼控制台创建 API Key；
    
-   把 Key 配置到环境变量中，例如：
    

export DASHSCOPE\_API\_KEY="sk-xxxx"

后续所有调用（curl / Python / Postman）都通过  
Authorization: Bearer $DASHSCOPE\_API\_KEY  
来使用，避免把 Key 写死在代码里。

5.3 在终端用 curl 完成一次 Qwen 请求（作业核心）

以官方 OpenAI 兼容 ChatCompletion 接口为例。

请求地址：

POST [https://dashscope.aliyuncs.com/compatible-mode/v1/chat/completions](https://dashscope.aliyuncs.com/compatible-mode/v1/chat/completions)

示例命令：

export DASHSCOPE\_API\_KEY="sk-xxxx" # 若已写入 shell 配置文件可忽略

curl --location '[https://dashscope.aliyuncs.com/compatible-mode/v1/chat/completions](https://dashscope.aliyuncs.com/compatible-mode/v1/chat/completions)'  
\--header "Authorization: Bearer $DASHSCOPE\_API\_KEY"  
\--header "Content-Type: application/json"  
\--data '{  
"model": "qwen-plus",  
"messages": \[  
{ "role": "system", "content": "You are a helpful assistant." },  
{ "role": "user", "content": "用一句话介绍一下 ZetaChain 是什么？" }  
\]  
}'

如果成功，返回结构会与 OpenAI 的 ChatCompletion 类似（含 choices\[0\].message.content）。  
即便是 401 / 403 等错误，也说明「网络连通 + 认证路径」是通的，只是权限或 Key 有问题，对 Day 2 作业来说依然有价值。

5.4 使用 Python（OpenAI SDK）调用 Qwen（兼容模式）

根据官方 OpenAI 兼容文档，可以使用最新版 openai SDK 直接指向 DashScope 端点：

from openai import OpenAI  
import os

def call\_qwen():  
client = OpenAI(  
api\_key=os.getenv("DASHSCOPE\_API\_KEY"),  
base\_url="[https://dashscope.aliyuncs.com/compatible-mode/v1](https://dashscope.aliyuncs.com/compatible-mode/v1)",  
)

```
completion = client.chat.completions.create(  
    model="qwen-plus",  # 也可使用其他受支持的 Qwen 模型  
    messages=[  
        {"role": "system", "content": "You are a helpful assistant."},  
        {"role": "user", "content": "帮我用一句话解释 ZetaChain 的核心特性。"},  
    ],  
)  

print(completion.choices[0].message.content)  
```

if **name** == "**main**":  
call\_qwen()

要点回顾：

-   api\_key 使用阿里云百炼的 Key；
    
-   base\_url 必须改为 [https://dashscope.aliyuncs.com/compatible-mode/v1](https://dashscope.aliyuncs.com/compatible-mode/v1) （或相应地域的 URL）；
    
-   其余参数、返回结构与 OpenAI ChatCompletion 基本一致。
    

5.5 使用 Postman 调用 Qwen 的参考配置

新建 Request：

-   Method：POST
    
-   URL：[https://dashscope.aliyuncs.com/compatible-mode/v1/chat/completions](https://dashscope.aliyuncs.com/compatible-mode/v1/chat/completions)
    

Headers：

Authorization: Bearer YOUR\_DASHSCOPE\_API\_KEY  
Content-Type: application/json

Body（raw + JSON）示例：

{  
"model": "qwen-plus",  
"messages": \[  
{ "role": "system", "content": "You are a helpful assistant." },  
{ "role": "user", "content": "你好，请简单介绍下 ZetaChain。" }  
\]  
}

观察返回：

-   正常：状态码 200，返回 choices；
    
-   异常：401（Key 问题）、429（限流）、4xx（参数错误）等。
    

5.6 常见错误与自查 Checklist

401 / invalid\_api\_key：

-   检查 API Key 是否正确，是否拷贝多了空格；
    
-   检查 Authorization 头格式是否为 Bearer <API\_KEY>。
    

403 / 权限不足：

-   检查对应地域、模型是否已在控制台开通。
    

4xx / Content-Type 或参数错误：

-   确认 Content-Type: application/json；
    
-   检查 model 名称是否为官方文档列出的合法值。
    

网络问题：

-   用浏览器或 ping / curl -v 检查是否被网络环境阻断。
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->












它有一个叫 **Universal EVM** 的东西，本质是「带跨链能力的 EVM」，你在这条链上写一个合约，就可以从这一个逻辑中心去操作多条链（包括以太坊、BNB、Solana 甚至比特币）的资产和合约，是一个专门为跨链 / 全链应用准备的开发环境

第二，它提供了 **Universal Assets 标准**，包括 Universal NFT 和 Universal Token，你可以按它给的标准直接做「可以在多链间铸造和转移的 NFT / ERC-20 代币」，不需要自己重新设计跨链协议。

第三，它列出了 **Connected Chains + Tutorials**：告诉你目前支持哪些链（EVM、Solana、Sui、TON、Bitcoin 等）以及可以对这些链做哪些操作，同时给了一组循序渐进的教程（Getting Started、First Universal Contract、Build a Web App、Swap、Messaging 等），让你可以从「跑通第一个通用合约」一路做到「实现一个跨多链的 Swap / 消息调用应用」。

简单来做个总结吧：**Developers 这页 = ZetaChain 上做跨链应用的入口导航：Universal EVM（怎么写）、Universal Assets（用什么标准发资产）、Connected Chains + Tutorials（能连谁、怎么一步步做出来）**
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
