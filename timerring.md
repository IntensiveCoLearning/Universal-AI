---
timezone: UTC+8
---

# neo

**GitHub ID:** timerring

**Telegram:** @timerring

## Self-introduction

Dev

## Notes

<!-- Content_START -->
# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->
## ZetaChain Universal App

Universal App 是部署在 ZetaChain Universal EVM 上的智能合约，支持从 Ethereum、BNB、Bitcoin 等多链接收调用、消息和代币，并可发起跨链调用和转账，实现复杂多链交易编排。​

## 核心概念

-   **Universal EVM**：ZetaChain 的 EVM 扩展，内置全链互操作性，现有的 EVM 合约可直接运行，并通过少量修改获得跨链能力。​
    
-   **Universal App**：继承 UniversalContract 的合约，通过 `onCall` 方法处理来自 Gateway 的跨链调用，接收 MessageContext、ZRC-20 代币和消息数据。​
    
-   **ZRC-20**：连接链的原生 gas 和 ERC-20 代币在 ZetaChain 上的表示，可无许可提现回原链。​
    
-   **Omnichain Smart Contract**：指 Universal App，支持 EVM 链、Bitcoin、Solana 等，未来扩展 TON 和 Cosmos。​
    

## 系统结构

ZetaChain 通过每个连接链上的单一 **Gateway 合约** 作为入口，用户从源链（如 Ethereum）调用 Gateway 发送代币和消息，触发 ZetaChain 上 Universal App 的 `onCall`；App 可调用 Gateway 发起出链转账或调用。​

多链用户经 Gateway 入链到 ZetaChain Universal App，App 处理后出链到目标链，支持 gas 抽象（入链仅源链 gas，出链由 App 管理 ZRC-20 gas）。​

## 关键特性

-   **Gas Abstraction**：入链调用仅需源链 gas，ZetaChain 执行免费；出链需 App 预换 ZRC-20 gas 代币覆盖目标链费用。​
    
-   **Bitcoin 支持**：Bitcoin 用户直接发 BTC+数据到 Gateway，无需 ZetaChain 账户或 gas 代币。​
    
-   **前向兼容**：新链接入后，现有 App 自动支持，无需修改合约。​
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->

-   **Universal NFT标准**可以让ERC-721类NFT在任意连接链上铸造和无缝转移，无需“包装”或外部桥接合约，支持多链间NFT真正的互操作。
    
-   **主要特性：**
    
    -   **链间真实所有权**：NFT拥有唯一且持续不变的Token ID，每条链上持有者和元数据保持一致，实现链无关的NFT归属。
        
    -   **标准基于OpenZeppelin**：开发者用熟悉的OpenZeppelin ERC-721库，利用UUPS可升级代理合约模式，方便安全扩展NFT逻辑。
        
    -   **开发方式灵活**：
        
        -   可以通过命令行工具直接创建新的Universal NFT项目。
            
        -   对已有ERC-721项目，只需引入ZetaChain官方合约包，并按模板增加Universal NFT逻辑即可快速升级项目。
            
    -   **链间转移流程**：
        
        -   ZetaChain作为中枢，NFT和其ID、元数据随转移在任意链间流转，整个过程保持链与链之间的信息、身份和归属一致。
            
        -   NFT可以在ZetaChain、Base、Ethereum等连接链间自由转移，所有链使用同一个tokenId和相同元数据。
            
-   **部署及转移核心步骤：**
    
    -   在各链上分别部署Universal NFT相关合约，并通过合约函数（setConnected、setUniversal）组合成信任网，实现链间协作。
        
    -   支持直接在ZetaChain上铸造，再任意转移到其它连接链，也能反向转移。
        
    -   转移和铸造都能用标准工具链自动完成，并且有详细命令行示例和相关合约源码可供参考。
        
-   **适用场景：**
    
    -   多链NFT游戏、市场、身份认证或需要NFT能链间自由切换的平台。
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->


**Universal Apps** 是指在 ZetaChain 区块链上运行的智能合约，它们能够原生连接多个区块链（如 Ethereum, Bitcoin, BNB, Solana 等），实现真正的多链交互和资产转移。

**核心特点**

-   能直接接收来自各连接链的合约调用、消息和 Token（如 ERC-20、BTC）转账。
    
-   可以主动向其它连接链触发合约调用与 Token 转账，支持复杂的、跨链多步操作。
    
-   对接多种链（EVM 类链、Bitcoin、Solana、TON、Sui，未来将支持 Cosmos 等）。
    
-   资产和 Gas 统一抽象为 ZRC-20 Token，用户可以无缝跨链（如：BTC 用户可直接向 Ethereum 地址转账 USDC）。
    
-   用户只需与各链上的 Gateway 合约交互，无需在每个链都创建账户或持有 Gas Token，跨链 Gas 自动处理。
    
-   合约可查询并自动处理跨链所需的 Gas，确保操作顺畅。
    
-   合约部署于 ZetaChain 的 Universal EVM，可以轻松把现有 EVM 合约移植，并稍作改动即可获得多链功能。
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
