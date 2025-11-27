---
timezone: UTC+8
---

# casey

**GitHub ID:** skcvan

**Telegram:** @skcvan

## Self-introduction

小白，加油

## Notes

<!-- Content_START -->
# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->
想搞一个全链留言板，任何人可以从任意链（Bitcoin、Ethereum、BNB Chain、Polygon、Solana…）往 ZetaChain 上发一条带文字的转账，合约自动把留言永久记录下来，并且可以在网页上看到来自不同链的留言

准备使用测试网，cli+hardhat
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->

**1\. Universal App（通用应用 / 全链应用）是什么？** Universal App 就是“一次部署、原生管理所有链资产和数据的智能合约”。 它只部署在 ZetaChain 上，却可以直接读写 Bitcoin、Ethereum、Solana、BSC、Polygon、Tron 等几十条链的原生资产和状态，不需要任何包装币（wBTC、wETH）、也不需要跨链桥。 一句话总结：写一份合约，就能像操作本地资产一样操作全网资产。

**2\. Gateway（网关）大概做什么？** Gateway 是 ZetaChain 在每条外部链（Bitcoin、ETH、Solana 等）上部署的一组轻节点 + 智能合约，负责三件事：

-   监听外部链的事件（比如你往某个地址转了 0.1 BTC）
    
-   把事件签名后上报给 ZetaChain 的观察者（Observers）
    
-   接收 ZetaChain 的指令，在外部链上执行最终操作（比如从 Gateway 地址把 BTC 转给你指定的接收者） 它相当于 ZetaChain 在每条链上的“手和眼睛”。
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->


成功安装zetachain cli

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/skcvan/images/2025-11-25-1764074715810-image.png)

-   Network name: `ZetaChain Testnet`
    
-   Chain ID: `7001`
    
-   Currency symbol: `ZETA`
    
-   EVM RPC（选一个用即可，例如）：
    
    -   `https://zetachain-athens-evm.blockpi.network/v1/rpc/public`
        
        或
        
    -   `https://rpc.ankr.com/zetachain_evm_athens_testnet`
        
-   EVM Explorer: `https://explorer.zetachain.com/`
    
-   Faucet 文档入口：`https://www.zetachain.com/docs/reference/faucet/`
    

根据通义千问官方的文档部署api，调用成功

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/skcvan/images/2025-11-25-1764074784923-image.png)
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->



# 对于zetachain的一些初步理解

ZetaChain 本质上是一个基于 Cosmos SDK 构建的权益证明（PoS）区块链。

相较于其他公链只能看到自己链上的数据，zetachain节点可以看到 BTC, ETH, BNB 等外部链上的交易。

并且在签名方面，普通节点只能用自己的私钥签名，而zeta节点不一样，举个例子：当需要从比特币金库里转账给用户时，没有任何一个节点能单独转账。节点们必须聚在一起（通过网络协议），每人拿出一部分“密钥碎片”，共同计算出一个合法的比特币签名。

# 任务

能进入官网的developer界面

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/skcvan/images/2025-11-24-1763947347511-image.png)

完成api平台注册

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/skcvan/images/2025-11-24-1763947702729-image.png)
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
