---
timezone: UTC+8
---

# neo

**GitHub ID:** timerring

**Telegram:** @timerring

## Self-introduction

Dev

## Notes

<!-- Content_START -->
# 2025-12-03
<!-- DAILY_CHECKIN_2025-12-03_START -->
### **Byte-level Byte Pair Encoding**

Qwen adopts a subword tokenization method called Byte Pair Encoding (BPE), which attempts to learn the composition of tokens that can represent the text with the fewest tokens. For example, the string `tokenization` is decomposed as `token` and `ization` (note that the space is part of the token). Especially, the tokenization of Qwen ensures that there is no unknown words and all texts can be transformed to token sequences.

### **Control Tokens**

Control tokens are special tokens inserted into the sequence that signifies meta information. For example, in pre-training, multiple documents may be packed into a single sequence. For Qwen, the control token `<|endoftext|>` is inserted after each document to signify that the document has ended and a new document will proceed. Common control tokens and their status with respect to Qwen can be found in the following table:

| Type | Qwen (training) | Note |
| --- | --- | --- |
| eod token | <\|endoftext\|> | end of document, which are inserted between documents inside a packed training sequence |
| bot token | <\|im_start\|> | start of each turn, which is prepended to each turn |
| eot token | <\|im_end\|> | end of each turn, which is appended to each turn |
| unk token | no unk token | BBPE ensures no unknown tokens for Qwen. |
| pad token | no pad token | Qwen does not make use of padded sequence in training. One could use any special token together with the attention masks returned by the tokenizer. It is commonly set the same as eod for Qwen. |
| bos token | no bos token | Qwen does not prepend a fixed token to each packed training sequence.[2] |
<!-- DAILY_CHECKIN_2025-12-03_END -->

# 2025-12-02
<!-- DAILY_CHECKIN_2025-12-02_START -->

Qwen-Agent provides atomic components such as LLMs and prompts, as well as high-level components such as Agents. The example below uses the Assistant component as an illustration, demonstrating how to add custom tools and quickly develop an agent that uses tools.

```
import json
import os

import json5
import urllib.parse
from qwen_agent.agents import Assistant
from qwen_agent.tools.base import BaseTool, register_tool

llm_cfg = {
    # Use the model service provided by DashScope:
    'model': 'qwen-max',
    'model_server': 'dashscope',
    # 'api_key': 'YOUR_DASHSCOPE_API_KEY',
    # It will use the `DASHSCOPE_API_KEY' environment variable if 'api_key' is not set here.

    # Use your own model service compatible with OpenAI API:
    # 'model': 'Qwen/Qwen2.5-7B-Instruct',
    # 'model_server': 'http://localhost:8000/v1',  # api_base
    # 'api_key': 'EMPTY',

    # (Optional) LLM hyperparameters for generation:
    'generate_cfg': {
        'top_p': 0.8
    }
}
system = 'According to the user\'s request, you first draw a picture and then automatically run code to download the picture ' + \
          'and select an image operation from the given document to process the image'

# Add a custom tool named my_image_gen：
@register_tool('my_image_gen')
class MyImageGen(BaseTool):
    description = 'AI painting (image generation) service, input text description, and return the image URL drawn based on text information.'
    parameters = [{
        'name': 'prompt',
        'type': 'string',
        'description': 'Detailed description of the desired image content, in English',
        'required': True
    }]

    def call(self, params: str, **kwargs) -> str:
        prompt = json5.loads(params)['prompt']
        prompt = urllib.parse.quote(prompt)
        return json.dumps(
            {'image_url': f'https://image.pollinations.ai/prompt/{prompt}'},
            ensure_ascii=False)


tools = ['my_image_gen', 'code_interpreter']  # code_interpreter is a built-in tool in Qwen-Agent
bot = Assistant(llm=llm_cfg,
                system_message=system,
                function_list=tools,
                files=[os.path.abspath('doc.pdf')])

messages = []
while True:
    query = input('user question: ')
    messages.append({'role': 'user', 'content': query})
    response = []
    for response in bot.run(messages=messages):
        print('bot response:', response)
    messages.extend(response)
```
<!-- DAILY_CHECKIN_2025-12-02_END -->

# 2025-12-01
<!-- DAILY_CHECKIN_2025-12-01_START -->


Gateway 接口是与通用应用(Universal Apps)交互的统一入口点 。Gateway 作为连接链(如以太坊、Solana 和比特币)上的合约与 ZetaChain 上通用应用之间的桥梁 。​

## 连接链上的 Gateway

连接链上的 Gateway 负责处理从外部链到 ZetaChain 的入站交易,包括合约调用和代币转移 。根据不同的连接链,Gateway 的实现方式有所不同:​

-   EVM 链使用智能合约
    
-   Solana 使用 Gateway 程序
    
-   比特币使用由观察者-签名者验证节点网络管理的 TSS MPC Gateway 地址​
    

Gateway 支持以下功能:

-   向通用应用或 ZetaChain 账户存入原生 Gas 代币
    
-   存入支持的 ERC-20 代币(包括 ZETA 代币)
    
-   存入原生 Gas 代币并调用合约(支持任意数据传递)
    
-   存入 ERC-20 代币并调用合约
    
-   直接进行合约调用​
    

## ZetaChain 上的 Gateway

ZetaChain 上的 Gateway 处理出站交易,即从通用应用到连接链上合约的调用和代币提取 。主要功能包括:​

-   将 ZRC-20 代币作为原生 Gas 或 ERC-20 代币提取到连接链
    
-   向连接链提取 ZETA 代币
    
-   提取代币并在连接链上进行合约调用
    
-   在连接链上进行合约调用​
    

## 特点与限制

目前每次只能存入或提取一种资产,未来协议更新将支持多资产操作 。Gateway 还支持跨链操作期间的回退处理机制,如果目标链上的调用失败,可以通过调用源链上的指定合约或直接发送到 EOA 账户进行退款 。
<!-- DAILY_CHECKIN_2025-12-01_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->



## ZRC-20 与 ERC-20 的核心区别

从开发者视角看，ZRC-20 是 ERC-20 的跨链增强版本，最关键的区别在于 ZRC-20 拥有**原生跨链能力**和**多链资产感知能力** 。​

## 技术实现差异

**铸造机制**：普通 ERC-20 可以由开发者自由部署和铸造，而 ZRC-20 只能由 ZetaChain 协议铸造 。当用户将 ERC-20 从原链存入时，原资产会被锁定在 TSS 地址中，同时在 ZetaChain 上铸造对应的 ZRC-20 。​

**跨链提取功能**：ZRC-20 内置 `withdraw()` 方法，可以将代币无需许可地提取到任何连接链作为原生资产，这是 ERC-20 不具备的核心特性 。例如，你可以在 ZetaChain 上调用合约，直接将资产提取到比特币或 BSC 网络。​

**资产表示方式**：同一个 ERC-20（如 USDT）在不同链上会被表示为不同的 ZRC-20——以太坊 USDT 和 BSC USDT 在 ZetaChain 上是两个独立的 ZRC-20 资产，但可以互相兑换 。这意味着 ZRC-20 代表的是真实的 BTC 或 ETH，而非包装资产 。​

## 通用资产应用场景示例

## 跨链收益聚合器

假设构建一个 DeFi 协议，用户可以存入任意链上的稳定币（ETH 上的 USDC、BSC 上的 BUSD、Polygon 上的 DAI）。智能合约通过 ZRC-20 标准统一管理这些资产，自动在不同链上寻找最高收益率的借贷池或流动性挖矿机会，并将收益以用户偏好的链和代币形式提取 。整个过程对用户是透明的——他们只需一次交易，合约就能完成跨多个链的复杂操作 。​

## 全链通用会员 NFT

创建一个会员系统，用户持有的 ZRC-20 代币可以在以太坊、BSC、比特币等任意链上验证和使用 。例如，一个游戏平台的会员通行证，玩家可以在以太坊上购买，在 Polygon 上玩游戏消费代币，在 BSC 上兑换奖励，所有操作都通过统一的 ZRC-20 标准实现，无需在每条链上重复部署和桥接资产 。
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->




## ZetaChain Universal App

Universal App 是部署在 ZetaChain Universal EVM 上的智能合约，支持从 Ethereum、BNB、Bitcoin 等多链接收调用、消息和代币，并可发起跨链调用和转账，实现复杂多链交易编排。​

## 核心概念

-   **Universal EVM**：ZetaChain 的 EVM 扩展，内置全链互操作性，现有的 EVM 合约可直接运行，并通过少量修改获得跨链能力。​
    
-   **Universal App**：继承 UniversalContract 的合约，通过 `onCall` 方法处理来自 Gateway 的跨链调用，接收 MessageContext、ZRC-20 代币和消息数据。​
    
-   **ZRC-20**：连接链的原生 gas 和 ERC-20 代币在 ZetaChain 上的表示，可无许可提现回原链。​
    
-   **Omnichain Smart Contract**：指 Universal App，支持 EVM 链、Bitcoin、Solana 等，未来扩展 TON 和 Cosmos。​
    

## 系统结构

ZetaChain 通过每个连接链上的单一 **Gateway 合约** 作为入口，用户从源链（如 Ethereum）调用 Gateway 发送代币和消息，触发 ZetaChain 上 Universal App 的 `onCall`；App 可调用 Gateway 发起出链转账或调用。​

多链用户经 Gateway 入链到 ZetaChain Universal App，App 处理后出链到目标链，支持 gas 抽象（入链仅源链 gas，出链由 App 管理 ZRC-20 gas）。​

## 关键特性

-   **Gas Abstraction**：入链调用仅需源链 gas，ZetaChain 执行免费；出链需 App 预换 ZRC-20 gas 代币覆盖目标链费用。​
    
-   **Bitcoin 支持**：Bitcoin 用户直接发 BTC+数据到 Gateway，无需 ZetaChain 账户或 gas 代币。​
    
-   **前向兼容**：新链接入后，现有 App 自动支持，无需修改合约。​
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->





-   **Universal NFT标准**可以让ERC-721类NFT在任意连接链上铸造和无缝转移，无需“包装”或外部桥接合约，支持多链间NFT真正的互操作。
    
-   **主要特性：**
    
    -   **链间真实所有权**：NFT拥有唯一且持续不变的Token ID，每条链上持有者和元数据保持一致，实现链无关的NFT归属。
        
    -   **标准基于OpenZeppelin**：开发者用熟悉的OpenZeppelin ERC-721库，利用UUPS可升级代理合约模式，方便安全扩展NFT逻辑。
        
    -   **开发方式灵活**：
        
        -   可以通过命令行工具直接创建新的Universal NFT项目。
            
        -   对已有ERC-721项目，只需引入ZetaChain官方合约包，并按模板增加Universal NFT逻辑即可快速升级项目。
            
    -   **链间转移流程**：
        
        -   ZetaChain作为中枢，NFT和其ID、元数据随转移在任意链间流转，整个过程保持链与链之间的信息、身份和归属一致。
            
        -   NFT可以在ZetaChain、Base、Ethereum等连接链间自由转移，所有链使用同一个tokenId和相同元数据。
            
-   **部署及转移核心步骤：**
    
    -   在各链上分别部署Universal NFT相关合约，并通过合约函数（setConnected、setUniversal）组合成信任网，实现链间协作。
        
    -   支持直接在ZetaChain上铸造，再任意转移到其它连接链，也能反向转移。
        
    -   转移和铸造都能用标准工具链自动完成，并且有详细命令行示例和相关合约源码可供参考。
        
-   **适用场景：**
    
    -   多链NFT游戏、市场、身份认证或需要NFT能链间自由切换的平台。
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->






**Universal Apps** 是指在 ZetaChain 区块链上运行的智能合约，它们能够原生连接多个区块链（如 Ethereum, Bitcoin, BNB, Solana 等），实现真正的多链交互和资产转移。

**核心特点**

-   能直接接收来自各连接链的合约调用、消息和 Token（如 ERC-20、BTC）转账。
    
-   可以主动向其它连接链触发合约调用与 Token 转账，支持复杂的、跨链多步操作。
    
-   对接多种链（EVM 类链、Bitcoin、Solana、TON、Sui，未来将支持 Cosmos 等）。
    
-   资产和 Gas 统一抽象为 ZRC-20 Token，用户可以无缝跨链（如：BTC 用户可直接向 Ethereum 地址转账 USDC）。
    
-   用户只需与各链上的 Gateway 合约交互，无需在每个链都创建账户或持有 Gas Token，跨链 Gas 自动处理。
    
-   合约可查询并自动处理跨链所需的 Gas，确保操作顺畅。
    
-   合约部署于 ZetaChain 的 Universal EVM，可以轻松把现有 EVM 合约移植，并稍作改动即可获得多链功能。
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
