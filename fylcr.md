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
