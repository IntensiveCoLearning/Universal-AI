---
timezone: UTC+8
---

# 黄文晔

**GitHub ID:** Eeeeye

**Telegram:** @Eeeeye

## Self-introduction

大三

## Notes

<!-- Content_START -->
# 2025-11-29
<!-- DAILY_CHECKIN_2025-11-29_START -->
### **搭建官方 Swap Demo 项目**

**创建项目骨架**

根据官方文档 直接使用CLI脚手架创建项目：

```
# 1）初始化项目
npx zetachain@next new --project swap
​
# 2）进入项目目录
cd swap
​
# 3）安装依赖
yarn
​
# 4）拉取 Solidity 依赖
forge soldeer update
​
# 5）编译合约
forge build
```

### **部署Swap合约到ZetaChain Testnet**

```
UNIVERSAL=$(npx tsx commands deploy --private-key $PRIVATE_KEY | jq -r .contractAddress) && echo $UNIVERSAL
​
0x7Ff868a3158c134aF373a4228d02bf52d7404473
```

### **配置跨链Swap所需要的参数**

**调用人地址（EVM）**

```
RECIPIENT=$(cast wallet address $PRIVATE_KEY) && echo $RECIPIENT
```

**查询“目标链资产”的 ZRC-20 地址**

```
ZRC20_ETHEREUM_ETH=$(zetachain q tokens show --symbol sETH.SEPOLIA -f zrc20) && echo $ZRC20_ETHEREUM_ETH
​
```

此时出现报错：

**网络请求失败**：`PostHogFetchNetworkError: Network error while fetching PostHog … ECONNREFUSED` 表示 CLI 在向远程服务获取数据时连不上（可能是本地网络代理、VPN、或临时的网络故障）。这会阻止 CLI 查询可用 ZRC-20 资产列表。

**代币符号不存在**：最后提示 `Token with symbol 'sETH.SEPOLIA' not found`，意味着当前网络环境中并没有名为 `sETH.SEPOLIA` 的 ZRC‑20 资产，或者 CLI 无法从链上获取到它。

查阅

```
zetachain q tokens list  
```

发现是当前没有sETH.SEPOLIA这个链 进而将命令转为，成功执行

```
ZRC20_ETHEREUM_ETH=$(zetachain q tokens show --symbol ETH.ETHSEP -f zrc20) && echo $ZRC20_ETHEREUM_ETH 
```

返回地址：

```
0x05BA149A7bd6dC1F937fA9046A9e05C05f3b18b0
```

### **发起跨链调用与问题定位**

-   通过 CLI 运行 `npx zetachain evm deposit-and-call --chain-id 11155111 --amount 0.001 --types address bytes bool --receiver $UNIVERSAL --values $ZRC20_ETHEREUM_ETH $RECIPIENT true` 发起跨链调用时，出现 `Failed to retrieve private key: Private key not found` 错误。这表明 CLI 无法找到用于签名交易的私钥。
    
-   查阅官方文档可知，部署或调用任何通用合约前，必须准备一个在 **ZetaChain 测试网** 和 **连接的 EVM 测试网** [上都有余额的私钥zetachain.com](http://上都有余额的私钥zetachain.com)。而 CLI 的 `deposit-and-call` 命令也提供了 `--private-key` 选项用于传入私钥。如果未指定，CLI 会尝试从账户管理工具读取默认账户，但若未导入私钥则会报错。
    

**解决方案与执行步骤**

1.  **导入或指定私钥**：
    
    -   通过 `zetachain accounts import --type evm --name default --private-key <私钥>` 导入 EVM 私钥（仅需一次）。之后可以不再在命令中指定私钥。
        
    -   或者在 `deposit-and-call` 命令中显式加入 `--private-key $PRIVATE_KEY` 参数，以便 CLI 正确签名交易：
        
        ```
        npx zetachain evm deposit-and-call \ 
          --chain-id 11155111 \ 
          --amount 0.001 \ 
          --receiver $UNIVERSAL \ 
          --private-key $PRIVATE_KEY \ 
          --types address bytes bool \ 
          --values $ZRC20_ETHEREUM_ETH $RECIPIENT true
        ```
        
    
    使用 `--private-key` 后即可避免“私钥未找到”的报错。
    
2.  **资金准备**：确保私钥对应地址在 ZetaChain 测试网及 Ethereum Sepolia 上都有足够资金支付 gas。文档中强调部署前需要在两个链上为私钥充值。
    

### **调用结果分析**

-   本次测试的来源链为 **Ethereum Sepolia (chain ID 11155111)**。通过 `deposit-and-call` 命令，用户从 Sepolia 发送 0.001 ETH 及 ABI 编码的参数到 ZetaChain。CLI 会将 Sepolia 的 ETH 锁定，并在 ZetaChain 上铸造等量 ZRC‑20 代币，随后调用 Universal Swap 合约。
    
-   在 ZetaChain 上，Universal Swap 合约的 `onCall` 函数接收跨链调用，其中包含：
    
    -   从源链转入的 ETH 的 ZRC‑20 表示；
        
    -   目标资产的 ZRC‑20 地址（如 Ethereum Sepolia ETH 对应的 ZRC‑20）；
        
    -   目标收款地址（Ethereum Sepolia 的 EVM 地址）；
        
    -   是否自动提现的布尔标记。
        
-   合约首先使用部分 ZRC‑20 资产兑换目的链的 gas 代币以支付跨链提款手续费；然后在 ZetaChain 的 Uniswap v2 池中将剩余 ZRC‑20 资产交换为目标 ZRC‑20；最后调用 Gateway 的 `withdraw`，将目标 ZRC‑20 烧毁并把等值原生资产发送到用户的目标链地址。整个过程由一次交易完成，用户无需在目的链预存 gas 或手动执行桥接。
<!-- DAILY_CHECKIN_2025-11-29_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->

## **ZRC-20和ERC-20的区别**

|   | ZRC‑20 | ERC‑20 |
| --- | --- | --- |
| 资产来源 | 代表其他链的原生代币或指定 ERC‑20 代币，通过锁定资产后铸造 | 在部署链上发行，资产只存在于该链 |
| 跨链能力 | 内置跨链存取功能，可从任何连接链存入或提取到原链 | 无跨链机制，需要借助桥或第三方协议 |
| 铸造限制 | 只能由 ZetaChain 协议铸造，受总流动性上限限制 | 任意合约可按逻辑铸造，不具备跨链流动性上限 |
| 统一接口 | 具备扩展方法用于查询跨链余额、执行跨链调用等 | 标准仅定义基本的转账与授权接口 |
| 典型应用 | 构建 Omnichain DEX、跨链借贷等可统一管理多链资产的 DeFi 协议 | 单链内的代币发行、交易与支付 |

**ZRC-20 Standard**

according to [zetachain.com](http://zetachain.com)

-   **Cross-Chain Support**: The ZRC-20 standard is a token standard on ZetaChain that facilitates the cross-chain transfer of assets. Tokens created using this standard can represent native tokens from other blockchains, such as Ethereum or Bitcoin, and can be moved across different chains without the need for intermediary services.
    
-   **Asset Locking and Minting**: When a user deposits an asset from a connected chain, the native token is locked in a trusted secure storage (TSS) or an ERC-20 wrapper on ZetaChain, and an equivalent ZRC-20 token is minted and sent to the user’s ZetaChain address.
    
-   **Withdrawal and Burning**: When a user wants to withdraw assets, the ZRC-20 tokens are burned, and the equivalent amount of the original asset is released from the secure storage.
    
-   **Cross-Chain Interoperability**: ZRC-20 allows for the seamless flow of assets between different chains by creating representations of native assets that are standardized across chains.
    

## **Universal Token / NFT: Cross-Chain**

Universal Token and Universal NFT standards enable seamless cross-chain asset deployment and transfer across all connected EVM-compatible chains. ZetaChain serves as the central routing hub, coordinating asset destruction on source chains and re-minting on destination chains.

### **Dual-Contract Architecture**

Each universal asset implementation requires two contract types:

**Universal Contract** (deployed on ZetaChain)

-   Handles asset minting on ZetaChain
    
-   Manages cross-chain inbound/outbound transfers
    
-   Coordinates inter-chain asset movements
    

**Connected Contract** (deployed on target EVM chains)

-   Manages asset minting on connected chains
    
-   Processes local chain deposit/withdrawal operations
    

### **Deployment Workflow**

1.  Deploy Universal contract on ZetaChain
    
2.  Deploy Connected contracts on target EVM chains (Ethereum, Polygon, BSC)
    
3.  Establish bidirectional trust relationships:
    
    -   Universal contract: `setConnected(zrc20, connectedAddress)`
        
    -   Connected contract: `setUniversal(universalAddress)`
        

## **Universal Asset Application Scenarios 通用资产应用场景**

### **Cross-Chain Yield Aggregation**

Universal Tokens enable sophisticated yield strategies across multiple chains. Users can deposit Bitcoin assets that automatically deploy across Ethereum lending protocols and other chain yield opportunities, with returns consolidated back to user accounts through ZetaChain's unified accounting.

### **Universal NFT Membership Protocol**

Enterprises can deploy Universal NFT passes that maintain consistent identity across all connected chains. Members can purchase NFTs on any supported chain and access privileges across multiple dApps without cross-chain bridging operations, leveraging unique cross-chain token identifiers.

### **Cross-Chain Payment Infrastructure**

Businesses can utilize Universal Tokens for streamlined multi-chain payroll and vendor payments. Payment initiations on ZetaChain automatically distribute assets to recipients across different blockchain networks, eliminating complex multi-wallet management and bridge dependencies.
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->


## **对 “全链应用 / Universal App 合约” 的直观理解**

universal App是zetachain上的一个范式，与传统方式需要在每条链上分别部署合约不同，全链应用允许我们**只在 ZetaChain 上部署一个主合约**。对于用户来说，无需切换网络，就可以直接从他们资产所在的任何链（包括bitcoin这样的非智能合约链）通过发送交易来调用Universal App合约

其核心价值就在于跨链协调，就像**今天workshop里面老师提到**的，用户无需掌握复杂的跨链流程，只需要使用universal App这样一个**黑盒**，进行功能的使用，然后所有复杂的路由逻辑都由部署在 ZetaChain 上的单一合约在后台完成。这大大便利了开发者的开发流程以及降低了用户的使用门槛

## **第一个 Hello World Demo**

-   对比hardhat/foundry:
    

通过查阅资料，发现hardhat强大在于庞大的插件生态系统和调试工具以及部署脚本灵活性，foundry则通过solidity实现了强大的性能，能够通过执行速度极快的测试

对于第一个Demo,我倾向于优先使用hardhat来完成快速原型开发。有多余的时间使用foundry,通过`@nomicfoundation/hardhat-foundry` 插件来实现无缝集成，以此升级测试速度和深度

-   Localnet/Testnet
    

Locatnet具有如下优势：

-   **极致速度与即时反馈**：交易打包和区块生成是即时的，无需等待，适合在开发过程中快速验证逻辑和迭代 。
    
-   **完全掌控与隐私**：可以随时重置整个网络状态，回到干净的起点 。所有开发和测试都在本地进行，代码在部署到主网前完全保密 。
    
-   **零成本测试**：可以无限生成测试用的以太币和任何ERC-20代币，无需担心资源耗尽
    

Testnet：

-   **真实环境验证**：测试网有真实的网络延迟、Gas竞争和区块时间，能暴露在Localnet中无法发现的时序、Gas或跨链交互问题。
    
-   **基础设施集成测试**：可以在这里完整测试DApp与钱包（如MetaMask）、区块浏览器、索引服务等第三方组件的集成情况。
    

对于第一个Demo,我将从Localnet出发进行开发工作，后续测试使用Testnet以发现更多开发中存在的问题，解决问题才能更好的进步😊

| 模块组件 |   |
| --- | --- |
| 智能合约 (业务逻辑) | 部署在 ZetaChain 上的单一合约，核心逻辑是记录/回应从任何链发来的消息。 |
| 跨链连接 (信息入口) | 用户在任何链（如以太坊、比特币）上向 ZetaChain 的 TSS 地址发送交易，附带消息数据，即可触发合约。 |
| 前端界面 (用户交互) | 一个 DApp 前端页面，内嵌通用钱包（ MetaMask），帮助用户在不同网络上构建并发送交易。 |
| RPC 节点 (链上通信) | 前端通过 RPC 提供商（如QuickNode、Alchemy）或 ZetaChain 的公共 RPC 与区块链网络交互。 |

### **核心设计**

**简单逻辑：**

-   用户从**任何支持的链**（如 Ethereum、BSC、Polygon）发送一条消息
    
-   合约记录并永久存储以下信息：
    
    -   消息内容（用户自定义）
        
    -   来源链 ID 和名称
        
    -   发送者地址（跨链映射后的地址）
        
    -   时间戳
        
    -   全局消息计数器
        

合约伪代码：

```
// 伪代码概念
function sendMessage(string memory message) external {
    // 记录跨链消息
    Message memory newMessage = Message({
        content: message,
        chainId: zetaContext.chainId,
        chainName: getChainName(zetaContext.chainId),
        sender: zetaContext.msgSender,
        timestamp: block.timestamp,
        messageId: totalMessages++
    });
    
    // 触发事件供前端监听
    emit MessageSent(
        newMessage.messageId,
        newMessage.content,
        newMessage.chainName,
        newMessage.sender
    );
}
```

这样既保持了helloworld的简洁，又体现出**zetachain universal App**的核心价值。
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->



## **今天的关键概念**

**通用区块链（Universal Blockchain）** 像 ZetaChain 这样的 L1，它自己就是一条链，但天生可以直接连很多其他链（Bitcoin / Ethereum / Solana 等），从一个中心统一管理不同链上的资产和调用。

直观理解：

-   一条专门用来“打通所有链”的 L1 公链，你可以把它想成“多链世界的操作系统”或“总控制台”。
    
-   它本身是一个 PoS L1（基于 Cosmos SDK），但内嵌了跨链能力，可以原生连接 Ethereum、Bitcoin、Solana 等各种链，包括没有智能合约的链（如 BTC）。\[
    

在实际应用中的作用：

-   对用户：只和 ZetaChain 打交道，却能在背后同时操作多条链上的资产和合约——不用频繁切网络、桥资产。
    
-   对开发者：只在一个地方（ZetaChain）写逻辑，就能控制多链资产/合约，相当于“只写一次，多链生效”。
    

**Universal EVM（通用 EVM）** ZetaChain 上的 EVM 版本。对开发者看起来像普通 EVM（Solidity、工具都能用），但它内置跨链能力：合约可以直接收、发来自其他链的消息和资产。

在实际应用中的作用：

-   开发者仍然写 Solidity，但多了“跨链能力”，比如：
    
    -   在一个合约函数里，同时读取 Ethereum 上某个池子的状态 + 操作 Bitcoin 资产 + 最终把结果发到 Solana 上。
        
    -   它是“通用应用 / Universal Apps”真正跑的执行环境。
        

**Omnichain Smart Contract（全链智能合约）**

部署在 ZetaChain 的合约，但可以操作“任意已连接链上的资产和数据”，所以叫 Omnichain。\[

与普通合约的对比：

-   普通合约：
    
    -   部署在哪条链，只能管理这条链上的状态和资产。
        
-   Omnichain 合约：
    
    -   部署在 ZetaChain 上，却能：
        
        -   管理 Ethereum、Bitcoin 等链上的资金金库；
            
        -   对这些链发交易（如调用 Uniswap、借贷协议等）；
            
        -   统一编排多链操作流程。
            

在实际应用中的作用：

-   大幅减少开发复杂度：
    
    -   过去：每条链部署一份合约 + 自己写消息传递/安全逻辑；
        
    -   现在：只要在 ZetaChain 部署一份 Omnichain 合约，由 ZetaChain 处理跨链消息与安全。
        
-   应用场景：跨链 DEX、跨链借贷、用 BTC 参与 DeFi 等。
    

**Universal App（通用应用）**

官方/代码层面的定义：

-   “Universal App 是部署在 ZetaChain 上、并且原生连接到其他区块链（Ethereum、BNB、Bitcoin 等）的智能合约（App）。”
    
-   这些应用通过 Gateway 接受来自各条链的调用和资产，统一在 ZetaChain 内部处理，再可以向外发起跨链操作。
    

具体例子（帮助理解应用价值）：

1）跨链支付 / 汇款

-   用户：只在 Bitcoin 钱包里签一笔交易，给某个 Universal App。
    
-   App：
    
    -   接收 BTC；
        
    -   在 ZetaChain 上把 BTC 换成 ZRC-20 USDC；
        
    -   再通过 Gateway 把 USDC 以原生 USDC 形式发到 Ethereum 的某个地址。
        

2）跨链 DEX / 聚合器

-   用户：在一个页面上输入“用 SOL 换成 Base 上的 ETH”；
    
-   Universal App：
    
    -   从 Solana 接收 SOL；
        
    -   中间可能经过多条链/多次 swap；
        
    -   最终在 Base 上把 ETH 打给用户；
        
    -   用户眼里只是一键 swap。
        

3）跨链借贷 / 做多

-   用 BTC 作为抵押，在多个 EVM 链上借出稳定币，再去做策略、挖矿，最后统一结算。
    

**Gateway** Gateway 是「入口 + 出口」：

-   在每一条外部链上（例如 Ethereum / Solana / Bitcoin），都有一个对应的 Gateway：
    
    -   EVM 链上是一个 Gateway 合约；
        
    -   Solana 上是一个 Gateway program；
        
    -   Bitcoin 上是一个由验证者共同管理的多签地址。 这些 Gateway 负责接收用户的交易，把“我要调用哪个 Universal App、要带多少代币、要执行什么操作”等信息传入 ZetaChain。
        
-   在 ZetaChain 链上也有一个 Gateway 合约：
    
    -   它负责把 Universal App 的指令执行到外部链上，比如：从 ZetaChain 把 ZRC-20 代币提到 Ethereum 变成原生 ETH，并且顺便在 Ethereum 上调用一个合约函数。
        

**各个概念之前的关系**

外围链用户/合约 → 各自链的 Gateway → ZetaChain（Universal Blockchain） → Universal EVM → Omnichain 合约 → Universal App → 再通过 Gateway 操作各外链资产和合约。

## **架构图**

```
                 [Bitcoin 链]
                      |
                 [Gateway-BTC]
                      |
[Solana 链] -- [Gateway-Solana]   [Ethereum 链] -- [Gateway-ETH]
                      \            /
                       \          /
                        \        /
                      [   ZetaChain   ]
                      [ Universal EVM ]
                      [ Universal Apps ]
                        /        \
                       /          \
          [Gateway-Zeta → 其它链调用/提币 ...]
​
```

## **拓展资料学习**

**1\. 为什么在 ZetaChain 上开发** 在我看来，在 ZetaChain 上开发的意义在于，它把多条区块链整合成一个统一的应用平台。我只需要在 ZetaChain 的 Universal EVM 上写一份合约，就可以同时调度 Bitcoin、Ethereum、Solana 等不同链上的资产和状态，而不用在每条链上分别部署合约、维护多套跨链桥逻辑，这大大降低了开发和安全成本。对用户而言，只需在自己熟悉的钱包和链上发起操作，后台由 ZetaChain 自动完成跨链路由和结算，使多链使用体验尽量接近“用一个应用、面对一个界面”的单链体验。

**2\. ZetaChain 架构设计概览** 我理解的 ZetaChain 架构是：以一条 PoS 公链为中心，在链上运行 Universal EVM 和 omnichain 合约，由验证人网络去观察外部区块链的事件，并通过各链上的 Gateway 完成资产和消息的跨链转发。外部资产会在 ZetaChain 上被映射成统一标准的代币，这样合约可以像操作本地资产一样统一管理多链资金。整体上它采用“中心 Hub + 多条辐射链”的模式，把复杂的多对多跨链关系简化为一条主链与多条外链的连接，为上层的 Universal App 提供一个简单而统一的多链基础设施。
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->




## **Getting start（CLI）**

测试官网的运行指令（在本地跑起3条链）：

```
 zetachain localnet start                                                                                                        ✔  21:32:50 
[LOCALNET] Starting anvil on port 8545 with args: -q
[LOCALNET] Skipping Ton...
[LOCALNET] Skipping Solana...
[LOCALNET] Skipping Sui...
[LOCALNET] EVM default wallet address: 0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266
[LOCALNET] EVM default wallet private key: 0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80
[LOCALNET] Default wallet mnemonic: test test test test test test test test test test test junk
​
Ethereum (11155112)
┌───────────────┬────────────────────────────────────────────┐
│ Contract      │ Address                                    │
├───────────────┼────────────────────────────────────────────┤
│ erc20Custody  │ 0xa85233C63b9Ee964Add6F2cffe00Fd84eb32338f │
├───────────────┼────────────────────────────────────────────┤
│ gateway       │ 0x09635F643e140090A9A8Dcd712eD6285858ceBef │
├───────────────┼────────────────────────────────────────────┤
│ zetaConnector │ 0x67d269191c92Caf3cD7723F116c85e6E9bf55933 │
├───────────────┼────────────────────────────────────────────┤
│ zetaToken     │ 0x7a2088a1bFc9d81c55368AE168C2C02570cB814F │
├───────────────┼────────────────────────────────────────────┤
│ USDC.ETH      │ 0x1fA02b2d6A771842690194Cf62D91bdd92BfE28d │
└───────────────┴────────────────────────────────────────────┘
​
​
ZetaChain (31337)
┌───────────────────┬────────────────────────────────────────────┐
│ Contract          │ Address                                    │
├───────────────────┼────────────────────────────────────────────┤
│ gateway           │ 0xB7f8BC63BbcaD18155201308C8f3540b07f84F5e │
├───────────────────┼────────────────────────────────────────────┤
│ uniswapV2Factory  │ 0xe7f1725E7734CE288F8367e1Bb143E90bb3F0512 │
├───────────────────┼────────────────────────────────────────────┤
│ uniswapV2Router02 │ 0x9fE46736679d2D9a65F0992F2272dE9f3c7fa6e0 │
├───────────────────┼────────────────────────────────────────────┤
│ uniswapV3Factory  │ 0xCf7Ed3AccA5a467e9e704C703E8D87F634fB0Fc9 │
├───────────────────┼────────────────────────────────────────────┤
│ uniswapV3Router   │ 0xa513E6E4b8f2a923D98304ec87F64353C4D5C853 │
├───────────────────┼────────────────────────────────────────────┤
│ zetaToken         │ 0x5FbDB2315678afecb367f032d93F642f64180aa3 │
├───────────────────┼────────────────────────────────────────────┤
│ ZRC-20 ETH.ETH    │ 0x2ca7d64A7EFE2D62A725E2B35Cf7230D6677FfEe │
├───────────────────┼────────────────────────────────────────────┤
│ ZRC-20 USDC.ETH   │ 0xd97B1de3619ed2c6BEb3860147E30cA8A7dC9891 │
├───────────────────┼────────────────────────────────────────────┤
│ ZRC-20 BNB.BNB    │ 0x65a45c57636f9BcCeD4fe193A602008578BcA90b │
├───────────────────┼────────────────────────────────────────────┤
│ ZRC-20 USDC.BNB   │ 0x05BA149A7bd6dC1F937fA9046A9e05C05f3b18b0 │
└───────────────────┴────────────────────────────────────────────┘
​
​
BNB (98)
┌───────────────┬────────────────────────────────────────────┐
│ Contract      │ Address                                    │
├───────────────┼────────────────────────────────────────────┤
│ erc20Custody  │ 0x70e0bA845a1A0F2DA3359C97E0285013525FFC49 │
├───────────────┼────────────────────────────────────────────┤
│ gateway       │ 0x0E801D84Fa97b50751Dbf25036d067dCf18858bF │
├───────────────┼────────────────────────────────────────────┤
│ zetaConnector │ 0x9d4454B023096f34B160D6B654540c56A1F81688 │
├───────────────┼────────────────────────────────────────────┤
│ zetaToken     │ 0x99bbA657f2BbC93c02D617f8bA121cB8Fc104Acf │
├───────────────┼────────────────────────────────────────────┤
│ USDC.BNB      │ 0xf953b3A269d80e3eB0F2947630Da976B896A8C5b │
└───────────────┴────────────────────────────────────────────┘
​
```

在shell中创建自己的钱包

```
npx zetachain accounts create --name mywallet                                                                            
Creating accounts for all supported types...
EVM account created successfully!
Key saved to: /home/eeeeye/.zetachain/keys/evm/mywallet.json
Address: 0x784f02F6a80c0D2Dc8485b9F05Ce2A549EDa60A3
SOLANA account created successfully!
Key saved to: /home/eeeeye/.zetachain/keys/solana/mywallet.json
Address: GNPPUzwT1jn4VroozW7BoPr2XdvMBXtgXD1aFLTUDZiG
SUI account created successfully!
Key saved to: /home/eeeeye/.zetachain/keys/sui/mywallet.json
Address: 0x98aec9ebf7d1797313c7e863e4ef4e7f89c7e87286f1e7375ca24934180e98ba
BITCOIN account created successfully!
Key saved to: /home/eeeeye/.zetachain/keys/bitcoin/mywallet.json
Address: bc1qy3tafmggfkgp5fappxf5cqsswg2zms7haru0r9
TON account created successfully!
Key saved to: /home/eeeeye/.zetachain/keys/ton/mywallet.json
Address: 0QCvck3o6tKGPvzkairgUWnWO0-vCKE0RvQdHZalpD1ak3p8
```

在shell中创建自己的钱包

```
npx zetachain accounts create --name mywallet                                                                            
Creating accounts for all supported types...
EVM account created successfully!
Key saved to: /home/eeeeye/.zetachain/keys/evm/mywallet.json
Address: 0x784f02F6a80c0D2Dc8485b9F05Ce2A549EDa60A3
SOLANA account created successfully!
Key saved to: /home/eeeeye/.zetachain/keys/solana/mywallet.json
Address: GNPPUzwT1jn4VroozW7BoPr2XdvMBXtgXD1aFLTUDZiG
SUI account created successfully!
Key saved to: /home/eeeeye/.zetachain/keys/sui/mywallet.json
Address: 0x98aec9ebf7d1797313c7e863e4ef4e7f89c7e87286f1e7375ca24934180e98ba
BITCOIN account created successfully!
Key saved to: /home/eeeeye/.zetachain/keys/bitcoin/mywallet.json
Address: bc1qy3tafmggfkgp5fappxf5cqsswg2zms7haru0r9
TON account created successfully!
Key saved to: /home/eeeeye/.zetachain/keys/ton/mywallet.json
Address: 0QCvck3o6tKGPvzkairgUWnWO0-vCKE0RvQdHZalpD1ak3p8
​
```

## **Faucet**

**将metamask的Ethereum钱包地址放入**[**https://zetachain.faucetme.pro/**](https://zetachain.faucetme.pro/)

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Eeeeye/images/2025-11-25-1764085622471-image.png)

在钱包中检查是否收到测试代币：

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Eeeeye/images/2025-11-25-1764085733607-image.png)

## **Explorer**

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Eeeeye/images/2025-11-25-1764076859772-image.png)

## **RPC & Contracts**

RPC 就是让你的电脑与区块链节点通信的入口，是区块链的 API 网关，所有读写都要通过它。

通读了官方文档中的以下部分

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Eeeeye/images/2025-11-25-1764077681273-image.png)

学习了4 种智能合约类型：

| 合约 | 类比 | 用途 |
| --- | --- | --- |
| connector | 跨链大脑 | 验证跨链消息、路由消息 |
| erc20Custody | 银行保险柜 | 锁仓原链 ERC20，映射到 ZetaChain |
| gateway | 跨链 API | 用户和 dApp 调用的入口 |
| tss | 多签安全系统 | 验证 ZetaChain 节点签名，确保安全跨链 |

## Qwen

-   虚拟环境启动：
    

source .venv/bin/activate

-   添加如下代码到qwen\_test.py解决代理问题：
    

os.environ\["HTTP\_PROXY"\] = ""

os.environ\["HTTPS\_PROXY"\] = ""

os.environ\["ALL\_PROXY"\] = ""

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Eeeeye/images/2025-11-25-1764086493033-image.png)
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->





## **Task1**

```
 npm install -g zetachain         
zetachain query chains list
```

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Eeeeye/images/2025-11-24-1764000144501-image.png)

## **Task2**

![截图 2025-11-24 23-39-08.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Eeeeye/images/2025-11-24-1764000078427-___2025-11-24_23-39-08.png)

generate the API key and use python visual environment to run [test.sh](http://test.sh)

here is the content of .sh(the "DASHSCOPE\_API\_KEY" has been set to my API\_KEY:

```
import os
from openai import OpenAI
​
client = OpenAI(
    # 若没有配置环境变量，请用百炼API Key将下行替换为：api_key="sk-xxx",
    api_key=os.getenv("DASHSCOPE_API_KEY"),
    base_url="https://dashscope.aliyuncs.com/compatible-mode/v1",
)
completion = client.chat.completions.create(
    model="qwen3-max",
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "你是谁？"},
    ],
    stream=True
)
for chunk in completion:
    print(chunk.choices[0].delta.content, end="", flush=True)
```

![截图 2025-11-24 23-53-41.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Eeeeye/images/2025-11-24-1764000048047-___2025-11-24_23-53-41.png)

## **Task3**

Join the community in wechat

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Eeeeye/images/2025-11-24-1764000110205-image.png)
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
