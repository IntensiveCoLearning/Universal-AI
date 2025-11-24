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
