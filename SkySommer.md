---
timezone: UTC+8
---

# Sky

**GitHub ID:** SkySommer

**Telegram:** @skysommer0317

## Self-introduction

Web3 product manager and engineer, LXDAO designer, AI enthusiast.

## Notes

<!-- Content_START -->
# 2025-12-03
<!-- DAILY_CHECKIN_2025-12-03_START -->
1111
<!-- DAILY_CHECKIN_2025-12-03_END -->

# 2025-12-01
<!-- DAILY_CHECKIN_2025-12-01_START -->

11
<!-- DAILY_CHECKIN_2025-12-01_END -->

# 2025-11-30
<!-- DAILY_CHECKIN_2025-11-30_START -->


11111
<!-- DAILY_CHECKIN_2025-11-30_END -->

# 2025-11-29
<!-- DAILY_CHECKIN_2025-11-29_START -->



## 1\. ZetaChain 开发环境 (Omnichain Toolkit)

### 1.1 核心概念速查表

| 概念 | 说明 | 关键点 |
| ZetaChain CLI | 指 npx zetachain (开发者工具箱)。 | 不同于 节点工具 zetacored。用于编排环境、脚手架生成。 |
| Localnet | 本地全链模拟环境。 | 基于 Docker 运行，包含 ZetaChain + BTC + ETH 模拟节点。 |
| Foundry | 智能合约开发框架。 | 使用 forge 命令。用 Solidity 编写测试和脚本，速度极快。 |
| Account | 账户体系。 | 兼容 EVM (0x...)。同一私钥可推导出 EVM 地址和 Cosmos 地址。 |
| RPC | 远程节点接口。 | 开发应用连接 HTTP RPC (默认端口 8545)。 |
| Faucet | 水龙头 (测试币领取)。 | 渠道：官方 Discord #zeta-faucet 频道。 |
| Explorer | 区块链浏览器。 | 用于查看 Transaction Hash 和跨链状态 (CCTX)。 |

### 1.2 环境搭建步骤 (Localnet)

**前置需求：**

-   Node.js (v18+)
    
-   **Docker** (必须运行，用于启动模拟网)
    
-   **Foundry** (运行 `curl -L https://foundry.paradigm.xyz | bash` 安装)
    

**操作流程：**

1.  **初始化项目 (使用官方模版)**
    
    Bash
    
    ```
    # 使用最新的 Foundry 模版
    npm create zetachain@latest my-hello-world
    cd my-hello-world
    npm install
    ```
    
2.  启动本地网络 (Localnet)
    
    利用 Docker 拉起微缩版宇宙。
    
    Bash
    
    ```
    npx zetachain localnet start
    ```
    
    _成功标志：终端显示 Local EVM、Bitcoin 节点已启动，且不报错。_
    
3.  **编译合约 (Foundry)**
    
    Bash
    
    ```
    forge build
    ```
    

### 1.3 切换至测试网 (Testnet: Athens-3)

当本地测试通过后，需连接公开测试网。

1.  **获取测试币 (Faucet):**
    
    -   加入 [ZetaChain Discord](https://discord.com/invite/zetachain)。
        
    -   在 `zeta-faucet-athens-3` 频道发送：`drip 0x你的钱包地址`。
        
2.  **配置环境:**
    
    -   在项目根目录创建 `.env` 文件。
        
    -   填入私钥：`PRIVATE_KEY=你的私钥（不带0x）`。
        
3.  **常用资源:**
    
    -   **Explorer:** [https://athens.explorer.zetachain.com/](https://athens.explorer.zetachain.com/)
        
    -   **Public RPC:** `https://zetachain-athens-evm.blockpi.network/v1/rpc/public`
        

* * *

## 2\. Qwen (通义千问) API 接入

### 2.1 账号与鉴权

1.  **注册:** 登录 [阿里云 DashScope 控制台](https://dashscope.console.aliyun.com/)。
    
2.  **API Key:** 在“API-KEY管理”中创建并复制 Key (格式如 `sk-xxx`)。
    
3.  **环境配置:**
    
    Bash
    
    ```
    # 安装 Python SDK
    pip install dashscope
    ```
    

### 2.2 基础调用代码 (Python)

创建一个 `test_qwen.py` 文件，测试 API 连通性。

Python

```
import dashscope
from http import HTTPStatus

# 建议将 Key 放入环境变量，此处仅为演示
dashscope.api_key = 'sk-你的API_KEY' 

def call_qwen_agent():
    # 构造消息体
    messages = [
        {'role': 'system', 'content': '你是一个精通区块链开发的 AI 助手。'},
        {'role': 'user', 'content': '请用一句话解释 ZetaChain 的 Localnet 是做什么的？'}
    ]

    # 发起请求 (使用 qwen-turbo 或 qwen-max)
    response = dashscope.Generation.call(
        model='qwen-turbo',
        messages=messages,
        result_format='message', 
    )

    # 简单处理响应
    if response.status_code == HTTPStatus.OK:
        content = response.output.choices[0]['message']['content']
        print(f"✅ Qwen 回复: {content}")
    else:
        print(f"❌ 请求失败: {response.code} - {response.message}")

if __name__ == '__main__':
    call_qwen_agent()
```

### 2.3 关键参数说明

-   **Model:** 推荐开发测试用 `qwen-turbo` (便宜快)，生产环境或复杂逻辑用 `qwen-max`。
    
-   **Messages:** `system` 定义人设，`user` 传入指令。后续做 Agent 时，需通过 prompt engineering 让 Qwen 输出 JSON 格式。
<!-- DAILY_CHECKIN_2025-11-29_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->




111
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->





这两天有点忙，先打个卡，明天一定补笔记
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->






先打个卡，明天来补笔记
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->







今天主要学习了环境配置方面的内容。
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
