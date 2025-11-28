---
timezone: UTC+8
---

# Lumi

**GitHub ID:** Lumi-Zheng

**Telegram:** @Lumi_Zheng

## Self-introduction

哈喽，大家好。

## Notes

<!-- Content_START -->
# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->
打卡
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->

改天补笔记
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->


## 一、zetachain核心概念与愿景

-   **1.1 ZetaChain 是什么？**
    
    -   **核心定义：** ZetaChain 是一个基于 **Cosmos SDK** 构建的去中心化 **权益证明（PoS）** L1 区块链，支持全链智能合约（Omnichain Smart Contracts）。
        
    -   **解决问题：** 解决了当前多链生态中资产和信息无法直接流通的问题（多链生态中的资产和数据隔离问题（跨链障碍））。
        
    -   **核心愿景：** 统一的互操作性层，使开发者能够构建真正的全链去中心化应用（Omnichain dApps）。成为 **Web3 的中立公共基础设施**，允许开发者在 ZetaChain 的**一个**智能合约中，管理和操作所有连接区块链上的**原生**资产。
        
-   **1.2 全链互操作性 (Omnichain Interoperability)**
    
    -   **定义：** 在单个 ZetaChain 智能合约中协调和管理所有连接链上的原生资产和数据。允许在 ZetaChain 上部署的 **Solidity 智能合约** 拥有与任何外部链（如比特币、以太坊）交互的能力。
        
    -   **核心优势：** 无需封装资产（Wrapped Assets）、无需桥接（Bridging）。
        
        -   **原生资产支持：** 可以操作 **原生比特币 (BTC)** 或 **以太坊 (ETH)**，无需通过桥接或封装成 WETH、WBTC 等衍生资产。
            
        -   **统一逻辑：** 开发者无需为不同的链编写和部署多个合约。
            
-   **1.3 ZETA 代币 (The ZETA Token)**
    

Gas 费（用于跨链交易）、安全抵押（Validator Staking）、跨链价值转移的结算层。

-   **Gas 费：** 用于支付 ZetaChain 上的交易费用，包括跨链消息的传递费用。
    
-   **PoS 抵押：** 验证者必须抵押 ZETA 才能参与网络安全维护。
    
-   **跨链结算：** ZETA 充当“通用货币”，用于在不同区块链之间转移价值时的原子交换和结算。
    

## 二、zetachain技术架构与组件 (Architecture)

-   **2.1 ZetaChain 核心网络**
    
    -   **共识机制：** 基于 Cosmos SDK 构建的 PoS (Proof-of-Stake) 机制，确保高速和安全性。
        
    -   **核心功能：** 协调和验证跨链交易，区块链本身处理内部交易和跨链逻辑验证。
        
-   **2.2 关键角色与组件**
    
    -   **ZetaCore:** ZetaChain 的核心引擎，负责维护状态和交易处理。
        
    -   **验证者 (Validators):** 通过抵押 ZETA 来获得奖励和惩罚，验证并打包区块，维护网络共识。
        
    -   **观察者 (Observers):**
        
        -   **观察：** 监听外部链（如比特币、以太坊）上的特定交易和事件。
            
        -   **报告：** 报告这些事件给 ZetaCore。
            
        -   **验证：** 通过多方签名（Multy-Party Computation, MPC）技术，**集体验证**外部交易的真实性。
            
    -   **签名者 (Signers):** 负责使用 ZetaChain 的私钥对外部链（如 BTC、ETH）上的转账进行签名，以完成资产的转移或操作（同样是观察者的一部分）。
        
-   **2.3 资产管理机制**
    
    -   **原生资产互操作性：** 外部链的资产（如原生 BTC）由 ZetaChain 的验证者网络通过 MPC 签名技术**共同保管**，而不是由单个实体控制。
        

## 三、开发者工具与合约 (Developers)

-   **3.1 ZRC-20 标准 (核心)**
    
    -   **定义：** ZetaChain 上的特殊合约标准，它代表了外部链上的原生资产。它允许 ZetaChain 上的合约**像操作 ERC-20 代币一样操作外部链上的原生资产**。
        
    -   **用途：** 允许开发者在 ZetaChain 上使用 Solidity 编写合约，并直接控制外部链的资产（如在 ZetaChain 上部署一个 DEX，直接交易原生 BTC）。
        
    -   **工作原理：** ZRC-20 充当一个“账本”，记录外部链资产在 ZetaChain 上的流动。
        
-   **3.2 ZRC-20 交互模型**
    
    -   **存款 (Deposit):** 当用户将原生资产（如 BTC）发送到 ZetaChain 的保管地址时，ZetaCore 会在 ZetaChain 上铸造相应的 ZRC-20 余额。
        
    -   **提款 (Withdraw):** 当用户在 ZetaChain 上销毁 ZRC-20 余额时，观察者网络会协作在外部链上释放相应的原生资产。
        
-   **3.3 编程环境**
    
    -   **支持语言：** Solidity（基于 EVM 兼容）。
        
    -   **开发工具：** Hardhat, Truffle 等 EVM 开发工具。
        

## 四、实践指南(Guides & SDK)

-   **4.1 环境设置 (Setup)**
    
    -   安装 Node.js、Yarn/NPM、MetaMask 钱包等。
        
    -   获取 **测试网 ZETA** 代币（Faucet）。
        
    -   **SDK：** 使用 **ZetaChain SDK** 来方便地构建跨链交易逻辑。
        
-   **4.2 创建并部署合约**
    
    -   通常使用 **Hardhat** 或 **Truffle** 等标准 EVM 工具进行合约编写、测试和部署。
        
-   **4.3 跨链交易测试**
    
    -   使用 `zrc20.deposit()` 和 `zrc20.withdraw()` 函数来模拟资产在不同链之间的进出。
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
