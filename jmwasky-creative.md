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
# 2025-12-07
<!-- DAILY_CHECKIN_2025-12-07_START -->
项目名称：NFT拍卖

目标场景：参加跨链拍卖的用户

关键功能：

1.  质押NFT和质押代币，参与拍卖
    
2.  拍卖竞价功能
    
3.  支付结算功能
    

技术路线：

1.  用户可以通过自然语音发送到qwen，由qwen理解和弹出确认框，用户确认质押NFT和代币，参与出价
    
2.  通过zetachain处理跨链代币，处理拍卖和结算
<!-- DAILY_CHECKIN_2025-12-07_END -->

# 2025-12-06
<!-- DAILY_CHECKIN_2025-12-06_START -->

```Python
基于zetachain cli 改造使用命令行调用方式执行，
目前根据官方文档写死通过localnet 转到BNB

import json
import os
import subprocess
from typing import Union

from annotated_types import UpperCase

from qwen_agent.tools import BaseTool
from qwen_agent.tools.base import register_tool

@register_tool('parse_swap_intent')
class ParseSwapIntent(BaseTool):
    description = '执行zetachain转账操作'
    parameters = {
        'type': 'object',
        'properties': {
            'chain': {
                'description':
                    '用户需要的chain名称',
                'type':
                    'string',
            },
            'amount': {
                'description':
                    '用户需要执行的金额数量',
                'type':
                    'string',
            },
            'tokenIn': {
                'description':
                    '用户需要转出的代币',
                'type':
                    'string',
            },
            'tokenOut': {
                'description':
                    '用户需要传成的代币',
                'type':
                    'string',
            },

        },
        'required': ['chain','tokenIn','tokenOut','amount'],
    }

    def call(self, params: Union[str, dict], **kwargs) -> str:
        params = self._verify_json_format_args(params)

        chain = params['chain']
        tokenIn = params['tokenIn']
        tokenOut = params['tokenOut']
        amount = params['amount']
        tokens = ['ETH', 'USDC', 'USDT', 'BTC', 'BNB', 'SOL', 'SUI']
        chains = ['Ethereum', 'Bitcoin', 'Solana', 'BNBChain', 'Sui']
        # 检查适配的代币
        if str(tokenIn).upper() not in [w.upper() for w in tokens]:
            print("原始代币名称不存在！")
            return f'{tokenIn}原始代币名称不存在,支持的代表列表{tokens}'
        elif str(tokenOut).upper() not in [w.upper() for w in tokens]:
            print(f"{tokenOut}转出代币名称不存在！")
            return f'{tokenOut}转出代币名称不存在,支持的代表列表{tokens}'
        elif str(chain).upper() not in  [w.upper() for w in chains]:
            print("链名称不存在！")
            return '链名称不存在'
        else:
            json_value = {"chain": chain, "tokenIn": tokenIn, "tokenOut": tokenOut, "amount": amount}
            self.handle_zetachain(json_value)
            return json.dumps(json_value, ensure_ascii=False)


    def create_private_key(self):
        """
            获取私钥地址
        """
        # 展开 ~ 为用户主目录
        anvil_path = os.path.expanduser("~/.zetachain/localnet/anvil.json")

        # 使用 jq 解析 JSON 文件，提取第一个私钥
        result = subprocess.run(
            ["jq", "-r", ".private_keys[0]", anvil_path],
            capture_output=True,
            text=True
        )

        if result.returncode == 0:
            private_key = result.stdout.strip()  # 成功获取私钥
            print("Private Key:", private_key)
        else:
            print("Error:", result.stderr)
        return private_key


    def deploy_contact(self, directory, rpc_url, private_key):
        """
        UNISWAP_ROUTER=$(jq -r '.["31337"].contracts[] | select(.contractType == "uniswapRouterInstance") | .address' ~/.zetachain/localnet/registry.json) && echo $UNISWAP_ROUTER

          #UNIVERSAL=$(forge create Universal --rpc-url http://localhost:8545 --private-key 0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80 --evm-version paris --broadcast --json | jq -r .deployedTo) && echo $UNIVERSAL
        """
        # 切换到目标目录
        os.chdir(directory)

        # 构建命令
        command = f"npx tsx commands/index.ts deploy --private-key {private_key} --rpc {rpc_url} | jq -r .contractAddress"

        try:
            # 执行命令并捕获输出
            result = subprocess.run(command, shell=True, check=True, stdout=subprocess.PIPE, stderr=subprocess.PIPE,
                                    text=True)

            # 获取合约地址
            contract_address = result.stdout.strip()

            print(f"Contract Address: {contract_address}")
            return contract_address
        except subprocess.CalledProcessError as e:
            print(f"Error executing command: {e}")
            print(f"stderr: {e.stderr}")
            return None

    def get_gateway_address(self, network_id):
        """
                    获取私钥地址
                """
        # 展开 ~ 为用户主目录
        anvil_path = os.path.expanduser("~/.zetachain/localnet/registry.json")

        # 使用 jq 解析 JSON 文件，提取第一个私钥
        result = subprocess.run(
            ["jq", "-r", f".[\"{network_id}\"].contracts[] | select(.contractType == \"gateway\") | .address",
                 anvil_path],
            capture_output=True,
            text=True
        )

        if result.returncode == 0:
            gateway = result.stdout.strip()  # 成功获取私钥
            print("gateway address:", gateway)
        else:
            print("Error:", result.stderr)
        return gateway

    def get_the_gas_token(self, chain_id):
        """
        ZRC20_BNB=$(jq -r '."98".chainInfo.gasZRC20' ~/.zetachain/localnet/registry.json) && echo $ZRC20_BNB
        """
        # 展开 ~ 为用户主目录
        anvil_path = os.path.expanduser("~/.zetachain/localnet/registry.json")

        # 使用 jq 解析 JSON 文件，提取第一个私钥
        result = subprocess.run(
            ["jq", "-r", f".\"{chain_id}\".chainInfo.gasZRC20",anvil_path],
            capture_output=True,
            text=True
        )

        if result.returncode == 0:
            result = result.stdout.strip()  # zrc-20 chain_id
            print("gas token address:", result)
        else:
            print("Error:", result.stderr)
        return result

    def get_wallet_address(self, private_key):
        """
        RECIPIENT=$(cast wallet address $PRIVATE_KEY) && echo $RECIPIENT
        """


        # 使用 jq 解析
        result = subprocess.run(
            ["cast", "wallet", "address", private_key],
            capture_output=True,
            text=True
        )

        if result.returncode == 0:
            result = result.stdout.strip()  # zrc-20 chain_id
            print("recipient address:", result)
        else:
            print("Error:", result.stderr)
        return result

    def swap_token(self, tokenIn_chain_name, rpc_address, chain_id, amount, gateway, private_key, receiver_address, sender, gas_token):
        """
        npx zetachain evm deposit-and-call --rpc http://localhost:8545 \
  --chain-id 11155112 \
  --gateway $GATEWAY_ETHEREUM \
  --amount 0.001 \
  --types address bytes bool \
  --receiver $UNIVERSAL \
  --private-key $PRIVATE_KEY \
  --values $ZRC20_BNB $RECIPIENT true
        """
        # 构建命令
        command = (f"npx zetachain {tokenIn_chain_name} deposit-and-call --rpc {rpc_address} --chain-id {chain_id} "
                   f" --amount {amount} --private-key {private_key} "
                   f" --gateway {gateway} --receiver {receiver_address} "
                   f" --types address bytes bool "
                   f"--values {gas_token} {sender} true")
        print(command)
        try:
            # 执行命令并捕获输出
            process = subprocess.Popen(command, shell=True,
                                    stdin=subprocess.PIPE,
                                    stdout=subprocess.PIPE, stderr=subprocess.PIPE,
                                    text=True)
            # 读取输出并检查是否需要确认
            stdout, stderr = process.communicate(input="y\n")
            #print(f"最终返回：{stdout}")
            # 获取合约地址
            result = stdout.strip()
            #print(f"result: {result}")
            return result
        except subprocess.CalledProcessError as e:
            print(f"Error executing command: {e}")
            print(f"stderr: {e.stderr}")
            return None


    def handle_zetachain(self, json_value):
        print(f"传入参数：{json_value}")
        chain = json_value['chain']
        tokenIn = json_value['tokenIn']
        tokenOut = json_value['tokenOut']
        amount = json_value['amount']
        my_address = "test_address"
        chain_id = "11155112"
        #chain_number = 84532
        print(f"参数：chain：{chain}, tokenIn:{tokenIn}, tokenOut:{tokenOut}, amount:{amount}")
        print(f"模拟执行转移代币操作----------")
        print(f"执行命令，把链上[{chain}]数量[{amount}]的[{tokenIn}]转换成[{tokenOut}].")

        chain_type = ["evm", "solana", "sui", "bitcoin", "ton"]
        #self.handle_test(123)
        # print(f"部署合约并返回合约地址：UNIVERSAL =$(npx tsx commands deploy --private-key $PRIVATE_KEY | jq -r.contractAddress) && echo $UNIVERSAL")
        # print(f"执行shell命令：ZRC20_ETHEREUM_ETH =$(zetachain q tokens show --symbol sETH.SEPOLIA -f zrc20) && echo $ZRC20_ETHEREUM_ETH")
        # print(f"执行shell命令,获取：RECIPIENT =$(cast wallet address $PRIVATE_KEY) & & echo $RECIPIENT")
        # print(f"命令：npx zetachain evm deposit-and-call \
        #           --chain-id {chain_id} \
        #           --amount {amount} \
        #           --types address bytes bool \
        #           --receiver {my_address} \
        #           --values $ZRC20_ETHEREUM_ETH $RECIPIENT true")

        rpc_address = "http://localhost:8545"
        gas_chain_id = 98
        from_chain_name = "evm"
        private_key = self.create_private_key()
        print(f"返回的私钥：{private_key}")
        deploy_address = self.deploy_contact("/Users/a1/zetachain/swap", rpc_address, private_key)
        gateway = self.get_gateway_address(chain_id)
        print(f"获取gateway地址：{gateway}")
        gas_token = self.get_the_gas_token(gas_chain_id)
        print(f"获取zrc-20 BNB gas token:{gas_token}")
        wallet_address = self.get_wallet_address(private_key)
        print(f"wallet_address address:{wallet_address}")
        swap_result = self.swap_token(from_chain_name, rpc_address, chain_id,
                                      str(amount), gateway, private_key, deploy_address, wallet_address, gas_token)
        print(f"swap_result:{swap_result}")

if __name__ == '__main__':
    swap = ParseSwapIntent()
    rpc_address = "http://localhost:8545"
    chain_id = "11155112"
    gas_chain_id = 98
    from_chain_name = "evm"
    private_key = swap.create_private_key()
    print(f"返回的私钥：{private_key}")
    deploy_address = swap.deploy_contact("/Users/a1/zetachain/swap", "http://localhost:8545", private_key)
    #0xffa7CA1AEEEbBc30C874d32C7e22F052BbEa0429
    #print(f"返回部署地址：{deploy_address}")
    gateway = swap.get_gateway_address(chain_id)
    print(f"获取gateway地址：{gateway}")
    gas_token = swap.get_the_gas_token(gas_chain_id)
    print(f"获取zrc-20 BNB gas token:{gas_token}")
    wallet_address = swap.get_wallet_address(private_key)
    print(f"wallet_address address:{wallet_address}")
    swap_result = swap.swap_token(from_chain_name, rpc_address, chain_id,
                                  "0.01", gateway, private_key,deploy_address, wallet_address, gas_token)
    print(f"swap_result:{swap_result}")
    
  
测试代码
from qwen_agent.agents import Assistant
from qwen_agent.tools.parse_swap_intent import ParseSwapIntent


def init_parse_agent_service():
    llm_cfg = {'model': 'qwen-max', 'api_key': 'xxxx',
               'model_server': 'dashscope'}
    system = ('According to the user\'s request, get the result with parse_swap_intent tool ,'
              'if the tool return a json data , reply the process is success and the json data')

    tools = [ParseSwapIntent(), 'code_interpreter']  # code_interpreter is a built-in tool in Qwen-Agent
    bot = Assistant(llm=llm_cfg, system_message=system, function_list=tools)

    return bot

def test_custom_parse_agent_object_fail():
    # Define the agent
    bot = init_parse_agent_service()

    # Chat
    messages = [{'role': 'user', 'content': '把我 50 USDC 兑换成 Ethereum 上的 USDT'}]
    for response in bot.run(messages=messages):
        print('bot response:', response)
    messages.extend(response)
    for result in response:
        print(result)
        if result['role'] == 'assistant' and result['content'] is not None:
            print(f"回复内容：{result['content']}")
```
<!-- DAILY_CHECKIN_2025-12-06_END -->

# 2025-12-05
<!-- DAILY_CHECKIN_2025-12-05_START -->


记录先打个卡
<!-- DAILY_CHECKIN_2025-12-05_END -->

# 2025-12-04
<!-- DAILY_CHECKIN_2025-12-04_START -->



前端层：用户界面

  解析意图层：使用qwen3解析自然语言，并返回json

  中间层：通过接收qwen3返回的json数据，执行用户的链操作，包括swap，tranfer等

    技术栈，使用CLI方式或者ZetaChain Toolkit

    以下是使用CLI方式的简单描述

1.  使用parse\_swap\_intent的值，把链名，目标代币，原始代币和额度数据解析出来
    
2.  调用后端zeta命令,执行swap、message等操作
    
    1.  获取钱包地址
        
    
    ```Shell
    PRIVATE_KEY
    ```
    
    -   确保swap合约已经部署好，没有部署合约的执行部署命令：
        
    
    ```Shell
    UNIVERSAL=$(npx tsx commands deploy --private-key $PRIVATE_KEY | jq -r .contractAddress) && echo $UNIVERSAL
    ```
    
    -   获取接收地址
        
    
    ```Shell
    RECIPIENT=$(cast wallet address $PRIVATE_KEY) && echo $RECIPIENT
    ```
    
    -   获取转化成目标代币的地址，根据json修改具体的值
        
    
    ```Shell
    ZRC20_ETHEREUM_ETH=$(zetachain q tokens show --symbol sETH.SEPOLIA -f zrc20) \
    && echo $ZRC20_ETHEREUM_ETH        
    ```
    
    -   执行转换命令，链ID填写对应的chain-id, 链名填写对应的 zetachain 白名单链名
        
    
    ```Shell
    npx zetachain 链名（evm/solana/sui/bitcon/ton）deposit-and-call --chain-id 链ID \
      --amount 金额 --types address bytes bool --receiver $UNIVERSAL \
      --values $ZRC20_ETHEREUM_ETH $RECIPIENT true    
    ```
    
3.  返回执行结果
<!-- DAILY_CHECKIN_2025-12-04_END -->

# 2025-12-03
<!-- DAILY_CHECKIN_2025-12-03_START -->




```Python
基于Qwen-Agent工程
用户请求Agent  -》 Agent 根据系统提示词调用工具获取返回结果 -》 Agent 根据工具返回的内容再做处理回复给用户

\Qwen-Agent\qwen_agent\tools\parse_swap_intent.py
import json
from typing import Union

from qwen_agent.tools import BaseTool
from qwen_agent.tools.base import register_tool

@register_tool('parse_swap_intent')
class ParseSwapIntent(BaseTool):
    description = '执行zetachain转账操作'
    parameters = {
        'type': 'object',
        'properties': {
            'chain': {
                'description':
                    '用户需要的chain名称',
                'type':
                    'string',
            },
            'amount': {
                'description':
                    '用户需要执行的金额数量',
                'type':
                    'string',
            },
            'tokenIn': {
                'description':
                    '用户需要转出的代币',
                'type':
                    'string',
            },
            'tokenOut': {
                'description':
                    '用户需要传成的代币',
                'type':
                    'string',
            },

        },
        'required': ['chain','tokenIn','tokenOut','amount'],
    }

    def call(self, params: Union[str, dict], **kwargs) -> str:
        params = self._verify_json_format_args(params)

        chain = params['chain']
        tokenIn = params['tokenIn']
        tokenOut = params['tokenOut']
        amount = params['amount']
        print(f"模拟执行转移代币操作----------")
        return json.dumps({"chain": chain, "tokenIn": tokenIn, "tokenOut": tokenOut, "amount": amount}, ensure_ascii=False)
        

\Qwen-Agent\tests\agents\test_custom_tool_object_zetachain.py
# Copyright 2023 The Qwen team, Alibaba Group. All rights reserved.
# 
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
# 
#    http://www.apache.org/licenses/LICENSE-2.0
# 
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.


from qwen_agent.agents import Assistant
from qwen_agent.tools.parse_swap_intent import ParseSwapIntent


def init_parse_agent_service():
    llm_cfg = {'model': 'qwen-max', 'api_key': 'xxxxx', 'model_server': 'dashscope'}
    system = ('According to the user\'s request, get the result with parse_swap_intent tool ,'
              'if the tool return a json data , reply the process is success and the json data')

    tools = [ParseSwapIntent(), 'code_interpreter']  # code_interpreter is a built-in tool in Qwen-Agent
    bot = Assistant(llm=llm_cfg, system_message=system, function_list=tools)

    return bot

# 测试文本请求
def test_custom_parse_agent_object():
    # Define the agent
    bot = init_parse_agent_service()

    # Chat
    messages = [{'role': 'user', 'content': '把我 50 USDT 兑换成 Polygon 上的 MATIC'}]
    for response in bot.run(messages=messages):
        print('bot response:', response)
    messages.extend(response)
    for result in response:
        print(result)
        if result['role'] == 'assistant' and result['content'] is not None:
            print(f"回复内容：{result['content']}")
```

![](https://variation.feishu.cn/space/api/box/stream/download/asynccode/?code=MTNlODAxOGRjYTdlNTE5ZjM2OTgzOGQxYTAzYmQyMjRfUHFhZDVsbjJFRnhkWkc4d2YyalA0RDZrbTlOc21HeVNfVG9rZW46Wkk0QmJDTER0b2VsQmJ4cjlIR2NkUzQ5bnhkXzE3NjQ3MzM0MTQ6MTc2NDczNzAxNF9WNA)
<!-- DAILY_CHECKIN_2025-12-03_END -->

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
