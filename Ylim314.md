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
# 2025-12-05
<!-- DAILY_CHECKIN_2025-12-05_START -->
日期：2025.12.05 进度：完成全栈 Demo 联调与演示准备

今天进行了Demo 串联。由于我在前两天已经完成了后端的多代币动态路由，今天的主要工作是将这些能力完整地映射到前端 UI 上，完成“自然语言 -\\> AI -\\> 链上执行 -\\> 状态反馈”的端到端闭环。

工程优化细节：

为了匹配后端的通用能力，我重构了 Streamlit 的 Sidebar 逻辑。之前的 UI 仅硬编码了 ETH 余额查询，无法体现“通用 DeFi”的优势。今天我引入了资产列表遍历逻辑，让侧边栏能够实时拉取 ZETA、zETH、zBTC 和 zBNB 的链上余额。

端到端测试记录：

我进行了一次全链路测试：

![DAY第二周第五天1.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Ylim314/images/2025-12-05-1764937327309-DAY______1.png)

1\. 输入指令：“把 1 个 ZETA 换成 BTC”

2\. 意图层：Qwen 准确识别 Target Token 为 "BTC"。

3\. 路由层：agent\\\_hand 自动映射到 BTC 的 ZRC-20 合约地址。

4\. 执行层：构建 Swap 交易并上链。

5\. 反馈层：等待区块确认后，侧边栏的 zBTC 余额发生变更。
<!-- DAILY_CHECKIN_2025-12-05_END -->

# 2025-12-04
<!-- DAILY_CHECKIN_2025-12-04_START -->

**日期**：2025.12.04 **进度**：完成意图解析层与通用接口层的双重升级

这两天的开发重点在于打破系统的“硬编码”限制，实现一个真正通用的 DeFi Agent。我将任务拆解为两部分并行推进：上游的 AI 意图解析优化，以及下游的 Web3 接口层泛化设计。

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Ylim314/images/2025-12-04-1764855271597-image.png)

在解析层（Brain），我重新设计了 System Prompt 的输出规范。为了支持多币种交易，我不再让 AI 模糊地输出意图，而是强制要求它识别并填充 `token_out` 字段。例如，当用户输入“换成 BTC”时，AI 必须准确提取出 “BTC” 这一符号，并将其标准化为大写。这为下游的执行提供了精准的参数输入，解决了之前只能默认兑换 ETH 的局限性。

在接口层（Hand），我基于解析层的升级对 `agent_hand.py` 进行了彻底的重构。核心改动是引入了 `TOKEN_MAP` 字典配置，将 ETH、BTC、BNB、USDC 等主流 ZRC-20 资产的合约地址与业务逻辑解耦。我改造了 Swap 函数的签名，使其支持传入动态的 `target_symbol` 参数。现在的执行流程变成了一个动态路由系统：AI 解析出目标代币符号 -> 接口层查表获取合约地址 -> 动态构建 Uniswap 交易路径。

通过这两天的迭代，我的 Agent 从一个只能执行固定脚本的 Demo，进化成了一个具备扩展性的通用协议适配器。现在只需在配置文件中增加新的代币地址，系统就能立刻支持新的交易对，而无需修改核心代码逻辑，这才是符合工程标准的开发范式。

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Ylim314/images/2025-12-04-1764855202651-image.png)
<!-- DAILY_CHECKIN_2025-12-04_END -->

# 2025-12-02
<!-- DAILY_CHECKIN_2025-12-02_START -->


**日期**：2025.12.02 **进度**：实现 Agent 的工具调用逻辑

今天的官方任务是学习 Qwen-Agent 框架并挂载工具。我仔细研究了一下官方文档，发现所谓的 Agent 核心其实就是“LLM 决策 + 本地函数执行”。既然我已经用 OpenAI SDK 写好了底层交互，就没有必要为了用框架而用框架，所以我决定继续完善自己手写的这个轻量级 Agent 架构。

重点梳理了项目中的“工具层”（Tools）。在我的架构里，`agent_hand.py` 实际上就是存放所有 Tool 的工具箱。之前写代码时只是把它们当作普通的 Python 函数，今天从 Agent 的视角重新审视，发现它们必须具备清晰的输入输出定义，才能被大模型准确调用。例如 `swap_native_to_token` 这个函数，它接收 amount 作为输入，返回 tx\_hash 作为输出，这就是一个标准的 Tool。

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Ylim314/images/2025-12-02-1764670893655-image.png)

为了让 Brain能驱动 Hand，我在中间层写了一个路由分发逻辑。虽然 Qwen 模型本身支持 Function Calling API，但我发现通过 Prompt Engineering 强制输出 JSON，然后在 Python 侧做 `if action == 'swap': call_swap()` 的显式路由，在工程上反而更可控。这相当于手动实现了一个简单的 ReAct 模式：模型推理意图 -> 输出结构化参数 -> 代码匹配工具 -> 执行并返回结果。

今天把之前零散的逻辑封装得更模块化了一些。相比于直接调用 API，这种“大模型挂载本地工具”的模式才是 Web3 Agent 的精髓。毕竟大模型只懂说话，只有给它装上 `web3.py` 这种“义肢”，它才能真正触碰到链上的资产。明天准备看看怎么优化一下解析逻辑，让它能处理更复杂的 DeFi 参数。
<!-- DAILY_CHECKIN_2025-12-02_END -->

# 2025-12-01
<!-- DAILY_CHECKIN_2025-12-01_START -->



**日期**：2025.12.01 **进度**：Qwen API 实战与意图解析层开发

今天是共学第二周的第一天，重心从链上交互转移到了 AI 智能体的构建上。虽然官方文档主要介绍了 Qwen 的基础调用，但我直接将其应用到了 DeFi 场景的意图识别中。

在技术选型上，我没有使用阿里云繁琐的原生 SDK，而是利用了 Qwen 兼容 OpenAI 协议的特性，直接通过 `openai` Python 库进行调用。这样做的好处是代码复用性极高，未来如果想切其他模型改动很小。模型方面我选择了 `qwen-plus`，在逻辑推理和 JSON 格式遵循上比开源版本更稳定。今天的核心工作是打磨 System Prompt（系统提示词）。为了让 AI 能准确控制区块链交易，我不能让它输出自然语言的废话，必须强制它输出结构化的 JSON。我在代码里明确定义了输出字段：action（操作类型）、token\_in（代币符号）、amount（数量）和 destination\_chain。

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Ylim314/images/2025-12-01-1764589459962-image.png)

调试过程中发现一个难点：当用户输入模糊指令（比如“买点币”）时，模型容易产生幻觉。我通过在 Prompt 里增加约束条件（“如果无法解析则返回 error 字段”），并在 Python 代码层增加了 `json.loads` 的异常捕获逻辑，有效解决了非结构化数据导致程序崩溃的问题。

目前 AI 大脑与 Web3 手脚已经通过这个 JSON 接口完成了对接。明天计划进一步完善 Agent 框架，尝试增加更多的工具调用能力。
<!-- DAILY_CHECKIN_2025-12-01_END -->

# 2025-11-30
<!-- DAILY_CHECKIN_2025-11-30_START -->




日期：2025.11.30

进度：完成 Universal DeFi 核心功能（Swap）开发

今天为了完成“跑通 DeFi Demo”的任务，我没有去跑官方现成的 CLI 脚本，而是选择硬磕，直接把 Swap 逻辑集成到了我的 Python Agent 里。前几天实现的转账只是 Layer 1 的基础操作，而今天的 Swap 才是真正的合约交互。

我主要攻克了 Uniswap V2 协议在 ZetaChain 上的调用方式。这比普通转账要复杂得多，首先需要找到 Athens-3 测试网的 Router 合约地址，并且必须在代码里硬编码对应的 ABI。最坑的地方在于参数构建，调用 `swapExactETHForTokens` 方法时，必须手动指定路由路径（Path），即从 Native ZETA 映射到 WZETA，再到目标 ZRC-20 ETH。如果路径不对，交易会直接 Revert。

在代码实现上，我新增了一个 `swap_native_to_token` 函数。这里有个工程细节要注意：由于涉及合约调用，Gas Limit 不能像普通转账那样给 21000，我把它调高到了 200000 以防止 Out of Gas。同时，为了防止交易长时间 pending 被卡住，我还在参数里计算了 deadline（当前时间戳 + 10分钟）。

![DAY61.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Ylim314/images/2025-11-30-1764498690214-DAY61.png)

测试过程比较顺利，我在前端输入“把 ZETA 换成 ETH”后，后端成功构建了交易并广播。我在侧边栏刷新资产时，明显看到 ZETA 减少而 zETH 余额从 0 变成了有数值的状态，这意味着我的 Agent 已经具备了操作 DEX（去中心化交易所）的能力，真正打通了通用 DeFi 的链路。
<!-- DAILY_CHECKIN_2025-11-30_END -->

# 2025-11-29
<!-- DAILY_CHECKIN_2025-11-29_START -->





**Day 5 学习日志：Web3 Agent 可视化与全流程闭环**

**日期**：2025.11.28 **进度**：完成Web UI 搭建与项目闭环

今天目标是将 Python 后端包装成可视化的 DApp。

**1\. 可视化实现** 使用了 Streamlit 框架搭建了类似 ChatGPT 的交互界面。为了提升专业度，我自定义了 CSS 样式，实现了深色模式的仪表盘布局。左侧实时显示钱包连接状态和链上资产（ZETA/zETH），右侧为 AI 对话窗口。

**2\. 核心功能验证** 在今天的测试中，我重点验证了“自然语言转账”功能。

-   **输入**：“帮我转 1 个 ZETA 给 Bob”
    
-   **执行**：AI 成功解析意图 -> 后端构建交易 -> 签名上链。
    
-   **结果**：通过侧边栏刷新发现，ZETA 余额从 2.99 变更为 1.99，证明链上交互真实成功。
    

![DAY5_1.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Ylim314/images/2025-11-29-1764406053891-DAY5_1.png)

**3\. 架构思考与待优化项** 目前 Agent 对于 Layer 1 的原生转账支持非常完美。但在测试“Swap”指令时，虽然 AI 能解析意图，但由于后端尚未部署 Uniswap Router 的交互逻辑（缺少 Approve 和 Swap 合约调用），导致 zETH 余额未变动。这也是未来优化的重点：引入 DEX SDK，实现真正的链上资产兑换。
<!-- DAILY_CHECKIN_2025-11-29_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->






**Day 4 学习日志：从原生交互进阶到智能合约读取**

日期：2025.11.28

进度：智能合约交互 (Smart Contract Interaction)

今天的主要精力都花在了如何让 Python 脚本与 ZetaChain 上的 ZRC-20 合约进行通信上。前两天虽然跑通了转账，但那只是针对原生 ZETA 代币的 Layer 1 操作，今天才算是真正触碰到了可编程区块链的核心——智能合约交互。

刚开始我觉得读取余额应该很简单，像查原生币一样调用个 API 就行。但实际写代码时才发现，操作 ZRC-20 代币（比如 ETH 合约资产）必须引入 ABI (Application Binary Interface) 的概念。我不得不去翻了 [web3.py](http://web3.py) 的文档，重新构建了一个包含 `balanceOf`、`decimals` 和 `symbol` 的最小化 ABI 列表。这里有个很重要的工程细节：实例化合约对象时，必须同时传入合约地址和 ABI，缺一不可。

在调试过程中，我深刻理解了 `call()` 和 `transact` 的区别。之前做转账用的是发交易，需要签名、消耗 Gas；而今天做的余额查询只是本地节点的状态读取，直接用 `.call()` 方法就能瞬间返回结果，完全不需要私钥签名。这一点对于后续设计 AI Agent 的查询逻辑很重要，因为查询操作是可以无限高频进行的。

今天还踩了一个非常坑的环境依赖 Bug。代码写好后运行一直报错提示找不到 `cytoolz` 模块。我排查了很久，最后发现是 VS Code 的终端环境自动切回了系统默认的 Python，而不是我配置好的 Anaconda 环境。这让我意识到工程开发中环境隔离的重要性，以后必须养成检查 `(base)` 标识的习惯，或者直接在脚本头部强制指定解释器路径。

另外在数据处理上也花了点时间。链上返回的余额是 uint256 格式的 Wei，数值巨大，直接显示给用户看肯定不行。我在代码里加了一步动态获取 `decimals()` 的逻辑，自动进行除法换算，这样不管是 6 位精度的 USDC 还是 18 位精度的 ETH，Agent 都能正确显示人类可读的数字。

![DAY41.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Ylim314/images/2025-11-28-1764335922077-DAY41.png)

目前的进度是已经能让 Agent “看到”合约里的资产了。明天打算引入 Streamlit 框架，赶紧把这个黑底白字的命令行界面换成一个 Web 网页，不然演示起来确实太干涩了
<!-- DAILY_CHECKIN_2025-11-28_END -->

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
