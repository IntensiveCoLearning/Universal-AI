---
timezone: UTC+8
---

# reece

**GitHub ID:** RSXLX

**Telegram:** @SXLX__

## Self-introduction

WEB3 ROOKIE

## Notes

<!-- Content_START -->
# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->
Universal App 是一种**跨链应用**形式：  
应用逻辑只写在 ZetaChain 上，但能让不同链的用户（ETH / BNB / Polygon / …）都像是在**同一条链上直接交互**。  

###   
ZetaChain 作为“连接层”，负责：

-   接收来源链发来的数据 / 资产
    
-   在 ZetaChain 上执行合约逻辑
    
-   把结果分发到目标链
    
-   所有这套跨链流程对用户透明
    

Universal App规划：ZetaChain 把这些参数带进来，

我在合约记录：

\- 谁调用了

\- 从哪条链来

\- 输入内容是什么

maybe当成一个print  

#   
  
明天计划：

1.  用 CLI 创建一个 universal app skeleton
    
2.  用 Hardhat 成功编译官方示例合约
    
3.  在测试网上部署 “跨链打印机”
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->

## Universal App = 一个合约控制所有链的资产

**Universal App是部署在 ZetaChain 的 Universal EVM 上 的智能合约，它可以 直接操作多条外部区块链上的资产与数据，而不需要部署多链版本的合约（很蛋疼的就是之前做一个项目做完evm就要继续肝solana，其实差不多但是工作量会翻倍，还有联调测试的一些环节）**

## Gateway 是 ZetaChain 的跨链消息入口 / 出口。

Gateway = 外部链 ↔ ZetaChain 的官方跨链通信层

Universal App 负责逻辑，Gateway 负责和每条链沟通、执行真实链上动作  
  
  

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/RSXLX/images/2025-11-26-1764166895768-image.png)
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->


未来两周希望可以理解到zetachain的特性，链与链交互原理之类的：  
① 每条外链上有 “ZetaChain 控制的链上账户”

例如：

-   BTC 主网：一个由 ZetaChain 验证者共同控制的 BTC 地址
    
-   ETH 主网：一个多签钱包，由验证者控制
    
-   BNB Chain：一个多签地址
    
-   Polygon：一个多签地址
    

这些统称为：  
👉 **ZetaChain external TSS accounts（外链门控账户）**

TSS = Threshold Signature Scheme 阈值签名

-   不是任何单个验证者能控制
    
-   达到阈值的大多数验证者才能签名
    
-   更像“去中心化多签”
    

* * *

# 🧩 ② 外链事件（交易）会被拉取到 ZetaChain

每个 ZetaChain 的验证者节点会同步：

-   BTC 链上来自用户的充值
    
-   ETH 链上的发送到 TSS 地址的交易
    
-   BNB Chain 的合约调用
    

当验证者检测到一个外链事件，会做：

🔹 提交外链证明  
🔹 ZetaChain 链内达成共识  
🔹 最终确认事件

这意味着：

> **ZetaChain 是一个多链事件的统一共识层**

* * *

# 🧩 ③ ZetaChain 合约执行后，可反向操作外链（跨链写入）

当 ZetaChain 链上的智能合约执行完成后，它可以触发：

-   外链资产转账
    
-   外链合约调用（例如 Uniswap swap）
    
-   外链 mint NFT
    
-   外链 burn 代币
    

这时 ZetaChain 会生成一个跨链消息（CCM）：  
👉 验证者 TSS 多签 → 在目标链执行交易

这就是“跨链执行”的本质。

* * *

# 📚 用一句话概括其跨链原理

> **ZetaChain 在外链上维持了一组去中心化的 TSS 多签账户；验证者监听外链事件，达成共识后触发链内合约；链内合约可以反向控制 TSS 账户在外链执行逻辑，从而实现安全的跨链读写。**

# AI可以直接部署

安装了ZetaChain CLI

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/RSXLX/images/2025-11-25-1764077203711-image.png)

**ZetaChain 测试网资源整理：**

## 1) RPC 入口（功能 & 地址）

脚本、DApp、钱包、CLI 都是通过 RPC 与区块链网络通信的。  
比如发送交易、查询余额、部署合约。

地址copy了doc里面的一个：[https://zetachain-athens.g.allthatnode.com/archive/evm](https://zetachain-athens.g.allthatnode.com/archive/evm)

## 2) Faucet 入口（功能 & 地址）

领取faucet用于消费，支付测试链的gas

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/RSXLX/images/2025-11-25-1764076580551-image.png)

地址：[https://zetachain.faucetme.pro/](https://zetachain.faucetme.pro/)

## 3) Explorer 入口（功能 & 地址）

查看交易、区块、地址余额、合约交互等。相当于一个庞大的数据库，记录链上发生的所有事情  
地址：[https://zetascan.com/](https://zetascan.com/)  
测试链一般是前面加testnet

API的调用

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/RSXLX/images/2025-11-25-1764077327184-image.png)

之前跑过其他的玩意，给透支了哈哈哈，官方能不能送点
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->




1 ZetaChain 的核心是 跨链智能合约（Omnichain Smart Contract）类似中转

2 看了浏览器

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/RSXLX/images/2025-11-24-1763986817074-image.png)

3 注册QWEN API

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/RSXLX/images/2025-11-24-1763986833813-image.png)
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
