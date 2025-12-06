---
timezone: UTC+8
---

# Qy

**GitHub ID:** pillowtalk-Qy

**Telegram:** @Qy

## Self-introduction

好好学习，天天向上

## Notes

<!-- Content_START -->
# 2025-12-06
<!-- DAILY_CHECKIN_2025-12-06_START -->
# **项目 ：全链 AI 宠物（Cross-Chain AI Pet）**

每条链都能“喂养”你的宠物，AI 让宠物个性成长。

## 概念：

你养一只“链宠物”，它的行为和性格是由：

-   你在哪条链做交易
    
-   链上发生的事情
    
-   你喂给它的 NFT / Token
    

来决定的。

Qwen AI 根据你的跨链行为**实时改写它的性格故事和台词**。

ZetaChain 负责同步“宠物状态”到所有链。
<!-- DAILY_CHECKIN_2025-12-06_END -->

# 2025-12-05
<!-- DAILY_CHECKIN_2025-12-05_START -->


-   NVIDIA A100 80GB
    
-   CUDA 12.1
    
-   vLLM 0.6.3
    
-   Pytorch 2.4.0
    
-   Flash Attention 2.6.3
    
-   Transformers 4.46.0
<!-- DAILY_CHECKIN_2025-12-05_END -->

# 2025-12-04
<!-- DAILY_CHECKIN_2025-12-04_START -->



-   NVIDIA A100 80GB
    
-   CUDA 12.1
    
-   Pytorch 2.3.1
    
-   Flash Attention 2.5.8
    
-   Transformers 4.46.0
    
-   AutoGPTQ 0.7.1+cu121 (Compiled from source code)
    
-   AutoAWQ 0.2.6
<!-- DAILY_CHECKIN_2025-12-04_END -->

# 2025-12-03
<!-- DAILY_CHECKIN_2025-12-03_START -->




# **朗链**

\*\*1.\*\*该项目的实现流程包括：加载文件 -> 读取文本 -> 文本分割 -> 文本向量化 -> 问题向量化 -> 将最相似的 k 个文本向量与问题向量进行匹配 -> 将匹配的文本作为上下文与问题一起添加到提示中 -> 提交给 Qwen2.5-7B-Instruct 以生成答案。

pip install langchain==0.0.174 pip install faiss-gpu

```
from transformers import AutoModelForCausalLM, AutoTokenizer
from abc import ABC
from langchain.llms.base import LLM
from typing import Any, List, Mapping, Optional
from langchain.callbacks.manager import CallbackManagerForLLMRun

model_name = "Qwen/Qwen2.5-7B-Instruct"

model = AutoModelForCausalLM.from_pretrained(
    model_name,
    torch_dtype="auto",
    device_map="auto"
)
tokenizer = AutoTokenizer.from_pretrained(model_name)

class Qwen(LLM, ABC):
     max_token: int = 10000
     temperature: float = 0.01
     top_p = 0.9
     history_len: int = 3

     def __init__(self):
         super().__init__()

     @property
     def _llm_type(self) -> str:
         return "Qwen"

     @property
     def _history_len(self) -> int:
         return self.history_len

     def set_history_len(self, history_len: int = 10) -> None:
         self.history_len = history_len

     def _call(
         self,
         prompt: str,
         stop: Optional[List[str]] = None,
         run_manager: Optional[CallbackManagerForLLMRun] = None,
     ) -> str:
         messages = [
             {"role": "system", "content": "You are Qwen, created by Alibaba Cloud. You are a helpful assistant."},
             {"role": "user", "content": prompt}
         ]
         text = tokenizer.apply_chat_template(
             messages,
             tokenize=False,
             add_generation_prompt=True
         )
         model_inputs = tokenizer([text], return_tensors="pt").to(model.device)
         generated_ids = model.generate(
             **model_inputs,
             max_new_tokens=512
         )
         generated_ids = [
             output_ids[len(input_ids):] for input_ids, output_ids in zip(model_inputs.input_ids, generated_ids)
         ]

         response = tokenizer.batch_decode(generated_ids, skip_special_tokens=True)[0]
         return response

     @property
     def _identifying_params(self) -> Mapping[str, Any]:
         """Get the identifying parameters."""
         return {"max_token": self.max_token,
                 "temperature": self.temperature,
                 "top_p": self.top_p,
                 "history_len": self.history_len}
```
<!-- DAILY_CHECKIN_2025-12-03_END -->

# 2025-12-01
<!-- DAILY_CHECKIN_2025-12-01_START -->






摆烂一天
<!-- DAILY_CHECKIN_2025-12-01_END -->

# 2025-11-30
<!-- DAILY_CHECKIN_2025-11-30_START -->







1./cosmos/auth/v1beta1/account\_info/{address}

**{ "info": { "address": "string", "pub\_key": { "type\_url": "string", "value": "string" }, "account\_number": "string", "sequence": "string" } }**

**2.**/cosmos/auth/v1beta1/accounts

**{ "accounts": \[ { "type\_url": "string", "value": "string" } \], "pagination": { "next\_key": "string", "total": "string" } }**

**3.**/cosmos/auth/v1beta1/accounts/{address}

**{ "account": { "type\_url": "string", "value": "string" } }**

**4.**/cosmos/auth/v1beta1/address\_by\_id/{id}

**{ "account\_address": "string" }**

**5.**/cosmos/auth/v1beta1/bech32

**{ "bech32\_prefix": "string" }**
<!-- DAILY_CHECKIN_2025-11-30_END -->

# 2025-11-29
<!-- DAILY_CHECKIN_2025-11-29_START -->








1.**将 AWQ 模型与 vLLM 结合使用**

vLLM 已支持 AWQ，这意味着您可以直接使用我们提供的 AWQ 模型，或者使用`AutoAWQ`vLLM 量化的模型。我们建议使用最新版本的 vLLM（`vllm>=0.6.1`），该版本对 AWQ 模型进行了性能优化；否则，性能可能无法得到充分发挥。

实际上，其用法与 vLLM 的基本用法相同。我们提供了一个简单的示例，说明如何使用 vLLM 启动与 OpenAI API 兼容的 API `Qwen2.5-7B-Instruct-AWQ`：

2.**使用 AutoAWQ 量化您自己的模型**

如果您想将自己的模型量化为 AWQ 量化模型，我们建议您使用 AutoAWQ。

```
pip install "autoawq<0.2.7"
```

假设您已经使用自己的数据集（例如 Alpaca 数据集）对基于模型的模型进行了微调`Qwen2.5-7B`，该模型名为`Qwen2.5-7B-finetuned`。要构建您自己的 AWQ 量化模型，您需要使用训练数据进行校准。
<!-- DAILY_CHECKIN_2025-11-29_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->









Workshop回放：

【ZetaChain 通用资产与跨链 DeFi 开发导论】 [https://www.bilibili.com/video/BV1zWSgBnEcE/?share\_source=copy\_web&vd\_source=fd6ac63c6fb1f02dcdf46371c30b2168](https://www.bilibili.com/video/BV1zWSgBnEcE/?share_source=copy_web&vd_source=fd6ac63c6fb1f02dcdf46371c30b2168)

1.观看昨天的回放，根据回放复盘前几天的操作内容
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->










摸鱼一天
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->











[ZetaChain](https://www.zetachain.com/docs)是一个与 EVM 兼容的区块链，具有内置的跨链互操作性。

部署在 ZetaChain 上的智能合约可以处理来自任何已连接区块链（例如以太坊、Base、Polygon、BNB、Solana 和比特币）的_入站合约调用。ZetaChain 上的合约也可以向已连接链上的合约发出出站合约调用_。合约调用可以包含任意数据、gas 费用以及支持的 ERC-20 代币等有效载荷。

从连接链转移到 ZetaChain 合约的 Gas 和 ERC-20 代币在 ZetaChain 上以[**ZRC-20**](https://www.zetachain.com/docs/developers/tokens/zrc20)（一种类似 ERC-20 的代币）的形式呈现。例如，当您使用 ETH 发起调用时，ZetaChain 上的合约会收到 ZRC-20 ETH。实际上，ETH 被锁定在 ZetaChain 托管的以太坊服务器上，同时等量的 ZRC-20 ETH 代币被铸造到 ZetaChain 的合约中。任何人都可以无需许可地将 ZRC-20 提取回原生资产。

用于实现这些传入（到 ZetaChain）和传出（从 ZetaChain）通信的机制称为[**网关（Gateway**](https://www.zetachain.com/docs/developers/evm/gateway) [）。每条连接的链都有一个网关，作为与 ZetaChain 上的合约交互的单一入口点。在以太坊和 Polygon 等连接的 EVM 链](https://www.zetachain.com/docs/developers/chains/evm)上存在网关合约， [Solana](https://www.zetachain.com/docs/developers/chains/solana)上存在网关程序，比特币上存在网关地址。ZetaChain[本身](https://www.zetachain.com/docs/developers/chains/zetachain)也有一个网关合约，用于触发传出调用。

例如，[您可以编写一个合约](https://www.zetachain.com/docs/developers/tutorials/swap)，用于处理来自一条链的传入代币，将其兑换为目标代币，然后提取到目标链。要使用此合约执行跨链兑换，用户可以调用一个网关（例如以太坊上的网关），并传入一定数量的 USDC、要调用的 ZetaChain 合约以及包含目标代币地址的数据负载。监视网关的 ZetaChain 验证者会注意到这笔交易，并向 ZetaChain 上的兑换合约发起跨链交易。兑换合约接收 USDC 和负载后，执行兑换操作，并将目标代币提取到目标链（例如比特币）。通过这笔交易，以太坊用户发起了一次跨链合约调用，调用了 ZetaChain 合约，该合约将 ZRC-20 ETH 兑换为 ZRC-20 BTC，并将比特币上的原生 BTC 提取给了接收方。

用户只需在选定的区块链上进行一笔交易，即可在不同区块链之间进行跨链兑换。他们无需担心 gas 费用、资产桥接或钱包切换等问题。当然，兑换只是其中一个例子。NFT 市场、借贷平台、多链治理机制、收益聚合器等等，都可以在 ZetaChain 上构建。

我们将这类合约称为[**通用应用**](https://www.zetachain.com/docs/developers/apps/intro)，因为它们可以跨所有连接的区块链协调复杂的跨链交易。将跨链应用编写成通用应用非常容易，因为大部分逻辑都封装在 ZetaChain 上的单个合约中，该合约可以处理传入的调用并向其他链上的合约发出调用。
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->












1.注册 / 配置好 ZetaChain 开发所需环境

2.注册 Qwen 账号并确认可以进入控制台。

3.加入 ZetaChain 中国开发者社区。

4.ZetaChain是首个能够跨所有区块链生态系统实现原生连接的通用区块链。

5.**通用POS机**

ZetaChain 的权益证明系统基于 Cosmos SDK 和 Comet BFT 构建，可确保高效的跨链操作：

-   5秒终结
    
-   核心验证者和观察者-签名者验证者确保网络和跨链安全
    
-   优先考虑所有连接链安全性的激励机制
    

6.**通用电子投票机**

通用 EVM 是一个可以从任何区块链调用的执行环境，允许开发人员：

-   管理和交互跨连接链的原生资产、合约和钱包
    
-   在熟悉的 EVM 环境中创建具有内置跨链功能的应用程序
    

7.**通用智能合约**

通用智能合约原生部署在 ZetaChain 上，可以读写任何连接的链。它通过以下方式简化跨链开发：

-   协调复杂的多链行动
    
-   获取不同网络的流动性
    
-   提供统一的跨链用户体验
    
-   必要时通过添加特定于链的合约来扩展功能
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
