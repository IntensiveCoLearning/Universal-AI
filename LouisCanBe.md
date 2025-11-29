---
timezone: UTC+8
---

# Louis

**GitHub ID:** LouisCanBe

**Telegram:** @louiscanbe

## Self-introduction

掌握前端nextjs，ts等开发，熟练应用ai辅助开发，有web项目上线能力；常用过claudecode、warp、cursor、aistudio等coding工具。了解过区块链基础内容，从上次黑客松关注到残酷共学。研究方向某垂直方向agentic system应用开发，目前AI产品实习中。

## Notes

<!-- Content_START -->
# 2025-11-29
<!-- DAILY_CHECKIN_2025-11-29_START -->
继续阅读学习
<!-- DAILY_CHECKIN_2025-11-29_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->

### 对“全链应用 / Universal App 合约”的直观理解

“全链应用”（Universal App）是部署在 **ZetaChain** 上的智能合约，其核心特征是**原生连接**到其他区块链网络（如以太坊、BNB、Solana、TON，甚至是非智能合约区块链如比特币）的能力。

这种能力使得全链应用能够实现**链条编排 (Chain Orchestration)**，即从一个智能合约中协调跨多个区块链的复杂交易。

1\. 核心功能与机制

-   **统一的开发框架：** ZetaChain 将不同的区块链生态系统统一在一个开发框架下，使得应用可以轻松地跨链运行。
    
-   **双向交互能力：**
    
    -   **接收：** 全链应用可以接受来自任何连接链的合约调用、消息和代币转移。
        
    -   **发起：** 它也可以触发对连接链的合约调用和代币转移。
        
    -   一个复杂的跨链交易序列和价值转移，只需由用户签署一次交易即可触发。例如，比特币用户可以通过与全链应用交互，将 USDC 发送给以太坊上的接收者。
        
-   **部署环境：** 全链应用部署在 ZetaChain 的 **Universal EVM (UEVM)** 上。该 UEVM 完全兼容以太坊的 EVM，这意味着开发者可以使用 Solidity 等 EVM 兼容语言。
    
-   **关键接口：** 当用户通过连接链上的 **Gateway 合约**调用全链应用时，应用会触发其核心方法 `onCall`。
    
    -   `onCall` 方法会接收到任意数据组成的消息（`message`）和以 **ZRC-20** 形式表示的代币。ZRC-20 代表来自连接链的原生 Gas 和受支持的 ERC-20 代币。
        
-   **Gas 抽象与无 Gas 执行：** 这是全链应用的一个强大特点。
    
    -   从连接链传入的全链应用调用，只需要在发起链上支付 Gas 费用。
        
    -   **在 ZetaChain 上的执行是无额外费用的 (Gasless)**。
        
    -   对于全链应用发起的**出站调用**（如向连接链提取代币或发起合约调用），应用会负责处理 Gas 费用，通常通过将一部分接收到的 ZRC-20 代币兑换成目标链 Gas 代币的 ZRC-20 来覆盖费用。
        

2\. 前瞻兼容性 (Forward Compatibility)

当您在 ZetaChain 上部署应用后，它会自动连接到 ZetaChain 生态系统中的所有网络。随着新的区块链被加入，您的应用无需修改代码即可获得对这些新网络的兼容性，从而实现了应用的**未来证明 (future-proofs)**。

* * *

### Hello World / Demo 模块构成

要实现一个 Hello World / Demo，您的项目通常需要以下模块，它们与 ZetaChain 的开发生态相对应：

1\. 合约 (Smart Contract)

这是项目的核心，即部署在 ZetaChain UEVM 上的 **Universal App 合约**。

-   **内容：** 需包含实现 `onCall` 方法的逻辑，用于处理来自外部链的消息和 ZRC-20 代币。
    
-   **目标：** 实现您希望的跨链逻辑，例如记录数据或进行简单的代币交换。
    
-   **示例：** 教程中包括“构建你的第一个全链合约” (Build your first universal contract)。
    

2\. 前端 (Frontend / Web App)

这是用户与合约交互的界面。

-   **内容：** 需要一个 Web 应用程序来：
    
    -   连接用户的钱包（如 MetaMask）。
        
    -   构建并发送跨链调用，与连接链上的 **Gateway 合约**交互。
        
    -   跟踪交易执行状态。
        
-   **工具支持：** ZetaChain 兼容 Ethers.js 等流行工具。
    

3\. RPC / 网络基础设施

RPC (Remote Procedure Call) 是前端和部署工具与区块链网络通信的机制。

-   **基础设施：** 您的合约部署和运行依赖于 ZetaChain 的底层架构，这是一个基于 Cosmos SDK 和 Comet BFT 共识引擎的 PoS 区块链。
    
-   **核心组件：** 节点操作者运行 **ZetaCore**（负责区块生产）和 **ZetaClient**（负责观察连接链事件并签名出站交易）。
    
-   **开发者体验：** 由于 ZetaChain EVM 兼容，开发者可以使用标准的 EVM 工具和 RPC 端点进行部署和交互。
    

* * *

**总结类比：**

如果将 ZetaChain 视为一个**全球语言翻译中心**，那么**全链应用**就是在这个中心工作的**首席翻译官**。无论用户说哪种“语言”（来自哪个区块链，如比特币、以太坊），只要通过**入境窗口 (Gateway)** 提交信息和资金，这位翻译官（全链应用）就能立即理解、处理（`onCall`），并用正确的“语言”和“货币”（通过 ZRC-20 和 Gas 抽象）向目标国家（目标区块链）发出指令，全程高效且无需用户担心中间的翻译费用（Gasless EVM Execution）。
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->


## 1\. 理解 “通用区块链 / Universal EVM / Universal App / Omnichain Smart Contract” 的基本含义。

**通用区块链 (Universal Blockchain) / ZetaChain** ZetaChain 是一个建立在 Cosmos SDK 和 Comet BFT 共识引擎之上的 **PoS 区块链**。ZetaChain 的核心目标是提供基础设施，使开发者能够创建可与**任何区块链**（包括像 Bitcoin 这样的非智能合约区块链、以及 Ethereum、Solana、TON 等）**无缝交互**的通用应用程序。ZetaChain 通过内置的全链互操作性功能，将多样化的区块链生态系统统一在一个单一的开发框架之下。

**Universal EVM (通用 EVM)** Universal EVM (通用 EVM) 是 ZetaChain 上的一个 **智能合约平台**，它内置了**全链互操作性功能**，从而能够开发通用应用。它也被称为通用以太坊虚拟机 (UEVM)。它扩展了标准的 EVM，使其具备全链功能，并且与 Ethereum 的 EVM 完全兼容，允许开发者使用 Solidity 或其他 EVM 兼容语言编写智能合约。此外，ZetaChain 提供了**无 Gas 的 EVM 执行环境**，这意味着当通用应用被连接链上的用户调用时，只需在连接链上支付 Gas，而在 ZetaChain 上的执行不会产生额外费用。

**Universal App (通用应用) / Omnichain Smart Contract (全链智能合约)** 通用应用是部署在 ZetaChain 上的一个**智能合约**，它**原生连接**到其他区块链，例如 Ethereum、BNB 和 Bitcoin。

**特点：**

-   它可以接收来自**任何连接链**的合约调用、消息和代币转移。
    
-   它可以触发对连接链的合约调用和代币转移。
    
-   通用应用能够**协调**跨越多链的复杂多步骤交易和价值转移，只需用户签署一次交易即可触发。
    
-   一旦部署在 ZetaChain 上，它会自动连接到 ZetaChain 生态系统中的所有网络，并且随着新区块链的加入，它会自动获得兼容性，无需修改合约源代码（**前向兼容性**）。
    
-   它能够通过**原生代币**（而非包装或合成版本）在所有连接链之间转移价值。
    

## 2\. 实践 / 作业：用自己的话写出定义

**○ Universal App 是什么？通用应用**是部署在 ZetaChain 的 **Universal EVM** 上的智能合约。它具有**全链互操作性**，这意味着它能直接接收来自任何外部连接的区块链（包括 EVM 链、Solana、Bitcoin 等）的消息、代币和合约调用。通用应用不仅能接收信息，还能向这些连接链发起调用或转移代币，从而实现从一个合约协调跨越多个区块链的复杂交易。

**○ Gateway 大概做什么？Gateway (网关)** 是用户或合约与 ZetaChain 上的通用应用进行交互的**单一入口点**。每个连接链上都会部署一个 Gateway 合约。它的主要功能包括：

1.  **接收存款和调用：** Gateway 合约暴露了方法，允许用户将代币存入通用应用并调用它们。例如，以太坊用户通过 Gateway 发送 ETH 和消息到通用应用。Bitcoin 用户只需将 BTC 和数据发送到 **Gateway 地址**即可调用通用应用。
    
2.  **代币和调用流出：** Gateway 也用于通用应用向连接链**提取代币**和**发起合约调用**。
    

## 3\. 画一张简单的架构图

**ZetaChain 架构简图描述：ZetaChain 中心 + 多条公链 + Gateway**

在 ZetaChain 的架构中，**ZetaChain 协议**位于中心，扮演着协调者的角色，并运行 **Universal EVM**。

1.  **中心 (ZetaChain Core):**
    

-   **ZetaChain (PoS 区块链):** 运行 Universal EVM 和 通用应用 (Universal Apps)。
    
-   **ZetaChain 验证者 (Validators):** 包含 ZetaCore 和 ZetaClient。**观察者-签名者 (Observer-Signer Validators)** 是一组关键的参与者，他们持续观察连接链上的相关事件/状态，并集体持有密钥，以便代表 ZetaChain 对连接链进行认证交互（签名出站交易）。
    

2.  **连接者 (Gateway Contracts / Addresses):**
    

-   在每一条连接的外部公链上，都部署了 **Gateway 合约**（或提供 Gateway 地址，如在 Bitcoin 网络上）。Gateway 是用户与 ZetaChain 上的通用应用交互的**入口点**。
    

3.  **外围链 (Connected Chains):**
    

这些是 ZetaChain 互操作的目标链，包括：

-   **EVM 兼容链:** Ethereum、BNB、Polygon、Base 等。
    
-   **非 EVM 链:** Solana、TON、Sui。
    
-   **非智能合约链:** Bitcoin。
    

**数据/调用流向描述：**

-   **入站流 (Connected Chain -> ZetaChain):** 外部链上的用户通过与该链上的 **Gateway 合约**交互，发起代币存入和通用应用调用。这些事件随后被 ZetaChain 的**观察者**捕获，并在 ZetaChain 上触发通用应用的执行。
    
-   **出站流 (ZetaChain -> Connected Chain):** 通用应用通过调用 ZetaChain 上的协议合约，指示 ZetaChain 的**观察者-签名者**，代表 ZetaChain 签署一笔交易，对目标连接链（如 Ethereum 或 BNB Chain）发起合约调用或代币提取。
    

![screenshot-20251126-235044.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/LouisCanBe/images/2025-11-26-1764172264865-screenshot-20251126-235044.png)![screenshot-20251126-235459.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/LouisCanBe/images/2025-11-26-1764172513162-screenshot-20251126-235459.png)
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->




了解了qwen，zetachain的部署使用，llm api调用没问题，链相关的内容还没太搞懂
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->





已加入社区，能正常访问页面。

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/LouisCanBe/images/2025-11-23-1763921560977-image.png)
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
