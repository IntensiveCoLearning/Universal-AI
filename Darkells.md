---
timezone: UTC+8
---

# Draken

**GitHub ID:** Darkells

**Telegram:** @Draken_Zeng

## Self-introduction

热爱技术

## Notes

<!-- Content_START -->
# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->
## **它到底是什么？**

ZetaChain是一个**独立的区块链**（不是以太坊L2，也不是跨链桥），核心目标就一条：  
**让不同链之间的交互像单链操作一样简单。**

-   比如：用比特币钱包里的BTC，直接打赏给以太坊地址上的创作者——全程只需1次确认，不用管gas费、不用等桥接。
    
-   本质是**把跨链复杂性藏在底层**，用户/开发者只看到结果。
    

## **它能干什么？**

1.  **Gas抽象**：
    
    -   用户只需持有ZETA代币，就能支付所有链的gas费（比如用ZETA付比特币矿工费+以太坊gas）。
        
    -   _亲测_：在测试网用0.01 BTC打赏以太坊地址，只扣了0.006 ZETA（约$0.003），没碰过BTC或ETH。
        
2.  **全链智能合约**：
    
    -   开发者用Solidity写代码，能直接读写其他链的状态（比如合约里查比特币余额）。
        
    -   _关键点_：不是“兼容所有链的虚拟机”，而是通过系统合约**安全地调用跨链数据**。
        
3.  **原子级跨链操作**：
    
    -   传统桥：BTC→WBTC→Swap→提现，任一环节失败资产可能卡住。
        
    -   ZetaChain：BTC→ETH一步完成，失败则全部回滚。
        

关于统一gas模型与官网例子测试的思考

指的是 **当在 ZetaChain 上发起跨链操作**，只需要 ZETA Gas，不需要准备其他链的 Gas。

但**当从外链（Base、BNB、Polygon）主动发交易时，必须使用外链的 Gas Token。**
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
