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
# 2025-12-01
<!-- DAILY_CHECKIN_2025-12-01_START -->
# Day7

## AI链上储钱罐

-   目标用户: 普通加密货币用户，尤其是那些对投资和理财感兴趣但缺乏专业知识的人。
    
-   想解决的问题: 许多用户不知道如何有效地管理和投资他们的加密资产。AI储钱罐通过智能合约和AI技术，帮助用户自动化投资决策，优化资产配置，实现财富增长。 至少支持 用户心理按摩 (搭配DCA使用)
    
-   使用方式: 用户存入稳定币, 通过AI聊天制定自己的储蓄计划, AI根据市场情况和用户风险偏好，自动调整投资组合，进行跨链资产配置和再平衡。 至少支持 BTC DCA策略; 依托ZetaChain能力, 支持更多类型的原生资产(SOL, SUI…)
    
-   特色: 原生资产智能投资, 降低包装币制造的风险; 心理按摩, 帮助用户坚持投资计划
<!-- DAILY_CHECKIN_2025-12-01_END -->

# 2025-11-30
<!-- DAILY_CHECKIN_2025-11-30_START -->

# Day 6

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Hu-Wentao/images/2025-11-30-1764504123576-image.png)

````markdown
## 运行Swap

## ZetaChain 测试网浏览器
https://testnet.zetascan.com

## 部署到测试网

```bash
export PRIVATE_KEY="<私钥>"
UNIVERSAL=$(npx tsx commands deploy --private-key $PRIVATE_KEY | jq -r .contractAddress) && echo $UNIVERSAL

echo $UNIVERSAL
```

部署合约成功: https://testnet.zetascan.com/address/0x5dEF5269e3a5E94b7A2bFE23DE5D704421547d53


## 询问ZetaChain AI文档内容
```bash
zetachain ai
```

## ZetaChain 资产注册表(foreign_coins) API
https://zetachain.blockpi.network/lcd/v1/public/zeta-chain/fungible/foreign_coins


# 你是从哪里发起的调用？

原始链上的Gateway通过 depositAndCall 发起调用，receiver 指向 ZetaChain 上的 Swap 通用合约。

# 最终在 ZetaChain 上发生了什么？
1. ZetaChain Gateway 触发Swap.onCall 
2. Swap 计算并支付目标链 gas
3. 用 Uniswap 把输入 ZRC20 换成 目标资产
4. 通过Gateway withdraw 到 $RECIPIENT 地址

````
<!-- DAILY_CHECKIN_2025-11-30_END -->

# 2025-11-29
<!-- DAILY_CHECKIN_2025-11-29_START -->



## Day5 Work

-   ZRC-20 和普通 ERC-20 的直观区别（从开发者视角）。
    

ERC-20 是自己控制的“本地资产”，ZRC-20 是“外链资产的映射”，开发者无法随意 mint/burn。

| 维度 | ERC-20 | ZRC-20 |
| --- | --- | --- |
| 资产来源 | 代币合约自己发行（原生资产） | 代表外链资产，由 ZetaChain 协议 mint/burn |
| Mint 权限 | 合约开发者可随意 mint（如果实现了） | 只能由 ZetaChain 协议 mint |
| Burn 权限 | 自定义实现 | 只能在 withdraw 时由协议 burn |

* * *

* * *

-   举一个「通用资产」可能的应用场景:
    

跨链储蓄场景: AI储钱罐

# **面向普通用户（To-C / 零门槛存钱体验）**

## **“AI 帮我存 100 美元”(BTC现货DCA)**

用户可自然语言触发：

> “我想每周存 100 USDT的BTC”

AI 会：

-   判断用户在哪条链有 USDT
    
-   找最便宜的 gas 路径
    
-   自动跨链到存钱罐
    
-   打印记录
    

* * *

## **应急基金规划（Emergency Fund）**

AI 分析用户历史消费（链上稳定币流出）：

-   过去三个月平均支出：$550/月
    
-   建议应急资金：$1650（3 个月）
    
-   当前已存：$200
    

自动规划：

> “我建议未来 30 天存满应急金，要我自动执行吗？”

* * *

## **AI 推荐最佳收益链**

AI 自动监控各链稳定币收益池：

-   Polygon AAVE：6%
    
-   BSC Venus：7.2%
    
-   Base 某 DeFi：8.1%
    

AI 说：

> “我帮你把 Polygon 的 USDC 调到 Base，每月多赚 0.5 美元。”

* * *

## **自动跨链调仓（Rebalancing）**

假设用户想保持：

-   70% 稳定币
    
-   30% ETH
    

AI 会自动跨链调整：

> “ETH 比例涨到 40% 了，已帮你卖出 10% 并跨链存入稳定池。”

* * *

## **风险等级匹配**

用户说：

> “我风险偏好很低。”

AI：

-   禁止高波动资产
    
-   不用杠杆
    
-   更频繁 rebalancing
    
-   选稳定币收益池
    
-   只用强安全链（ETH/ZetaChain）
    

* * *

## **目标导向存钱（Goal-based Saving）**

用户设定：

-   买 MacBook：$1500
    
-   结婚基金：$10,000
    
-   去日本旅行：$2000
    

AI 会：

-   分析用户每月自由现金流
    
-   自动规划存钱节奏
    
-   跨链收集资产
    

* * *

## **通知 + 解释（AI Financial Coach）**

AI 通过 ZetaChain 获取数据：

-   钱包本周支出
    
-   收益池变化
    
-   储蓄进度
    

用自然语言解释：

> “你今天领取的空投价值 27 美金，我已自动存入。不错的一天！”
<!-- DAILY_CHECKIN_2025-11-29_END -->

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
