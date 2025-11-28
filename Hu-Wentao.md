---
timezone: UTC+8
---

# 胡文涛

**GitHub ID:** Hu-Wentao

**Telegram:** @wyatt_hu

## Self-introduction

全栈开发者, Flutter, Python

## Notes

<!-- Content_START -->
# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->
## **📌 Part 1：Universal App 的第一个功能（打印 / 记录 / 简单逻辑）**

我的 Universal App（AI 全链存钱罐 OmniPiggy）的第一个要实现的最小功能是：

### **👉 在任意链向 ZetaChain 发出“存钱”操作时，合约能够：**

### **「收到跨链消息 → 记录存款金额 → 打印（emit event）存款日志」**

具体来说：

1.  **用户从任意链（ETH/BSC/Polygon/Bitcoin L2 等）向存钱罐发送一笔资金或消息**
    
2.  **ZetaChain 的 Universal App 合约通过 CCM（Cross-Chain Message）接收消息**
    
3.  合约做两件事情：
    
    -   **记录（store）用户地址与存款金额**到 mapping
        
    -   **打印（emit）一条 event 日志**
        
        ```
        event DepositReceived(address user, uint amount, string sourceChain);
        ```
        

这是 Universal App 最小可用逻辑：  
**跨链 → 记录 → 打印成果**  
为后续的 AI 分析与跨链调仓打好基础。

未来会扩展成：

-   AI 现金流分析
    
-   跨链调仓
    
-   自动分配储蓄策略
    
-   收益展示
    

但第一步是确保 **跨链消息能正确到达 + 合约能记录 + 能打印日志**。

* * *

# **📌 Part 2：Hello World Demo 的开发工作流选择**

为了快速完成 Hello World，我决定采用以下开发方式：

* * *

## **🔧 开发工具：Hardhat + ZetaChain CLI（官方推荐）**

原因：

-   Hardhat 对 EVM 开发者最友好
    
-   ZetaChain 官方示例大都使用 Hardhat
    
-   编写、部署、验证都比较简单
    
-   插件生态成熟（ethers、dotenv、gas-reporter 等）
    

也可以考虑 Foundry，但 Hardhat 文档更适配 Universal Apps 初学者。

* * *

## **🌐 开发环境：使用 ZetaChain 测试网（Theta Testnet）**

选择理由：

-   本地链无法模拟 ZetaChain 的跨链消息流程（观察者 → 签名者 → CCM）
    
-   测试实时跨链消息必须用测试网
    
-   Alpha/Theta Testnet 的 faucet 免费发 ZETA
    
-   测试日志能在 ZetaScan 上直接看到
    

测试目标：

-   在 ETH → ZetaChain 发一条消息
    
-   在 ZetaChain 上打印 event：`DepositReceived(...)`
    

测试链用例如：

-   **ZetaChain Testnet**（核心部署位置）
    
-   **Goerli / Sepolia（或 BSC testnet） → ZetaChain**
    

便于完整展示 Universal App 的“全链互操作”能力。
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->

## Universal App 是什么？

UniversalApp是部署在ZetaChain上的智能合约, 兼容EVM以及BTC, Solana, Ton, Sui等链, 实现复杂的跨链操作.

ZetaChain与普通跨链桥的不同之处在于它可以触发方法调用, 甚至支持BTC这类不可编程的链.

## Gateway 大概做什么？

Gateway就是一个内置复杂跨链功能的跨链桥入口, 通过监听不同链的事件, 触发函数后发送到其他链上, 实现跨链操作.

## 画一张简单的架构图：

ZetaChain 中心 + Bitcoin / Ethereum / Solana 等外围链 + Gateway。
<img width="628" height="501" alt="image" src="https://github.com/user-attachments/assets/84d5402a-65a5-4509-b6f5-7d60c6a5136a" />
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->


![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Hu-Wentao/images/2025-11-25-1764083711830-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Hu-Wentao/images/2025-11-25-1764083769243-image.png)

```ini
## 测试币领取

Faucet 	Chain / Asset 
https://cloud.google.com/application/web3/faucet/zetachain/testnet 	ZetaChain ZETA
https://zetachain.faucetme.pro/ 	ZetaChain ZETA
https://console.optimism.io/faucet 	Ethereum, Base  以太坊，基础
https://faucets.chain.link/ 	Ethereum, Base, Avalanche, Arbitrum, Polygon
以太坊、基础、雪崩、任意、多边形
https://faucet.circle.com/ 	USDC
https://faucet.solana.com/ 	Solana  索拉纳
https://faucet.polygon.technology/ 	Polygon  多边形
https://testnet.binance.org/faucet-smart 	BSC
https://mempool.space/testnet4/faucet 	Bitcoin Testnet 4  比特币测试网 4
https://faucet.testnet4.dev/ 	Bitcoin Testnet 4  比特币测试网 4
https://faucet.triangleplatform.com/bitcoin/testnet 	Bitcoin Testnet 4  比特币测试网 4
https://signetfaucet.com/ 	Bitcoin Signet  比特币签名
```

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Hu-Wentao/images/2025-11-25-1764084513671-image.png)
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->



![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Hu-Wentao/images/2025-11-24-1763967158684-image.png)![Screenshot 2025-11-24 at 14.53.06.jpeg](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Hu-Wentao/images/2025-11-24-1763967208922-Screenshot_2025-11-24_at_14.53.06.jpeg)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Hu-Wentao/images/2025-11-24-1763967232290-image.png)
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
