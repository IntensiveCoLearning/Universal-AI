---
timezone: UTC+8
---

# Steph

**GitHub ID:** kuove

**Telegram:** @stephkuove

## Self-introduction

对web3感兴趣的开发者

## Notes

<!-- Content_START -->
# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->
![wechat_2025-11-27_220450_264.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/kuove/images/2025-11-27-1764252340702-wechat_2025-11-27_220450_264.png)

使用测试链实现hello信息传递
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->

Universal apps在24号的笔记中总结过，

-   可以接收已连接的链上的合约调用
    
-   可以触发合约调用，进行代币在不同链上的转换
    
-   完全兼容 EVM 链（如以太坊、BNB 和 Polygon）、比特币、Solana、TON 和 Sui。
    
-   原生 Gas 和支持的 ERC-20 代币以 ZRC-20 代币的形式呈现。ZRC-20 代币无需许可即可提取回其原始链。
    

[**Gateway**](https://www.zetachain.com/docs/developers/evm/gateway)

适配不同的链的网关，与Universal apps进行连接交互

The implementation of the gateway depends on the connected chain:

-   EVM chains: a gateway smart contract
    
-   Solana: a gateway program
    
-   Bitcoin: a TSS MPC gateway address managed by a network of observer-signer validators
    

[https://embed.figma.com/design/mYXNTORUuvGVaQ01SF7h9Y/Gateway-for-Universal-Apps?node-id=0-1&p=f&embed-host=notion&footer=false&theme=system](https://embed.figma.com/design/mYXNTORUuvGVaQ01SF7h9Y/Gateway-for-Universal-Apps?node-id=0-1&p=f&embed-host=notion&footer=false&theme=system)

支持以下特性（在不同的链上）

-   将原生 Gas 代币存入 ZetaChain 上的Universal apps或账户
    
-   将支持的 ERC-20 代币（包括 ZETA 代币）存入 ZetaChain 上的Universal apps或账户
    
-   存入原生 gas 代币并向通用Universal apps发出合约调用（可传递任意数据）
    
-   存入受支持的 ERC-20 代币，并向Universal apps发出合约调用（可传递任意数据）。
    
-   向Universal apps发出合约调用（传递任意数据）
    

在ZetaChain上的特性（从Universal apps向其他链上调用合约或者提取代币）

-   将 ZRC-20 代币作为原生 gas 或 ERC-20 代币提取到已连接的链上
    
-   将 ZETA 代币提取到连接的链
    
-   将代币提取到已连接的链上并进行合约调用
    
-   在连接链上进行合约调用
    

目前只支持单链调用，同时支持回滚，调用失败时可以取消当前的操作
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->


1.  注册qwen，获取api，能在本地通过node调用
    
2.  阅读[**Getting Started**](https://www.zetachain.com/docs/developers/tutorials/intro)
    
3.  [Faucet](https://www.zetachain.com/docs/reference/faucet)： [这个用过](https://zetachain.faucetme.pro/)
    
4.  [RPC](https://www.zetachain.com/docs/reference/network/api)
    
5.  [Explorer](https://www.zetachain.com/docs/reference/explorers)
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->



-   Universal apps:
    
    -   can receive contract calls and tokens from users and contracts on connected chains
        
    -   can trigger contract calls and token transfers to connected chains
        
    -   can automatically handle gas for cross-chain transactions
        
    -   are fully compatible with EVM chains (like Ethereum, BNB, and Polygon), Bitcoin, Solana, TON, Sui. Support for Cosmos (through IBC) and other chains is coming soon.
        
-   Native gas and supported ERC-20 tokens are represented as ZRC-20 tokens. A ZRC-20 token can be permissionlessly withdrawn back to its original chain.
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
