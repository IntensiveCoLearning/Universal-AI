---
timezone: UTC+8
---

# lihua

**GitHub ID:** Free-riding

**Telegram:** @nowhere

## Self-introduction

Again and again ~

## Notes

<!-- Content_START -->
# 2025-12-02
<!-- DAILY_CHECKIN_2025-12-02_START -->
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

只要类用 `@register_tool('xxx')` 装饰，字符串 `'xxx'` 就可以作为工具名出现在 `function_list` 里  
如果模型配置正确、工具定义无误，你会在返回结果中看到 Agent 已经自动选择调用 `to_upper` 工具，并把工具返回的结果整合进自然语言回答。

可以把一次工具调用的流程想象成这样：

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

# 添加API key到环境变量

## 1.linux

### 1.1添加永久性环境变量

执行以下命令来将环境变量设置追加到`~/.bashrc` 文件中。

```
# 用您的百炼API Key代替YOUR_DASHSCOPE_API_KEY
echo "export DASHSCOPE_API_KEY='YOUR_DASHSCOPE_API_KEY'" >> ~/.bashrc
```

执行以下命令，使变更生效。

```bash
source ~/.bashrc
```

重新打开一个终端窗口，运行以下命令检查环境变量是否生效

```bash
echo $DASHSCOPE_API_KEY
```

### 1.2添加临时性环境变量

执行以下命令

```bash
# 用您的百炼API Key代替YOUR_DASHSCOPE_API_KEY
export DASHSCOPE_API_KEY="YOUR_DASHSCOPE_API_KEY"
```

执行以下命令，验证该环境变量是否生效

```bash
echo $DASHSCOPE_API_KEY
```

## [2.Windows](http://2.Windows)

### 2.1添加永久性环境变量

1.在PowerShell(win + R中输入cmd后回车)中运行以下命令

```powershell
# 用您的百炼API Key代替YOUR_DASHSCOPE_API_KEY
[Environment]::SetEnvironmentVariable("DASHSCOPE_API_KEY", "YOUR_DASHSCOPE_API_KEY", [EnvironmentVariableTarget]::User)
```

2.打开一个新的PowerShell窗口。

3.在新的PowerShell窗口运行以下命令，检查环境变量是否生效。

4.echo $env:DASHSCOPE\_API\_KEY

## 2.2使用临时环境变量

如果您仅希望在当前会话中使用该环境变量，可以在PowerShell中运行以下命令。

```powershell
# 用您的百炼API Key代替YOUR_DASHSCOPE_API_KEY
$env:DASHSCOPE_API_KEY = "YOUR_DASHSCOPE_API_KEY"
```

您可以在当前会话运行以下命令检查环境变量是否生效。

```powershell
echo $env:DASHSCOPE_API_KEY
```

# 3.**安装SDK**

您可以在终端运行以下命令：

```shell
npm install --save openai
# 或者
yarn add openai
```

# 4.更改Ubuntu的密码（我忘了自己的密码了）

### **在 Windows 里用管理员权限打开 PowerShell**

1.  按 Win 键，输入 PowerShell
    
2.  右键 **“Windows PowerShell”** → 选 **“以管理员身份运行”**
    

### **2️⃣ 用 root 账户进入你这个叫 Ubuntu 的发行版**

在管理员 PowerShell 里执行：

```
wsl -d Ubuntu -u root
```

-   如果能进到一个类似 root@xxxx:~# 的终端，说明成功了 → 继续看第 3 步
    
-   如果这条命令报错，把**完整输出**复制给我，我再给你针对性的命令（比如 wsl -l -v 看具体名字，或用 ubuntu config --default-user root 等）。
    

### **3️⃣ 在这个发行版里找到你的普通用户名字**

在这个 root 终端里执行：

```
ls /home
```

你会看到一个或多个目录名，比如可能有 lzh，那你的用户名就是 lzh（如果是别的名字，就记下那个）。

### **4️⃣ 给这个用户重置密码**

假设你的用户名是 lzh（如果不是就换成你自己的）：

```
passwd lzh
```

然后按提示输入两遍 **新密码**（输入时不显示任何字符是正常现象，直接输入后按回车）。

看到类似：

```
password updated successfully
```

说明密码修改成功。

### **5️⃣ 退出 root，回到正常用户，再用 sudo**

1.  在 root 终端里退出：
    

```
exit
```

# 调用API key模板

```
import OpenAI from "openai";

const openai = new OpenAI(
    {
        // 若没有配置环境变量，请用百炼API Key将下行替换为：apiKey: "sk-xxx",
        apiKey: process.env.DASHSCOPE_API_KEY,
        baseURL: "https://dashscope.aliyuncs.com/compatible-mode/v1"
    }
);

async function main() {
    const completion = await openai.chat.completions.create({
        model: "qwen-plus",  //此处以qwen-plus为例，可按需更换模型名称。模型列表：https://help.aliyun.com/zh/model-studio/getting-started/models
        messages: [
            { role: "system", content: "You are a helpful assistant." },
            { role: "user", content: "如何学习web3？" }
        ],
    });
    console.log(JSON.stringify(completion))
}

main();
```

NODE运行main函数

```
node main.js
```

发生鉴权错误

API key is invalid

1.可能base\_url未设置

2.可能api key 过期了（我的就是这种情况）

最后输出

```
{"choices":[{"message":{"role":"assistant","content":"学习 Web3 是一个非常有前景的方向，因为它是互联网的下一代演进方向，融合了区块链、去中心化技术、密码学和新型经济模型。以下是一个系统性的学习路径，帮助你从零开始掌握 Web3：\n\n---\n\n### 一、理解 Web3 的核心概念\n\n1. **Web1 → Web2 → Web3 演变**\n   - Web1：只读（静态网页）\n   - Web2：可读可写（社交平台、中心化服务）\n   - Web3：可读可写可拥有（用户拥有数据和资产）\n\n2. **Web3 核心思想**\n   - 去中心化（Decentralization）\n   - 用户主权（Ownership）\n   - 无需信任（Trustless）\n   - 开放性与可组合性（Open & Composable）\n\n3. **关键技术支撑**\n   - 区块链（如 Ethereum、Solana）\n   - 智能合约\n   - 加密钱包（如 MetaMask）\n   - 分布式存储（如 IPFS、Arweave）\n   - 去中心化身份（DID）\n   - DAO（去中心化自治组织）\n\n---\n\n### 二、学习基础技术栈\n\n#### 1. 学习区块链基础知识\n- 了解比特币和以太坊的工作原理\n- 掌握共识机制（PoW、PoS）\n- 理解区块结构、哈希、Merkle 树等\n- 推荐资源：\n  - 《精通比特币》（Andreas Antonopoulos）\n  - 《以太坊白皮书》\n  - Coursera 上的“Bitcoin and Cryptocurrency Technologies”课程\n\n#### 2. 掌握以太坊生态\n- 了解 EVM（以太坊虚拟机）\n- 学习 Gas、交易、账户模型\n- 熟悉主流 Layer2（如 Arbitrum、Optimism）\n- 工具：Etherscan、Remix、Alchemy、Infura\n\n#### 3. 学习智能合约开发\n- 编程语言：**Solidity**\n  - 语法类似 JavaScript\n  - 学习变量、函数、修饰符、事件、继承等\n- 开发框架：\n  - Hardhat 或 Foundry（推荐初学者用 Hardhat）\n  - Truffle（较老但仍有项目使用）\n- 实践项目：\n  - 编写一个简单的代币（ERC-20）\n  - 创建 NFT 合约（ERC-721 / ERC-1155）\n  - 构建投票合约或众筹合约\n\n> 推荐学习平台：\n> - [CryptoZombies](https://cryptozombies.io/)（互动式 Solidity 教程）\n> - [Solidity 官方文档](https://docs.soliditylang.org/)\n> - [OpenZeppelin Contracts](https://openzeppelin.com/contracts/)（安全合约库）\n\n#### 4. 学习前端与 Web3 集成\n- 使用 **JavaScript / TypeScript** 连接区块链\n- 库：\n  - `ethers.js` 或 `web3.js`（与区块链交互）\n  - `wagmi` + `viem`（现代 React 开发推荐）\n- 前端框架：\n  - React + Next.js 是主流选择\n- 实现功能：\n  - 连接钱包（MetaMask）\n  - 发送交易\n  - 读取链上数据（余额、NFT 列表等）\n\n> 推荐项目：做一个 NFT 展示网站，支持连接钱包并查看用户拥有的 NFT。\n\n---\n\n### 三、深入进阶领域\n\n#### 1. DeFi（去中心化金融）\n- 学习 AMM（自动做市商）、借贷协议、稳定币\n- 研究 Uniswap、Aave、Compound 等协议\n- 尝试集成 DeFi 协议到自己的应用中\n\n#### 2. NFT 与元宇宙\n- 创建和部署 NFT\n- 理解元数据标准（如 Metadata URI、IPFS 存储）\n- 探索 NFT 在游戏、艺术、社交中的应用\n\n#### 3. DAO 与治理\n- 学习如何通过提案和投票管理项目\n- 使用 Aragon、Snapshot 等工具\n- 设计简单的治理合约\n\n#### 4. 安全与审计\n- 学习常见漏洞：重入攻击、整数溢出、权限控制等\n- 使用 Slither、MythX 等工具进行静态分析\n- 阅读 [Damn Vulnerable DeFi](https://www.damnvulnerabledefi.xyz/) 挑战\n\n---\n\n### 四、参与社区与实践\n\n1. **加入社区**\n   - Discord、Telegram（项目官方群）\n   - Reddit（r/ethdev、r/web3js）\n   - Twitter/X 关注核心开发者\n\n2. **参加黑客松**\n   - ETHGlobal、Gitcoin Grants、Polygon Builders\n   - 锻炼实战能力，结识同行\n\n3. **贡献开源项目**\n   - GitHub 上参与 OpenZeppelin、Hardhat 插件等\n   - 提交 PR、撰写文档\n\n4. **构建自己的项目**\n   - 示例项目：\n     - 一个去中心化的博客平台（内容上链 + IPFS 存储）\n     - 一个 DAO 投票系统\n     - 一个简单的 DeFi 聚合器\n\n---\n\n### 五、持续学习建议\n\n| 阶段 | 目标 | 时间建议 |\n|------|------|--------|\n| 第1-2月 | 掌握区块链基础 + Solidity 入门 | 每天 1-2 小时 |\n| 第3-4月 | 开发智能合约 + 前后端集成 | 动手做项目 |\n| 第5-6月 | 深入 DeFi/NFT/DAO + 参与社区 | 输出内容、投稿 |\n\n---\n\n### 六、推荐学习路线图（免费资源）\n\n1. [ethereum.org/learn](https://ethereum.org/en/learn/)\n2. [Buildspace](https://buildspace.so/) — 实战项目驱动\n3. [Speed Run Ethereum](https://speedrunethereum.com/) — 快速上手合约开发\n4. [Mastering Ethereum](https://github.com/ethereumbook/ethereumbook) — 免费电子书\n5. YouTube 频道：\n   - Dapp University\n   - Patrick Collins\n   - Smart Contract Programmer\n\n---\n\n### 总结\n\n> **学习 Web3 = 区块链基础 + 智能合约开发 + 前端集成 + 社区参与**\n\n关键在于：**动手实践 + 持续输出 + 积极交流**\n\n不要害怕犯错，Web3 生态仍在早期，现在入场正是最佳时机！\n\n如果你告诉我你的背景（比如是程序员、设计师、产品经理还是完全新手），我可以为你定制更具体的学习计划 😊"},"finish_reason":"stop","index":0,"logprobs":null}],"object":"chat.completion","usage":{"prompt_tokens":24,"completion_tokens":1516,"total_tokens":1540,"prompt_tokens_details":{"cached_tokens":0}},"created":1764602302,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-b0966207-9433-4743-9335-4e67cd0a1c50"}
```
<!-- DAILY_CHECKIN_2025-12-01_END -->

# 2025-11-30
<!-- DAILY_CHECKIN_2025-11-30_START -->






# 跨链SWAP

普通的链只能从它当前链进行swap

ZetaChain的swap工作流程

从其他链上的U转换成ZRC-20ETH，在转化成对应的token，最后提到目标链上

Ethereum ETH ——> **ZRC20 ETH.ETH---------swap-------------ZRC20 BTC** ——>Bitcoin BTC

```
function onCrossChainCall(
    bytes calldata origin, //源链信息
    uint256 originChainID, //源链 ChainID
    address zrc20, //源链 ZRC-20 代币合约地址
    uint256 amount, //代币数量
    bytes calldata message //跨链信息（包括了目标链ID，目标资产，数量等等）
) external {
    //1.解析信息
    (address targetToken, uint256 minAmount) = abi.decode(
        message,
        (address, uint256)
    );
    
    //2.跨链交互逻辑的实现
    uint256 amountOut = calculateSwapAmount(amount);
    require(amountOut >= minAmount, "invalid output");

    //3.提取到目标链上
    IZRC20(targetToken).withdraw(amountOut, recipient, targetChainID);
}
```
<!-- DAILY_CHECKIN_2025-11-30_END -->

# 2025-11-29
<!-- DAILY_CHECKIN_2025-11-29_START -->







# ZetaChain函数

```
interface IZRC20{
    #存储
    function deposit() external payable;
    #提取
    function withdrraw(uint256 amount)external;
    #转账
    function transfer（address to,uint256 amount）external returns（bool）;
    #查询余额
    function balanceOf(address account) external view returns (uint256);
}
```
<!-- DAILY_CHECKIN_2025-11-29_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->








# 跨链

也称互操作性，是指不同的区块链系统之间能够进行**数据和资产**的通信和协同工作的能力。

### **跨链的常见实现方式**

**跨链桥**（Cross-chain Bridge）

-   用户在源链锁定资产，目标链铸造等值的“封装资产”（如 WBTC、wETH）。 ------- A锁B铸，B销毁A释放，中间会有消息监听
    

### ZetaChain的跨链

-   星线型结构
    

ZetaChain作为中心hub，其他链通过ZetaChain进行交互

# Universal EVM(通用的以太坊虚拟机)

也成为全链以太坊虚拟机，使ZetaChain可以直接操作以太坊和其他链上的资产。以往在不同链上开发跨链应用，需要学习不同环境下的技术：

-   Solana rust
    
-   Sui Move
    
-   Bitcoin 模板
    

ZetaChain使用通用智能合约完成不同链上的操作，极大降低了学习成本

# Universal Contract(通用智能合约)

ZetaChain内置了不同跨链操作的智能合约模板，向外暴露接口，内部相当于是黑盒，用户只需关注接口的规范性，不需要关心底层做的交互

# Universal DAPP(通用应用)

能实时的读取并更新连接网络状态。得益于ZetaChain的GateWay客户端，这个客户端充当了观测者和验证者。

ZetaChain有三种不同的节点：观察者，验证者，签名者（有不同的激励机制来确保安全性）

状态监听：

观测者监听其他链的网络（BitCoin,ETH等）的信息，这些网络与ZetaChian的GateWay发生交互后，观测者就会读取这些网络的状态，经过验证者的5秒的出块确认（未来会继续降低）。在通过GateWay与目标链进行交互。

资产转移：

其他链上符合要求的资产会转化成ZRC-20（通用资产）放到一个pool里面，用于资产的流动。

# GateWay(网关)

不同的链，ZetaChain对应了不同的网关，部署了不同网关合约

大致的跨链流程为：A链——网关——ZetaChain——通用合约处理——网关——B链

# 架构

TODO

ZetaChain 的共识机制
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->









加班，明天补笔记
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->










### **1\. 通用区块链（Universal Blockchain）**

-   **含义**：指一种支持跨多条区块链运行、具备广泛兼容性和互操作能力的区块链架构或网络。
    
-   **特点**：
    
    -   不局限于单一链的共识、状态或资产；
        
    -   通常通过中继、桥接、共享安全或模块化设计实现多链协同；
        
    -   可作为“中间层”或“聚合层”，协调多个异构链之间的交互。
        
-   **示例项目**：Cosmos（通过 IBC 协议）、Polkadot（通过 XCM 和平行链）、LayerZero、Chainlink CCIP 等。
    

* * *

### **2\. Universal EVM（通用以太坊虚拟机）**

-   **含义**：一种在多条区块链上统一部署和运行 EVM（Ethereum Virtual Machine）兼容智能合约的能力。
    
-   **特点**：
    
    -   允许开发者用 Solidity 编写一次合约，即可部署到多个 EVM 兼容链（如 Polygon、Arbitrum、Optimism、BNB Chain、Avalanche C-Chain 等）；
        
    -   “Universal” 强调的是跨链一致性：相同的字节码、相同的调用逻辑、相同的账户模型；
        
    -   有时也指通过抽象层实现“一次部署，多链运行”的运行时环境。
        
-   **目标**：消除 EVM 链之间的碎片化，提升开发效率和用户体验。
    
-   **相关技术**：EVM 等效性（EVM Equivalence）、统一账户系统、跨链合约部署工具。
    

* * *

### **3\. Universal App（通用应用）**

-   **含义**：指可在多条区块链上无缝运行、无需为每条链单独部署或适配的应用程序。
    
-   **特点**：
    
    -   用户无论使用哪条链（如 Ethereum、Base、zkSync、Solana 等），都能访问同一个应用；
        
    -   应用后端或智能合约逻辑能自动感知并适配不同链的环境；
        
    -   通常依赖跨链消息传递（Cross-chain Messaging）或通用账户抽象（如 ERC-4337 + 跨链钱包）。
        
-   **目标**：提升用户体验（无感知切换链）、聚合流动性、避免应用碎片化。
    

* * *

### **4\. Omnichain Smart Contract（全链智能合约）**

-   **含义**：一种原生支持跨多条区块链执行逻辑的智能合约，可在多个链之间传递状态、资产或消息，并保持一致性。
    
-   **关键特征**：
    
    -   合约逻辑分布在多个链上，但被视为一个整体；
        
    -   通过跨链通信协议（如 LayerZero、Wormhole、Axelar、Hyperlane）实现链间同步；
        
    -   支持跨链条件执行（例如：在 Chain A 上锁定资产，自动在 Chain B 上释放）。
        
-   **与传统跨链桥的区别**：
    
    -   传统桥：资产从 A 链“搬”到 B 链（1:1 锚定）；
        
    -   Omnichain 合约：逻辑本身跨链存在，状态动态同步，无需“搬运”。
        

在 Web3 中，**Universal App** 通常指：

> **一种无需强制用户绑定特定钱包、链或身份，即可在多链、多钱包、多设备上无缝使用的去中心化应用（dApp）。**

**核心特征：**

-   **链无关（Chain-agnostic）**：支持以太坊、Polygon、Solana、Arbitrum 等多条区块链。
    
-   **钱包无关（Wallet-agnostic）**：用户可以用 MetaMask、Coinbase Wallet、Phantom、Smart Wallet（如 ERC-4337 账户抽象钱包）等任意钱包登录。
    
-   **基于去中心化身份（DID）**：用户以一个统一的身份（如 ENS、.eth 名称，或 DID 标准如 EIP-725/EIP-4361）登录，而不是依赖某个钱包地址。
    
-   **状态可迁移 / 跨链一致体验**：用户在不同链上的资产、行为、声誉可被聚合，提供一致的 UI/UX。
    

在 Web3 中，Gateway 通常指：

连接 Web2 与 Web3、或连接不同区块链/协议之间的“入口”或“桥梁”服务。

| 去中心化身份网关（Identity Gateway） | 允许用户用邮箱、Google 登录等方式创建/恢复钱包（如 Web3Auth、Privy），降低 Web3 入门门槛。 |
| 跨链网关（Cross-chain Gateway） | 实现资产或消息在不同链之间传递，比如 LayerZero、Synapse、Wormhole 提供的跨链桥服务（虽然“Bridge”更常用，但有时也称 Gateway）。 |
| API 网关（Web3 API Gateway） | 开发者通过统一接口访问多链数据，如Alchemy、Infura、Moralis提供的节点+API服务，隐藏底层区块链复杂性。 |
| 应用入口网关（App Gateway） | 某些 dApp 或钱包提供“网关”功能，让用户从 Web2 应用（如 Twitter、Gmail）直接进入 Web3 体验（例如：通过 Farcaster Frame 嵌入互动）。 |

# ZetaChain 架构图

```
          +---------------------+
          |                     |
          |    ZetaChain        |  ← 中心链（共识层 + 跨链逻辑）
          |   (Central Chain)   |
          |                     |
          +----------+----------+
                     |
          +----------+----------+
          |                     |
          |      Gateway        |  ← 跨链网关（监听/中继/验证）
          |                     |
          +----------+----------+
                     |
      +--------------+--------------+
      |              |              |
+-----v-----+  +-----v-----+  +-----v-----+
|           |  |           |  |           |
|  Bitcoin  |  | Ethereum  |  |  Solana   |
| (外链 A)  |  | (外链 B)  |  | (外链 C)  |
|           |  |           |  |           |
+-----------+  +-----------+  +-----------+
```
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->











# ZetaChain和区块链一些基础

## ZetaChain

-   ZetaChain是 主链（Layer 1）
    
-   全链智能合约，开发者编写一次智能合约，即可在任意连接链上接收事件并执行逻辑
    
-   ZetaChain 的独特优势在于支持非智能合约链如：Bitcoin
    
-   通过协议层原生支持跨链消息与资产，无需桥或封装
    

## Layer 1 和Layer 2

### L1

-   Layer 1 是指底层的主区块链协议本身，它主要负责共识机制（如 PoW、PoS），数据存储（账本），原生代币（如 BTC、ETH）和基础安全性与去中心化
    
-   L1 的扩容通过**直接修改底层协议**实现
    
-   安全性高（直接由主链共识保障）
    
-   去中心化程度通常较高
    
-   无需依赖外部系统
    
-   升级困难（需全网共识，硬分叉风险）
    
-   扩容能力有限（如 Bitcoin TPS ≈ 7，Ethereum ≈ 15–30）
    
-   交易费用高、速度慢（尤其在高负载时）
    

### L2

-   **Layer 2 是构建在 Layer 1 之上的附加协议或网络**，目的是**提升交易吞吐量、降低费用，同时继承 L1 的安全性**。L2 本身不独立维护完整区块链，而是将大部分计算或存储“移出主链”，只在必要时与 L1 交互。
    
-   **高 TPS**：可达数千甚至上万笔/秒（如 zkSync 目标 2000+ TPS）
    
-   **低 Gas 费**：交易成本降低 10–100 倍
    
-   **快速确认**：部分操作秒级完成
    
-   **兼容 EVM**：多数 Rollup 支持以太坊智能合约无缝迁移
    
-   **复杂性高**：用户需理解“桥接”“提款延迟”等概念
    
-   **提款延迟**（Optimistic Rollup）：通常需 7 天挑战期
    
-   **中心化风险**（早期 L2）：排序器（Sequencer）可能由单一实体控制
    
-   **碎片化**：L2 之间互通困难（如 Arbitrum 和 Optimism 资产不能直接互转）
    

## ZetaChain的领水地址(需要登陆Discord)

[FAUCETME](https://zetachain.faucetme.pro/)

## 重新注册了Discord

## 重新注册了国内版的阿里云百炼（国外的需要用非内地手机号）

[阿里云百炼](https://bailian.console.aliyun.com/?tab=model#/efm/model_experience_center/text)

## 下载了PostMan

[PostMan](https://www.postman.com/downloads/)

需要注意处理器版本，看看是不是RAM版本

## 初始化ZetaChain环境

```
npx zetachain@latest new --project hello
cd hello
yarn
forge soldeer update
```

初始化成功后，会自动生成一个通用合约模板

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

// 引入 ZetaChain 协议合约中的 UniversalContract 接口
import "@zetachain/protocol-contracts/contracts/zevm/interfaces/UniversalContract.sol";

// 继承 UniversalContract，成为“全链合约”
contract Universal is UniversalContract {
    // 定义一个事件，用于日志输出
    event HelloEvent(string, string);

    // 重写 onCall 方法，处理跨链调用
    function onCall(
        MessageContext calldata context,   // 跨链消息的上下文信息
        address zrc20,                     // 发送的 ZRC-20 代币地址（如 zBTC, zETH）
        uint256 amount,                    // 发送的代币数量
        bytes calldata message             // 用户自定义的跨链消息数据
    ) external override onlyGateway {      // 仅允许 ZetaChain 网关调用
        // 从 message 中解码出一个字符串
        string memory name = abi.decode(message, (string));
        // 触发事件，打印 "Hello: [name]"
        emit HelloEvent("Hello: ", name);
    }
}
```

我当时报错说是编译器是0.8.30以上版本，在0.8.26前面加^就行了表示当前合约支持大于0.8.26,小于0.9.0的版本

* * *

## **一、**`onCall` **是什么？**

在 ZetaChain 的 **Universal Contract（通用合约）模型**中：

> `onCall` **是跨链消息到达 ZetaChain 时自动触发的回调函数**。

当用户在**任意已连接链**（如 Bitcoin、Ethereum、Dogecoin）上向 ZetaChain 发起跨链请求（例如发送资产 + 附加消息），ZetaChain 的协议层会：

1.  验证该跨链交易；
    
2.  将其封装为一个标准化的“跨链消息”；
    
3.  **自动调用目标合约的** `onCall` **方法**，传递相关参数。
    
4.  因此，`onCall` 就是合约的“跨链入口”。
    

## **二、**`onCall` **的四个参数详解**

### **1.** `MessageContext calldata context`

这是一个结构体，包含跨链调用的**元信息**。定义通常如下（来自 ZetaChain SDK）：

```
struct MessageContext {
    address origin;      // 源链标识（如 Bitcoin 链的地址常量）
    address caller;      // 源链上发起交易的用户地址（映射为 Zeta 格式）
    uint256 nonce;       // 防重放攻击的随机数
    bytes32 hash;        // 消息哈希
}
```

* * *

### **2.** `address zrc20`

-   表示用户在源链上发送的**原生资产**（如 BTC、ETH、DOGE）在 ZetaChain 上对应的 **ZRC-20 代币地址**。
    
-   例如：BTC → `zBTC`，ETH → `zETH`。
    
-   若用户**只发消息没发资产**，则 `zrc20 = address(0)`。
    

* * *

### **3.** `uint256 amount`

-   用户在源链发送的资产数量（已转换为 ZRC-20 的最小单位）。
    
-   例如：发送 0.01 BTC → `amount = 1_000_000`（satoshi）。
    
-   若无资产转移，则为 `0`。
    

### **4.** `bytes calldata message`

-   **用户自定义的任意数据**，是你实现通用逻辑的关键。
    
-   通常使用 `abi.encode(...)` 在源链构造，用 `abi.decode(...)` 在 `onCall` 中解析。
    

string memory name = abi.decode(message, (string));表示用户在源链发送了一个字符串（如 “Alice”），合约将其解码并用于事件。

## **三、**`onlyGateway` **修饰符：安全核心**

```
external override onlyGateway
```

-   `onlyGateway` 是 ZetaChain 协议提供的**关键安全机制**。
    
-   它确保 `onCall` **只能由 ZetaChain 的官方网关合约（Gateway）调用**。
    
-   **普通用户或其它合约无法直接调用** `onCall`，防止伪造跨链消息。
    

## 四、执行流程示例

假设用户想从 **Ethereum** 调用这个合约，并传入名字 `"Alice"`：

使用 ZetaChain SDK（如 @zetachain/zeta-eth）：

```
const message = abi.encode(["string"], ["Alice"]);
await zetaConnector.send({
  destinationAddress: "0xYourUniversalContractOnZeta",
  message: message,
  zrc20: "0x0000000000000000000000000000000000000000", // 无资产
  amount: 0,
});
```

步骤 2：ZetaChain 处理

-   ZetaChain 观察者检测到 Ethereum 上的调用；
    
-   验证通过后，在 ZetaChain 主网执行：
    

```
universalContract.onCall(
    context: { origin: ETHEREUM_CHAIN, caller: userAddrOnZeta, ... },
    zrc20: 0x0,
    amount: 0,
    message: 0x...（"Alice" 的 ABI 编码）
);
```

### **步骤 3：合约执行**

-   解码 `message` 得到 `"Alice"`；
    
-   触发事件：`HelloEvent("Hello: ", "Alice")`；
    
-   开发者可通过事件监听器得知跨链调用成功。
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->
















# 第一份WEB3笔记

## 一.ZetaChain开发文档入口

-   [https://www.zetachain.com/docs/developers](https://www.zetachain.com/docs/developers)
    

## 二.ZetaChain是什么（粗浅的理解）

1.  一个可以原生访问任何区块链的通用区块链
    
2.  可以跨链去调用不同的DAPP
    
3.  使用“全链智能合约”，一个合约可以监听和响应多条链上的事件，并在任意链上执行操作。
    

## 三.配置ZetaChain开发环境

### 1\. 配置Ubuntu远程开发环境（以下所有操作都在远程环境中进行）

(1)具体的视频资料：[wsl + vscode](https://www.bilibili.com/video/BV1fu4m1A79r/?spm_id_from=333.1007.top_right_bar_window_custom_collection.content.click&vd_source=429af1a95897362b1265c3245c2759bb)

(2)在资源管理器中查看远程环境中的文件：

```
在地址栏输入（只能是wsl2）：
\\wsl$
然后回车就行
```

(3)查看当前的wsl版本

```
wsl --list --verbose
输出示例：
  NAME            STATE           VERSION
* Ubuntu-22.04    Running         2
  Debian          Stopped         1
```

### 2.下载Foundry

在远程环境中使用以下命令（速度巨慢）

```
curl -L https://foundry.paradigm.xyz | bash
```

### 3.下载Node.js

官网命令参考：适用于Linux且使用nvm和npm的Node.js

```
# 下载并安装 nvm：
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash

# 代替重启 shell
\. "$HOME/.nvm/nvm.sh"

# 下载并安装 Node.js：
nvm install 24

# 验证 Node.js 版本：
node -v # Should print "v24.11.1".

# 验证 npm 版本：
npm -v # Should print "11.6.2".

```

但是在第一句命令就遇到了错误（悲~）：

```
fatal: unable to access 'https://github.com/nvm-sh/nvm.git/': GnuTLS recv error (-110): The TLS connection was not properly terminated.
Failed to clone nvm repo. Please report this!
```

以下是AI给出的解决方案，亲测可用

```
# 1. 手动克隆 nvm 仓库到 ~/.nvm
git clone https://github.com/nvm-sh/nvm.git ~/.nvm

# 2. 激活 nvm（临时）
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # 加载 nvm

# 3. 添加到 shell 配置文件（永久生效）
echo 'export NVM_DIR="$HOME/.nvm"' >> ~/.bashrc
echo '[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"' >> ~/.bashrc
echo '[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"' >> ~/.bashrc

# 4. 重新加载配置
source ~/.bashrc

# 5.安装完成后验证
nvm --version
nvm install --lts
nvm use --lts
node -v
npm -v
```

## 四.注册Qwen 账号

控制台入口：[阿里云控制台](https://home.console.alibabacloud.com/home/dashboard/ProductAndService)
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
