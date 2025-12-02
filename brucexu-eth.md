---
timezone: UTC+12
---

# Bruce Xu

**GitHub ID:** brucexu-eth

**Telegram:** @brucexu_eth

## Self-introduction

Learning AI and Web3.

## Notes

<!-- Content_START -->
# 2025-12-02
<!-- DAILY_CHECKIN_2025-12-02_START -->
今天主要看 Qwen AI 相关的内容。
<!-- DAILY_CHECKIN_2025-12-02_END -->

# 2025-11-29
<!-- DAILY_CHECKIN_2025-11-29_START -->

[https://www.zetachain.com/docs/developers/evm](https://www.zetachain.com/docs/developers/evm)

ZetaChain uses a hub-and-spoke architecture:

-   Hub: ZetaChain, the main coordination layer for all cross-chain activity.
    
-   Spokes: External blockchains (EVM, Solana, Sui, Ton, and Bitcoin) connected with standardized protocols.
    

All cross-chain messages and transactions pass through ZetaChain, ensuring consistent handling, easier integration of new chains, and a single point for enforcing security and validation rules.

**Observer-Signer Validators**

-   Monitor ZetaChain and connected chains for cross-chain events.
    
-   Vote on event validity; upon majority agreement, coordinate outbound transactions.
    
-   Sign outbound transactions using a Threshold Signature Scheme (TSS) so no single validator controls the signing key.
    

* * *

ZetaChain is used as a ledger for cross chain events.

## [**Protocol Contracts**](https://www.zetachain.com/docs/developers/evm#protocol-contracts)

To enable interaction between users, applications, and the ZetaChain network, protocol contracts are deployed both on ZetaChain and on connected external chains.

**On ZetaChain**

| Contract | Purpose |
| --- | --- |
| GatewayZEVM | Primary entry point for outbound transactions. Handles asset withdrawals, external contract calls, and ZRC-20 mint/burn logic. |
| ZRC-20 | ERC-20–compliant tokens representing assets from connected chains, enabling fungible asset transfers within ZetaChain. |
| ContractRegistry | Stores and provides metadata for deployed protocol contracts (e.g., gateway, ZRC-20s) to ensure consistent references across the network. |

**On Connected EVM Chains**

| Contract | Purpose |
| --- | --- |
| GatewayEVM | Entry point for inbound transactions. Handles deposits, contract calls to ZetaChain, and emits events for observers to track. |
| ERC20Custody | Holds ERC-20 assets deposited for cross-chain transfers, ensuring secure custody until transactions are processed. |
| ContractRegistry | Stores and provides metadata for deployed protocol contracts on the connected chain, allowing clients and services to locate the correct contract addresses. |

**On Other Connected Chains (Solana, Sui, TON, etc.)**

| Contract | Purpose |
| --- | --- |
| Gateway | Entry point for initiating and receiving cross-chain transactions between the connected chain and ZetaChain. Functions are adapted to the chain’s native runtime (e.g., Solana program, Sui Move module, TON smart contract). |

## [**Cross-Chain Transaction Workflow**](https://www.zetachain.com/docs/developers/evm#cross-chain-transaction-workflow)

The process differs for **incoming** (connected chain → ZetaChain) and **outgoing** (ZetaChain → connected chain) transactions, but both follow a secure, validator-driven workflow.

ZetaChain removes much of the friction of building cross-chain applications. Instead of juggling different SDKs, bridges, and security models, you get a single platform that handles cross-chain messaging, asset movement, and contract calls for you.

TODO make a cross chain tx to have a try on mainnet and Sol

[https://www.zetachain.com/docs/developers/evm/gateway](https://www.zetachain.com/docs/developers/evm/gateway)

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/brucexu-eth/images/2025-11-29-1764381138415-image.png)

[https://www.zetachain.com/docs/developers/evm/gas](https://www.zetachain.com/docs/developers/evm/gas)

TODO Ethereum’s EIP 1559 fee model

## [**Outgoing Calls and Withdrawals**](https://www.zetachain.com/docs/developers/evm/gas#outgoing-calls-and-withdrawals)

Universal apps on ZetaChain can initiate calls to contracts on connected chains or facilitate withdrawals of ZRC-20 tokens back to a connected chain. These operations require a “withdraw gas fee,” which is calculated based on the gas limit of the target chain.

It’s important to query the current gas fee, approve the Gateway to spend the necessary amount, and ensure the gas ZRC-20 token balance is sufficient. If the Gateway cannot transfer the required fee to itself, the operation will fail.

ZRC-20 token refers to the outgoing chain’s ZRC-20 tokens, so you need to estimate the gas first, then submit the tx. ZetaChain will help you to send TXs with same (or similar) gas settings to make it work. In this way, the gas token will be deducted from ZetaChain directly. ZetaChain can even get some tips if they want to. Similar to CEX withdrawal.

| Chain ID | ZRC-20 | Fee Amount | Fee Token |
| --- | --- | --- | --- |
| 1 | USDC.ETH | 0.0000222162735 | ETH.ETH |
| 1 | PEPE.ETH | 0.0000222162735 | ETH.ETH |

To calculate fees for a different gas limit, please, check out use the `query fees` command:

```
npx zetachain query fees
```

[https://www.zetachain.com/docs/developers/evm/cctx](https://www.zetachain.com/docs/developers/evm/cctx)

Cross-chain transactions (CCTXs) can be classified into two main types: incoming and outgoing.

**Incoming transactions** (connected chain → ZetaChain) are initiated on a connected chain and result in a transaction on ZetaChain. An incoming transaction consists of two transactions:

-   Inbound: a transaction is initiated and observed on the connected chain.
    
-   Outbound: the corresponding transaction is broadcasted and executed on ZetaChain.
    

**Outgoing transactions** (ZetaChain → connected chain) are initiated on ZetaChain and result in a transaction on a connected chain. An outgoing transaction consists of two transactions:

-   Inbound: A transaction is initiated and observed on ZetaChain.
    
-   Outbound: The corresponding transaction is broadcasted and executed on the connected chain.
    

Tracking a CCTX involves querying ZetaChain’s Cosmos SDK HTTP API with an inbound transaction hash to get a CCTX hash.

TODO intents on ZetaChain?

TODO What will happen if ZetaChain doesn’t have enough token on the connected outgoing chain? For example, if I want to swap 1eth from mainnet to BNB on BSC, but on ZetaChain contract on BSC, only 1BNB left. What will happen?

## What is Threshold Signature Scheme?

In a _t-of-n_ TSS, the private key is divided into _n_ unique shares (held by different parties) and at least _t_ of those parties must collaborate to sign a transaction. This approach is designed to eliminate the single point of failure inherent in traditional single-key wallets: no single person holds the entire private key, so compromising one share does not compromise the whole key

TSS has emerged as a solution in blockchain systems to enhance security and trust. By distributing key control among multiple parties, TSS ensures that **“the key never exists”** in one place
<!-- DAILY_CHECKIN_2025-11-29_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->



[https://www.zetachain.com/docs/start/app](https://www.zetachain.com/docs/start/app)

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/brucexu-eth/images/2025-11-27-1764275585407-image.png)

ZetaChain is like a bridge.

In this example a universal app:

-   accepts 6 ETH and "I want BNB" (bytes that represent the destination token) from Ethereum
    
-   swaps 6 ZRC-20 ETH for ZRC-20 BNB using a decentralized exchange on ZetaChain
    
-   calls the Gateway contract to withdraw ZRC-20 BNB and call a contract on the BNB chain
    

Of course, a single universal app call can trigger more than one call to different chains.

DeFi and Oracle is very important.

TODO implement a demo to use ETH buy BTC on BTC.

* * *

Incoming call to universal apps do not incur additional gas costs. But outgoing calls from universal apps require gas, need to deposit ZRC-20 gas token.

In practice it means that a universal app would swap a fraction of the incoming ZRC-20 token to the ZRC-20 of the gas token of the chain to which an outgoing call is made. When an outgoing call or withdrawal is made, a required amount of ZRC-20 gas token is deducted to cover the gas fees on the destination chain.

TODO how ZRC-20 and gas fee calculated? if the token price is uncertain?

* * *

[https://www.zetachain.com/docs/start/build](https://www.zetachain.com/docs/start/build)

One of the core capabilities of universal apps on ZetaChain is the ability to transfer value using native tokens across all connected chains. This means users can perform transactions involving the actual native assets of each blockchain, rather than wrapped or synthetic versions.

ZetaChain runs smart contracts on its Universal Ethereum Virtual Machine (UEVM), which is fully compatible with Ethereum's EVM.

Another compelling feature of ZetaChain is its Gasless EVM Execution Environment. Universal apps run on ZetaChain's EVM and can be called by users and contracts from all connected chains. When a universal app is called from a connected chain, it requires gas payment only on that connected chain.

Execution on ZetaChain does not impose any additional fees, making ZetaChain a cost-effective, gasless environment for deploying contracts.

TODO how ZetaChain make profits?

* * *

[https://www.zetachain.com/docs/developers/architecture/overview](https://www.zetachain.com/docs/developers/architecture/overview)

At a high level, ZetaChain is a Proof of Stake (PoS) blockchain built on the Cosmos SDK and Comet BFT consensus engine.

TODO check Cosmos [https://github.com/cosmos/cosmos-sdk](https://github.com/cosmos/cosmos-sdk)

Contained within each validator is the ZetaCore and ZetaClient. ZetaCore is responsible for producing the blockchain and maintaining the replicated state machine. ZetaClient is responsible for observing events on connected chains and signing outbound transactions.

Validators are comprised of 2 different roles: Core Validators and Observer-Signer Validators. Fees from transactions and rewards are distributed to validators in return for their service of processing transactions and keeping the network secure.
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->




Created a new education tool for anaylzing solidity [https://github.com/brucexu-eth/evm-runtime-visualizer](https://github.com/brucexu-eth/evm-runtime-visualizer) with Gemini 3 Pro.

## Storage vs Memory vs Calldata

```
function sumArray(uint[] memory array) public pure returns (uint) {
    uint sum = 0;
    for (uint i = 0; i < array.length; i++) {
        sum += array[i];
    }
    return sum;
}
```

memory in the function params indicate variable array is a temporary variable only exists during function execution. It won't store in the storage, but it might be changed later in the function.

Calldata is used for function arguments that are passed in from an external caller, such as a user or another smart contract. Calldata is read-only, meaning that it cannot be modified by the function.

```
function isOwner(address ownerAddress) public view returns (bool) {
    return ownerAddress == msg.sender;
}
```

ownerAddress is defined in calldata, and it will be loaded in calldata, and read-only. therefore, gas cost is lowest.

The function only needs to read the value of `ownerAddress` to compare it to the `msg.sender` value, so it doesn’t need to be stored in memory or storage.

```
contract ColorStorage {
    mapping(address => string) private favoriteColors;
    
    function setFavoriteColor(string calldata vs memory color) public {
        favoriteColors[msg.sender] = color;
    }
    
    function getFavoriteColor(address userAddress) public view returns (string memory) {
        return favoriteColors[userAddress];
    }
}
```

Use calldata:

Deployment gas

| transaction cost | 421340 gas |
| execution cost | 341378 gas |

Set color: red gas

| transaction cost | 24854 gas |
| execution cost | 3346 gas |

Use memory:

Deployment gas

| transaction cost | 454211 gas |
| execution cost | 371809 gas |

Set color: red gas

| transaction cost | 25317 gas |
| execution cost | 3809 gas |

Slightly reduced.

```
TypeError: Data location can only be specified for array, struct or mapping types, but "calldata" was given.
 --> test.sol:8:31:
  |
8 |     function getFavoriteColor(address calldata userAddress) public view returns (string memory) {
  |           
```

Only specify data location for complex data types.

In summary, `memory` is used for temporary variables that are only needed during the execution of a function, `calldata` is used for function arguments that are passed in from an external caller and cannot be modified, and `storage` is used to permanently store data on the blockchain that can be accessed and modified by any function within the contract.
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->





Qwen API + SDK installed.

Starting with Qwen3, the models are named using the scheme `Qwen3[-size][-type][-date]`:

-   `size`: the notation of the structure and the parameter counts. Dense models use the total saved parameters, e.g., `4B` and `32B`, while MoE models use the total saved parameters and the activated parameters for each token with a prepended `A`, e.g., `30B-A3B` and `235B-A22B`.
    
-   `type`: there are currently 4 types:
    
    -   `-Instruct`: the instruction following models that follow the predefined chat template, used for conducting tasks in conversations, downstream fine-tuning, etc.
        
    -   `-Thinking`: the thinking models that follow the predefined chat template and use chain-of-thoughts (CoT) to think deeply about the questions, used for solving complex problems.
        
    -   `-Base`: the pre-trained models that do not know the predefined chat template, used for in-context learning, downstream fine-tuning, etc.
        
    -   No type: the models with hybrid thinking modes.
        
-   `date`: the released date in yearmonth format, e.g., `2507`.
    

## **Tokens & Tokenization**

token 代表模型处理和生成的基本单位。它们可以表示人类语言中的文本（常规 token），或者表示特定功能，如编程语言中的关键字（控制 token [\[1\]](https://qwen.readthedocs.io/zh-cn/latest/getting_started/concepts.html#special)）。通常，使用 tokenizer 将文本分割成常规 token ，这些 token 可以是单词、子词或字符，具体取决于所采用的特定 tokenization 方案，并按需为 token 序列添加控制 token 。词表大小，即模型识别的唯一 token 总数，对模型的性能和多功能性有重大影响。大型语言模型通常使用复杂的 tokenization 来处理人类语言的广阔多样性，同时保持词表大小可控。Qwen 词表相对较大，有 15 1646 个 token。

工作原理：用 gemini canvas 做了个交互式动画，真的很赞

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/brucexu-eth/images/2025-11-25-1764104561324-image.png)

LLM 的背后是一层层 Transformer 里向量计算，所以第一步需要将文本转换成对应的 token id。有的 token 可能是一个单词或者文字，有的可能是两个，也可能是一个长单词切割成不同的。所以计算非常简单，1K Token 上下文，就是 1-2K 的字符或者 words，然后可以换算成具体的文章和输入的文本数量。

那么 200K token 的 context 就约等于：

**~1 本长篇小说** 200k Token 大约等于 25-30 万汉字。相当于把《三体》第一部输入进去还有富余。

**150+ 页 PDF 文档** 可以一次性读完几份复杂的财报、法律合同或学术论文，并进行跨文档分析。

**10h 会议录音转录** 即使是长达数小时的会议记录，模型也能从头读到尾，总结全程细节。

**词表大小 (Vocabulary Size) 的意义**

你提到的 **Qwen 有 151,646 个 Token**，这就是它的词表大小。你可以把它想象成模型随身携带的“新华字典”。

-   **词表大**意味着模型认识更多的“整词”。比如“人工智能”，大词表可能把它作为一个 ID (Token)；小词表可能要把它拆成“人工”+“智能” (2个 Token)。
    
-   **效率高**词表越大，表示同样一段话所需的 Token 数量越少。这对中文尤其重要（因为很多英文模型词表对中文支持不好，导致中文被拆得稀碎，特别费钱费算力）。
    
-   **代价**词表也不是越大越好。词表越大，模型预测下一个 Token 时，需要从更多选项中做选择（计算量变大），且模型的“嵌入层”参数量会显著增加。
    

## Token 数量 vs. KB / MB 文件大小

容易混淆的点：

1.  **token 数量**
    
    -   是“模型视角”的长度。
        
    -   需要先过一遍 tokenizer，算“切出来多少块 token”。
        
2.  **文件大小（KB / MB）**
    
    -   是“存储视角”的长度。
        
    -   跟编码方式（UTF-8/UTF-16）、空格、换行、图片、压缩等都有关。
        

同一个 1MB 的 PDF 文件：

-   如果大量是英文 + 很多空格，token 可能相对少。
    
-   如果是高密度中文或代码，token 可能很多。
    

所以：

> 不能简单用“文件多少 MB”推断能不能塞进 200k 上下文；  
> 更准确的做法是用对应 tokenizer 实际数一遍 token 数量。

### 如何估算 token 和内容长度

-   不同模型 / tokenizer 下，**1 token ≠ 固定字数**：
    
    -   英文里常用估算：1 token ≈ 0.75 个英文单词。
        
    -   中文里，很多 tokenizer 会把一个汉字或常用词当作 1 个 token，但有时会多一点或少一点。
        
-   200k 是**总上下文**：包括 system prompt、你所有历史对话、文件内容，以及模型输出本身。
    

### 需要警惕的一点：有效记忆 vs 标称 context

现在很多文章和 benchmark 都在讨论：  
虽然标称 200k、1M，但在**超过 50–80k 左右**时，模型对远端信息的准确检索会明显变差，尤其是复杂推理任务。[Stefano Demiliani+1](https://demiliani.com/2025/11/02/understanding-llm-performance-degradation-a-deep-dive-into-context-window-limits/?utm_source=chatgpt.com)

所以，实战里你可以理解为：

> 200k 是“最大工作记忆空间”，  
> 真正在里面做高精度查找/推理时，**有效带宽往往低于这个上限**，要配合分块、索引、RAG 等手段。

TODO [https://qwen.readthedocs.io/zh-cn/latest/index.html](https://qwen.readthedocs.io/zh-cn/latest/index.html) 模型的功能整个本地跑一遍，如果涉及到微调等，使用低复杂度开源模型
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->






[https://www.zetachain.com/docs/developers/tutorials/hello/](https://www.zetachain.com/docs/developers/tutorials/hello/)

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/brucexu-eth/images/2025-11-25-1764033488074-image.png)

初始化 example 之后，这个 onCall 方法就是用于做跨链使用的。通过发送消息，然后通知其他的链接上的链进行操作。

```
forge create Universal --rpc-url https://zetachain-athens-evm.blockpi.network/v1/rpc/public --private-key $PKK --broadcast
[⠊] Compiling...
No files changed, compilation skipped
Deployer: 0xD41dC0b5c27c8554D5fb0495279151d4E5500e3B
Deployed to: 0x3392828C9D464D2222F5b6F720B6Ca2772685940
Transaction hash: 0xfbafa81ee69fd294714a73e6b0bf65b1c1a0a225132a9812c2dd6a2c07ffd997
```

My First ZetaChain TX.

测试币水龙头 [https://www.zetachain.com/docs/reference/faucet/](https://www.zetachain.com/docs/reference/faucet/)

84532 │ base\_sepolia

```
zetachain evm call --receiver 0x3392828C9D464D2222F5b6F720B6Ca2772685940 --chain-id 84532 --types string --values alice --private-key $PKK

From:   0xD41dC0b5c27c8554D5fb0495279151d4E5500e3B
To:     0x3392828C9D464D2222F5b6F720B6Ca2772685940 on ZetaChain
Call on revert: false

Contract call details:
Function parameters: alice
Parameter types: ["string"]

? Proceed with the transaction? yes
Transaction hash: 0xe8427df51c87cfba654dbc05937e5b445a3c5ea528f9285ffbb5041651683a6a
```

具体的交易 params on base，sent to zetachain gateway on base, includes params, payload, etc.

```
Function: call(address, bytes, (address,bool,address,bytes,uint256))
#	Name	Type	Data
1	receiver	address
0x3392828C9D464D2222F5b6F720B6Ca2772685940
2	payload	bytes
0x00000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000005616c696365000000000000000000000000000000000000000000000000000000
2	revertOptions.revertAddress	address
0xD41dC0b5c27c8554D5fb0495279151d4E5500e3B
2	revertOptions.callOnRevert	bool
false
2	revertOptions.abortAddress	address
0x0000000000000000000000000000000000000000
2	revertOptions.revertMessage	bytes
0x
2	revertOptions.onRevertGasLimit	uint256
200000
```

[https://testnet.zetascan.com/tx/0x24450579034209dddbd436c15ad54c5634d0a1b30a5d0cb89c985d07f3d06a03](https://testnet.zetascan.com/tx/0x24450579034209dddbd436c15ad54c5634d0a1b30a5d0cb89c985d07f3d06a03)

TODO bytes 能存多少数据？如果是一个很复杂的跨链合约调用，如何传递参数呢？

```
zetachain q cctx --hash 0xe8427df51c87cfba654dbc05937e5b445a3c5ea528f9285ffbb5041651683a6a
84532 → 7001 ✅ OutboundMined
CCTX:     0xb1438fb6c86ddbc325fbd99266e63431906e5f55c60ad4b97cd4a7865d1b6402
Tx Hash:  0xe8427df51c87cfba654dbc05937e5b445a3c5ea528f9285ffbb5041651683a6a (on chain 84532)
Tx Hash:  0x24450579034209dddbd436c15ad54c5634d0a1b30a5d0cb89c985d07f3d06a03 (on chain 7001)
Sender:   0xD41dC0b5c27c8554D5fb0495279151d4E5500e3B
Receiver: 0x3392828C9D464D2222F5b6F720B6Ca2772685940
Message:  00000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000005616c696365000000000000000000000000000000000000000000000000000000
```

[https://sepolia.basescan.org/tx/0xe8427df51c87cfba654dbc05937e5b445a3c5ea528f9285ffbb5041651683a6a](https://sepolia.basescan.org/tx/0xe8427df51c87cfba654dbc05937e5b445a3c5ea528f9285ffbb5041651683a6a)

zetachain q cctx --help

Usage: zetachain query cctx \[options\]

Queries the real-time status of a cross-chain transaction by its inbound transaction hash. You can control

polling frequency, timeout, and target RPC endpoint.

大概工作流程：

1.  在 base 发送了一个 call 包括调用的跨链合约、method、参数等到 zetachain 的 base gateway
    
2.  链下的代码监听事件，然后操作 zetachain 的地址 **0x735b14BB79463307AAcBED86DAf3322B1e6226aB** 来在 zetachain 上面重放请求，包括地址和 message
    
3.  之后就调用了合约的对应方法，并且在 zetachain 的合约打印了 event 消息
    

这样实现了跨链功能。

TODO ZetaChain 的 0x6c533f7fE93fAE114d0954697069Df33C9B74fD7 GatewayZEVM 负责重放请求，代码实现是怎么样的？了解一下 [https://testnet.zetascan.com/tx/0x24450579034209dddbd436c15ad54c5634d0a1b30a5d0cb89c985d07f3d06a03](https://testnet.zetascan.com/tx/0x24450579034209dddbd436c15ad54c5634d0a1b30a5d0cb89c985d07f3d06a03) [https://testnet.zetascan.com/address/0xe91D93D4eFd6347bD9fA5fdb5B24394CFB22a6Df?tab=contract](https://testnet.zetascan.com/address/0xe91D93D4eFd6347bD9fA5fdb5B24394CFB22a6Df?tab=contract)

TODO 看起来 ZetaChain 支付了跨链消息的 gas，在经济性上如何形成闭环？越多跨链请求，似乎是越多 gas 消耗，而且是各个链的。

TODO UniversalContract 的合约代码包括什么？具体实现什么功能？import "@zetachain/protocol-contracts/contracts/zevm/interfaces/UniversalContract.sol";

A universal contract must implement the `onCall` function. This function is triggered when the contract receives a call from a connected chain via the Gateway. The function processes incoming data, which includes:

-   `context`: A `MessageContext` struct containing:
    
    -   `chainID`: The chain ID of the connected chain that initiated the cross-chain call.
        
    -   `sender`: The address (EOA or contract) that called the Gateway on the connected chain.
        
    -   `origin`: Deprecated.
        
-   `zrc20`: The address of the ZRC-20 token representing assets from the source chain.
    
-   `amount`: The amount of tokens transferred.
    
-   `message`: The encoded payload data.
    

In this example, `onCall` decodes the message into a string and emits an event.

`onCall` should only be called by the Gateway to ensure that it is only called as a response to a call on a connected chain and that you can trust the values of the function parameters. This is enforced by the `onlyGateway` modifier, which is inherited from `UniversalContract`.

* * *

[https://getfoundry.sh/guides/project-setup/soldeer/](https://getfoundry.sh/guides/project-setup/soldeer/)

Foundry has been using git submodules to handle dependencies up until now.

A new approach has been in the making, [soldeer.xyz](http://soldeer.xyz), which is a Solidity native dependency manager built in Rust and open sourced (check the repository [https://github.com/mario-eth/soldeer](https://github.com/mario-eth/soldeer)).

```
forge soldeer install @openzeppelin-contracts~5.0.2
```

This will download the dependency from the central repository and install it into a `dependencies` directory.

Soldeer can manage two types of dependency configuration: using `soldeer.toml` or embedded in the `foundry.toml`. In order to work with Foundry, you have to define the `[dependencies]` config in the `foundry.toml`. This will tell the `soldeer CLI` to define the installed dependencies there. E.g.

```
# Full reference https://github.com/foundry-rs/foundry/tree/master/crates/config [profile.default]auto_detect_solc = falsebytecode_hash = "none"fuzz = { runs = 1_000 }libs = ["dependencies"] # <= This is important to be addedgas_reports = ["*"] [dependencies] # <= Dependencies will be added under this config"@openzeppelin-contracts" = { version = "5.0.2" }"@uniswap-universal-router" = { version = "1.6.0" }"@prb-math" = { version = "4.0.2" }forge-std = { version = "1.8.1" }
```

* * *

Solidity Bytes

`bytes` 本身是「可变长度字节数组」，理论上没有 Solidity 级别的固定上限，长度用 `uint256` 存，所以上限是 `2^256-1` 个字节，但在现实里远远达不到，真正限制是 gas 和区块 gas limit。

### 固定长度：`bytes1` ~ `bytes32`

-   编译期长度固定，不能变。
    
-   占用的 storage 空间 = 对应字节数（放在 32 字节 slot 里）。
    

关于 bit bytes 之类的知识 [https://x.com/brucexu\_eth/status/1902631404862881952](https://x.com/brucexu_eth/status/1902631404862881952)

### 单个 `bytes` 状态变量在 storage 中的布局（简单理解版）

只需要记住：

-   如果长度 ≤ 31 字节：
    
    -   整个 `bytes`（数据 + 长度）会塞在同一个 32 字节的 storage slot 中。
        
-   如果长度 > 31 字节：
    
    -   slot 里存的是「长度 + 数据起始位置标记」，真正的数据拆开放在后续若干 slot（从 `keccak256(slot)` 开始）。
        

这意味着：

-   小 `bytes`（例如 20 字节地址、32 字节哈希等）存储很省空间。
    
-   长 `bytes` 会分布在很多 slot，上下链和修改都会更贵。
    

* * *

How to decode AIB-encoded string like:

```
00000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000005616c696365000000000000000000000000000000000000000000000000000000
```

### 1\. Break the string into 32-byte words

The decoded message hidden in that EVM bytecode is the string: **"alice"**.

This is a standard **ABI-encoded string**. The Ethereum Virtual Machine (EVM) processes data in 32-byte "words" (which represent 64 hexadecimal characters).

Here is the step-by-step breakdown of how to decode it.

* * *

### 1\. Break the string into 32-byte words

To make sense of the data, we split the long string into chunks of 64 characters (32 bytes) each.

**Word 1 (The Offset):** `0000000000000000000000000000000000000000000000000000000000000020`

-   **Value:** Hex `0x20` = Decimal `32`.
    
-   **Meaning:** This tells the EVM that the actual data for this dynamic type (string) starts **32 bytes** after the beginning of this slot. Since one word is 32 bytes, it points to the very next line.
    

**Word 2 (The Length):** `0000000000000000000000000000000000000000000000000000000000000005`

-   **Value:** Hex `0x05` = Decimal `5`.
    
-   **Meaning:** This indicates the length of the string is **5 characters**.
    

**Word 3 (The Content):** `616c69636500000000000000000000000000000000000000000000000000000`

-   **Value:** `616c696365...`
    
-   **Meaning:** This is the actual text data, encoded in Hexadecimal (ASCII/UTF-8). Strings are left-aligned in EVM storage, so the data is at the start, and the rest is "padding" (zeros).
    

这里的数字都是 hex 16 进制，所以 64 位存储空间是 32 bytes，1 bytes = 8 bits = 2 hex 存储空间。
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->







今天开始学习了，先打个卡测试一下功能。
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
