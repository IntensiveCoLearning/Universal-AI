---
timezone: UTC+8
---

# 荒原

**GitHub ID:** zylg-create

**Telegram:** @Devasteland

## Self-introduction

加速探索Web3，looking for alpha

## Notes

<!-- Content_START -->
# 2025-12-07
<!-- DAILY_CHECKIN_2025-12-07_START -->
两周完结了，下周要开始做Demo，大佬们不要再拷打新人了。

想着可以用AI做一些有意思的东西。
<!-- DAILY_CHECKIN_2025-12-07_END -->

# 2025-12-06
<!-- DAILY_CHECKIN_2025-12-06_START -->

学一下DeFi，之前不太了解DeFi具体是干什么的，只知道质押、金库智能合约。

## **LSDFi**

`Liquid Derivatives Finance 流动性质押衍生品金融` 建立在`Liquid staking + staking derivatives` 之上 所谓流动性质押，就是用户把 ETH 或者其他加密资产质押(`staking`)用来支持网络，同时代表一种质押权益的代币(`LSD / LST`)，它是可以`流动`的，也就是可以被交易。通过 LST，持有者可以既获得质押奖励，同时也可以将手中的 LST 进行 DeFi 实用 `借贷`、`抵押`、`流动性挖矿` (`liquidity-pool`)、`收益耕种` (`yield farming`)、`发行稳定币` (`stablecoin`)、或者参与组合策略 (yield-aggregation) 等。

### **通常 LSDFi 的流程如下：**

1.质押 (Staking) — 用户将原生加密资产 (例如 ETH) staking 到某个“流动质押 (liquid staking)”协议。

2.发行衍生代币 (LSD / LST) — 协议给用户一个代币 (比如 stETH、rETH、wstETH 等)，这个代币代表你质押的资产 + 随时间累积的质押奖励。

3.DeFi 使用 / 投资 (Composability) — 你可以将这个代币 (LST) 用到其他 DeFi 协议里：借贷、提供流动性、作为抵押品、参与收益耕作等。

获得双重收益 / 功能 — 你既持续获得 staking 奖励 (因为底层资产仍在质押中)，也能通过 DeFi 活动获取额外收益 (如利息、手续费、奖励代币等)；同时保持流动性 (你可以交易 / 转让 LST)。

### **在跨链场景下**

流动性质押会更加灵活，比如

在链 A 质押获得 LST

桥到链 B 借稳定币

再桥到链 C 做 LP 收手续费

再回链 A 抵押获得更多 LST（杠杆策略）

## **收益管理**

当前的区块链有几个痛点：

`收益来源碎片化` 每一条链都由自己的 DeFi，Staking，流动性池等等

`流动性碎片话` 各链的 USDT/USDC 都不一样

`用户操作成本极高`  南。 `用户作成本极高`

zetachain 有几个优势。首先是不同链的加密货币可以用 ZRC20 统一标识，也就是说，不同加密货币在 zetachain 侧的交换会非常方便，不需要 burn-mint 这种形式 (我只是说在 zetachain 内部，zetachain 和别的链交互的时候还是得 burn-mint 的) 昨天刚刚看了`Messaging`。zetachain 对于 ERC20 的跨链交易非常友好。
<!-- DAILY_CHECKIN_2025-12-06_END -->

# 2025-12-04
<!-- DAILY_CHECKIN_2025-12-04_START -->


发现远程工作的公司好像都喜欢在周四晚上开会，正好赶上另一家今晚发布新工具，又去学了一下mcp的使用。

今晚的作业就速成一下。

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/zylg-create/images/2025-12-04-1764857495132-image.png)
<!-- DAILY_CHECKIN_2025-12-04_END -->

# 2025-12-02
<!-- DAILY_CHECKIN_2025-12-02_START -->



### 1.成功调用Qwen的API

我选用了Qwen的qwen-plus模型

调用参数是

```python
data = {
    "model": "qwen-plus",  # 你选择的模型，这里用的是 qwen-plus
    "messages": [
        {"role": "user", "content": "请生成一段关于 ZetaChain 的介绍。"}
    ],
    "temperature": 0.7,  # 生成内容的创意程度，0.7是一个适中的值
    "max_tokens": 1000  # 最大返回 tokens 数量
}
```

终端返回的结果

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/zylg-create/images/2025-12-02-1764676073824-image.png)

### 2.学习使用Qwen-Agent

[https://github.com/QwenLM/Qwen-Agent](https://github.com/QwenLM/Qwen-Agent)文档写的还是很详细的，结合AI很轻松就能跑通官方示例（but，我让他给我画只小狗结果给我画了只猫）

后面又做了一个把字符串转大写的Tool，确认 Agent 能自动调用这个 Tool 并返回结果

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/zylg-create/images/2025-12-02-1764680877931-image.png)
<!-- DAILY_CHECKIN_2025-12-02_END -->

# 2025-12-01
<!-- DAILY_CHECKIN_2025-12-01_START -->





尝试在本地部署第一份合约，输入自己的私钥已经连接成功了，但我不知道怎么把私钥“隐藏起来”（群里大佬已详细解答，我自己尝试一遍后会写笔记）

![fa6539e20a52d6248a2959a47c60a13e.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/zylg-create/images/2025-12-01-1764587262621-fa6539e20a52d6248a2959a47c60a13e.png)

合约部署完就是跑通前端

![fe3bab2d2ba1583b7d2d818a49214dd8.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/zylg-create/images/2025-12-01-1764591986863-fe3bab2d2ba1583b7d2d818a49214dd8.png)

也是发送成功了，我还自己加了一个Sepolia的选项（其他测试网都没币）

后面还要学swap和messaging，头有点大
<!-- DAILY_CHECKIN_2025-12-01_END -->

# 2025-11-30
<!-- DAILY_CHECKIN_2025-11-30_START -->








### 自己想做的第一个 Universal App 想实现的“打印 / 记录 / 简单逻辑”是什么。

简单加减逻辑，例如 “增加积分 / 减少积分”

### 为后续的 Hello World Demo 决定一种工作流：

使用 CLI + Foundry

其实我觉得用测试网完成更适合小白，我上次写的一个彩票Dapp就是在测试网上做的，一个合约端和一个前端就搞定了。不过这次机会难得，好不容易把环境搭建好了，正好多学一点。

-   **ZRC-20 vs ERC-20**：
    
    -   ERC-20：链内代币
        
    -   ZRC-20：跨链代币，ZetaChain 托管和编排资产
        
-   **核心能力**：
    
    -   单接口管理多链资产
        
    -   支持跨链存取与交换
        
    -   支持 Gas 代币 + ERC-20 资产
        
-   **应用场景**：
    
    -   跨链 DeFi（DEX、借贷、投资组合管理）
        
    -   跨链钱包资产统一显示
        
    -   通用 NFT 通行证（初步设想）：购买一次 NFT ，能在不同链的活动或游戏中使用。
<!-- DAILY_CHECKIN_2025-11-30_END -->

# 2025-11-29
<!-- DAILY_CHECKIN_2025-11-29_START -->










-   画一张简单的架构图：
    
    ZetaChain 中心 + Bitcoin / Ethereum / Solana 等外围链 + Gateway。
    

![64ed5d9019d0af0f05719232d98e1e2f.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/zylg-create/images/2025-11-29-1764394975371-64ed5d9019d0af0f05719232d98e1e2f.png)

这边贴一段看到的讲**Ominichain是啥的**笔记

ZRC20文檔中看到 **Ominichain**（全鏈）這個區塊鏈架構概念，很好奇這啥

> Omni代表全部、所有

先講講這Ominichain目標：「讓開發者只寫一份合約，就能控制所有區塊鏈上的資產與數據；讓使用者只用一個錢包，就能操作所有鏈上的應用。」

這與「跨鏈」或「多鏈」有點不同，下面來詳細的說說：

演進過程：從單鏈->全鏈

1.  單鏈：在Ethereum上的資產只能在Ethereum用
    
2.  多鏈與跨鏈橋：為了去Solana或BSC，需要用「跨鏈橋」把資產鎖住，然後在另一遍mint「包裝代幣（Wrapped Token）」（如WBTC），但問題是跨鏈橋風險高，可能被駭客攻擊，而且體驗差，需要不段切換網路，買不同的gas token
    
3.  全鏈（Omnchain），例如ZetaChain：會部署一個「通用合約（Universal Contract），這就是大腦，可以直接指揮Ethereum轉帳ETH，指揮Bitcoin轉帳BTC，不需要真的把BTC般過來，可以「原生的」，而非包裝的，也就是鏈抽象，使用者不需要知道現在後端跑的是啥鏈，Ominichain厲害的就是支援非智能合約鏈，像是Bitcoin，傳統比較困難，但在Ominichain架構可以用evmCall寫邏輯，然後Zetachain會幫忙在Bitcoin上發送原生BTC交易
    

對於開發者來說的好處：只要開發一次（Universal Contract），就可以部署各處。

對於使用者好處：不再強調「把資產跨過去」，而是強調「把訊息傳過去，統一管理」
<!-- DAILY_CHECKIN_2025-11-29_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->











**Universal App 是什么？**  
Universal App 是部署在 ZetaChain 上的智能合约，但它不是局限于某一条链的合约。它能同时接受来自任意连接链 (例如 Ethereum、Bitcoin、Solana……) 的资产、消息或合约调用，也能向任意连接链发送资产/调用。这样，开发者只用写一个合约，就能做到跨所有支持链的 dApp，用户也能用同一个界面 / 钱包操作不同链资产，无需切换网络或用桥 + wrapper。

**Gateway 大概做什么？**

我的理解Gateway类似于网关，但在zeta chain中的应用我还在摸索。
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->












ok，成功调用Qwen的API，我问了一个问题“你会给一个新来的AI开发者什么建议？”

下面是他的回答

![](blob:https://stackedit.cn/ea76bfcc-3965-4f95-8178-c78ea608c198)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/zylg-create/images/2025-11-25-1764078041118-image.png)

cmd调用的，没有自动分行

RPC/API endpoints ：[https://www.zetachain.com/docs/reference/network/api](https://www.zetachain.com/docs/reference/network/api)

Faucet ：[https://www.zetachain.com/docs/reference/faucet](https://www.zetachain.com/docs/reference/faucet)

Explorers ：[https://www.zetachain.com/docs/reference/explorers](https://www.zetachain.com/docs/reference/explorers)

zeta测试币领取成功

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/zylg-create/images/2025-11-25-1764078060168-image.png)
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->













今天一直在捣鼓zetachain的环境部署，视频提到的功能基本在Git Bash上实现了

![](blob:https://stackedit.cn/c90d4cd6-7573-4dd4-8936-ae915d38eaec)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/zylg-create/images/2025-11-24-1763993878555-image.png)

“注册 Qwen 账号并确认可以进入控制台。”没太懂什么意思，进入阿里云的控制台了，想使用Qwen的API，没成功。
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
