---
timezone: UTC+8
---

# 迷路的笛卡尔

**GitHub ID:** spryo9

**Telegram:** @ExponentialLifeee

## Self-introduction

浙江大学在读

## Notes

<!-- Content_START -->
# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->
今天学校事情太多，很多学习资料都没有看，workshop也因为组会错过。周末补上。  
1.环境配置选择Foundry (Rust/Solidity): **首先** 其速度极快，用 Solidity 写测试。是现在高阶开发者的心头好，但学习曲线陡峭。虽然我没有 Rust 基础，但还是想N+100挑战一下。  
2.网络选择：我决定使用 **Testnet (Athens 测试网)**。  
虽然本地链速度快，但作为第一次体验，我想亲眼看到真实的跨链交互（例如在比特币测试网浏览器上看到交易），这能让我对“全链互操作性”有更直观的体感。
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->

今天把昨天没有做的部分补齐了一下：

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/spryo9/images/2025-11-26-1764157976016-image.png)

```

在Faucet上的领取了测试币
成功调用了qwen的api
这个过程中学习了一下json,简单易上手


import requests
```

```
import json
```

```
url = "https://dashscope.aliyuncs.com/compatible-mode/v1/chat/completions"
```

```
headers = {
```

```
    "Authorization": f"Bearer {API_KEY}", 
```

```
    "Content-Type": "application/json"
```

```
}

```

```
data = {
```

```
    "model": "qwen-plus",
```

```
    "messages": [
```

```
        {"role": "user", "content": "你好，你是谁？"}
```

```
    ]
```

```
}

```

```
try:
```

```
    response = requests.post(url, headers=headers, json=data)

```

```
    if response.status_code == 200:
```

```
        result = response.json()  

```

```
        ai_message = result["choices"][0]["message"]["content"]
```

```
        print("AI回答：", ai_message)

```

```
        usage = result["usage"]
```

```
        print(f"\n本次消耗：{usage['total_tokens']} tokens")
```

```
    else:
```

```
        print(f"请求失败，状态码：{response.status_code}")
```

```
        print("错误信息：", response.text)
```

```
except Exception as e:
```

```
    print("出错了！", e)


```

之前第一天写过一版：  
实现方式：用户在比特币网络中转账给TSS地址btc,Message/Memo 附带了 ZetaChain 上的合约地址和参数，zeta的观察者超过66%的比例投票确认btc到账TSS地址，在zeta链上生成zrc-20 btc，调用用户指定 `onCrossChainCall` 函数，执行智能合约代码。之后zeta在流动性池（liquidity pool），结合自动市商（AMM）在确保大致(zrc-20)btc \* eth=常量的算法下，兑换出相应的zrc-20eth,之后合约调用withdraw函数，TSS验证者在eth网络签名，并广播，给以太坊的用户转入eth。整个过程中zeta代币作为btc/eth的中间桥梁。  
补充的逻辑如下：

1.用户发起交易 -> 生成 Tx Hash。

2\. ZetaChain Validators (Observers)：扫描到 Tx Hash -> 验证 -> 提交到 ZetaChain 共识层。

3\. ZetaChain Core：由于共识达成，触发 zEVM 中的合约逻辑 -> 状态改变（State Transition）。

4\. TSS Signers：合约要求提现 -> 验证者们计算签名碎片 -> 合成出 有效签名。

5\. External Chain：验证签名 -> 资金转移  

关于我眼中的 uiversal 和getway  
1.Universal App 就是一种无视链的存在的超级应用。

2\. 以前，如果我想从比特币网络转钱去以太坊玩游戏，我需要下载好几个钱包，进行复杂的跨链操作，不仅慢还容易丢币。

3\. Universal App 就像现在的微信支付，不管底层是哪家银行（哪条链），用户只需要操作一次，就能在任何地方使用任何资产。它把复杂的跨链技术藏在了后台，只给用户最简单的体验。

4\. Gateway 大概做什么？ Gateway（网关）就像是 ZetaChain 连接外部世界的“翻译器”。

5\. 比如比特币（BTC）原本是无法在智能合约里运行的。Gateway 的作用就是把外面的 BTC 接收进来，给它穿上一层 ZetaChain 能识别的“外套”（ZRC-20），让它能在 ZetaChain 内部被全链合约调度和交易。当需要提现时，Gateway 再帮它脱掉外套，把它变回原生的 BTC 发送回比特币网络。它是连接“孤岛”和“枢纽”的关键桥梁。  
  

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/spryo9/images/2025-11-26-1764162706109-image.png)
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->


环境搭建 _完成了，还用了 WSL + Foundry/Hardhat_ 配置。

在AI的帮助下编写 Swap.sol，初步理解了 \*ZRC-20\*\*（跨链资产）、\*\*SystemContract\*\*（系统合约）和 Uniswap Router（交易逻辑）。  

```
// SPDX-License-Identifier: MIT
```

```
pragma solidity 0.8.7;
```

```
import "@zetachain/protocol-contracts/contracts/zevm/SystemContract.sol";
```

```
import "@zetachain/protocol-contracts/contracts/zevm/interfaces/zContract.sol";
```

```
import "@zetachain/toolkit/contracts/BytesHelperLib.sol";
```

```
import "@zetachain/protocol-contracts/contracts/zevm/interfaces/IZRC20.sol";
```

```
interface IUniswapV2Router02 {
```

```
    function swapExactTokensForTokens(
```

```
        uint amountIn,
```

```
        uint amountOutMin,
```

```
        address[] calldata path,
```

```
        address to,
```

```
        uint deadline
```

```
    ) external returns (uint[] memory amounts);
```

```
}
```

```
contract Swap is zContract {
```

```
    SystemContract public immutable systemContract;
```

```
    // 修正点 1: 使用报错信息里提供的正确 checksum 地址
```

```
    address public constant UNISWAP_ROUTER = 0x2cA7d64A7eFE2D62a04Be5E8DD160FdD46A2C37a;
```

```
    constructor(address systemContractAddress) {
```

```
        systemContract = SystemContract(systemContractAddress);
```

```
    }
```

```
    function onCrossChainCall(
```

```
        zContext calldata context,
```

```
        address zrc20,
```

```
        uint256 amount,
```

```
        bytes calldata message
```

```
    ) external virtual override {
```

```
        address targetToken = BytesHelperLib.bytesToAddress(message, 0);
```

```
        address recipient = BytesHelperLib.bytesToAddress(message, 20);
```

```
        uint256 outputAmount = amount;
```

```
        if (zrc20 != targetToken) {
```

```
            IZRC20(zrc20).approve(UNISWAP_ROUTER, amount);
```

```
            address[] memory path = new address[](2);
```

```
            path[0] = zrc20;
```

```
            path[1] = targetToken;
```

```
            uint256[] memory amounts = IUniswapV2Router02(UNISWAP_ROUTER).swapExactTokensForTokens(
```

```
                amount,
```

```
                0, 
```

```
                path,
```

```
                address(this), 
```

```
                block.timestamp + 300
```

```
            );
```

```
            outputAmount = amounts[1];
```

```
        }
```

```
        // 修正点 2: 把 address 类型的 recipient 转换成 bytes 类型
```

```
        // 因为比特币地址不仅仅是 0x 开头的，所以这里统一要求用 bytes
```

```
        IZRC20(targetToken).withdraw(abi.encodePacked(recipient), outputAmount);
```

```
    }
```

```
}

```
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->



# self-introduction:

本科地理信息科学，目前在浙江大学读研，从事地物光谱研究。web3小白，只有一些二级市场交易的经验。但深深认同web3的理念，读过很多行业早期人物的作品，例如猫说，囤比特币等等。想通过这次学习，加深自己对web3领域的理解，最好希望自己能在3年后毕业时，进入自己目前心仪的web3公司从事自己想做的板块。

# 补习了一下基础知识：

\### 智能合约的三大铁律：

1.不可篡改

代码落子无悔，因此区块链开发需要极其谨慎

2.透明公开

智能合约的代码和存储的数据，对全世界所有人可见

3.确定性

同样的输入，永远得到同样的输出

\### 智能合约组成：

1.状态函数（state variables)：

这是合约用来“存数据”的地方。数据是永久写在区块链硬盘上的

2.函数(functions)

3.事件（events)

广播一下具体的合约交易

\### 交易逻辑（自己粗糙的理解）：

情景：通过zetachain来实现eth和btc的交易

实现方式：用户在比特币网络中转账给TSS地址btc,Message/Memo 附带了 ZetaChain 上的合约地址和参数，zeta的观察者超过66%的比例投票确认btc到账TSS地址，在zeta链上生成zrc-20 btc，调用用户指定 `onCrossChainCall` 函数，执行智能合约代码。之后zeta在流动性池（liquidity pool），结合自动市商（AMM）在确保大致(zrc-20)btc \* eth=常量的算法下，兑换出相应的zrc-20eth,之后合约调用withdraw函数，TSS验证者在eth网络签名，并广播，给以太坊的用户转入eth。整个过程中zeta代币作为btc/eth的中间桥梁。  
  
补习了一下基础知识：

\### 智能合约的三大铁律：

1.不可篡改

代码落子无悔，因此区块链开发需要极其谨慎

2.透明公开

智能合约的代码和存储的数据，对全世界所有人可见

3.确定性

同样的输入，永远得到同样的输出

\### 智能合约组成：

1.状态函数（state variables)：

这是合约用来“存数据”的地方。数据是永久写在区块链硬盘上的

2.函数(functions)

3.事件（events)

广播一下具体的合约交易

\### 交易逻辑（自己粗糙的理解）：

情景：通过zetachain来实现eth和btc的交易

实现方式：用户在比特币网络中转账给TSS地址btc,Message/Memo 附带了 ZetaChain 上的合约地址和参数，zeta的观察者超过66%的比例投票确认btc到账TSS地址，在zeta链上生成zrc-20 btc，调用用户指定 `onCrossChainCall` 函数，执行智能合约代码。之后zeta在流动性池（liquidity pool），结合自动市商（AMM）在确保大致(zrc-20)btc \* eth=常量的算法下，兑换出相应的zrc-20eth,之后合约调用withdraw函数，TSS验证者在eth网络签名，并广播，给以太坊的用户转入eth。整个过程中zeta代币作为btc/eth的中间桥梁。

  
已经注册 / 配置好 ZetaChain 开发所需环境，确认能访问 Developers 页面。

已经注册 Qwen 账号并确认可以进入控制台。
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
