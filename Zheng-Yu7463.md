---
timezone: UTC+8
---

# Cyoid

**GitHub ID:** Zheng-Yu7463

**Telegram:** @Cyoid

## Self-introduction

人工智能在读本科

## Notes

<!-- Content_START -->
# 2025-12-01
<!-- DAILY_CHECKIN_2025-12-01_START -->
![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Zheng-Yu7463/images/2025-12-01-1764598124673-image.png)

langchain v1 create\_agent调用

指定base\_url model key

获取流式输出 model.stream()

模型：Qwen3 235B
<!-- DAILY_CHECKIN_2025-12-01_END -->

# 2025-11-30
<!-- DAILY_CHECKIN_2025-11-30_START -->

-   **项目名称（暂定）：** UniYield (Universal Yield)
    
-   **目标用户：**
    
    -   手里有闲置资金（如 USDC 或 ETH），想寻找全网（不仅仅是单一链）最高收益的普通投资者。
        
    -   厌倦了在该链和那个链之间切来切去、管理 Gas 费的 DeFi 农民。
        
-   **想解决的问题：**
    
    -   **收益不仅限于一条链：** 也许最高的稳定币收益在 Polygon 上，而用户的钱在 ETH 主网上。传统路径需要：跨链桥 -> 换 Gas -> 存入，损耗极高且麻烦。
        
    -   **Gas 焦虑：** 用户不想为了去 Polygon 挖矿而专门去买 MATIC。
        
-   **跨链 / 通用资产使用方式（技术实现）：**
    
    1.  **通用入口：** 用户可以在任意支持的链（如 Ethereum, BSC, Bitcoin）存入资产（例如存入 Native USDC）。
        
    2.  **全链路由（Omnichain Strategy）：** ZetaChain 上的智能合约充当“大脑”。它持有策略逻辑，比如监测到目前 GMX (Arbitrum) 的收益率最高。
        
    3.  **自动兑换与执行：**
        
        -   合约接收用户的 USDC (ZRC-20)。
            
        -   通过 ZetaChain 内部的 Uniswap v2/v3 池子（ZRC-20 pairs）自动将资产通过跨链消息传递（Messaging）调度到目标链。
            
        -   如果目标是 ETH 链上的收益协议，合约会自动处理资产的出站调用。
            
    4.  **收益复投：** 获得的收益可以跨链汇集回 ZetaChain，再次分配到收益最高的链，或者直接以用户指定的代币（如 BTC）结算给用户。
<!-- DAILY_CHECKIN_2025-11-30_END -->

# 2025-11-29
<!-- DAILY_CHECKIN_2025-11-29_START -->


### 核心命令与配置记录

```
npm config set registry https://registry.npmmirror.com
sudo npm install -g zetachain --legacy-peer-deps
sudo npm install -g yarn
yarn install
```

```
zetachain new --project swap
forge build
```

```
# 获取部署后的合约地址
UNIVERSAL=$(npx tsx commands/deploy.ts --private-key $PRIVATE_KEY | jq -r .contractAddress)
echo "My Swap Contract Address: $UNIVERSAL"
```

**Q: 你是从哪里发起的调用？**

> 我是从 **本地终端 (CLI)** 发起的调用，通过 RPC 接口与 ZetaChain Athens 测试网进行交互。在实际的跨链场景中，这个调用通常始于 **外部链（如 Ethereum Sepolia）** 的用户钱包，用户将资产（如 ETH）发送到 ZetaChain 的 TSS（阈值签名）地址。

**Q: 最终在 ZetaChain 上发生了什么？**

> 1.  **资产映射：** 外部链存入的资产被 ZetaChain 捕捉，并以 **ZRC-20 代币** 的形式铸造在 ZetaChain 上。
>     
> 2.  **通用合约触发：** 我部署的 `Universal.sol` (Swap) 合约被自动调用，触发了 `onCrossChainCall` 函数。
>     
> 3.  **链上逻辑执行：** 在 ZetaChain 的 EVM 兼容层中，合约逻辑被执行（例如：将 ZRC-20 ETH 兑换为 ZRC-20 BTC 或其他代币）。
>     
> 4.  **状态变更/提现：** 兑换后的资产直接存储在我的接收地址中，或者如果是跨链提现，ZetaChain 会销毁 ZRC-20 代币，并在目标链上释放原生资产给接收者。
>
<!-- DAILY_CHECKIN_2025-11-29_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->



**1\. ZRC-20 和普通 ERC-20 的直观区别（开发者视角）**

虽然在写代码时，ZRC-20 也可以用 `transfer`、`approve` 这些熟悉的接口，但我觉得两者在**底层逻辑**上有两个最大的不同：

-   **“记账” vs “遥控”**： 普通 ERC-20 只是合约内部的一个记账数字，转来转去资产都在这条链上。而 ZRC-20 更像是外部原生资产（比如 BTC 或 ETH）的\*\*“远程遥控器”\*\*。你在 ZetaChain 上操作 ZRC-20，实际上是在映射外部链上的资产状态。
    
-   **提现逻辑（Withdraw）**： 这是 ZRC-20 最特殊的地方。普通 ERC-20 的燃烧（Burn）就是把币毁了；但 ZRC-20 如果调用 `withdraw`，不仅是销毁 ZetaChain 上的代币，还会**直接触发** ZetaChain 协议层在**比特币主网或以太坊主网**上发起一笔真实的转账交易给目标地址。这一点是普通 ERC-20 做不到的。
    

**2\. 「通用资产」应用场景脑洞**

**场景：全链通用的“原生资产”理财池**

-   **痛点**：现在的 DeFi，我想存 BTC 赚利息，通常得先把 BTC 跨链包装成 WBTC，这中间有跨链桥的风险。
    
-   **通用资产玩法**： 用户可以直接从 **Bitcoin 钱包**把 BTC 转入 ZetaChain 的协议地址。 在 ZetaChain 上，这些 BTC 自动变成 ZRC-20 BTC 并存入一个流动性池子。 其他的用户（比如来自以太坊的用户）可以直接借走这些 BTC（在以太坊端收到真实的 BTC），或者用 ETH 来做抵押。 **核心优势**：用户全程持有的都是原生资产（Native Asset），不需要持有包装资产（Wrapped Asset），也不需要手动去操作复杂的跨链桥，体验就像在同一个 App 里操作一样。
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->




**“全链涂鸦墙”**

> 这是一块立在 ZetaChain 上的**公共黑板**。
> 
> 无论你是住在“比特币村”还是“以太坊城”，你都不用出门（不用跨链桥），直接在家里发一条短信（发交易），这块黑板上就会自动刻上：
> 
> **“来自 \[比特币/以太坊\] 的 \[你的地址\]，在 \[时间\] 到此一游。”**
> 
> **同时加入类似于微信摇一摇和QQ空间漂流瓶的功能，打造一个链上的随机漂流平台**

采用CLI + Hardhat + 测试网
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->





**Q1: Universal App (通用应用) 是什么？**

> “一次部署，全链连接。”

Universal App 是**部署在 ZetaChain 上**的一种特殊智能合约。

-   **中心化逻辑：** 它的代码只存在于 ZetaChain 这一条链上，不需要在以太坊、BNB 或 Polygon 上重复部署。
    
-   **全链连接：** 虽然它只在 ZetaChain 上，但它可以通过 ZetaChain 的连接器，直接管理和操作与之相连的所有外部链（如 Bitcoin、Ethereum）上的资产和数据。
    
-   **用户体验：** 对于用户来说，就像在使用一个“万能遥控器”，只需按下一个按钮（在任意链交互），就可以控制所有连接设备（其他链的资产）。
    

**Q2: Gateway (网关) 大概做什么？**

> “进出的海关与传送门。”

Gateway 是 ZetaChain 与外部区块链（如 Ethereum, Bitcoin）进行沟通的接口。

-   **入口（Inbound）：** 当用户在外部链（比如以太坊）上进行操作（如存款）时，Gateway 负责接收这些资产或信息，并告诉 ZetaChain：“嘿，有一笔钱进来了。”
    
-   **出口（Outbound）：** 当 ZetaChain 上的 Universal App 处理完逻辑，需要把资产发回外部链时，Gateway 负责执行这笔转账或调用。
    
-   **关键点：** 它让 ZetaChain 能够安全地“持有”和“释放”外部链上的原生资产。
    

**尝试绘制的架构图：**

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Zheng-Yu7463/images/2025-11-26-1764144906221-image.png)
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->






-   安装尝试Zeta cli ✅
    
-   ZetaChain Node / RPC / Faucet / Explorer / 测试币获取 ✅
    
-   Qwen api调通 ✅
    

zeta安装

[水龙头：https://www.zetachain.com/docs/reference/faucet](https://www.zetachain.com/docs/reference/faucet)  
CLI：[https://www.zetachain.com/docs/reference/cli](https://www.zetachain.com/docs/reference/cli)

![5841764075926_.pic.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Zheng-Yu7463/images/2025-11-25-1764076057014-5841764075926_.pic.jpg)

Qwen api调试

![5831764075765_.pic.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Zheng-Yu7463/images/2025-11-25-1764076081778-5831764075765_.pic.jpg)

领水：

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Zheng-Yu7463/images/2025-11-25-1764076885653-image.png)
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->







-   添加共学微信群 ✅
    
-   注册阿里云百炼进入后台 ✅
    
-   浏览zetachain的docs ✅
    

![5741763982727_.pic.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Zheng-Yu7463/images/2025-11-24-1763983327443-5741763982727_.pic.jpg)![5771763983057_.pic.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Zheng-Yu7463/images/2025-11-24-1763983371064-5771763983057_.pic.jpg)![5761763982904_.pic.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Zheng-Yu7463/images/2025-11-24-1763985277909-5761763982904_.pic.jpg)
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
