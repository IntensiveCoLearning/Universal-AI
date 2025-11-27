---
timezone: UTC+8
---

# chutianying

**GitHub ID:** slucifersz

**Telegram:** @Chutianying

## Self-introduction

hello

## Notes

<!-- Content_START -->
# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->
-   解本次共学的整体方向与黑客松赛道（通用 DeFi / 通用 AI 应用）。
    
-   完成 ZetaChain / Qwen 账号与基础环境准备。
    

**学习资料**

-   ZetaChain 总文档
    
    [https://www.zetachain.com/docs/](https://www.zetachain.com/docs/)
    
-   ZetaChain Developers（入口总览）
    
    [https://www.zetachain.com/docs/developers](https://www.zetachain.com/docs/developers)
    
-   Qwen 总文档（Quickstart / Key Concepts）
    
    [https://qwen.readthedocs.io/zh-cn/latest/](https://qwen.readthedocs.io/zh-cn/latest/)
    

**扩展资料（可选）**

-   Qwen API 平台
    
    [https://qwen.ai/](https://qwen.ai/)
    

**实践 / 作业**

-   注册 / 配置好 ZetaChain 开发所需环境（浏览 Docs，确认能访问 Developers 页面）。
    
-   注册 Qwen 账号并确认可以进入控制
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->

好，学到了很多
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->


# **ZetaChain 学习报告**

## **一、前言：为什么要学习 ZetaChain？**

在当前 Web3 生态中，“跨链互操作性”始终是一个关键难题——链与链之间像一座座孤岛，资产、合约、数据难以自由流动。Rollup 激增、链数量不断增加，更让跨链需求从“可有可无”变成“刚需”。

ZetaChain 的出现，为这个老大难问题提供了一套独特、具工程可行性的解决路径。它不仅号称“首个通用跨链智能合约平台”，更重要的是，它试图把跨链消息传递、跨链资产管理、跨链 DApp 构建，都统一到一个链上完成。

因此，系统性学习 ZetaChain，不仅是了解一个新公链，更是理解 Web3 未来架构可能的样子。

* * *

## **二、ZetaChain 的核心理念与框架**

### **1\. 全链互操作（Omnichain）**

ZetaChain 的目标是构建一个不依赖中介桥的跨链基础设施，使开发者能够以“一条链的体验”构建跨链应用（Omnichain DApps）。

与传统跨链桥不同，ZetaChain 强调：

-   **链之间无需直接互信**
    
-   **跨链逻辑可直接写在智能合约里**
    
-   **ZetaChain 自己承担验证、签名与价值管理**
    

这让开发者可以像写普通智能合约一样，调用外链的状态或资产。

* * *

### **2\. 去中心化观察者（Decentralized Observers）**

ZetaChain 网络中的节点担任“观察者”，负责监听：

-   比特币链
    
-   EVM 链
    
-   Cosmos 链
    
-   其他集成链
    

观察者们共同验证外链事件，并在 ZetaChain 上生成跨链消息，使其作为一种 **跨链状态层** 存在。

这比传统桥的“几个人签名”方式更加安全。

* * *

### **3\. ZetaChain 智能合约：跨链逻辑内嵌**

ZetaChain 支持 Solidity，让开发者可以直接编写：

```
function sendToOtherChain(address target, bytes memory data) public {
    // 跨链调用逻辑
}
```

一个合约即可控制多链资产和执行逻辑，而无需在每条链部署版本。

* * *

### **4\. 原生管理比特币等非智能合约链**

ZetaChain 最大的亮点是：

➡ **能够原生管理 Bitcoin、Litecoin 等无智能合约能力的链的资产**  
这意味着：

-   你可以在 ZetaChain 上写一个 DApp
    
-   让用户用比特币参与
    
-   无需桥、无需包裹版 BTC
    

这在跨链领域极具创新性。

* * *

## **三、ZetaChain 的主要功能模块**

### **1\. 通用消息传递（Cross-Chain Messaging）**

类似 LayerZero，但更偏向链级实现。  
用途包括：

-   跨链调用合约
    
-   跨链同步状态
    
-   多链游戏逻辑
    
-   多链身份管理
    

* * *

### **2\. 通用跨链资产处理（Omnichain Asset Layer）**

ZetaChain 能够托管和控制多条链上的资产，并在应用层实现：

-   跨链交换
    
-   跨链 DeFi 流程（借贷、质押）
    
-   多链流动性池
    

* * *

### **3\. ZETA 代币作为 Gas & 跨链安全核心**

ZETA 代币有三个关键用途：

-   **支付跨链交易的 gas**
    
-   **惩罚与激励共识节点**
    
-   **确保跨链资产处理的安全性**
    

其代币经济模型与跨链安全机制深度绑定。

* * *

## **四、与其它跨链方案的对比**

| 项目 | 核心方式 | 是否支持 BTC 类链 | 安全模型 |
| --- | --- | --- | --- |
| LayerZero | 轻客户端 + Oracle | 否 | 依赖外部 |
| Axelar | 网状路由链 | 否（间接） | Cosmos 网络 |
| Cosmos IBC | 客户端验证 | 只限 Cosmos 内部 | 强安全 |
| Bridges（各种桥） | 多签/轻客户端 | 偶尔 | 风险最高 |
| ZetaChain | 全链状态层 + 去中心化观察者 | 支持，不需包装 | 链级安全 |

ZetaChain 的定位就是补齐：

-   支持非智能链
    
-   合约层原生跨链
    
-   开发者体验统一
    

这是其它方案难以做到的。

* * *

## **五、典型应用场景**

### **1\. 跨链 Swap**

例如：  
用户在 BTC 链支付 0.01 BTC，  
合约在 BNB Chain 上帮他换出 USDT。

无需桥，也无需 wrapped 资产。

* * *

### **2\. 多链 DeFi**

如：

-   用 BTC 抵押在 ZetaChain 上借 ETH
    
-   在不同链执行套利逻辑
    
-   多链收益整合
    

* * *

### **3\. 跨链 NFT / 游戏**

例如：  
一个 NFT 能在 ETH 链铸造，在 Polygon 链展示，在 BNB 链参与游戏。

* * *

### **4\. 多链钱包与身份（Omnichain DID）**

用户只需一个身份，就能跨链管理资产与权限。

* * *

## **六、学习 ZetaChain 的心得体会**

### **1\. ZetaChain 本质上是“跨链 Layer 1”**

它不是桥，不是消息协议，而是一个新的链级范式：  
**让跨链逻辑成为智能合约可以直接编写的原生能力。**

这在开发视角上极大降低了跨链应用门槛。

* * *

### **2\. 工程实现难度高，但方向正确**

直接管理 BTC、跨链监听、全链状态机……  
这些都是工程难题，  
但 ZetaChain 在设计上非常务实，不追求“看起来最完美”的模型，而是：

-   去中心化验证
    
-   合理安全边界
    
-   开发友好
    

在多链时代，这种路线有现实意义。

* * *

### **3\. 跨链应用会是未来的 Web3 主流方向**

随着链数量越来越多，单链 DApp 会变得像单体应用一样局限。  
跨链应用可以实现：

-   多链价值的自由流动
    
-   用户无感切换链
    
-   生态间真正的互联
    

ZetaChain 可能成为这个时代的重要基础设施。

* * *

## **七、总结**

ZetaChain 的价值在于，它不是修补跨链桥的方案，而是**重新定义跨链应用开发的底层结构**。  
通过全链状态层、去中心化观察者、跨链智能合约等机制，它为开发者提供了构建多链应用的完整工具链。

对于想从事 Web3、跨链技术或 Omnichain 方向的学习者，它是一个非常值得深入研究的项目。
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
