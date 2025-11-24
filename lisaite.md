---
timezone: UTC+8
---

# Seth

**GitHub ID:** lisaite

**Telegram:** @lst060115

## Self-introduction

大三 入圈一年 正在学习solidity和区块链底层

## Notes

<!-- Content_START -->
# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->
今天配置了在wsl里根据教程配置了环境，本人算是小白以及不了解zetachain所以今天主要是用gemini总结了zetachain的这个项目，大致的能了解其运行 在节点方面不光是有一个传统的pos共识，还要一个名为zetaclient的东西，主要分为两个作用其一是监听外部链比如bitcoin的内存池和eth的事件日志，其二是利用TSS技术持有外部链私钥的片段只有2/3的节点同意才可以执行交易签名。

全链智能合约的话不是看的太懂大概说一下。zetachain资产的锚定主要通过外链往zetachain指定的同一外链账户里转账来往自己zetachain的对应账户里充钱（词穷了不知道怎么形容了）。在zetachain里有zrc-20标准比如zrc-20的btc或eth，然后在zetachain编写智能合约来操作zrc-20代币，抽象的实现了全链智能合约我在这方面以小白之见觉得非常好，且不说eth就说btc不能实现图灵完备，我只需要简单的进行一个转账操作，那么就可以在zetachain里实现图灵完备来从事各种金融衍生活动最后提现只需要销毁代币就可以把btc返回到bitcion链上退回给用户。

  
B. 跨链消息传递 (Cross-Chain Messaging, CCM)对于不需要复杂逻辑的场景，ZetaChain 提供轻量级的消息传递功能。

-   开发者可以在一条链上调用合约，通过 ZetaChain 中继，将数据或价值传递到另一条链上的合约。这类似于 LayerZero 的模式，但 ZetaChain 提供了状态层的统一管理_。_**能力有限没看懂**
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
