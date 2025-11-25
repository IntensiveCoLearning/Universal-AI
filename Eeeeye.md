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
