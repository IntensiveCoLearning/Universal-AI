---
timezone: UTC+8
---

# Jintol

**GitHub ID:** JintolChan

**Telegram:** @JintolOfficial

## Self-introduction

区块链技术爱好者

## Notes

<!-- Content_START -->
# 2025-12-03
<!-- DAILY_CHECKIN_2025-12-03_START -->
# 设计parse\_swap\_intent(text)

```
import re
from typing import Optional, Dict


SUPPORTED_CHAINS = {
    "ethereum": ["ethereum", "eth mainnet", "mainnet"],
    "base": ["base"],
    "arbitrum": ["arbitrum", "arb"],
    "optimism": ["optimism", "op"],
    "polygon": ["polygon", "matic"],
    "bsc": ["bsc", "bnb chain", "binance smart chain"],
    "linea": ["linea"],
    "scroll": ["scroll"],
    "zksync": ["zksync", "zk sync"],
}

SUPPORTED_TOKENS = [
    "USDC",
    "USDT",
    "ETH",
    "WETH",
    "BTC",
    "WBTC",
    "DAI",
]


def _detect_chain(text: str) -> Optional[str]:
    lower = text.lower()
    for chain_id, aliases in SUPPORTED_CHAINS.items():
        for alias in aliases:
            if alias in lower:
                return chain_id
    return None


def _find_token_positions(text: str) -> Dict[str, int]:
    lower = text.lower()
    positions = {}
    for symbol in SUPPORTED_TOKENS:
        idx = lower.find(symbol.lower())
        if idx != -1:
            positions[symbol] = idx
    return positions


def _detect_tokens(text: str) -> (Optional[str], Optional[str]):
    """
    Heuristic:
    - First mentioned token -> tokenIn
    - Second mentioned token -> tokenOut
    """
    positions = _find_token_positions(text)
    if not positions:
        return None, None

    # sort by position
    sorted_tokens = sorted(positions.items(), key=lambda x: x[1])
    if len(sorted_tokens) == 1:
        token_in = sorted_tokens[0][0]
        token_out = None
    else:
        token_in = sorted_tokens[0][0]
        token_out = sorted_tokens[1][0]

    return token_in, token_out


def _detect_amount(text: str, token_in: Optional[str]) -> Optional[str]:
    # try to find "<number> <tokenIn>" or "<number><tokenIn>"
    if token_in:
        pattern = rf"(\d+(\.\d+)?)\s*{re.escape(token_in)}"
        m = re.search(pattern, text, flags=re.IGNORECASE)
        if m:
            return m.group(1)

    # fallback: first number in text
    m = re.search(r"(\d+(\.\d+)?)", text)
    if m:
        return m.group(1)

    return None


def parse_swap_intent(text: str) -> Dict[str, Optional[str]]:
    """
    Parse a natural language swap instruction into structured JSON.
    Example:
      "swap 10 USDC to ETH on Base" ->
      {
        "chain": "base",
        "tokenIn": "USDC",
        "tokenOut": "ETH",
        "amount": "10"
      }
    """
    chain = _detect_chain(text)
    token_in, token_out = _detect_tokens(text)
    amount = _detect_amount(text, token_in)

    return {
        "chain": chain,
        "tokenIn": token_in,
        "tokenOut": token_out,
        "amount": amount,
    }
```
<!-- DAILY_CHECKIN_2025-12-03_END -->

# 2025-12-02
<!-- DAILY_CHECKIN_2025-12-02_START -->

# **Qwen‑Agent 框架的核心组成**

| 组件 | 作用 | 关键特性 |
| --- | --- | --- |
| LLM（Large Language Model） | 提供自然语言理解与生成能力，是整个系统的“大脑”。 | - 支持指令跟随（instruction‑following）- 能够在对话中调用工具（tool‑calling）- 具备多轮上下文记忆能力 |
| Agent | 对外的统一入口，负责把用户的自然语言请求转化为可执行的计划并调度 LLM 与 Tools。 | - 接收用户输入 → 交给 LLM 进行意图解析- 根据解析结果生成 Task Plan（任务计划）- 负责调度工具调用、结果聚合以及记忆更新 |
| Tools（工具库） | 实际执行具体功能的模块，形式上是 函数/API，可以是本地函数、外部服务或自定义插件。 | - 支持 单步、并行、多轮 调用- 每个工具都有 schema（参数、返回值）供 LLM 参考- 可动态注册/卸载，灵活扩展 |
| Memory（记忆模块） | 持久化对话上下文、任务状态和中间结果，帮助 Agent 在长对话或复杂任务中保持连贯性。 | - 短期记忆：基于上下文窗口的即时缓存- 长期记忆：向向量数据库或 KV 存储写入关键信息- 自动摘要/检索，防止上下文溢出 |

# **组件之间的工作流**

1.  **用户输入** → 进入 **Agent**。
    
2.  **Agent** 调用 **LLM**，让模型解释意图并生成 **任务计划**（哪些工具需要被调用、调用顺序、所需参数）。
    
3.  根据计划，**Agent** 调度 **Tools**：  
    
    -   单步调用：直接执行并返回结果。
        
    -   并行调用：同时触发多个工具，等待全部完成后聚合。
        
    -   多轮调用：在一次对话中多次往返工具，直至任务完成。
        
4.  每一次工具调用的输入/输出都会写入 **Memory**，供后续步骤检索。
    
5.  完成所有工具调用后，**Agent** 再次让 **LLM** 根据收集到的结果生成最终的自然语言回复给用户。
<!-- DAILY_CHECKIN_2025-12-02_END -->

# 2025-12-01
<!-- DAILY_CHECKIN_2025-12-01_START -->


熟悉 Qwen 的基础参数
<!-- DAILY_CHECKIN_2025-12-01_END -->

# 2025-11-30
<!-- DAILY_CHECKIN_2025-11-30_START -->




# 通用 DeFi 项目 idea

## **通用跨链收益聚合器**

**让用户只需存一次钱即可自动获得全链最佳收益。**

* * *

### **目标用户**

-   持有多链资产的小白
    
-   想获得 DeFi 收益但不懂跨链、桥接、Gas 管理的人
    
-   想降低被桥攻击的风险的用户
    
-   希望用「一个界面」掌握所有链收益机会的投资者
    

* * *

### **想解决的问题**

传统 DeFi 存在明显痛点：

1.  **收益碎片化**  
    每条链都有不同的 LST / LP 池，年化不同。
    
2.  **跨链成本高**  
    需要桥接，安全风险高、Gas 高。
    
3.  **用户难以判断放哪条链收益最高**  
    需要不断监控多个链的 APR。
<!-- DAILY_CHECKIN_2025-11-30_END -->

# 2025-11-29
<!-- DAILY_CHECKIN_2025-11-29_START -->





# Swap实践（测试网）

CLI 创建一个新的 ZetaChain 项目：

```
zetachain new --project swap
```

安装依赖：

```
cd swap
yarn
```

使用 Foundry 的包管理器拉取 Solidity 依赖项：

```
forge soldeer update
```

部署合约：

```
UNIVERSAL=$(npx tsx commands deploy --private-key $PRIVATE_KEY | jq -r .contractAddress) && echo $UNIVERSAL
```

从 Solana 兑换到 Ethereum Sepolia

```
npx zetachain solana deposit-and-call \
  --recipient $UNIVERSAL \
  --types address bytes bool \
  --values $ZRC20_ETHEREUM_ETH $RECIPIENT true \
  --chain-id 901 \
  --private-key $SOLANA_PRIVATE_KEY \
  --amount 0.01
```
<!-- DAILY_CHECKIN_2025-11-29_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->








# **ZRC-20**

ZRC-20 是一种代币标准，集成到 ZetaChain 的 Omnichain 智能合约平台中。借助 ZRC-20，开发者可以构建 dApp，在任何连接的链上协调原生资产。这使得从单一平台构建 Omnichain DeFi 协议和 dApp变得极其简单。

已连接区块链的原生 Gas 代币和已列入白名单的 ERC-20 代币可以存入 ZetaChain，生成 ZRC-20 代币。在存入过程中，原生/ERC-20 代币将被转移并锁定在 TSS 地址/ERC-20 托管合约中，ZetaChain 会铸造 ZRC-20 代币并存入接收地址。

ZRC-20 代币可以从 ZetaChain 提现到连接的区块链。提现过程中，ZRC-20 代币会在 ZetaChain 上销毁，原生/ERC-20 代币会通过 TSS 地址/ERC-20 托管合约转移到连接链上的接收地址。

# ZRC-20 和普通 ERC-20 的直观区别

**部署环境**：ERC‑20 部署在以太坊或其他 EVM 兼容链上；ZRC‑20 部署在 ZetaChain 的全链 Omnichain 环境，天然支持多链互操作。

**跨链能力**：ERC‑20 本身不具备跨链功能，需要使用桥接或包装合约；ZRC‑20 通过内置的接口实现 1:1 跨链映射（如 zBTC、zBNB），无需额外桥。

**原生资产支持**：ZRC‑20 可以直接包装其他链的原生资产（BTC、BSC 等），在 ZetaChain 上表现为原生代币；ERC‑20 只能包装 ERC‑20 代币或通过合约实现包装。

**地址与标识**：ZRC‑20 使用 ZetaChain 的统一地址体系，跨链时保持同一合约地址；ERC‑20 在每条链上都有独立的合约地址，需要映射表维护。

**Gas 费用**：在 ZetaChain 上执行 ZRC‑20 操作的 gas 计费基于 ZetaChain 原生代币（ZETA），而 ERC‑20 操作则使用部署链的原生代币（如 ETH）。

**安全模型**：ZRC‑20 受益于 ZetaChain 的跨链验证机制（Omnichain 验证器），提供更统一的安全审计；ERC‑20 的安全性仅限于单链的合约审计。

# 「通用资产」可能的应用场景

## 跨链身份凭证

利用通用资产作为去中心化身份（DID）令牌，用户在不同链上验证身份、签署合约或领取空投，无需重复 KYC 流程。

通过去中心化身份标识符（Decentralized Identifier，DID）在不同链之间实现统一的身份认证与权限证明，能够在多个区块链网络上安全、可信地共享用户的身份属性。
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->










# **Swap**

Swap合约是一个部署在 ZetaChain 上的通用应用程序。它使用户能够通过一次跨链调用在不同区块链之间进行代币兑换。代币以 ZRC-20 的形式接收，可以选择使用 Uniswap v2 流动性进行兑换，并提取回连接的链。

Swap合约执行以下步骤：

1.  接收来自连接链的跨链调用以及原生代币或 ERC-20 代币。
    
2.  解码消息有效载荷以提取：
    
    -   目标代币（ZRC-20）的地址
        
    -   收件人在目的地链上的地址
        
3.  查询将目标代币发送回目标链所需的提现 gas 费用。
    
4.  如果提现到连接的链，则使用 Uniswap v2 池将一部分传入的代币兑换成 ZRC-20 gas 代币，以支付提现费用。
    
5.  将剩余余额兑换成目标代币。
    
6.  将交换后的代币提取给目标链上的接收者。
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->












# Universal App

universal app是 ZetaChain 上的智能合约，它原生连接到以太坊、BNB 和比特币等其他区块链。

与普通合约不同，universal app可以接收来自任何已连接链的合约调用、消息和代币转移。它还可以触发合约调用，并在已连接链上进行代币转移。这些能力使通用应用能够协调跨链的复杂多步骤交易。

例如，比特币用户可以通过通用应用程序将USDC发送给以太坊上的收款人。以太坊用户可以在ZetaChain上购买NFT，并将其发送到他们在BNB链上的账户。

universal app程序部署在 ZetaChain 的通用 EVM 上，该 EVM 扩展了全链互操作性功能。这意味着您现有的合约可以在 ZetaChain 上开箱即用，并且经过一些修改后，它们可以获得强大的全链功能。

# Gateway

Gateway 是一个接口，它作为 ZetaChain 上连接链上的合约与通用应用程序之间交互的统一入口点。

ZetaChain 上的 Gateway 可促进对外交易：从通用应用程序向连接链上的合约调用和提取代币。

Gateway支持以下功能：

-   将 ZRC-20 代币作为原生 gas 或 ERC-20 代币提取到已连接的链上
    
-   将 ZETA 代币提取到连接的链
    
-   将代币提取到已连接的链上并进行合约调用
    
-   在连接链上进行合约调用
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->














# ZetaChain ZETA 水龙头

[https://cloud.google.com/application/web3/faucet](https://cloud.google.com/application/web3/faucet)

[https://zetachain.faucetme.pro/](https://zetachain.faucetme.pro/)
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->















# 开发环境配置

### 安装node.js

```
sudo apt install nodejs

```

### 安装npm

```
npm install --global npm
```

### 安装Git

```
sudo apt install git
```

### 安装 jq

```

sudo apt install jq
```

### 安装 Foundry

```
curl -L https://foundry.paradigm.xyz | bash
```

### 安装 ZetaChain CLI

```
npm install -g zetachain
zetachain query chains list //尝试运行
```
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
