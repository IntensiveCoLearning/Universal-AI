---
timezone: UTC+8
---

# fylcr

**GitHub ID:** fylcr

**Telegram:** @fylcr

## Self-introduction

之前就知道zetachain了，想要仔细地学习一下

## Notes

<!-- Content_START -->
# 2025-12-01
<!-- DAILY_CHECKIN_2025-12-01_START -->
# 使用 Python 调用 Qwen 的简单实例

1.  新建一个 python 文件，写入
    

```
import os
from openai import OpenAI


client = OpenAI(
    api_key="你的 qwen api key",
    base_url="https://dashscope.aliyuncs.com/compatible-mode/v1",  
)

completion = client.chat.completions.create(
    model="qwen-plus",
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "介绍zetachain"},
    ],
)
print(completion.model_dump_json())
```

在这个程序里，调用的是通义千问 Plus 这个模型，成本比较低，表现也比较好。

（可能之后一些简单的操作会交给通义千问 Flash，价格会更低）

1.  运行此python文件，返回
    

```
{"id":"chatcmpl-df4a6881-aaa3-4d5c-b941-2d996a01e940","choices":[{"finish_reason":"stop","index":0,"logprobs":null,"message":{"content":"ZetaChain 是一个专注于跨链互操作性的区块链项目，旨在实现不同区块链之间的无缝通信和价值转移，而无需依赖传统的桥接合约或封装资产。它致力于成为“全链”（omnichain）生态系统的核心基础设施，支持开发者构建能够在所有主流区块链上运行的去中心化应用（dApps）。\n\n以下是 ZetaChain 的一些关键特点和创新：\n\n---\n\n### 1. **无需信任的跨链通信**\nZetaChain 通过其独特的共识机制和去中心化的验证者网络，实现了对多条区块链的状态感知和交互能力。它可以直接监听比特币、以太坊、BNB Chain、Solana 等外部区块链的事件（如交易、智能合约调用），并在无需托管资产的情况下触发跨链操作。\n\n- 支持**原生资产跨链转移**：用户可以直接将 BTC、ETH 等资产转移到其他链上使用，而不需要封装（wrapped tokens）。\n- 使用 **Threshold签名机制** 和 PoS 共识来保证安全性和去中心化。\n\n---\n\n### 2. **全链智能合约（Omnichain Smart Contracts）**\nZetaChain 引入了“全链智能合约”的概念，允许开发者编写一段逻辑，响应来自任何支持链的事件。\n\n例如：\n- 当某人在比特币网络发送一笔特定交易时，可以自动在以太坊上调用某个合约；\n- 在一个链上触发流动性提供、NFT 铸造或游戏奖励发放等操作。\n\n这大大简化了跨链 dApp 的开发流程。\n\n---\n\n### 3. **统一的流动性与账户抽象**\nZetaChain 提供统一的流动性和账户模型，使得用户可以用单一界面与多个链交互，提升用户体验。\n\n- 用户无需在每条链上都持有 gas 费用代币，ZetaChain 可以自动处理费用支付（例如用 ZETA 支付跨链操作的手续费）。\n- 支持 Gas 抽象和批量跨链交易。\n\n---\n\n### 4. **原生代币：ZETA**\n- **ZETA** 是 ZetaChain 的原生代币，用于：\n  - 支付交易手续费；\n  - 质押参与 PoS 共识（验证者和委托人）；\n  - 治理投票（参与协议升级决策）。\n\n---\n\n### 5. **支持异构链**\n不同于一些只支持 EVM 链的跨链方案，ZetaChain 支持**异构区块链**，包括：\n- Ethereum（EVM）\n- Binance Smart Chain\n- Polygon\n- Avalanche\n- Bitcoin（非智能合约链）\n- Solana\n- Cosmos 生态链等\n\n这意味着即使是像比特币这样没有智能合约功能的链，也能参与到复杂的跨链逻辑中。\n\n---\n\n### 6. **安全性设计**\n- 不需要锁定或锚定资产在中间桥合约中，从而减少黑客攻击风险。\n- 外部链的状态由去中心化的验证者集监控，确保数据真实性。\n- 利用轻客户端技术和预言机机制验证外部链上的交易。\n\n---\n\n### 7. **应用场景**\nZetaChain 支持多种创新用例：\n- 全链 DeFi 协议（跨链借贷、交易、收益聚合）；\n- 跨链游戏资产与奖励分发；\n- 比特币生态的可编程扩展（Bitcoin+Smart Contract）；\n- 去中心化身份与社交恢复钱包；\n- 一键式跨链桥解决方案。\n\n---\n\n### 发展状态（截至 2024 年）\n- 主网上线时间：2024 年上半年（具体视进展而定）；\n- 已获得多家知名风投支持，如 Pantera Capital、Wintermute、OKX Ventures 等；\n- 测试网已开放，开发者可部署 omnichain 合约；\n- 社区活跃，文档和 SDK 完善，支持 TypeScript/JavaScript 开发。\n\n---\n\n### 总结\nZetaChain 是下一代跨链基础设施的重要代表之一。它的目标不是成为另一个“最快的链”，而是成为一个连接所有链的“中枢神经系统”。通过消除封装资产、桥接风险和开发复杂性，ZetaChain 正在推动区块链走向真正的互联互通时代。\n\n> 🌐 官方网站：[https://zetachain.com](https://zetachain.com)  \n> 💻 文档：[https://docs.zetachain.com](https://docs.zetachain.com)\n\n如果你是开发者，ZetaChain 提供了友好的工具链和 SDK，非常适合构建真正意义上的“全链应用”。","refusal":null,"role":"assistant","annotations":null,"audio":null,"function_call":null,"tool_calls":null}}],"created":1764599194,"model":"qwen-plus","object":"chat.completion","service_tier":null,"system_fingerprint":null,"usage":{"completion_tokens":986,"prompt_tokens":23,"total_tokens":1009,"completion_tokens_details":null,"prompt_tokens_details":{"audio_tokens":null,"cached_tokens":0}}}
```
<!-- DAILY_CHECKIN_2025-12-01_END -->

# 2025-11-30
<!-- DAILY_CHECKIN_2025-11-30_START -->

主要还是想做聚币器，把n条链上的资产自动转移到一条链上，就这样。
<!-- DAILY_CHECKIN_2025-11-30_END -->

# 2025-11-29
<!-- DAILY_CHECKIN_2025-11-29_START -->


# 在 ZetaChain 上部署第一个 Universal Contract

我根据[这个文档](https://www.zetachain.com/docs/developers/tutorials/hello)部署了 Universal Contract。由于官方文档里使用的是 Forge 而我使用的是 Hardhat，所以稍微的与文档有些出入。

## 新建一个 Hardhat 项目

-   创建一个新的文件夹
    

```
mkdir hardhat-example
cd hardhat-example
```

-   初始化 Hardhat 项目
    

```
npx hardhat --init
```

选择“hardhat-3”（默认选项，直接回车即可）- “.”（默认选项，直接回车即可）- “minimal”（注意，这不是默认选项，这是第三个）- “true”（补齐缺的文件，默认选项，直接回车即可）

-   检查 Hardhat 配置
    

运行

```
npx hardhat --help
```

若返回

```
Hardhat version 3.0.16

Usage: hardhat [GLOBAL OPTIONS] <TASK> [SUBTASK] [TASK OPTIONS] [--] [TASK ARGUMENTS]

AVAILABLE TASKS:

...
```

则 Hardhat 配置无误

运行

```
npx hardhat test
```

若返回

```
Downloading solc 0.8.28
Downloading solc 0.8.28 (WASM build)
No contracts to compile
No Solidity tests to compile

Running Solidity tests
  0 passing
```

则 Hardhat 安装完整

## 编写合约

-   写入代码
    

在项目根目录下创建 contracts\\Universal.sol，将下面的示例代码复制到此文件

```
// SPDX-License-Identifier: MIT
pragma solidity 0.8.26;
 
import "@zetachain/protocol-contracts/contracts/zevm/interfaces/UniversalContract.sol";
 
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

-   安装依赖
    

运行

```
npm install @zetachain/protocol-contracts
```

-   更改配置
    

打开 hardhat.config.ts，将

```
...

export default defineConfig({
  solidity: {
    version: "0.8.28",
  },
...
```

中的 version: "0.8.28" 改为合约中的 0.8.26 版本，即改成

```
...

export default defineConfig({
  solidity: {
    version: "0.8.26",
  },
...
```

-   测试代码
    

运行

```
npx hardhat build
```

若返回

```
...
Compiled 1 Solidity file with solc 0.8.26 (evm target: cancun)
No Solidity tests to compile
```

则代码无误，可以准备部署

## 部署合约

-   配置 Hardhat Ignition
    

创建 ignition\\modules\\Universal.ts，将下面代码复制到此文件里

```
import { buildModule } from "@nomicfoundation/hardhat-ignition/modules";

const UniversalModule = buildModule("UniversalModule", (m) => {
  const universal = m.contract("Universal");
  return { universal };
});

export default UniversalModule;
```

-   更改配置
    

打开 hardhat.config.ts，添加

```
import hardhatIgnitionViemPlugin from "@nomicfoundation/hardhat-ignition-viem";
```

到

```typescript
import { defineConfig } from "hardhat/config";
```

的下一行；

添加

```typescript
  plugins: [hardhatIgnitionViemPlugin],
```

到

```
export default defineConfig({
```

和

```
  solidity: {
    version: "0.8.26",
  },
```

之间，以使用 Hardhat Ignition 插件；

添加

```typescript
  networks: {
    zetachain_athens_evm: {
      type: "http",
      url: "https://zetachain-athens-evm.blockpi.network/v1/rpc/public",
      accounts: ["你钱包的私钥"],
    },
  },
```

到

```
  solidity: {
    version: "0.8.26",
  },
```

和

```
});
```

以告诉 Hardhat Ignition 部署到哪个网络。

最后，hardhat.config.ts 应该是下面这个样子

```
import { defineConfig } from "hardhat/config";
import hardhatIgnitionViemPlugin from "@nomicfoundation/hardhat-ignition-viem";

export default defineConfig({
  plugins: [hardhatIgnitionViemPlugin],
  solidity: {
    version: "0.8.26",
  },
  networks: {
    zetachain_athens_evm: {
      type: "http",
      url: "https://zetachain-athens-evm.blockpi.network/v1/rpc/public",
      accounts: ["你钱包的私钥"],
    },
  },
});
```

-   部署到 Zetachain 测试网
    

运行

```
npx hardhat ignition deploy ignition/modules/Universal.ts --network zetachain_athens_evm
```

然后选择“yes”，之后会返回合约地址

完整返回为

```
√ Confirm deploy to network zetachain_athens_evm (7001)? ... yes
Hardhat Ignition 🚀

Deploying [ UniversalModule ]

Batch #1
  Executed UniversalModule#Universal

[ UniversalModule ] successfully deployed 🚀

Deployed Addresses

UniversalModule#Universal - 0x8FC714012a3E5eEA15237199490b69641C42B2C5
```

在 [ZetaChain 测试网浏览器](https://testnet.zetascan.com/address/0x8FC714012a3E5eEA15237199490b69641C42B2C5)上可以看到详情

## 在 Base Sepolia 上 Call **Universal Contract**

运行

```
npx zetachain evm call --chain-id 84532 --receiver 通用合约地址 --private-key 你钱包的私钥 --types string --values hello
```

（要确保钱包里有足够的钱付 gas，之前已经在 ZetaChain 的测试网上领取过了，所以没有提到这句；而我的钱包在 Base Sepolia 是没有钱的，所以我需要在[水龙头](https://learnweb3.io/faucets/base_sepolia/)获取一些水）

返回

```
From:   0x864d36A061E2f6f72FbFeAF193B1E7B6dD10b7Ba
To:     0x8FC714012a3E5eEA15237199490b69641C42B2C5 on ZetaChain
Call on revert: false

Contract call details:
Function parameters: hello
Parameter types: ["string"]

? Proceed with the transaction? yes
Transaction hash: 0x3b467a9e30ac52e49b854d27313c902bd3dc98b0a721e44e67727111dc72dac9
```

在 [Base Sepolia 浏览器](https://sepolia.basescan.org/tx/0x3b467a9e30ac52e49b854d27313c902bd3dc98b0a721e44e67727111dc72dac9) 上可以看到详情

## 一些分析

可以在 [Base Sepolia 浏览器](https://sepolia.basescan.org/tx/0x3b467a9e30ac52e49b854d27313c902bd3dc98b0a721e44e67727111dc72dac9) 上看到，我的钱包向 0x0c487a766110c85d301d96e33579c5b317fa4995 这个地址 call 了一下，这个地址就应该是 ZetaChain 在 Base Sepolia 上的网关，之后通过这个网关在传递到 Zetachain 上  
（但是分析 ZetaChain 上的数据，看到合约并没有受到这个消息，可能是在部署的时候出了问题，以后需要注意一些）
<!-- DAILY_CHECKIN_2025-11-29_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->



# ZRC-20 VS ERC-20

ZRC-20 只能通过 ZetaChain 协议铸造，而 ERC-20 可以不经许可地部署。ZRC-20 具有跨链地能力，而 ERC-20 不能跨链。

# Zetachain 的使用场景

跨链资金转账：使用 Zetachain 就不需要调用 Bridge 的 API，可以通过纯智能合约实现。

聚合资金：通过智能合约，可以自动地将多链资产聚集在同一个钱包里。
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->




# 我想做的第一个 Universal App

实现所有链的资产都汇集到同一条链的同一个地址上。

（目前打算用hardhat和测试网）
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->





# 什么是 Universal App？

Universal App 是 ZetaChain 上地原生智能合约，可以与其他链进行交互。

（就是一个能和其他很多链交互的智能合约）

# Gateway 可以干什么？

Gateway 是连接 ZetaChain 和其他链的桥梁。有了 Gateway 的存在，其他连可以与 ZetaChain 交互，ZetaChain 也可以与其他链交互。

而且 Gateway 在跨链失败的时候还会原路退款，可以保障资金安全。（这一点比 Wormhole 好，之前我使用 Wormhole 的时候失败了导致我资金受损）

下图就是 ZetaChain 中心 + Bitcoin / Ethereum / Solana 等外围链 + Gateway 的关系

![未标题-1.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/fylcr/images/2025-11-26-1764167990200-___-1.png)
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->






# 安装 ZetaChain CLI

1.  安装 Node.js
    
2.  在终端输入
    

```
npm install -g zetachain@latest
```

1.  安装完成后，输入
    

```
zetachain -V
```

返回

```
7.4.0
```

安装完成

# 关于 RPC、水龙头、浏览器

## RPC

RPC 类似于调用节点的api。我们不可能在使用环境是运行一个节点（除非是light node，不过现在我不知道 ZetaChain 有没有轻节点），所以我们使用 RPC 来与别人运行的公共节点交互。  
ZetaChain 不同部分有不同的 RPC（我不知道这到底怎么形容，也许是像其他项目的平行链，不过这是之后要了解的内容），比如 EVM 部分有一个 RPC，Cosmos 部分有一个RPC。

（在浏览器里可以快速为 MetaMask 添加 EVM RPC）

## 水龙头

水龙头对于经常撸水的人来说应该不陌生，它就是向一个钱包地址发送小额代币以支付gas的存在。一般情况下一个水龙头要干一些事才能获得代币（否则就很容易撸到没水），比如我使用 [FAUCETME](https://zetachain.faucetme.pro/) 这个水龙头获取代币时就要求验证 Discord 账号。（不过我一直在浏览器上看不见水龙头的发送代币的交易）

## 浏览器

浏览器就是来看 ZetaChain 上的地址、代币和交易的。在浏览器可以查看某一个钱包的余额、合约的源代码、交易的细节等等。

# 使用 Qwen API

1.  打开[阿里百炼控制台](https://bailian.console.aliyun.com/?tab=model#/api-key)，生成一个 api key。
    
2.  打开 [通义千问API参考文档](https://www.alibabacloud.com/help/zh/model-studio/qwen-api-reference)，点击“在线调试”
    
3.  选择“中国大陆（北京）”，填入 api key。
    
4.  点击“发送请求”，之后响应结果会出现内容。
    

```
data: {"choices":[{"delta":{"content":null,"role":"assistant","reasoning_content":""},"index":0,"logprobs":null,"finish_reason":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"finish_reason":null,"logprobs":null,"delta":{"content":null,"reasoning_content":"好的，用户"},"index":0}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":null,"reasoning_content":"问“"},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":null,"reasoning_content":"你是谁？"},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":null,"reasoning_content":"”，我需要先确定用户"},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":null,"reasoning_content":"的需求。可能他们想"},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":null,"reasoning_content":"了解我的身份、"},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":null,"reasoning_content":"功能或者背景。作为通"},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":null,"reasoning_content":"义千问，我应该"},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":null,"reasoning_content":"介绍自己的名称、开发"},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":null,"reasoning_content":"公司，以及主要功能"},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":null,"reasoning_content":"。\n\n首先，要"},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":null,"reasoning_content":"回答我是谁，应该"},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":null,"reasoning_content":"明确说出我是通"},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":null,"reasoning_content":"义千问，由阿里"},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":null,"reasoning_content":"云研发的超大规模语言"},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":null,"reasoning_content":"模型。然后，可能需要"},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":null,"reasoning_content":"简要说明我的能力"},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":null,"reasoning_content":"，比如回答问题、创作"},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":null,"reasoning_content":"文字、编程等"},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":null,"reasoning_content":"，这样用户知道能用"},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":null,"reasoning_content":"我做什么。\n\n还要注意语气"},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":null,"reasoning_content":"要友好，保持"},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":null,"reasoning_content":"简洁，避免使用太"},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":null,"reasoning_content":"专业的术语，让用户容易理解"},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":null,"reasoning_content":"。可能需要加入"},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":null,"reasoning_content":"一些表情符号或者"},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":null,"reasoning_content":"亲切的用语，让"},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":null,"reasoning_content":"回答更生动。\n\n另外，"},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":null,"reasoning_content":"用户可能有更"},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":null,"reasoning_content":"深层的需求，比如想确认"},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":null,"reasoning_content":"我是否可靠，或者是否有"},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":null,"reasoning_content":"特定功能。所以需要"},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":null,"reasoning_content":"强调我的训练数据和"},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":null,"reasoning_content":"应用场景，比如支持"},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":null,"reasoning_content":"多语言、经过"},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":null,"reasoning_content":"大量数据训练，"},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":null,"reasoning_content":"适用于各种任务。\n\n最后，"},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":null,"reasoning_content":"可以邀请用户提出"},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":null,"reasoning_content":"具体问题，这样能更好地"},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":null,"reasoning_content":"帮助他们。确保"},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":null,"reasoning_content":"回答结构清晰，先介绍"},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":null,"reasoning_content":"身份，再功能"},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":null,"reasoning_content":"，最后邀请互动。"},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":"你好！我是通","reasoning_content":null},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":"义千问，是","reasoning_content":null},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":"阿里巴巴集团旗下的通","reasoning_content":null},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":"义实验室自主研发的超","reasoning_content":null},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":"大规模语言模型。你可以叫我","reasoning_content":null},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":"通义千问，或者","reasoning_content":null},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":"我的英文名Qwen。","reasoning_content":null},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":"我能够帮助你：\n\n","reasoning_content":null},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":"- **回答问题**","reasoning_content":null},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":"：无论是学术问题、","reasoning_content":null},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":"常识问题还是专业领域问题","reasoning_content":null},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":"，我都可以尝试为你","reasoning_content":null},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":"解答。\n- **创作文字","reasoning_content":null},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":"**：写故事、写","reasoning_content":null},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":"公文、写邮件、","reasoning_content":null},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":"写剧本、逻辑推理、","reasoning_content":null},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":"编程等等，我","reasoning_content":null},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":"都可以帮你完成。\n- **","reasoning_content":null},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":"表达观点**：对某些","reasoning_content":null},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":"话题，我可以提供自己的","reasoning_content":null},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":"看法和建议。\n","reasoning_content":null},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":"- **玩游戏**：我们可以","reasoning_content":null},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":"一起玩文字游戏，如","reasoning_content":null},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":"猜谜语、","reasoning_content":null},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":"成语接龙等。\n\n我","reasoning_content":null},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":"支持多种语言，包括","reasoning_content":null},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":"但不限于中文、英文、德","reasoning_content":null},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":"语、法语、西班牙","reasoning_content":null},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":"语等百种语言。","reasoning_content":null},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":"如果你有任何问题或需要帮助","reasoning_content":null},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"delta":{"content":"，随时告诉我！😊","reasoning_content":null},"finish_reason":null,"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[{"finish_reason":"stop","delta":{"content":"","reasoning_content":null},"index":0,"logprobs":null}],"object":"chat.completion.chunk","usage":null,"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: {"choices":[],"object":"chat.completion.chunk","usage":{"prompt_tokens":22,"completion_tokens":382,"total_tokens":404,"completion_tokens_details":{"reasoning_tokens":210},"prompt_tokens_details":{"cached_tokens":0}},"created":1764076775,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-9f61858b-7204-4b9b-98cd-66ada377fe7d"}

data: [DONE]
```

（点击“解析内容”，可以将原始内容转换为可读的内容。）

这应该是最简单的调用方法了，不过这有点不太符合今天的目标，下面演示一下怎么使用 Postman 调用 api。

1.  新建一个 post 类型，请求地址为 [https://dashscope.aliyuncs.com/compatible-mode/v1/chat/completions](https://dashscope.aliyuncs.com/compatible-mode/v1/chat/completions)
    
2.  将 body 换成 raw，选择 JSON 格式，输入
    

```
{
  "model": "qwen-plus",
  "messages": [
    {
      "role": "system",
      "content": "You are a helpful assistant."
    },
    {
      "role": "user",
      "content": "你是谁？"
    }
  ],
  "stream": true,
  "stream_options": {
    "include_usage": true
  },
  "top_p": 0.8,
  "temperature": 0.7,
  "enable_thinking": true
}
```

1.  新建一个 Header，Key 为 Authorization，value 为 Bearer 你的 api key。
    
2.  点击 send，然后就是200响应。
    

今天的内容就是这些。
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->







# 注册 qwen api 账号

1.  打开阿里云百炼平台。（国内的打开[这个网站](https://bailian.console.aliyun.com/)，国外的打开[这个链接](https://modelstudio.console.alibabacloud.com/)）
    
2.  注册账号时输入手机号，设置用户名和密码。
    
3.  注册完成。
    

（看计划今天有操作难度的就只有这个，另外一个任务是确保能够打开[zetachain的文档页面](https://www.zetachain.com/docs/developers)）
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
