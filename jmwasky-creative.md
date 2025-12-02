---
timezone: UTC+8
---

# 梁超

**GitHub ID:** jmwasky-creative

**Telegram:** @issac

## Self-introduction

java开发，了解智能合约，熟悉使用dify，coze，ai编程工具

## Notes

<!-- Content_START -->
# 2025-12-02
<!-- DAILY_CHECKIN_2025-12-02_START -->
-   自定义两个数相加的took
    

```Shell
git clone https://github.com/QwenLM/Qwen-Agent.git
cd Qwen-Agent
pip install -e ./
```

```Python
在Qwen-Agent目录的代码里面增加自定义的代码
.\Qwen-Agent\qwen_agent\tools\add_two_number.py
add_two_number.py

import json
import urllib
from typing import Union

from qwen_agent.tools import BaseTool
from qwen_agent.tools.base import register_tool


@register_tool('add_number')
class AddNumber(BaseTool):
    description = '获取数字a和数字b相加的结果返回用户'
    parameters = {
        'type': 'object',
        'properties': {
            'a': {
                'description':
                    '第一个数字',
                'type':
                    'string',
            },
            'b': {
                'description':
                    '第二个数字',
                'type':
                    'string',
            }
        },
        'required': ['a','b'],
    }

    def call(self, params: Union[str, dict], **kwargs) -> str:
        params = self._verify_json_format_args(params)

        a = int(params['a'])
        b = int(params['b'])
        return json.dumps({"result": a+b}, ensure_ascii=False)

.\Qwen-Agent\tests\tools\test_add_number.py
from qwen_agent.tools.add_two_number import AddNumber

def test_add_number():
    tool = AddNumber()
    res = tool.call({'a': '1', 'b':'2'})
    print(res)


if __name__ == '__main__':
    print("123")
   
```

-   返回结果
    

![](https://variation.feishu.cn/space/api/box/stream/download/asynccode/?code=MzE4ZDA5ZjUwMzYxYzM0YTM1MzY3ZTNhN2U0YmE3MWZfYXRXTUc2U2V1eTlVUzlhOXBCaWVYZ2NqdHVoMWRXUkxfVG9rZW46WTJ2VmI3WkRyb2V6NWJ4RDNMN2NGY0U4blNmXzE3NjQ2NjcyODM6MTc2NDY3MDg4M19WNA)
<!-- DAILY_CHECKIN_2025-12-02_END -->

# 2025-12-01
<!-- DAILY_CHECKIN_2025-12-01_START -->

-   实践流程  
    

1.Apikey 去控制台获取 [https://bailian.console.aliyun.com/](https://bailian.console.aliyun.com/)

  2.使用python方式调用接口

```Python
import json
from openai import OpenAI


def get_qwen_content(input):
    with open('apikey.txt', 'r', encoding='utf-8') as f:
        content = f.read()

    client = OpenAI(
        # 若没有配置环境变量，请用百炼API Key将下行替换为：api_key="sk-xxx"
        # 新加坡和北京地域的API Key不同。获取API Key：https://www.alibabacloud.com/help/zh/model-studio/get-api-key
        api_key=content,
        # 使用北京地域的模型，需将base_url替换为：https://dashscope.aliyuncs.com/compatible-mode/v1
        base_url="https://dashscope.aliyuncs.com/compatible-mode/v1",
    )

    completion = client.chat.completions.create(
        # 此处以qwen-plus为例，可按需更换模型名称。模型列表：https://www.alibabacloud.com/help/zh/model-studio/getting-started/models
        model="qwen-plus",
        messages=[
            {"role": "system", "content": "You are a helpful assistant."},
            {"role": "user", "content": input},
        ],
        # Qwen3模型通过enable_thinking参数控制思考过程（开源版默认True，商业版默认False）
        # 使用Qwen3开源版模型时，若未启用流式输出，请将下行取消注释，否则会报错
        # extra_body={"enable_thinking": False},
    )
    #print(completion.model_dump_json())
    qwen_result = json.loads(completion.model_dump_json())
    print(qwen_result)
    choices = qwen_result['choices']
    result = ""
    for c in choices:
        result = c['message']['content']
    return result

if __name__ == '__main__':
    print(get_qwen_content("介绍一下zetachain"))
```

-   在终端打印返回内容。
    

![](https://variation.feishu.cn/space/api/box/stream/download/asynccode/?code=OWE3ZTI4Zjk3ZTZkMWIzZGViYTI4NDc1NWQ5NmJmYmJfSUwzaEFxTkZiN1ZDM3JwcDdta1htcTdnNjVFZjhaUmpfVG9rZW46TjN2aWJMM1hab251Z2J4a0lTcmMwc2tSbmdoXzE3NjQ1ODExODA6MTc2NDU4NDc4MF9WNA)

-   在笔记中记录：你选择了哪个模型？调用参数是怎样的？
    

调用参数model和messages，需要配置系统角色和输入用户提问内容。需要提前获取apiKey
<!-- DAILY_CHECKIN_2025-12-01_END -->

# 2025-11-30
<!-- DAILY_CHECKIN_2025-11-30_START -->


# NFT 拍卖中心

## 目标客户：NFT平台和爱好者

## 解决问题：NFT平台统一竞拍代币，自动托管

## 使用方式：以下文字使用ai总结完善

| 步骤 | 简洁描述 | 核心目标 |
| 1. 启动拍卖 (Escrow) | 卖家在 NFT 所在的链上发布拍卖信息，并将 NFT 锁定托管给平台合约。 | 确保 NFT 不被移动，等待最终清算。 |
| 2. 跨链竞价 (Bidding) | 竞拍者可以使用 任何连接链 上的 任何代币（如 ETH、SOL、MATIC、BNB 等）进行出价。 | ZetaChain 充当清算中枢， 接收并验证所有跨链出价。 |
| 3. 资金锁定与即时退还 | 竞拍者的出价金额会被锁定在 ZetaChain 的智能合约中。一旦出价被超越，平台会即时解除锁定，资金自动退还给前一位竞拍者。 | 保证资金安全和流动性， 避免资金长期占压。 |
| 4. 自动化结算 (Atomic Settlement) | 拍卖结束，ZetaChain 上的智能合约自动且原子化地完成最终清算： | 确保交易原子性 (要么都成功，要么都失败)。 |
| 5. 收益分配 | 平台扣除手续费后，将 NFT 发送给赢家，将拍卖款项 发送给卖家，并将所有未得标的资金 退还给竞拍者。 | 交易完成，各方获利。 |
<!-- DAILY_CHECKIN_2025-11-30_END -->

# 2025-11-29
<!-- DAILY_CHECKIN_2025-11-29_START -->



# 选择本地测试网部署swap合约

## 本地部署

![](https://variation.feishu.cn/space/api/box/stream/download/asynccode/?code=MDhmNWUwYzkwMGI3ZDFjNTVhMzJkZjdkYmYyZDcxYzJfM3BHTGxXdmpSeVpibnZiZE5SdnNmQ29JcDRpeHpvR2NfVG9rZW46SXZwNGJjeWNQb3dJR3B4WG1VUmNpemtZbk1iXzE3NjQ0MTExNjU6MTc2NDQxNDc2NV9WNA)![](https://variation.feishu.cn/space/api/box/stream/download/asynccode/?code=MWJhZWY2MjcyOGFkZDg3NzRlMzgyN2EzMzc1YTI3MWJfZVFoSWRYWXRvaXVaWjJJV2hwNnk2UUl6SzhXU1c4TWlfVG9rZW46S3hMS2JDUEt4b3NlSVR4YWRIMmM4NXphbnlkXzE3NjQ0MTExNjU6MTc2NDQxNDc2NV9WNA)![](https://variation.feishu.cn/space/api/box/stream/download/asynccode/?code=OTE0OGJlYWUyYmM5NmI3NTRkYzE3MWFkYjgxZjIwZGZfcFZzTUZ4NU82MmJSRTRrUDFJOUJPQUdJRWNyNTJDSnJfVG9rZW46RDNQR2Jlbzd3b0NQU0l4WVhOTmNmTWhubmxoXzE3NjQ0MTExNjU6MTc2NDQxNDc2NV9WNA)

  Forge build

![](https://variation.feishu.cn/space/api/box/stream/download/asynccode/?code=NjM5YWJmMDZiMzRkMjUxYzBmYjA2YWE5NDEzZDFlZTdfMnVkajAxcW0wcE9ZRWE3bkJsbEdLMUhkNDcyeDZDdlRfVG9rZW46VlRVNGIzUThSbzFvN3d4c1FLMGNsN2t0bjBjXzE3NjQ0MTExNjU6MTc2NDQxNDc2NV9WNA)

### swap

1.  获取提取到目标链的Gas
    
2.  通过DEX报价验证
    
3.  如果不足gasZRC20需要换取足够的gasZRC20
    
4.  将剩余的部分兑换成目标代币
    

![](https://variation.feishu.cn/space/api/box/stream/download/asynccode/?code=MDRjMDYwYWVjZDdjZDc2NjdiZmM3ZTE3MzFmYjU3MWZfSDhGd0xJWjlNTkZuemtvSldlQ2JGNEF4ZWdqQ2ZVVFFfVG9rZW46WmZYaGJPSnJXbzZ1VFh4MGxxa2N0SkNGbmlmXzE3NjQ0MTEyMTU6MTc2NDQxNDgxNV9WNA)
<!-- DAILY_CHECKIN_2025-11-29_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->




-   zrc-20为zetachain的代币， Universal Token 是ERC-20的同质化代币，Universal NFT 是ERC-721的非同质化代币
    
-     ERC-20代币存入zetachain，写在TSS地址/ERC-20智能合约，ERC-20跟ZRC-20代币一起铸造后发到接收者的钱包上。
    
      ZRC-20是经过铸造的ERC-20代币
    
-   举一个「通用资产」可能的应用场景（比如跨链储蓄、通用 NFT 通行证等）。
    

  跨链游戏，跨链合同整合，跨链资产
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->





-   全链路应用，包括前端，Universal Contract, ZetaChain , Rpc
    
-   第一个Universal 应用实现类似跨链聊天室的功能。连接钱包后，在不同的链上可以互相发消息。
    
-   后续的工作流，使用 CLI+Foundry+测试网实现
    
-   今天了解粗略了解了swap和messaging
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->






# 基础知识点

-   Universal App 是什么？
    

A universal app is a smart contract on ZetaChain that is natively connected to other blockchains like Ethereum, BNB and Bitcoin. Universal app 是zetachain的智能合约

可以在各自不同链不同系统之间进行数据交换

-   Gateway 大概做什么？
    

  gateway用于处理不同区块链之间的交易，统一处理完成后与zetachain处理inbount和outbound事务

-   Universal EVM
    

用于运行universal app 的智能合约平台，zetachain是基于Cosmos SDK和 CometBFT and Cosmet EVM

-   Omnichain Smart Contract
    

全链智能合约

# ZetaChain简单架构图

![20251126-233812.jpeg](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/jmwasky-creative/images/2025-11-26-1764171615482-20251126-233812.jpeg)
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->








-   安装或尝试使用 ZetaChain CLI（本地或云端环境均可）。
    

npx zetachain@next new

```Plain
// 创建模板代码
npx zetachain@latest new --project hello
cd hello
yarn
forge soldeer update
```

```TypeScript
// SPDX-License-Identifier: MIT
pragma solidity 0.8.26;
 
import "@zetachain/protocol-contracts/contracts/zevm/interfaces/UniversalContract.sol";
 
contract Universal is UniversalContract {
    event HelloEvent(string, string);
    // 触发代码并接受gatewa返回的数据
    function onCall(
        // context 包括 chainID-链的id  sender 合约地址
        MessageContext calldata context,
        // zrc20代币地址
        address zrc20,
        // 数量
        uint256 amount,
        // 数据
        bytes calldata message
    ) external override onlyGateway { // 只能由网关调用，保证真实可靠
        string memory name = abi.decode(message, (string));
        emit HelloEvent("Hello: ", name);
    }
}
```

-   了解测试网 RPC、Faucet、Explorer 的入口，记录在自己的笔记中。
    

测试网RPC：[https://www.zetachain.com/docs/reference/network/api](https://www.zetachain.com/docs/reference/network/api)

1.目前有主网和测试网

2.用于调用zetachain的地址。各种钱包调用zetachain的地址都不一样

Faucet：水龙头，用来获取代币[https://www.zetachain.com/docs/reference/faucet](https://www.zetachain.com/docs/reference/faucet)

Explorer：[https://www.zetachain.com/docs/reference/explorers](https://www.zetachain.com/docs/reference/explorers)

-   在终端或 Postman 里完成一次 Qwen API 的简单请求（哪怕只是 200 报错也可以，先打通连通性）。
    

```Shell
curl https://dashscope.aliyuncs.com/api/v1/services/aigc/text-generation/generation \
  -X POST \
  -H "Authorization: Bearer xxxxx" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "qwen-max",
    "input": {
      "messages": [
        {"role": "user", "content": "你好，Qwen！"}
      ]
    },
    "parameters": {
      "result_format": "message"
    }
  }'
  
```

通过qwen提问如何使用终端访问api

![](https://variation.feishu.cn/space/api/box/stream/download/asynccode/?code=MGI3ZDZkOGUyMTAyNGUwYjg5NWRkNWM1NzUwYmUzNzNfWlFKM0dRUWFJT0lvdFluSUVJMTNMWWlnekdxMXN3YnFfVG9rZW46U1FZZ2JPU0x2b2tiUkN4c2ZNRmMxV1U2bkVlXzE3NjQwODUzNzc6MTc2NDA4ODk3N19WNA)

获取apikey并替换后成执行

![](https://variation.feishu.cn/space/api/box/stream/download/asynccode/?code=NGMwYjljZGUyODM2ZDczYmRlZGVkZTQ5ODFhMzlkZDVfcXBlU2hFdzJpV3N3VzRRY1cydk5Mc3FRVEpodGY3Tk9fVG9rZW46WjYya2JCdHk0bzFZbTR4N0JEWWNGWVlMbk5oXzE3NjQwODUzNzc6MTc2NDA4ODk3N19WNA)
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->









1.注册了qwen账号

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/jmwasky-creative/images/2025-11-24-1763971119732-image.png)

2.配置zetaChain开发环境

官网引导：[Intro – ZetaChain Docs](https://www.zetachain.com/docs/developers/tutorials/intro)

-   安装zeta环境
    

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/jmwasky-creative/images/2025-11-24-1763980260724-image.png)

查看

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/jmwasky-creative/images/2025-11-24-1763980309419-image.png)

-   安装foundry
    

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/jmwasky-creative/images/2025-11-24-1763980065854-image.png)
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
