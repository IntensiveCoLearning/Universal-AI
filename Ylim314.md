---
timezone: UTC+8
---

# ChaplinX

**GitHub ID:** Ylim314

**Telegram:** @Tikey19

## Self-introduction

本科大三区块链工程在读

## Notes

<!-- Content_START -->
# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->
**日期**：2025.11.26 **进度**：完成从 AI 指令到链上 Write 操作的闭环

今天的主要工作是解决“只读不写”的问题。昨天的脚本只能查余额，今天是真枪实弹地发了一笔交易到 ZetaChain 测试网。

![DAY3-1.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Ylim314/images/2025-11-26-1764147412732-DAY3-1.png)

**1\. 环境变量管理** 为了解决私钥硬编码的安全隐患，引入了 python-dotenv 库。创建了 .env 文件来存放 PRIVATE\_KEY，并在代码中通过 os.getenv 加载。这是一个比较标准的工程实践，避免代码上传 GitHub 时泄露私钥。

**2\. 交易构建流程** 实现了一个 send\_transaction 函数。这里复习了一下以太坊的交易结构，主要包含 nonce、gas、gasPrice、to、value 和 chainId。 特别注意了 nonce 的获取，用 w3.eth.get\_transaction\_count 拿到当前账户的交易计数，这是为了防止重放攻击。

**3\. 签名与广播** 使用了 [web3.py](http://web3.py) 的 sign\_transaction 方法进行离线签名，然后通过 send\_raw\_transaction 把签名后的二进制数据广播到 RPC 节点。 最后加了一个 wait\_for\_transaction\_receipt，这样脚本不会发完就停，而是会挂起等待区块确认，体验更好。

![DAY3-2.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Ylim314/images/2025-11-26-1764147430928-DAY3-2.png)

**遇到的问题与解决**

-   **私钥混淆问题**：一开始报错 `Non-hexadecimal digit found`，排查后发现是在 .env 里错误地填入了阿里云的 API Key（sk-开头），而不是钱包私钥。修正后读取正常。
    
-   **Chain ID 问题**：构建交易时必须指定 chainId: 7001 (Athens-3)，否则会因为 EIP-155 防重放机制导致交易被节点拒绝。
    
-   **AI 指令边界**：测试“销毁”指令时 AI 报错，通过调整指令为“转账给任意地址”成功绕过，实现了等效操作。
    

**明日计划** 研究 ZRC-20 标准的 Approve 和 Swap 方法，尝试调用 Uniswap 风格的合约接口。
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->

### 通用 AI 残酷共学 - Day 2 学习日志：Agent 账户系统与链上交互

📅 日期： 2025.11.25 ChaplinX

✅ 进度： Module 3 & 4 (AI 意图解析 + Web3 账户集成)

今天完成了 AI Agent 的核心控制层开发，实现了从“自然语言”到“链上模拟执行”的完整链路。

特别是升级了 agent\_[hand.py](http://hand.py)，从单纯的只读查询升级为具备账户管理能力的工程架构。

**1\. 账户管理模块 (**`eth_account` **集成)**

-   **功能：** 引入 `eth_account` 库，替代了硬编码私钥的做法。
    
-   **实现：** 编写 `setup_account(w3)` 函数，支持本地生成临时账户/加载环境变量私钥，为后续的交易签名 (`sign_transaction`) 做好安全准备。
    
-   **代码亮点：** 启用了 `Account.enable_unaudited_hdwallet_features()` 以支持更灵活的钱包操作。
    

**2\. AI 意图 -> 链上执行闭环**

-   **流程：** 用户输入自然语言 -> Qwen 模型解析为 JSON -> Python 脚本校验链上余额 -> 模拟交易构建。
    
-   **优化：** 增加了结构化的日志输出 (`[AI 决策]`, `[网络]`, `[余额]`)，大大提升了调试效率。
    

**🐛 Bug：**[**Web3.py**](http://Web3.py) **的 Checksum 地址校验陷阱**

-   **现象：** 在将 AI 解析出的全小写地址传给 `w3.eth.get_balance` 时，抛出 `InvalidAddress` 异常。
    
-   **技术原理：** EVM 链遵循 EIP-55 标准，要求地址通过大小写混合来实现校验和，防止输入错误。Python 的 `web3` 库默认开启此严格检查。
    
-   **修复代码：**
    
    Python
    
    ```
    # 在查询前强制清洗地址格式
    checksum_address = w3.to_checksum_address(raw_address)
    balance = w3.eth.get_balance(checksum_address)
    ```
    

💡 工程心得

-   **类型安全：** 区块链开发中，Address 类型（Checksum）和 Amount 类型（Wei/Ether转换）的处理必须极其严谨，AI 的输出往往是不稳定的，必须在中间层做强校验。
    
-   **安全意识：** 哪怕是黑客松代码，也要养成不把 Private Key 硬编码在代码里的习惯，改用 `os.getenv` 或本地生成的临时账户测试。
    

![DAY2-1.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Ylim314/images/2025-11-25-1764058839554-DAY2-1.png)![DAY2-2.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Ylim314/images/2025-11-25-1764058851420-DAY2-2.png)

📅 明日计划

-   **交易上链：** 完成 `build_transaction` 构建真实的 ZRC-20 转账交易。
    
-   **签名发送：** 使用本地账户对交易进行签名 (`w3.eth.account.sign_transaction`) 并广播到测试网。
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->


### 📝 通用 AI 残酷共学 - Day 1 学习日志

📅 日期： 2025.11.24

👤 学员： ChaplinX

✅ 进度： 完成环境配置 (Module 0) / 跑通 AI 意图识别与链上交互原型 (Module 3 & 4)

🚀 核心成果

今天完成了一个 **Read-Only AI Agent** 的最小可行性产品 (MVP)，实现了从“自然语言指令”到“链上状态检查”的完整闭环。

**项目架构：**

1.  **Agent Brain (Python + Qwen-Plus):** 通过 System Prompt 工程，强制 LLM 输出严格的 JSON 格式（Action/Token/Amount/Chain），解决了自然语言到机器语言的非结构化转换问题。
    
2.  **Agent Hand (Web3.py + ZetaChain Athens-3):** 实现了 RPC 连接、地址校验和余额查询逻辑。
    

🛠️ 踩坑与解决方案 (Troubleshooting)

**1\. Web3.py 的 Checksum 地址校验机制**

-   **问题描述：** 在调用 `w3.eth.get_balance(address)` 时报错 `web3.exceptions.InvalidAddress: ...only accepts checksum addresses`。
    
-   **原因分析：** Web3.py 强制要求 EIP-55 格式的地址校验和（即大小写混合），而我直接使用了全小写的测试网地址。这是为了防止转账错误的安全机制，但在开发初期容易被忽略。
    
-   **解决方案：** 在传入任何地址前，必须经过格式化处理：
    
    Python
    
    ```
    # 修复前：直接调用报错
    # balance = w3.eth.get_balance(address)
    
    # 修复后：增加一步转换
    checksum_address = w3.to_checksum_address(address)
    balance = w3.eth.get_balance(checksum_address)
    ```
    

**2\. 多环境下的 RPC 隔离**

-   **问题描述：** 初期 MetaMask 和代码连接的是 ZetaChain Mainnet (ChainID 7000)，导致测试币到账后查不到余额。
    
-   **复盘：** 必须严格区分开发环境。代码中已固化测试网 RPC：`https://zetachain-athens-evm.blockpi.network/v1/rpc/public` (ChainID 7001)。
    

**3\. Python 环境依赖管理**

-   **经验：** 在 VS Code 中必须显式指定解释器路径（如 Anaconda 环境），避免调用到 Windows Store 自带的空壳 Python 导致 `ModuleNotFoundError`。
    

🔮 明日计划

目前 Agent 只能“看”不能“动”。

-   **计划 1：** 生成本地钱包账户 (Account)，导出私钥并安全配置到 `.env`。
    
-   **计划 2：** 实现 `build_transaction` 和 `sign_transaction`，让 AI 真的能发出一笔上链交易。
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
