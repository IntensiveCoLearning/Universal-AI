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
