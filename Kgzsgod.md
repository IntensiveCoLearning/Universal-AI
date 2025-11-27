---
timezone: UTC+8
---

# K9z4g0d

**GitHub ID:** Kgzsgod

**Telegram:** @kgzsgod

## Self-introduction

大四计算机在读，熟悉区块链基础概论，以及DeFi等区块链扩展应用，希望可以在此次共学中学到知识

## Notes

<!-- Content_START -->
# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->
## 自己想做的第一个 Universal App 想实现的“打印 / 记录 / 简单逻辑”是什么。

> 想做一个全链留言板，用户可以从任何链提交留言，ZetaChain 统一记录。

## 为后续的 Hello World Demo 决定一种工作流：

> 使用Foundry + ZetaChain测试网。
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->

### Qwen

刚看到notion中还有实践作业。。。

使用终端的curl命令完成了对qwen api 请求

![](https://xvadt6dwbiv.feishu.cn/space/api/box/stream/download/asynccode/?code=YzlhZmFiZjI3YzVhYzJlZGE3MmIxZmViODk4Y2Q2MjNfc3EzZnp0Q1o5bElOd3A5QkpsalZVUGVrTk1zVkdyZFZfVG9rZW46S21BbGJ2cEF2b3VxWnR4eGhVcWNhNTFkbk9nXzE3NjQxNDM0MTg6MTc2NDE0NzAxOF9WNA)

通过以下python代码可以在终端与qwen进行交互

```Python
import os
from openai import OpenAI

client = OpenAI(
    api_key=os.getenv("DASHSCOPE_API_KEY"),
    base_url="https://dashscope.aliyuncs.com/compatible-mode/v1",
)

messages = [
    {"role": "system", "content": "You are a helpful assistant."}
]

print("Qwen CLI 已启动，输入 exit 退出。")

while True:
    user_input = input("我: ")
    if user_input.lower() in ["exit", "quit"]:
        break

    messages.append({"role": "user", "content": user_input})

    response = client.chat.completions.create(
        model="qwen-plus",
        messages=messages
    )

    reply = response.choices[0].message.content
    print("Qwen:", reply)

    messages.append({"role": "assistant", "content": reply})
```

![](https://xvadt6dwbiv.feishu.cn/space/api/box/stream/download/asynccode/?code=ZTgyNjBhYzRjOGYwNDcyZjY0Y2MwNTM0MjQ3NGNiNjlfTTFnYzBlYkNhbU1BbjV1bzU0dFRkOUVTZUtxMHpoQ1lfVG9rZW46WXlmNGJoQ3pqb1NZa0d4MTNjSmNLZ2JMbmdmXzE3NjQxNDM0MTg6MTc2NDE0NzAxOF9WNA)

之前的实践作业在前两天的笔记都有提到，就不一一返回重新截取了

### Gateway

Gateway就是ZetaChain的智能合约与其他链进行交互的接口，可以通过这个接口进行消息传输，发送交易等操作

针对不同的连接链有不同的操作方式

Gateway主要实现了的功能需要区分连接链上的功能和zetachain的功能

连接链上的Gateway的功能：

-   存入原生代币作为gas向zetachain的合约发送调用请求
    
-   受zetachain支持的erc-20代币也可向zetachain的合约发送请求
    

zetachain的Gateway的功能：

-   将zeta或者erc-20代币提取到连接链上
    
-   将代币提取到连接链上发送合约调用
    

Gateway的架构图

![](https://xvadt6dwbiv.feishu.cn/space/api/box/stream/download/asynccode/?code=YWVlZDUyMzE4MjVlM2FkZDU0MmJiZTJhN2YwOTk3ZmRfSXFCcUdWZHdyc0VXaGxFQk9DOXdEaTlpa2lnQ0YyVm1fVG9rZW46SzVQdmJjbGdpb1B6WGR4V01Od2M1em8zblpkXzE3NjQxNDM0MTg6MTc2NDE0NzAxOF9WNA)
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->


## 部署在本地的universal合约

```Solidity
npx zetachain localnet start
```

使用如上终端命令，打开zetachain的anvil本地测试环境

在命令行中输入`forge build`编译foundry项目中的Universal合约

在输入如下命令将Universal合约部署上链

```Shell
forge create Universal \
  --rpc-url http://127.0.0.1:8545 \
  --private-key $PRIVATE_KEY \
  --broadcast
[⠊] Compiling...
No files changed, compilation skipped
Deployer: 0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266
Deployed to: 0x5bf5b11053e734690269C6B9D438F8C9d48F528A
Transaction hash: 0x57d6b6f7efdec82004cc021f837e2d7bee84a92e477d5c6f3e6a0817033f68c2
```

可以看到已经完成部署上链，发送方和接收方的地址以及交易的哈希

Universal合约就是实现了当其他链通过zetachain跨链调用这个合约时，他会解码传入的内容，并触发打印名字的事件（并没有实际资产转移，只单纯输出消息）

## 部署在测试网的Universal合约

首先先将zetachain testnet添加到metamask中

![](https://xvadt6dwbiv.feishu.cn/space/api/box/stream/download/asynccode/?code=M2Q2ODI3NWIxN2FjYjc3MGQ5OWQ0OGFhYTJlYjRlMzhfeDdQdThONkRuSXd3a2RUNVhKZkRjYkE2R0FhTXZTV3BfVG9rZW46TjJmSWJ0WVJub2ZReWl4Zkh0bmN0OEJCbkZKXzE3NjQwNzU5OTU6MTc2NDA3OTU5NV9WNA)

[https://zetachain.faucetme.pro/](https://zetachain.faucetme.pro/)

通过官方水龙头获取zeta代币，然后将私钥改成测试网地址的私钥，在输入如下命令即可将Universal合约部署在测试网上

```Shell
forge create Universal \
  --rpc-url https://zetachain-athens-evm.blockpi.network/v1/rpc/public \
  --private-key $PRIVATE_KEY \
  --broadcast \
  --json

{
  "deployer": "0xAa79ED23Adf4A21fE58736d60fEEcec17BD9E33B",
  "deployedTo": "0x525087bA0FF1f84c8E93846f1D0fC7D5709D37Af",
  "transactionHash": "0x64680383161a811a369e60adaaf34f0b7bcdbba1f725642ea3cee212e66c83d0"
}
```

得到的交易哈希可以从区块链浏览器中查到[https://testnet.zetascan.com/](https://testnet.zetascan.com/)

![](https://xvadt6dwbiv.feishu.cn/space/api/box/stream/download/asynccode/?code=ZTY3YTZlYjg5N2NjYWRjNTFhNTZiZjA5ZThjYzljNzlfdk56TEVadk9JdFQ4NHNvVTQ4STBlS1h6UjlrRW44SnhfVG9rZW46WTFWdWJRTmttbzVrR0N4b0xXbGN1dVhUbmplXzE3NjQwNzU5OTU6MTc2NDA3OTU5NV9WNA)
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->



搭建了zetachain的cli环境

阅读了zetachain的官方文档并搜索了相关资料，理清了他的逻辑

zetachian并不是传统意义上的桥接跨链，而是通过在zetachain上部署智能合约，实现从一个链到另一个链的操作

比如现在我想要从比特币链上转账btc到以太坊链上，我只需要在zetachain上部署执行转账合约，此时中间的过程：

1.  比特币链触发转账操作
    
2.  zetachain链监听到该交易
    
3.  zetachain链节点达成共识
    
4.  执行跨链转账合约逻辑
    
5.  生成转账交易
    

对比传统跨链来说确实有优势，速度快延迟短，也不需要考虑各种跨链fee
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
