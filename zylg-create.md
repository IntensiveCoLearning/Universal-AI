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
